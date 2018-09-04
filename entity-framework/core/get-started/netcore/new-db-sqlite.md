---
title: Introdução ao .NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
description: Introdução ao .NET Core usando o Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993686"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="bd580-103">Introdução ao EF Core no aplicativo de console do .NET Core com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="bd580-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="bd580-104">Neste tutorial, você criará um aplicativo de console do .NET Core que executa acesso a dados em um banco de dados SQLite usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="bd580-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="bd580-105">Você usará migrações para criar o banco de dados com base no modelo.</span><span class="sxs-lookup"><span data-stu-id="bd580-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="bd580-106">Veja [ASP.NET Core – Novo banco de dados](xref:core/get-started/aspnetcore/new-db) para ter uma versão do Visual Studio usando o ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="bd580-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="bd580-107">Exiba o exemplo deste artigo no GitHub (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite)).</span><span class="sxs-lookup"><span data-stu-id="bd580-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd580-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="bd580-108">Prerequisites</span></span>

* [<span data-ttu-id="bd580-109">SDK do .NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="bd580-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="bd580-110">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="bd580-110">Create a new project</span></span>

* <span data-ttu-id="bd580-111">Crie um novo projeto de console:</span><span class="sxs-lookup"><span data-stu-id="bd580-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a><span data-ttu-id="bd580-112">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="bd580-112">Install Entity Framework Core</span></span>

<span data-ttu-id="bd580-113">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="bd580-113">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="bd580-114">Este passo a passo usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd580-114">This walkthrough uses SQLite.</span></span> <span data-ttu-id="bd580-115">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="bd580-115">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="bd580-116">Instalar Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="bd580-116">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="bd580-117">Execute `dotnet restore` para instalar os novos pacotes.</span><span class="sxs-lookup"><span data-stu-id="bd580-117">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="bd580-118">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="bd580-118">Create the model</span></span>

<span data-ttu-id="bd580-119">Defina um contexto e classes de entidade que compõem seu modelo.</span><span class="sxs-lookup"><span data-stu-id="bd580-119">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="bd580-120">Crie um novo arquivo *Model.cs* com o seguinte conteúdo.</span><span class="sxs-lookup"><span data-stu-id="bd580-120">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="bd580-121">Dica: em um aplicativo real, você coloca cada classe em um arquivo separado e coloca a cadeia de conexão em um arquivo de configuração ou na variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="bd580-121">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="bd580-122">Para simplificar o tutorial, tudo está contido em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="bd580-122">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="bd580-123">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="bd580-123">Create the database</span></span>

<span data-ttu-id="bd580-124">Assim que você tiver um modelo, use as [migrações](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd580-124">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="bd580-125">Execute `dotnet ef migrations add InitialCreate` para realizar scaffolding de uma migração e criar o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="bd580-125">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="bd580-126">Execute `dotnet ef database update` para aplicar a nova migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd580-126">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="bd580-127">Este comando cria o banco de dados antes de aplicar as migrações.</span><span class="sxs-lookup"><span data-stu-id="bd580-127">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="bd580-128">O banco de dados SQLite *blogging.db*\* está no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="bd580-128">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="bd580-129">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="bd580-129">Use the model</span></span>

* <span data-ttu-id="bd580-130">Abra *Program.cs* e substitua o conteúdo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="bd580-130">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="bd580-131">Teste o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="bd580-131">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="bd580-132">Um blog é salvo no banco de dados, e os detalhes de todos os blogs são exibidos no console.</span><span class="sxs-lookup"><span data-stu-id="bd580-132">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="bd580-133">Alteração do modelo:</span><span class="sxs-lookup"><span data-stu-id="bd580-133">Changing the model:</span></span>

- <span data-ttu-id="bd580-134">Se você fizer alterações no modelo, poderá usar o comando `dotnet ef migrations add` para realizar scaffolding de uma nova [migração](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="bd580-134">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="bd580-135">Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), é possível usar o comando `dotnet ef database update` para aplicar as alterações do esquema no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd580-135">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="bd580-136">O EF Core usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd580-136">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="bd580-137">O mecanismo de banco de dados do SQLite não dá suporte a determinadas alterações de esquema que têm suporte na maioria dos outros bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="bd580-137">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="bd580-138">Por exemplo, não há suporte para a operação `DropColumn`.</span><span class="sxs-lookup"><span data-stu-id="bd580-138">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="bd580-139">As migrações do EF Core geram código para essas operações.</span><span class="sxs-lookup"><span data-stu-id="bd580-139">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="bd580-140">Mas se você tentar aplicá-las a um banco de dados ou gerar um script, o EF Core gerará exceções.</span><span class="sxs-lookup"><span data-stu-id="bd580-140">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="bd580-141">Veja [Limitações do SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="bd580-141">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="bd580-142">Para novos desenvolvimentos, considere descartar o banco de dados e criar um novo em vez de usar migrações quando o modelo for alterado.</span><span class="sxs-lookup"><span data-stu-id="bd580-142">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd580-143">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="bd580-143">Additional Resources</span></span>

* [<span data-ttu-id="bd580-144">Introdução ao ASP.NET Core MVC no Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="bd580-144">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="bd580-145">Introdução ao ASP.NET Core MVC com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd580-145">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="bd580-146">Introdução ao ASP.NET Core e ao Entity Framework Core usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd580-146">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
