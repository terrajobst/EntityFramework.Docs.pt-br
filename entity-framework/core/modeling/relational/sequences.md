---
title: "Sequências - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="sequences"></a><span data-ttu-id="312b4-102">Sequências</span><span class="sxs-lookup"><span data-stu-id="312b4-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="312b4-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="312b4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="312b4-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="312b4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="312b4-105">Uma sequência gera um sequencial valores numéricos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="312b4-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="312b4-106">As sequências não são associadas uma tabela específica.</span><span class="sxs-lookup"><span data-stu-id="312b4-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="312b4-107">Convenções</span><span class="sxs-lookup"><span data-stu-id="312b4-107">Conventions</span></span>

<span data-ttu-id="312b4-108">Por convenção, as sequências não foram introduzidas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="312b4-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="312b4-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="312b4-109">Data Annotations</span></span>

<span data-ttu-id="312b4-110">Você não pode configurar uma sequência usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="312b4-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="312b4-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="312b4-111">Fluent API</span></span>

<span data-ttu-id="312b4-112">Você pode usar a API fluente para criar uma sequência no modelo.</span><span class="sxs-lookup"><span data-stu-id="312b4-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="312b4-113">Você também pode configurar o aspecto adicional de sequência, como seu esquema, o valor inicial e o incremento.</span><span class="sxs-lookup"><span data-stu-id="312b4-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="312b4-114">Depois que uma sequência é introduzida, pode usá-lo para gerar valores para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="312b4-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="312b4-115">Por exemplo, você pode usar [valores padrão](default-values.md) para inserir o próximo valor da sequência.</span><span class="sxs-lookup"><span data-stu-id="312b4-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
