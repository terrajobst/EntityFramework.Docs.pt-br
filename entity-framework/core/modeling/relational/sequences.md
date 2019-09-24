---
title: Sequências-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: ce02b9840e58102a60c1d8eacf6810365104d7d7
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196912"
---
# <a name="sequences"></a><span data-ttu-id="41fbb-102">Sequências</span><span class="sxs-lookup"><span data-stu-id="41fbb-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="41fbb-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="41fbb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="41fbb-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="41fbb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="41fbb-105">Uma sequência gera valores numéricos sequenciais no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="41fbb-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="41fbb-106">As sequências não estão associadas a uma tabela específica.</span><span class="sxs-lookup"><span data-stu-id="41fbb-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="41fbb-107">Convenções</span><span class="sxs-lookup"><span data-stu-id="41fbb-107">Conventions</span></span>

<span data-ttu-id="41fbb-108">Por convenção, as sequências não são introduzidas no modelo.</span><span class="sxs-lookup"><span data-stu-id="41fbb-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="41fbb-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="41fbb-109">Data Annotations</span></span>

<span data-ttu-id="41fbb-110">Você não pode configurar uma sequência usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="41fbb-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="41fbb-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="41fbb-111">Fluent API</span></span>

<span data-ttu-id="41fbb-112">Você pode usar a API fluente para criar uma sequência no modelo.</span><span class="sxs-lookup"><span data-stu-id="41fbb-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/Sequence.cs?highlight=7)] -->
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

<span data-ttu-id="41fbb-113">Você também pode configurar o aspecto adicional da sequência, como seu esquema, valor inicial e incremento.</span><span class="sxs-lookup"><span data-stu-id="41fbb-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
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

<span data-ttu-id="41fbb-114">Depois que uma sequência é introduzida, você pode usá-la para gerar valores para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="41fbb-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="41fbb-115">Por exemplo, você pode usar [valores padrão](default-values.md) para inserir o próximo valor da sequência.</span><span class="sxs-lookup"><span data-stu-id="41fbb-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
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
