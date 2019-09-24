---
title: Incluindo & excluindo tipos-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197381"
---
# <a name="including--excluding-types"></a>Incluindo & excluindo tipos

A inclusão de um tipo no modelo significa que o EF tem metadados sobre esse tipo e tentará ler e gravar instâncias de/para o banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, os tipos expostos em `DbSet` Propriedades no contexto são incluídos em seu modelo. Além disso, os `OnModelCreating` tipos mencionados no método também são incluídos. Finalmente, todos os tipos que são encontrados pela exploração recursiva das propriedades de navegação de tipos descobertos também são incluídos no modelo.

**Por exemplo, na listagem de código a seguir, todos os três tipos são descobertos:**

* `Blog`Porque ele é exposto em uma `DbSet` Propriedade no contexto

* `Post`Porque ele é descoberto por meio `Blog.Posts` da propriedade de navegação

* `AuditEntry`Porque ele é mencionado em`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para excluir um tipo do modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para excluir um tipo do modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
