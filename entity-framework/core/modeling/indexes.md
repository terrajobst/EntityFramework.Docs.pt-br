---
title: Índices – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995474"
---
# <a name="indexes"></a>Índices

Os índices são um conceito comum entre vários armazenamentos de dados. Embora sua implementação no armazenamento de dados pode variar, eles são usados para fazer pesquisas com base em uma coluna (ou conjunto de colunas) mais eficiente.

## <a name="conventions"></a>Convenções

Por convenção, um índice é criado em cada propriedade (ou conjunto de propriedades) que são usados como uma chave estrangeira.

## <a name="data-annotations"></a>Anotações de dados

Não é possível criar índices usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para especificar um índice em uma única propriedade. Por padrão, os índices são não exclusivo.

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

Você também pode especificar que um índice deve ser exclusivo, que significa que não há duas entidades podem ter o mesmo valor (es) para a propriedade fornecido (s).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Você também pode especificar um índice em mais de uma coluna.

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
> Há apenas um índice por um conjunto distinto de propriedades. Se você usar a API Fluent para configurar um índice em um conjunto de propriedades que já tem um índice definido, por convenção ou configuração anterior, em seguida, você estará alterando a definição de índice. Isso é útil se você deseja configurar um índice que foi criado por convenção.
