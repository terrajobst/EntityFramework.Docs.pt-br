---
title: Propriedades obrigatórios/opcionais – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995491"
---
# <a name="required-and-optional-properties"></a>Propriedades obrigatórias e opcionais

Uma propriedade é considerada opcional se ele for válido para que ele contenha `null`. Se `null` não é um valor válido a ser atribuído a uma propriedade, em seguida, ele é considerado uma propriedade necessária.

## <a name="conventions"></a>Convenções

Por convenção, uma propriedade cujo tipo CLR pode conter nulos será configurada como opcional (`string`, `int?`, `byte[]`, etc.). As propriedades cujo tipo CLR não pode conter nulos serão configuradas conforme necessário (`int`, `decimal`, `bool`, etc.).

> [!NOTE]  
> Uma propriedade cujo tipo CLR não pode conter nulo não pode ser configurada como opcional. A propriedade sempre será considerada necessário pelo Entity Framework.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para indicar que uma propriedade é necessária.

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

Você pode usar a API Fluent para indicar que uma propriedade é necessária.

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
