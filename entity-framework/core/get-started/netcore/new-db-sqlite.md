---
title: "Introdução ao .NET Core – Novo banco de dados – EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: "Introdução ao .NET Core usando o Entity Framework Core"
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 3becf75e7a513a3aa18c3c2daf628b65327365b0
ms.sourcegitcommit: 0858f157b806f4a881b94ddbeecf1ece1d53e1e0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="1c5d9-104">Introdução ao EF Core no aplicativo de console do .NET Core com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="1c5d9-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="1c5d9-105">Neste passo a passo, você criará um aplicativo de console do .NET Core que executa acesso a dados básicos em um banco de dados SQLite usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="1c5d9-106">Você usará as migrações para criar o banco de dados de seu modelo.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="1c5d9-107">Veja [ASP.NET Core – Novo banco de dados](xref:core/get-started/aspnetcore/new-db) para ter uma versão do Visual Studio usando o ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!NOTE]  
> <span data-ttu-id="1c5d9-108">O [SDK do .NET Core](https://www.microsoft.com/net/download/core) não dá mais suporte a `project.json` ou ao Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-108">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="1c5d9-109">Recomendamos que você [migre do project.json para csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="1c5d9-109">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="1c5d9-110">Se você estiver usando o Visual Studio, recomendamos que você migre para o [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="1c5d9-110">If you are using Visual Studio, we recommend you migrate to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

> [!TIP]  
> <span data-ttu-id="1c5d9-111">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c5d9-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1c5d9-112">Prerequisites</span></span>

<span data-ttu-id="1c5d9-113">Você precisará atender aos pré-requisitos a seguir para concluir este passo a passo:</span><span class="sxs-lookup"><span data-stu-id="1c5d9-113">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="1c5d9-114">Um sistema operacional que dá suporte ao .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-114">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="1c5d9-115">[O SDK do .NET Core](https://www.microsoft.com/net/core) 2.0 (embora as instruções possam ser usadas para criar um aplicativo com uma versão anterior, com poucas modificações).</span><span class="sxs-lookup"><span data-stu-id="1c5d9-115">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="1c5d9-116">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="1c5d9-116">Create a new project</span></span>

* <span data-ttu-id="1c5d9-117">Crie uma nova pasta `ConsoleApp.SQLite` para seu projeto e use o comando `dotnet` para preenchê-la com um aplicativo .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-117">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="1c5d9-118">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1c5d9-118">Install Entity Framework Core</span></span>

<span data-ttu-id="1c5d9-119">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="1c5d9-120">Este passo a passo usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-120">This walkthrough uses SQLite.</span></span> <span data-ttu-id="1c5d9-121">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1c5d9-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="1c5d9-122">Instalar Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="1c5d9-122">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="1c5d9-123">Edite manualmente `ConsoleApp.SQLite.csproj` para adicionar um DotNetCliToolReference ao Microsoft.EntityFrameworkCore.Tools.DotNet:</span><span class="sxs-lookup"><span data-stu-id="1c5d9-123">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

<span data-ttu-id="1c5d9-124">`ConsoleApp.SQLite.csproj` deve conter os seguintes:</span><span class="sxs-lookup"><span data-stu-id="1c5d9-124">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="1c5d9-125">Observação: os números de versão usados acima estavam corretos no momento da publicação.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-125">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="1c5d9-126">Execute `dotnet restore` para instalar os novos pacotes.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-126">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="1c5d9-127">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="1c5d9-127">Create the model</span></span>

<span data-ttu-id="1c5d9-128">Defina um contexto e classes de entidade que compõem seu modelo.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-128">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="1c5d9-129">Crie um novo arquivo *Model.cs* com o seguinte conteúdo.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-129">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="1c5d9-130">Dica: em um aplicativo real, você colocaria cada classe em um arquivo separado, e colocaria a cadeia de conexão em um arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-130">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="1c5d9-131">Para simplificar o tutorial, estamos colocando tudo em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-131">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="1c5d9-132">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="1c5d9-132">Create the database</span></span>

<span data-ttu-id="1c5d9-133">Assim que você tiver um modelo, use as [migrações](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-133">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="1c5d9-134">Execute `dotnet ef migrations add InitialCreate` para realizar scaffolding de uma migração e criar o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-134">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="1c5d9-135">Execute `dotnet ef database update` para aplicar a nova migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-135">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="1c5d9-136">Este comando cria o banco de dados antes de aplicar as migrações.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-136">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="1c5d9-137">Ao usar caminhos relativos com o SQLite, o caminho será relativo ao assembly principal do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-137">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="1c5d9-138">Neste exemplo, o binário principal é `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, portanto, o banco de dados SQLite estará em `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-138">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="1c5d9-139">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="1c5d9-139">Use your model</span></span>

* <span data-ttu-id="1c5d9-140">Abra *Program.cs* e substitua o conteúdo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="1c5d9-140">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="1c5d9-141">Teste o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1c5d9-141">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="1c5d9-142">Um blog é salvo no banco de dados, e os detalhes de todos os blogs são exibidos no console.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-142">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="1c5d9-143">Alteração do modelo:</span><span class="sxs-lookup"><span data-stu-id="1c5d9-143">Changing the model:</span></span>

- <span data-ttu-id="1c5d9-144">Se você fizer alterações em seu modelo, use o comando `dotnet ef migrations add` para realizar o scaffolding de uma nova [migração](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para fazer as alterações correspondentes de esquema no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-144">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="1c5d9-145">Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), use o comando `dotnet ef database update` para aplicar as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-145">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="1c5d9-146">O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-146">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="1c5d9-147">O SQLite não oferece suporte a todas as migrações (alterações de esquema) devido às limitações no SQLite.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-147">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="1c5d9-148">Veja [Limitações do SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="1c5d9-148">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="1c5d9-149">Para novos desenvolvimentos, considere descartar o banco de dados e criar um novo em vez de usar migrações quando o modelo for alterado.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-149">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c5d9-150">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="1c5d9-150">Additional Resources</span></span>

* <span data-ttu-id="1c5d9-151">[.Net Core – Novo banco de dados com SQLite](xref:core/get-started/netcore/new-db-sqlite) – um tutorial EF de console de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="1c5d9-151">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="1c5d9-152">Introdução ao ASP.NET Core MVC no Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="1c5d9-152">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="1c5d9-153">Introdução ao ASP.NET Core MVC com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c5d9-153">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="1c5d9-154">Introdução ao ASP.NET Core e ao Entity Framework Core usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c5d9-154">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
