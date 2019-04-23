---
title: Incluir e excluir tipos – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: f533b24312af37634ce4957e43c39ce776bf0bf0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929791"
---
# <a name="including--excluding-types"></a><span data-ttu-id="f7be3-102">Incluir e excluir tipos</span><span class="sxs-lookup"><span data-stu-id="f7be3-102">Including & Excluding Types</span></span>

<span data-ttu-id="f7be3-103">Incluindo os meios de modelo EF tem metadados sobre que tipo e tentarão ler e gravar as instâncias de banco de dados de/para um tipo.</span><span class="sxs-lookup"><span data-stu-id="f7be3-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="f7be3-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="f7be3-104">Conventions</span></span>

<span data-ttu-id="f7be3-105">Por convenção, os tipos que são expostos no `DbSet` propriedades em seu contexto estão incluídas em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="f7be3-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="f7be3-106">Além disso, os tipos que são mencionadas no `OnModelCreating` método também estão incluídos.</span><span class="sxs-lookup"><span data-stu-id="f7be3-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="f7be3-107">Por fim, todos os tipos que são encontrados pelo recursivamente Explorando as propriedades de navegação de tipos descobertos também estão incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="f7be3-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="f7be3-108">**Por exemplo, na listagem de código a seguir são descobertos todos os três tipos:**</span><span class="sxs-lookup"><span data-stu-id="f7be3-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="f7be3-109">`Blog` porque ele é exposto em um `DbSet` propriedade no contexto</span><span class="sxs-lookup"><span data-stu-id="f7be3-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="f7be3-110">`Post` porque ele é descoberto por meio de `Blog.Posts` propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="f7be3-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="f7be3-111">`AuditEntry` porque ele é mencionado na `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="f7be3-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="f7be3-112">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="f7be3-112">Data Annotations</span></span>

<span data-ttu-id="f7be3-113">Você pode usar anotações de dados para excluir um tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="f7be3-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="f7be3-114">API fluente</span><span class="sxs-lookup"><span data-stu-id="f7be3-114">Fluent API</span></span>

<span data-ttu-id="f7be3-115">Você pode usar a API Fluent para excluir um tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="f7be3-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
