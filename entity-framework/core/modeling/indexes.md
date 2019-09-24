---
title: Índices-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197251"
---
# <a name="indexes"></a><span data-ttu-id="1cbb9-102">Índices</span><span class="sxs-lookup"><span data-stu-id="1cbb9-102">Indexes</span></span>

<span data-ttu-id="1cbb9-103">Os índices são um conceito comum entre vários armazenamentos de dados.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="1cbb9-104">Embora sua implementação no armazenamento de dados possa variar, elas são usadas para fazer pesquisas com base em uma coluna (ou conjunto de colunas) mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="1cbb9-105">Convenções</span><span class="sxs-lookup"><span data-stu-id="1cbb9-105">Conventions</span></span>

<span data-ttu-id="1cbb9-106">Por convenção, um índice é criado em cada propriedade (ou conjunto de propriedades) que são usados como uma chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1cbb9-107">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="1cbb9-107">Data Annotations</span></span>

<span data-ttu-id="1cbb9-108">Não é possível criar índices usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1cbb9-109">API fluente</span><span class="sxs-lookup"><span data-stu-id="1cbb9-109">Fluent API</span></span>

<span data-ttu-id="1cbb9-110">Você pode usar a API fluente para especificar um índice em uma única propriedade.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="1cbb9-111">Por padrão, os índices são não exclusivos.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

<span data-ttu-id="1cbb9-112">Você também pode especificar que um índice deve ser exclusivo, o que significa que duas entidades podem ter os mesmos valores para as propriedades especificadas.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="1cbb9-113">Você também pode especificar um índice em mais de uma coluna.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> <span data-ttu-id="1cbb9-114">Há apenas um índice por conjunto distinto de propriedades.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="1cbb9-115">Se você usar a API fluente para configurar um índice em um conjunto de propriedades que já tem um índice definido, seja por convenção ou configuração anterior, você alterará a definição desse índice.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="1cbb9-116">Isso será útil se você quiser configurar ainda mais um índice que foi criado por convenção.</span><span class="sxs-lookup"><span data-stu-id="1cbb9-116">This is useful if you want to further configure an index that was created by convention.</span></span>
