---
title: Introdução ao .NET Framework – Novo banco de dados – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="e0a66-102">Introdução ao EF Core no .NET Framework com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="e0a66-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="e0a66-103">Neste passo a passo, você compilará um aplicativo de console que executa acesso a dados básicos em um banco de dados do Microsoft SQL Server usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e0a66-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="e0a66-104">Você usará as migrações para criar o banco de dados de seu modelo.</span><span class="sxs-lookup"><span data-stu-id="e0a66-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="e0a66-105">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="e0a66-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0a66-106">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e0a66-106">Prerequisites</span></span>

<span data-ttu-id="e0a66-107">Você precisará atender aos pré-requisitos a seguir para concluir este passo a passo:</span><span class="sxs-lookup"><span data-stu-id="e0a66-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="e0a66-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e0a66-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="e0a66-109">Versão mais recente do Gerenciador de Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="e0a66-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="e0a66-110">Versão mais recente do Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="e0a66-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="e0a66-111">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="e0a66-111">Create a new project</span></span>

* <span data-ttu-id="e0a66-112">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0a66-112">Open Visual Studio</span></span>

* <span data-ttu-id="e0a66-113">Arquivo > Novo > Projeto...</span><span class="sxs-lookup"><span data-stu-id="e0a66-113">File > New > Project...</span></span>

* <span data-ttu-id="e0a66-114">No menu à esquerda, selecione Modelos > Visual C# > Área de Trabalho Clássica do Windows</span><span class="sxs-lookup"><span data-stu-id="e0a66-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="e0a66-115">Selecione o modelo de projeto **Aplicativo de console (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="e0a66-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="e0a66-116">Certifique-se de que esteja direcionando para o **.NET Framework 4.5.1** ou posterior</span><span class="sxs-lookup"><span data-stu-id="e0a66-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="e0a66-117">Dê um nome ao projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="e0a66-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="e0a66-118">Instalar o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e0a66-118">Install Entity Framework</span></span>

<span data-ttu-id="e0a66-119">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="e0a66-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="e0a66-120">Este passo a passo usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e0a66-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="e0a66-121">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e0a66-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="e0a66-122">Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="e0a66-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="e0a66-123">Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="e0a66-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="e0a66-124">Mais adiante neste passo a passo, usaremos também algumas Entity Framework Tools para manter o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e0a66-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="e0a66-125">Portanto, também instalaremos o pacote de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="e0a66-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="e0a66-126">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="e0a66-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="e0a66-127">Criar seu modelo</span><span class="sxs-lookup"><span data-stu-id="e0a66-127">Create your model</span></span>

<span data-ttu-id="e0a66-128">Agora é hora de definir um contexto e classes de entidade que compõem seu modelo.</span><span class="sxs-lookup"><span data-stu-id="e0a66-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="e0a66-129">Projeto > Adicionar Classe...</span><span class="sxs-lookup"><span data-stu-id="e0a66-129">Project > Add Class...</span></span>

* <span data-ttu-id="e0a66-130">Insira *Model.cs* como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="e0a66-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="e0a66-131">Substitua o conteúdo do arquivo pelo seguinte código</span><span class="sxs-lookup"><span data-stu-id="e0a66-131">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> <span data-ttu-id="e0a66-132">Em um aplicativo real, você colocaria cada classe em um arquivo separado, e colocaria a cadeia de conexão no arquivo `App.Config`, e a leria usando `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="e0a66-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="e0a66-133">Para simplificar, estamos colocando tudo em um único arquivo de código para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="e0a66-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="e0a66-134">Criar seu banco de dados</span><span class="sxs-lookup"><span data-stu-id="e0a66-134">Create your database</span></span>

<span data-ttu-id="e0a66-135">Agora que você tem um modelo, use as migrações para criar um banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="e0a66-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="e0a66-136">Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="e0a66-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="e0a66-137">Execute `Add-Migration MyFirstMigration` para realizar scaffolding de uma migração a fim de criar o conjunto inicial de tabelas para seu modelo.</span><span class="sxs-lookup"><span data-stu-id="e0a66-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="e0a66-138">Execute `Update-Database` para aplicar a nova migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e0a66-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="e0a66-139">Como o banco de dados ainda não existe, ele será criado para você antes que a migração seja aplicada.</span><span class="sxs-lookup"><span data-stu-id="e0a66-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="e0a66-140">Se você fizer alterações futuras em seu modelo, use o comando `Add-Migration` para realizar o scaffolding de uma nova migração para fazer as alterações correspondentes de esquema no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e0a66-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="e0a66-141">Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), use o comando `Update-Database` para aplicar as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e0a66-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="e0a66-142">O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e0a66-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="e0a66-143">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="e0a66-143">Use your model</span></span>

<span data-ttu-id="e0a66-144">Agora você pode usar o modelo para executar o acesso aos dados.</span><span class="sxs-lookup"><span data-stu-id="e0a66-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="e0a66-145">Abra *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="e0a66-145">Open *Program.cs*</span></span>

* <span data-ttu-id="e0a66-146">Substitua o conteúdo do arquivo pelo seguinte código</span><span class="sxs-lookup"><span data-stu-id="e0a66-146">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="e0a66-147">Depurar > Iniciar Sem Depuração</span><span class="sxs-lookup"><span data-stu-id="e0a66-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="e0a66-148">Você verá que um blog é salvo no banco de dados, e os detalhes de todos os blogs são impressos no console.</span><span class="sxs-lookup"><span data-stu-id="e0a66-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![imagem](_static/output-new-db.png)
