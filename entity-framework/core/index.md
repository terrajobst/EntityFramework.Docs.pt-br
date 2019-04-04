---
title: Visão Geral – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: fa0695be29668789a179f9a0d6330f3361dbac29
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2019
ms.locfileid: "58131426"
---
# <a name="entity-framework-core"></a><span data-ttu-id="6cc88-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6cc88-102">Entity Framework Core</span></span>

<span data-ttu-id="6cc88-103">O EF (Entity Framework) Core é uma versão leve, extensível, de [software livre](https://github.com/aspnet/EntityFrameworkCore) e multiplataforma da popular tecnologia de acesso a dados do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6cc88-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="6cc88-104">O EF Core pode servir como um ORM (Mapeador de Objeto Relacional), permitindo que os desenvolvedores de .NET trabalhem com um banco de dados usando objetos do .NET e eliminando a necessidade de grande parte do código de acesso aos dados que eles geralmente precisam escrever.</span><span class="sxs-lookup"><span data-stu-id="6cc88-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="6cc88-105">O EF Core é compatível com vários mecanismos de banco de dados, consulte detalhes em [Provedores de Banco de Dados](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="6cc88-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="6cc88-106">O modelo</span><span class="sxs-lookup"><span data-stu-id="6cc88-106">The Model</span></span>

<span data-ttu-id="6cc88-107">Com o EF Core, o acesso a dados é executado usando um modelo.</span><span class="sxs-lookup"><span data-stu-id="6cc88-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="6cc88-108">Um modelo é composto por classes de entidade e um objeto de contexto que representa uma sessão com o banco de dados, o que permite consultar e salve dados.</span><span class="sxs-lookup"><span data-stu-id="6cc88-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="6cc88-109">Consulte [Criar um modelo](modeling/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="6cc88-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="6cc88-110">Você pode gerar um modelo de um banco de dados existente, codificar manualmente um modelo para coincidir com o banco de dados ou usar migrações da classe EF para criar um banco de dados do modelo e, em seguida, desenvolvê-lo à medida que o modelo for alterado com o tempo.</span><span class="sxs-lookup"><span data-stu-id="6cc88-110">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model, and then evolve it as your model changes over time.</span></span>

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
            optionsBuilder.UseSqlServer(
                @"Server=(localdb)\mssqllocaldb;Database=Blogging;Integrated Security=True");
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

## <a name="querying"></a><span data-ttu-id="6cc88-111">Consultar</span><span class="sxs-lookup"><span data-stu-id="6cc88-111">Querying</span></span>

<span data-ttu-id="6cc88-112">Instâncias de suas classes de entidade são recuperadas do banco de dados usando a LINQ (Consulta Integrada à Linguagem).</span><span class="sxs-lookup"><span data-stu-id="6cc88-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="6cc88-113">Consulte [Consultar dados](querying/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="6cc88-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="6cc88-114">Salvar Dados</span><span class="sxs-lookup"><span data-stu-id="6cc88-114">Saving Data</span></span>

<span data-ttu-id="6cc88-115">Dados são criados, excluídos e modificados no banco de dados usando as instâncias de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="6cc88-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="6cc88-116">Consulte [Salvar dados](saving/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="6cc88-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="6cc88-117">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="6cc88-117">Next steps</span></span>

<span data-ttu-id="6cc88-118">Para ver tutoriais introdutórios, confira [Introdução ao Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="6cc88-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

