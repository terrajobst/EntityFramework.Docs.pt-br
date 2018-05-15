---
title: Introdução ao .NET Framework – Banco de dados existente – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="fc7f6-102">Introdução ao EF Core no .NET Framework com um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="fc7f6-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="fc7f6-103">Neste passo a passo, você compilará um aplicativo de console que executa acesso a dados básicos em um banco de dados do Microsoft SQL Server usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="fc7f6-104">Você usará engenharia reversa para criar um modelo do Entity Framework com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="fc7f6-105">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc7f6-106">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="fc7f6-106">Prerequisites</span></span>

<span data-ttu-id="fc7f6-107">Você precisará atender aos pré-requisitos a seguir para concluir este passo a passo:</span><span class="sxs-lookup"><span data-stu-id="fc7f6-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="fc7f6-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="fc7f6-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="fc7f6-109">Versão mais recente do Gerenciador de Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="fc7f6-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="fc7f6-110">Versão mais recente do Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="fc7f6-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="fc7f6-111">Banco de dados de blog</span><span class="sxs-lookup"><span data-stu-id="fc7f6-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="fc7f6-112">Banco de dados de blog</span><span class="sxs-lookup"><span data-stu-id="fc7f6-112">Blogging database</span></span>

<span data-ttu-id="fc7f6-113">Este tutorial usa um banco de dados de **blog** em sua instância do LocalDb como o banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="fc7f6-114">Se você já tiver criado o banco de dados de **blog** como parte de outro tutorial, ignore estas etapas.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="fc7f6-115">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fc7f6-115">Open Visual Studio</span></span>

* <span data-ttu-id="fc7f6-116">Ferramentas > Conectar ao Banco de Dados...</span><span class="sxs-lookup"><span data-stu-id="fc7f6-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="fc7f6-117">Selecione **Microsoft SQL Server** e clique em **Continuar**</span><span class="sxs-lookup"><span data-stu-id="fc7f6-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="fc7f6-118">Insira **(localdb)\mssqllocaldb** como o **Nome do Servidor**</span><span class="sxs-lookup"><span data-stu-id="fc7f6-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="fc7f6-119">Insira **mestre** como o **Nome do Banco de Dados** e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="fc7f6-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="fc7f6-120">O banco de dados mestre agora é exibido em **Conexões de Dados** no **Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="fc7f6-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="fc7f6-121">Clique com botão direito do mouse no banco de dados em **Gerenciador de Servidores** e selecione **Nova Consulta**</span><span class="sxs-lookup"><span data-stu-id="fc7f6-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="fc7f6-122">Copie o script, listado abaixo, no editor de consultas</span><span class="sxs-lookup"><span data-stu-id="fc7f6-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="fc7f6-123">Clique com o botão direito do mouse no editor de consultas e selecione **Executar**</span><span class="sxs-lookup"><span data-stu-id="fc7f6-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="fc7f6-124">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="fc7f6-124">Create a new project</span></span>

* <span data-ttu-id="fc7f6-125">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fc7f6-125">Open Visual Studio</span></span>

* <span data-ttu-id="fc7f6-126">Arquivo > Novo > Projeto...</span><span class="sxs-lookup"><span data-stu-id="fc7f6-126">File > New > Project...</span></span>

* <span data-ttu-id="fc7f6-127">No menu à esquerda, selecione Modelos > Visual C# > Windows</span><span class="sxs-lookup"><span data-stu-id="fc7f6-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="fc7f6-128">Selecione o modelo de projeto do **Aplicativo de Console**</span><span class="sxs-lookup"><span data-stu-id="fc7f6-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="fc7f6-129">Certifique-se de que esteja direcionando para o **.NET Framework 4.5.1** ou posterior</span><span class="sxs-lookup"><span data-stu-id="fc7f6-129">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="fc7f6-130">Dê um nome ao projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="fc7f6-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="fc7f6-131">Instalar o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fc7f6-131">Install Entity Framework</span></span>

<span data-ttu-id="fc7f6-132">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="fc7f6-133">Este passo a passo usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="fc7f6-134">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="fc7f6-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="fc7f6-135">Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="fc7f6-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="fc7f6-136">Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="fc7f6-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="fc7f6-137">Para habilitar a engenharia reversa de um banco de dados existente, precisamos instalar outros pacotes.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="fc7f6-138">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="fc7f6-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="fc7f6-139">Fazer engenharia reversa em seu modelo</span><span class="sxs-lookup"><span data-stu-id="fc7f6-139">Reverse engineer your model</span></span>

<span data-ttu-id="fc7f6-140">Agora é hora de criar o modelo EF com base em seu banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="fc7f6-141">Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="fc7f6-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="fc7f6-142">Execute o seguinte comando para criar um modelo do banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="fc7f6-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="fc7f6-143">O processo de engenharia reversa criou classes de entidade e um contexto derivado com base no esquema do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="fc7f6-144">As classes de entidade são objetos C# simples que representam os dados que você vai consultar e salvar.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

<span data-ttu-id="fc7f6-145">O contexto representa uma sessão com o banco de dados e permite que você veja e salve as instâncias das classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a><span data-ttu-id="fc7f6-146">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="fc7f6-146">Use your model</span></span>

<span data-ttu-id="fc7f6-147">Agora você pode usar o modelo para executar o acesso aos dados.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="fc7f6-148">Abra *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="fc7f6-148">Open *Program.cs*</span></span>

* <span data-ttu-id="fc7f6-149">Substitua o conteúdo do arquivo pelo seguinte código</span><span class="sxs-lookup"><span data-stu-id="fc7f6-149">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="fc7f6-150">Depurar > Iniciar Sem Depuração</span><span class="sxs-lookup"><span data-stu-id="fc7f6-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="fc7f6-151">Você verá que um blog é salvo no banco de dados, e os detalhes de todos os blogs são impressos no console.</span><span class="sxs-lookup"><span data-stu-id="fc7f6-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![imagem](_static/output-existing-db.png)
