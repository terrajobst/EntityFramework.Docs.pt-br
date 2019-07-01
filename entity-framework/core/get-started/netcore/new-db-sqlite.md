---
title: Introdução ao .NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
description: Introdução ao .NET Core usando o Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e6996630e399659807d23304993c8e19c11ca6f5
ms.sourcegitcommit: 83c1e2fc034e5eb1fec1ebabc8d629ffcc7c0632
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67351336"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="9a43f-103">Introdução ao EF Core no aplicativo de console do .NET Core com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="9a43f-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="9a43f-104">Neste tutorial, você criará um aplicativo de console do .NET Core que executa acesso a dados em um banco de dados SQLite usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9a43f-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="9a43f-105">Você usará [migrações](xref:core/managing-schemas/migrations/index) para criar o banco de dados com base no modelo.</span><span class="sxs-lookup"><span data-stu-id="9a43f-105">You use [migrations](xref:core/managing-schemas/migrations/index) to create the database from the model.</span></span> <span data-ttu-id="9a43f-106">Veja [ASP.NET Core – Novo banco de dados](xref:core/get-started/aspnetcore/new-db) para ter uma versão do Visual Studio usando o ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="9a43f-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="9a43f-107">[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="9a43f-107">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a43f-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="9a43f-108">Prerequisites</span></span>

* [<span data-ttu-id="9a43f-109">SDK do .NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="9a43f-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="9a43f-110">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="9a43f-110">Create a new project</span></span>

* <span data-ttu-id="9a43f-111">Crie um novo projeto de console:</span><span class="sxs-lookup"><span data-stu-id="9a43f-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="9a43f-112">Alterar o diretório atual</span><span class="sxs-lookup"><span data-stu-id="9a43f-112">Change the current directory</span></span>

<span data-ttu-id="9a43f-113">Em etapas subsequentes, é preciso emitir comandos `dotnet` com relação ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9a43f-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="9a43f-114">Podemos alterar o diretório atual para o diretório do aplicativo como este:</span><span class="sxs-lookup"><span data-stu-id="9a43f-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="9a43f-115">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9a43f-115">Install Entity Framework Core</span></span>

<span data-ttu-id="9a43f-116">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="9a43f-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="9a43f-117">Este passo a passo usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="9a43f-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="9a43f-118">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9a43f-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="9a43f-119">Instalar Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="9a43f-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="9a43f-120">Execute `dotnet restore` para instalar os novos pacotes.</span><span class="sxs-lookup"><span data-stu-id="9a43f-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9a43f-121">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="9a43f-121">Create the model</span></span>

<span data-ttu-id="9a43f-122">Defina um contexto e classes de entidade que compõem seu modelo.</span><span class="sxs-lookup"><span data-stu-id="9a43f-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="9a43f-123">Crie um novo arquivo *Model.cs* com o seguinte conteúdo.</span><span class="sxs-lookup"><span data-stu-id="9a43f-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="9a43f-124">Dica: em um aplicativo real, você coloca cada classe em um arquivo separado e coloca a cadeia de conexão em um arquivo de configuração ou na variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="9a43f-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="9a43f-125">Para simplificar o tutorial, tudo está contido em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="9a43f-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="9a43f-126">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="9a43f-126">Create the database</span></span>

<span data-ttu-id="9a43f-127">Assim que você tiver um modelo, use as [migrações](xref:core/managing-schemas/migrations/index) para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9a43f-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="9a43f-128">Execute `dotnet ef migrations add InitialCreate` para realizar scaffolding de uma migração e criar o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="9a43f-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="9a43f-129">Execute `dotnet ef database update` para aplicar a nova migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9a43f-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="9a43f-130">Este comando cria o banco de dados antes de aplicar as migrações.</span><span class="sxs-lookup"><span data-stu-id="9a43f-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="9a43f-131">O banco de dados SQLite *blogging.db* está no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="9a43f-131">The *blogging.db* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="9a43f-132">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="9a43f-132">Use the model</span></span>

* <span data-ttu-id="9a43f-133">Abra *Program.cs* e substitua o conteúdo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="9a43f-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="9a43f-134">Teste o aplicativo do console.</span><span class="sxs-lookup"><span data-stu-id="9a43f-134">Test the app from the console.</span></span> <span data-ttu-id="9a43f-135">Veja a [observação do Visual Studio](#vs) para executar o aplicativo do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a43f-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="9a43f-136">Um blog é salvo no banco de dados, e os detalhes de todos os blogs são exibidos no console.</span><span class="sxs-lookup"><span data-stu-id="9a43f-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="9a43f-137">Alteração do modelo:</span><span class="sxs-lookup"><span data-stu-id="9a43f-137">Changing the model:</span></span>

- <span data-ttu-id="9a43f-138">Se você fizer alterações no modelo, poderá usar o comando `dotnet ef migrations add` para realizar scaffolding de uma nova [migração](xref:core/managing-schemas/migrations/index).</span><span class="sxs-lookup"><span data-stu-id="9a43f-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="9a43f-139">Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), é possível usar o comando `dotnet ef database update` para aplicar as alterações do esquema no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9a43f-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="9a43f-140">O EF Core usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9a43f-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="9a43f-141">O mecanismo de banco de dados do SQLite não dá suporte a determinadas alterações de esquema que têm suporte na maioria dos outros bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="9a43f-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="9a43f-142">Por exemplo, não há suporte para a operação `DropColumn`.</span><span class="sxs-lookup"><span data-stu-id="9a43f-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="9a43f-143">As migrações do EF Core geram código para essas operações.</span><span class="sxs-lookup"><span data-stu-id="9a43f-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="9a43f-144">Mas se você tentar aplicá-las a um banco de dados ou gerar um script, o EF Core gerará exceções.</span><span class="sxs-lookup"><span data-stu-id="9a43f-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="9a43f-145">Veja [Limitações do SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="9a43f-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="9a43f-146">Para novos desenvolvimentos, considere descartar o banco de dados e criar um novo em vez de usar migrações quando o modelo for alterado.</span><span class="sxs-lookup"><span data-stu-id="9a43f-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="9a43f-147">Executar usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9a43f-147">Run from Visual Studio</span></span>

<span data-ttu-id="9a43f-148">Para executar este exemplo do Visual Studio, você deve definir o diretório de trabalho manualmente para ser a raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="9a43f-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="9a43f-149">Se você não definir o diretório de trabalho, o seguinte `Microsoft.Data.Sqlite.SqliteException` será gerado: `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="9a43f-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="9a43f-150">Para definir o diretório de trabalho:</span><span class="sxs-lookup"><span data-stu-id="9a43f-150">To set the working directory:</span></span>

* <span data-ttu-id="9a43f-151">No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="9a43f-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="9a43f-152">Selecione a guia **Depurar** no painel esquerdo.</span><span class="sxs-lookup"><span data-stu-id="9a43f-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="9a43f-153">Defina o **Diretório de trabalho** como o diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="9a43f-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="9a43f-154">Salve as alterações.</span><span class="sxs-lookup"><span data-stu-id="9a43f-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a43f-155">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="9a43f-155">Additional Resources</span></span>

* [<span data-ttu-id="9a43f-156">Tutorial: introdução ao EF Core no ASP.NET Core com um novo banco de dados usando o SQLite</span><span class="sxs-lookup"><span data-stu-id="9a43f-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="9a43f-157">Tutorial: introdução às Razor Pages no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a43f-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="9a43f-158">Tutorial: Razor Pages com o Entity Framework Core no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a43f-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
