---
title: Introdução ao .NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Introdução ao .NET Core usando o Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911496"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="7047f-104">Introdução ao EF Core no aplicativo de console do .NET Core com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="7047f-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="7047f-105">Neste passo a passo, você criará um aplicativo de console do .NET Core que executa acesso a dados em um banco de dados SQLite usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7047f-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="7047f-106">Você usará migrações para criar o banco de dados com base no modelo.</span><span class="sxs-lookup"><span data-stu-id="7047f-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="7047f-107">Veja [ASP.NET Core – Novo banco de dados](xref:core/get-started/aspnetcore/new-db) para ter uma versão do Visual Studio usando o ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="7047f-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="7047f-108">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="7047f-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7047f-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="7047f-109">Prerequisites</span></span>

<span data-ttu-id="7047f-110">[O SDK do .NET Core](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="7047f-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="7047f-111">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="7047f-111">Create a new project</span></span>

* <span data-ttu-id="7047f-112">Crie um novo projeto de console:</span><span class="sxs-lookup"><span data-stu-id="7047f-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="7047f-113">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7047f-113">Install Entity Framework Core</span></span>

<span data-ttu-id="7047f-114">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="7047f-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="7047f-115">Este passo a passo usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="7047f-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="7047f-116">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="7047f-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="7047f-117">Instalar Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="7047f-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="7047f-118">Execute `dotnet restore` para instalar os novos pacotes.</span><span class="sxs-lookup"><span data-stu-id="7047f-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="7047f-119">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="7047f-119">Create the model</span></span>

<span data-ttu-id="7047f-120">Defina um contexto e classes de entidade que compõem seu modelo.</span><span class="sxs-lookup"><span data-stu-id="7047f-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="7047f-121">Crie um novo arquivo *Model.cs* com o seguinte conteúdo.</span><span class="sxs-lookup"><span data-stu-id="7047f-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="7047f-122">Dica: em um aplicativo real, você coloca cada classe em um arquivo separado e coloca a cadeia de conexão em um arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="7047f-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="7047f-123">Para simplificar o tutorial, tudo está contido em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="7047f-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="7047f-124">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="7047f-124">Create the database</span></span>

<span data-ttu-id="7047f-125">Assim que você tiver um modelo, use as [migrações](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7047f-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="7047f-126">Execute `dotnet ef migrations add InitialCreate` para realizar scaffolding de uma migração e criar o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="7047f-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="7047f-127">Execute `dotnet ef database update` para aplicar a nova migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7047f-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="7047f-128">Este comando cria o banco de dados antes de aplicar as migrações.</span><span class="sxs-lookup"><span data-stu-id="7047f-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="7047f-129">O banco de dados SQLite *blogging.db*\* está no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="7047f-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="7047f-130">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="7047f-130">Use your model</span></span>

* <span data-ttu-id="7047f-131">Abra *Program.cs* e substitua o conteúdo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="7047f-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="7047f-132">Teste o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7047f-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="7047f-133">Um blog é salvo no banco de dados, e os detalhes de todos os blogs são exibidos no console.</span><span class="sxs-lookup"><span data-stu-id="7047f-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="7047f-134">Alteração do modelo:</span><span class="sxs-lookup"><span data-stu-id="7047f-134">Changing the model:</span></span>

- <span data-ttu-id="7047f-135">Se você fizer alterações em seu modelo, use o comando `dotnet ef migrations add` para realizar o scaffolding de uma nova [migração](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para fazer as alterações correspondentes de esquema no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7047f-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="7047f-136">Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), use o comando `dotnet ef database update` para aplicar as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7047f-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="7047f-137">O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7047f-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="7047f-138">O SQLite não oferece suporte a todas as migrações (alterações de esquema) devido às limitações no SQLite.</span><span class="sxs-lookup"><span data-stu-id="7047f-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="7047f-139">Veja [Limitações do SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="7047f-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="7047f-140">Para novos desenvolvimentos, considere descartar o banco de dados e criar um novo em vez de usar migrações quando o modelo for alterado.</span><span class="sxs-lookup"><span data-stu-id="7047f-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7047f-141">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7047f-141">Additional Resources</span></span>

* <span data-ttu-id="7047f-142">[.Net Core – Novo banco de dados com SQLite](xref:core/get-started/netcore/new-db-sqlite) – um tutorial EF de console de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="7047f-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="7047f-143">Introdução ao ASP.NET Core MVC no Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="7047f-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="7047f-144">Introdução ao ASP.NET Core MVC com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7047f-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="7047f-145">Introdução ao ASP.NET Core e ao Entity Framework Core usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7047f-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
