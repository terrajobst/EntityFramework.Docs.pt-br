---
title: Incluindo e excluindo tipos - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052596"
---
# <a name="including--excluding-types"></a><span data-ttu-id="beea1-102">Incluindo e excluindo tipos</span><span class="sxs-lookup"><span data-stu-id="beea1-102">Including & Excluding Types</span></span>

<span data-ttu-id="beea1-103">Incluindo um tipo no significa modelo EF com metadados sobre que tipo e tenta ler e gravar instâncias do/para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="beea1-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="beea1-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="beea1-104">Conventions</span></span>

<span data-ttu-id="beea1-105">Por convenção, os tipos que são expostos no `DbSet` propriedades de seu contexto, estão incluídas em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="beea1-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="beea1-106">Além disso, os tipos mencionados o `OnModelCreating` método também estão incluídos.</span><span class="sxs-lookup"><span data-stu-id="beea1-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="beea1-107">Por fim, todos os tipos que são encontrados por recursivamente explorar as propriedades de navegação de tipos descobertos também estão incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="beea1-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="beea1-108">**Por exemplo, na listagem de código a seguir são descobertos todos os três tipos:**</span><span class="sxs-lookup"><span data-stu-id="beea1-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="beea1-109">`Blog`porque ela é exposta em um `DbSet` propriedade no contexto</span><span class="sxs-lookup"><span data-stu-id="beea1-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="beea1-110">`Post`porque ele é descoberto por meio de `Blog.Posts` propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="beea1-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="beea1-111">`AuditEntry`porque ele é mencionado na`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="beea1-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="beea1-112">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="beea1-112">Data Annotations</span></span>

<span data-ttu-id="beea1-113">Você pode usar as anotações de dados para excluir um tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="beea1-113">You can use Data Annotations to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="beea1-114">API fluente</span><span class="sxs-lookup"><span data-stu-id="beea1-114">Fluent API</span></span>

<span data-ttu-id="beea1-115">Você pode usar a API fluente para excluir um tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="beea1-115">You can use the Fluent API to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
