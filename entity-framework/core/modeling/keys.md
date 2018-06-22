---
title: Chaves (primária) - Core de EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052566"
---
# <a name="keys-primary"></a><span data-ttu-id="5867d-102">Chaves (primária)</span><span class="sxs-lookup"><span data-stu-id="5867d-102">Keys (primary)</span></span>

<span data-ttu-id="5867d-103">Uma chave serve como o principal identificador exclusivo para cada instância de entidade.</span><span class="sxs-lookup"><span data-stu-id="5867d-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="5867d-104">Ao usar um banco de dados relacional, isso mapeia para o conceito de um *chave primária*.</span><span class="sxs-lookup"><span data-stu-id="5867d-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="5867d-105">Você também pode configurar um identificador exclusivo que não é a chave primária (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="5867d-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="5867d-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="5867d-106">Conventions</span></span>

<span data-ttu-id="5867d-107">Por convenção, uma propriedade chamada `Id` ou `<type name>Id` serão configurados como a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="5867d-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="5867d-108">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="5867d-108">Data Annotations</span></span>

<span data-ttu-id="5867d-109">Você pode usar as anotações de dados para configurar uma única propriedade para ser a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="5867d-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="5867d-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="5867d-110">Fluent API</span></span>

<span data-ttu-id="5867d-111">Você pode usar a API fluente para configurar uma única propriedade para ser a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="5867d-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="5867d-112">Você também pode usar a API fluente para configurar várias propriedades para a chave de uma entidade (conhecida como uma chave composta).</span><span class="sxs-lookup"><span data-stu-id="5867d-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="5867d-113">Chaves compostas só podem ser configuradas usando a API fluente - convenções nunca irá configurar uma chave composta e você não pode usar as anotações de dados para configurar um.</span><span class="sxs-lookup"><span data-stu-id="5867d-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
