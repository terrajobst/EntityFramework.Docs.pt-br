---
title: Chaves alternativas – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996965"
---
# <a name="alternate-keys"></a>Chaves alternativas

Uma chave alternativa serve como um identificador exclusivo alternativo para cada instância de entidade além da chave primária. Chaves alternativas podem ser usadas como o destino de uma relação. Ao usar um banco de dados relacional mapeia para o conceito de um índice/restrição unique nas colunas de chave alternativas e um ou mais restrições de chave estrangeira que referenciam as colunas.

> [!TIP]  
> Se você quiser impor exclusividade de uma coluna, em seguida, você deseja que um índice exclusivo em vez de uma chave alternativa, consulte [índices](indexes.md). No EF, chaves alternativas fornecem maior funcionalidade que os índices exclusivos, porque eles podem ser usados como o destino de uma chave estrangeira.

Chaves alternativas normalmente são introduzidas para você quando necessário e não é preciso configurá-las manualmente. Ver [convenções](#conventions) para obter mais detalhes.

## <a name="conventions"></a>Convenções

Por convenção, uma chave alternativa é introduzida para você quando você identifica uma propriedade, que não é a chave primária, como o destino de uma relação.

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

Você pode usar a API Fluent para configurar uma única propriedade para ser uma chave alternativa.

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

Você também pode usar a API Fluent para configurar várias propriedades para ser uma chave alternativa (conhecida como uma chave composta de alternativa).

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
