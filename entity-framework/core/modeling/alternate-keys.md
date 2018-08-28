---
title: Chaves alternativas – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996965"
---
# <a name="alternate-keys"></a><span data-ttu-id="93157-102">Chaves alternativas</span><span class="sxs-lookup"><span data-stu-id="93157-102">Alternate Keys</span></span>

<span data-ttu-id="93157-103">Uma chave alternativa serve como um identificador exclusivo alternativo para cada instância de entidade além da chave primária.</span><span class="sxs-lookup"><span data-stu-id="93157-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="93157-104">Chaves alternativas podem ser usadas como o destino de uma relação.</span><span class="sxs-lookup"><span data-stu-id="93157-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="93157-105">Ao usar um banco de dados relacional mapeia para o conceito de um índice/restrição unique nas colunas de chave alternativas e um ou mais restrições de chave estrangeira que referenciam as colunas.</span><span class="sxs-lookup"><span data-stu-id="93157-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="93157-106">Se você quiser impor exclusividade de uma coluna, em seguida, você deseja que um índice exclusivo em vez de uma chave alternativa, consulte [índices](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="93157-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="93157-107">No EF, chaves alternativas fornecem maior funcionalidade que os índices exclusivos, porque eles podem ser usados como o destino de uma chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="93157-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="93157-108">Chaves alternativas normalmente são introduzidas para você quando necessário e não é preciso configurá-las manualmente.</span><span class="sxs-lookup"><span data-stu-id="93157-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="93157-109">Ver [convenções](#conventions) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="93157-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="93157-110">Convenções</span><span class="sxs-lookup"><span data-stu-id="93157-110">Conventions</span></span>

<span data-ttu-id="93157-111">Por convenção, uma chave alternativa é introduzida para você quando você identifica uma propriedade, que não é a chave primária, como o destino de uma relação.</span><span class="sxs-lookup"><span data-stu-id="93157-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="93157-112">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="93157-112">Data Annotations</span></span>

<span data-ttu-id="93157-113">Chaves alternativas não podem ser configuradas usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="93157-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="93157-114">API fluente</span><span class="sxs-lookup"><span data-stu-id="93157-114">Fluent API</span></span>

<span data-ttu-id="93157-115">Você pode usar a API Fluent para configurar uma única propriedade para ser uma chave alternativa.</span><span class="sxs-lookup"><span data-stu-id="93157-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="93157-116">Você também pode usar a API Fluent para configurar várias propriedades para ser uma chave alternativa (conhecida como uma chave composta de alternativa).</span><span class="sxs-lookup"><span data-stu-id="93157-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
