---
title: Comprimento máximo - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052666"
---
# <a name="maximum-length"></a>Comprimento máximo

A configuração de um comprimento máximo fornece uma dica para o repositório de dados sobre o tipo de dados apropriado a ser usado para uma determinada propriedade. Comprimento máximo só se aplica a tipos de dados de matriz, como `string` e `byte[]`.

> [!NOTE]  
> Entity Framework não faz nenhuma validação de comprimento máximo antes de passar os dados para o provedor. É até o repositório de dados ou provedor para validar se apropriado. Por exemplo, quando voltados para o SQL Server, excedendo o comprimento máximo resultará em uma exceção como o tipo de dados da coluna subjacente não permitirá excesso de dados a ser armazenado.

## <a name="conventions"></a>Convenções

Por convenção, ele é mantido até que o provedor de banco de dados para escolher um tipo de dados apropriado para as propriedades. Para propriedades que têm um comprimento, o provedor de banco de dados geralmente escolherá um tipo de dados que permite que o maior tamanho de dados. Por exemplo, Microsoft SQL Server usará `nvarchar(max)` para `string` propriedades (ou `nvarchar(450)` se a coluna é usada como uma chave).

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar um comprimento máximo para uma propriedade. Neste exemplo, direcionando o SQL Server, isso resultaria no `nvarchar(500)` tipo de dados que está sendo usado.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar um comprimento máximo para uma propriedade. Neste exemplo, direcionando o SQL Server, isso resultaria no `nvarchar(500)` tipo de dados que está sendo usado.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
