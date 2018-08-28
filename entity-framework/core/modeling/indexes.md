---
title: Índices – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995474"
---
# <a name="indexes"></a><span data-ttu-id="a4201-102">Índices</span><span class="sxs-lookup"><span data-stu-id="a4201-102">Indexes</span></span>

<span data-ttu-id="a4201-103">Os índices são um conceito comum entre vários armazenamentos de dados.</span><span class="sxs-lookup"><span data-stu-id="a4201-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="a4201-104">Embora sua implementação no armazenamento de dados pode variar, eles são usados para fazer pesquisas com base em uma coluna (ou conjunto de colunas) mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="a4201-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="a4201-105">Convenções</span><span class="sxs-lookup"><span data-stu-id="a4201-105">Conventions</span></span>

<span data-ttu-id="a4201-106">Por convenção, um índice é criado em cada propriedade (ou conjunto de propriedades) que são usados como uma chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="a4201-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a4201-107">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="a4201-107">Data Annotations</span></span>

<span data-ttu-id="a4201-108">Não é possível criar índices usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="a4201-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a4201-109">API fluente</span><span class="sxs-lookup"><span data-stu-id="a4201-109">Fluent API</span></span>

<span data-ttu-id="a4201-110">Você pode usar a API Fluent para especificar um índice em uma única propriedade.</span><span class="sxs-lookup"><span data-stu-id="a4201-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="a4201-111">Por padrão, os índices são não exclusivo.</span><span class="sxs-lookup"><span data-stu-id="a4201-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="a4201-112">Você também pode especificar que um índice deve ser exclusivo, que significa que não há duas entidades podem ter o mesmo valor (es) para a propriedade fornecido (s).</span><span class="sxs-lookup"><span data-stu-id="a4201-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="a4201-113">Você também pode especificar um índice em mais de uma coluna.</span><span class="sxs-lookup"><span data-stu-id="a4201-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="a4201-114">Há apenas um índice por um conjunto distinto de propriedades.</span><span class="sxs-lookup"><span data-stu-id="a4201-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="a4201-115">Se você usar a API Fluent para configurar um índice em um conjunto de propriedades que já tem um índice definido, por convenção ou configuração anterior, em seguida, você estará alterando a definição de índice.</span><span class="sxs-lookup"><span data-stu-id="a4201-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="a4201-116">Isso é útil se você deseja configurar um índice que foi criado por convenção.</span><span class="sxs-lookup"><span data-stu-id="a4201-116">This is useful if you want to further configure an index that was created by convention.</span></span>
