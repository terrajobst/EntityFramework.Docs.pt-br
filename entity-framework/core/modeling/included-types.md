---
title: Incluir e excluir tipos – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: f533b24312af37634ce4957e43c39ce776bf0bf0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929791"
---
# <a name="including--excluding-types"></a>Incluir e excluir tipos

Incluindo os meios de modelo EF tem metadados sobre que tipo e tentarão ler e gravar as instâncias de banco de dados de/para um tipo.

## <a name="conventions"></a>Convenções

Por convenção, os tipos que são expostos no `DbSet` propriedades em seu contexto estão incluídas em seu modelo. Além disso, os tipos que são mencionadas no `OnModelCreating` método também estão incluídos. Por fim, todos os tipos que são encontrados pelo recursivamente Explorando as propriedades de navegação de tipos descobertos também estão incluídos no modelo.

**Por exemplo, na listagem de código a seguir são descobertos todos os três tipos:**

* `Blog` porque ele é exposto em um `DbSet` propriedade no contexto

* `Post` porque ele é descoberto por meio de `Blog.Posts` propriedade de navegação

* `AuditEntry` porque ele é mencionado na `OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
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

Você pode usar anotações de dados para excluir um tipo de modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para excluir um tipo de modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
