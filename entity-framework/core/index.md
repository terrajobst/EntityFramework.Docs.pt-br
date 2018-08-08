---
title: Visão Geral – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: d2c40356fc3b37251f95b08ee8bf07ed4eab7b80
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614302"
---
# <a name="entity-framework-core"></a><span data-ttu-id="5f463-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5f463-102">Entity Framework Core</span></span>

<span data-ttu-id="5f463-103">O Entity Framework (EF) Core é uma versão de multiplaforma leve, extensível e de plataforma cruzada da popular tecnologia de acesso a dados do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5f463-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="5f463-104">O EF Core pode servir como um ORM (Mapeador de Objeto Relacional), permitindo que os desenvolvedores de .NET trabalhem com um banco de dados usando objetos do .NET e eliminando a necessidade de grande parte do código de acesso aos dados que eles geralmente precisam escrever.</span><span class="sxs-lookup"><span data-stu-id="5f463-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="5f463-105">O EF Core é compatível com vários mecanismos de banco de dados, consulte detalhes em [Provedores de Banco de Dados](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="5f463-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="5f463-106">Se você quiser aprender escrevendo código, recomendamos um dos nossos guias de [Introdução](get-started/index.md) para ajudá-lo a começar a usar o EF Core.</span><span class="sxs-lookup"><span data-stu-id="5f463-106">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="5f463-107">Novidades no EF Core</span><span class="sxs-lookup"><span data-stu-id="5f463-107">What is new in EF Core</span></span>

<span data-ttu-id="5f463-108">Se você estiver familiarizado com EF Core e quiser ir diretamente para os detalhes dos lançamentos mais recentes:</span><span class="sxs-lookup"><span data-stu-id="5f463-108">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="5f463-109">**[Novidades no EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="5f463-109">**[What is new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="5f463-110">**[Fazer upgrade dos aplicativos existentes para o EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="5f463-110">**[Upgrading existing applications to EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="5f463-111">Obter o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5f463-111">Get Entity Framework Core</span></span>

<span data-ttu-id="5f463-112">[Instale o pacote NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) para o provedor de banco de dados que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="5f463-112">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="5f463-113">Por exemplo, para instalar o provedor do SQL Server no desenvolvimento de multiplaforma com a ferramenta `dotnet` na linha de comando:</span><span class="sxs-lookup"><span data-stu-id="5f463-113">For example, to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="5f463-114">Ou, no Visual Studio, usando o Console do Gerenciador de Pacotes:</span><span class="sxs-lookup"><span data-stu-id="5f463-114">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="5f463-115">Consulte [Provedores de Banco de Dados](providers/index.md) para obter informações sobre provedores disponíveis e [Instalar o EF Core](get-started/install/index.md) para etapas de instalação mais detalhadas.</span><span class="sxs-lookup"><span data-stu-id="5f463-115">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="5f463-116">O modelo</span><span class="sxs-lookup"><span data-stu-id="5f463-116">The Model</span></span>

<span data-ttu-id="5f463-117">Com o EF Core, o acesso a dados é executado usando um modelo.</span><span class="sxs-lookup"><span data-stu-id="5f463-117">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="5f463-118">Um modelo é composto por classes de entidade e um contexto derivado que representa uma sessão com o banco de dados, permitindo que você consulte e salve os dados.</span><span class="sxs-lookup"><span data-stu-id="5f463-118">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="5f463-119">Consulte [Criar um modelo](modeling/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="5f463-119">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="5f463-120">Você pode gerar um modelo de um banco de dados existente, codificar manualmente um modelo para coincidir com seu banco de dados ou usar Migrações do EF para criar um banco de dados do seu modelo (e evolui-lo conforme o modelo muda com o tempo).</span><span class="sxs-lookup"><span data-stu-id="5f463-120">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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

## <a name="querying"></a><span data-ttu-id="5f463-121">Consultar</span><span class="sxs-lookup"><span data-stu-id="5f463-121">Querying</span></span>

<span data-ttu-id="5f463-122">Instâncias de suas classes de entidade são recuperadas do banco de dados usando a LINQ (Consulta Integrada à Linguagem).</span><span class="sxs-lookup"><span data-stu-id="5f463-122">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="5f463-123">Consulte [Consultar dados](querying/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="5f463-123">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="5f463-124">Salvar Dados</span><span class="sxs-lookup"><span data-stu-id="5f463-124">Saving Data</span></span>

<span data-ttu-id="5f463-125">Dados são criados, excluídos e modificados no banco de dados usando as instâncias de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="5f463-125">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="5f463-126">Consulte [Salvar dados](saving/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="5f463-126">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
