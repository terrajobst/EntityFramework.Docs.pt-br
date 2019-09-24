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
# <a name="indexes"></a>Índices

Os índices são um conceito comum entre vários armazenamentos de dados. Embora sua implementação no armazenamento de dados possa variar, elas são usadas para fazer pesquisas com base em uma coluna (ou conjunto de colunas) mais eficiente.

## <a name="conventions"></a>Convenções

Por convenção, um índice é criado em cada propriedade (ou conjunto de propriedades) que são usados como uma chave estrangeira.

## <a name="data-annotations"></a>Anotações de dados

Não é possível criar índices usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para especificar um índice em uma única propriedade. Por padrão, os índices são não exclusivos.

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

Você também pode especificar que um índice deve ser exclusivo, o que significa que duas entidades podem ter os mesmos valores para as propriedades especificadas.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Você também pode especificar um índice em mais de uma coluna.

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
> Há apenas um índice por conjunto distinto de propriedades. Se você usar a API fluente para configurar um índice em um conjunto de propriedades que já tem um índice definido, seja por convenção ou configuração anterior, você alterará a definição desse índice. Isso será útil se você quiser configurar ainda mais um índice que foi criado por convenção.
