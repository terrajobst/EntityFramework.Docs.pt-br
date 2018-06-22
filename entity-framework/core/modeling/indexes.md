---
title: Índices - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054880"
---
# <a name="indexes"></a><span data-ttu-id="1217e-102">Índices</span><span class="sxs-lookup"><span data-stu-id="1217e-102">Indexes</span></span>

<span data-ttu-id="1217e-103">Os índices são um conceito comuns em vários repositórios de dados.</span><span class="sxs-lookup"><span data-stu-id="1217e-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="1217e-104">Embora sua implementação no repositório de dados pode variar, eles são usados para fazer pesquisas com base em uma coluna (ou conjunto de colunas) mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="1217e-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="1217e-105">Convenções</span><span class="sxs-lookup"><span data-stu-id="1217e-105">Conventions</span></span>

<span data-ttu-id="1217e-106">Por convenção, um índice é criado em cada propriedade (ou conjunto de propriedades) que são usados como uma chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="1217e-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1217e-107">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="1217e-107">Data Annotations</span></span>

<span data-ttu-id="1217e-108">Índices não podem ser criados usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="1217e-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1217e-109">API fluente</span><span class="sxs-lookup"><span data-stu-id="1217e-109">Fluent API</span></span>

<span data-ttu-id="1217e-110">Você pode usar a API fluente para especificar um índice em uma única propriedade.</span><span class="sxs-lookup"><span data-stu-id="1217e-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="1217e-111">Por padrão, os índices são não exclusivo.</span><span class="sxs-lookup"><span data-stu-id="1217e-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="1217e-112">Você também pode especificar que um índice deve ser exclusivo, que significa que não há duas entidades podem ter o mesmo valor (es) para a propriedade fornecido (s).</span><span class="sxs-lookup"><span data-stu-id="1217e-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="1217e-113">Você também pode especificar um índice em mais de uma coluna.</span><span class="sxs-lookup"><span data-stu-id="1217e-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="1217e-114">Há apenas um índice por um conjunto distinto de propriedades.</span><span class="sxs-lookup"><span data-stu-id="1217e-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="1217e-115">Se você usar a API fluente para configurar um índice em um conjunto de propriedades que já tem um índice definido, por convenção ou configuração anterior, em seguida, você estará alterando a definição de índice.</span><span class="sxs-lookup"><span data-stu-id="1217e-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="1217e-116">Isso é útil se você quiser configurar um índice que foi criado por convenção.</span><span class="sxs-lookup"><span data-stu-id="1217e-116">This is useful if you want to further configure an index that was created by convention.</span></span>
