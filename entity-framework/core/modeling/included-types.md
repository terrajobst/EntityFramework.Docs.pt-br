---
title: Incluindo & excluindo tipos-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197381"
---
# <a name="including--excluding-types"></a><span data-ttu-id="7daef-102">Incluindo & excluindo tipos</span><span class="sxs-lookup"><span data-stu-id="7daef-102">Including & Excluding Types</span></span>

<span data-ttu-id="7daef-103">A inclusão de um tipo no modelo significa que o EF tem metadados sobre esse tipo e tentará ler e gravar instâncias de/para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7daef-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="7daef-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="7daef-104">Conventions</span></span>

<span data-ttu-id="7daef-105">Por convenção, os tipos expostos em `DbSet` Propriedades no contexto são incluídos em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="7daef-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="7daef-106">Além disso, os `OnModelCreating` tipos mencionados no método também são incluídos.</span><span class="sxs-lookup"><span data-stu-id="7daef-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="7daef-107">Finalmente, todos os tipos que são encontrados pela exploração recursiva das propriedades de navegação de tipos descobertos também são incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="7daef-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="7daef-108">**Por exemplo, na listagem de código a seguir, todos os três tipos são descobertos:**</span><span class="sxs-lookup"><span data-stu-id="7daef-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="7daef-109">`Blog`Porque ele é exposto em uma `DbSet` Propriedade no contexto</span><span class="sxs-lookup"><span data-stu-id="7daef-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="7daef-110">`Post`Porque ele é descoberto por meio `Blog.Posts` da propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="7daef-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="7daef-111">`AuditEntry`Porque ele é mencionado em`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="7daef-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="7daef-112">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="7daef-112">Data Annotations</span></span>

<span data-ttu-id="7daef-113">Você pode usar as anotações de dados para excluir um tipo do modelo.</span><span class="sxs-lookup"><span data-stu-id="7daef-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="7daef-114">API fluente</span><span class="sxs-lookup"><span data-stu-id="7daef-114">Fluent API</span></span>

<span data-ttu-id="7daef-115">Você pode usar a API fluente para excluir um tipo do modelo.</span><span class="sxs-lookup"><span data-stu-id="7daef-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
