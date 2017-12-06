---
title: "Propriedades obrigatórios/opcionais - Core de EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="c1a59-102">Propriedades obrigatórias e opcionais</span><span class="sxs-lookup"><span data-stu-id="c1a59-102">Required and Optional Properties</span></span>

<span data-ttu-id="c1a59-103">Uma propriedade é considerada opcional se ela é válida para conter `null`.</span><span class="sxs-lookup"><span data-stu-id="c1a59-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="c1a59-104">Se `null` não é um valor válido a ser atribuído a uma propriedade, em seguida, ele é considerado uma propriedade necessária.</span><span class="sxs-lookup"><span data-stu-id="c1a59-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="c1a59-105">Convenções</span><span class="sxs-lookup"><span data-stu-id="c1a59-105">Conventions</span></span>

<span data-ttu-id="c1a59-106">Por convenção, uma propriedade cujo tipo CLR pode conter nulo será configurada como opcional (`string`, `int?`, `byte[]`, etc.).</span><span class="sxs-lookup"><span data-stu-id="c1a59-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="c1a59-107">Propriedades cujo tipo CLR não pode conter nulos serão configuradas conforme necessário (`int`, `decimal`, `bool`, etc.).</span><span class="sxs-lookup"><span data-stu-id="c1a59-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="c1a59-108">Uma propriedade cujo tipo CLR não pode conter nulo não pode ser configurada como opcional.</span><span class="sxs-lookup"><span data-stu-id="c1a59-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="c1a59-109">A propriedade será sempre considerada exigido pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c1a59-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c1a59-110">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="c1a59-110">Data Annotations</span></span>

<span data-ttu-id="c1a59-111">Você pode usar as anotações de dados para indicar que uma propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="c1a59-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="c1a59-112">API fluente</span><span class="sxs-lookup"><span data-stu-id="c1a59-112">Fluent API</span></span>

<span data-ttu-id="c1a59-113">Você pode usar a API fluente para indicar que uma propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="c1a59-113">You can use the Fluent API to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
