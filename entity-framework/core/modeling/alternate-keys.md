---
title: Chaves alternativas - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys"></a>Chaves alternativas

Uma chave alternativa serve como um identificador exclusivo alternativo para cada instância de entidade além da chave primária. Chaves alternativas podem ser usadas como o destino de uma relação. Ao usar um banco de dados relacional mapeia para o conceito de um índice/restrição unique em colunas-chave alternativas e um ou mais restrições de chave estrangeira que referenciam as colunas.

> [!TIP]  
> Se você quiser impor exclusividade de uma coluna, em seguida, você deseja que um índice exclusivo em vez de uma chave alternativa, consulte [índices](indexes.md). No EF, chaves alternativas fornecem maior funcionalidade de índices exclusivos, porque eles podem ser usados como o destino de uma chave estrangeira.

Chaves alternativas normalmente são introduzidas para você quando necessário e você não precisa configurá-los manualmente. Consulte [convenções](#conventions) para obter mais detalhes.

## <a name="conventions"></a>Convenções

Por convenção, uma chave alternativa é introduzida para você quando você identifica uma propriedade, o que não é a chave primária, como o destino de uma relação.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
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

Chaves alternativas não podem ser configuradas usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar uma única propriedade para ser uma chave alternativa.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
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

Você também pode usar a API fluente para configurar várias propriedades para ser uma chave alternativa (conhecida como uma chave composta de alternativa).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
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
