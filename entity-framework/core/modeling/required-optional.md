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
# <a name="required-and-optional-properties"></a>Propriedades obrigatórias e opcionais

Uma propriedade é considerada opcional se ela é válida para conter `null`. Se `null` não é um valor válido a ser atribuído a uma propriedade, em seguida, ele é considerado uma propriedade necessária.

## <a name="conventions"></a>Convenções

Por convenção, uma propriedade cujo tipo CLR pode conter nulo será configurada como opcional (`string`, `int?`, `byte[]`, etc.). Propriedades cujo tipo CLR não pode conter nulos serão configuradas conforme necessário (`int`, `decimal`, `bool`, etc.).

> [!NOTE]  
> Uma propriedade cujo tipo CLR não pode conter nulo não pode ser configurada como opcional. A propriedade será sempre considerada exigido pelo Entity Framework.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para indicar que uma propriedade é necessária.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para indicar que uma propriedade é necessária.

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
