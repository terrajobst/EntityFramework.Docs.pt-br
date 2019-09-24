---
title: Chaves alternativas-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197467"
---
# <a name="alternate-keys"></a>Chaves alternativas

Uma chave alternativa serve como um identificador exclusivo alternativo para cada instância de entidade, além da chave primária. Chaves alternativas podem ser usadas como o destino de uma relação. Ao usar um banco de dados relacional, ele é mapeado para o conceito de um índice/restrição exclusivo nas colunas de chave alternativas e uma ou mais restrições Foreign Key que fazem referência às colunas.

> [!TIP]  
> Se você quiser apenas impor a exclusividade de uma coluna, então você deseja um índice exclusivo em vez de uma chave alternativa, consulte [índices](indexes.md). No EF, chaves alternativas fornecem maior funcionalidade do que índices exclusivos, pois eles podem ser usados como o destino de uma chave estrangeira.

As chaves alternativas são normalmente introduzidas quando necessário e você não precisa configurá-las manualmente. Consulte [convenções](#conventions) para obter mais detalhes.

## <a name="conventions"></a>Convenções

Por convenção, uma chave alternativa é introduzida para você quando você identifica uma propriedade, que não é a chave primária, como o destino de uma relação.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Anotações de dados

Chaves alternativas não podem ser configuradas usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma única propriedade para ser uma chave alternativa.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

Você também pode usar a API Fluent para configurar várias propriedades para serem uma chave alternativa (conhecida como uma chave alternativa composta).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
