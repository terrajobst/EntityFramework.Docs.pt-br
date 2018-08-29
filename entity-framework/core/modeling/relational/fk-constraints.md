---
title: Restrições de chave estrangeira – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993982"
---
# <a name="foreign-key-constraints"></a>Restrições de chave estrangeira

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Uma restrição foreign key é introduzida para cada relação no modelo.

## <a name="conventions"></a>Convenções

Por convenção, as restrições de chave estrangeira são nomeadas `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Para chaves estrangeiras compostas `<foreign key property name>` torna-se uma lista de sublinhado separado dos nomes de propriedade de chave estrangeira.

## <a name="data-annotations"></a>Anotações de dados

Nomes de restrição de chave estrangeira não podem ser configurados usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar o nome de restrição de chave estrangeira para uma relação.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
