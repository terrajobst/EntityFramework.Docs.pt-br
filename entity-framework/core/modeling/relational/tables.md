---
title: Mapeamento de tabela-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656099"
---
# <a name="table-mapping"></a>Mapeamento de tabela

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

O mapeamento de tabela identifica quais dados de tabela devem ser consultados e salvos no banco de dado.

## <a name="conventions"></a>Convenções

Por convenção, cada entidade será configurada para ser mapeada para uma tabela com o mesmo nome que a propriedade `DbSet<TEntity>` que expõe a entidade no contexto derivado. Se nenhum `DbSet<TEntity>` for incluído para a entidade especificada, o nome da classe será usado.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar a tabela para a qual um tipo é mapeado.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Você também pode especificar um esquema ao qual a tabela pertence.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar a tabela para a qual um tipo é mapeado.

``` csharp
using Microsoft.EntityFrameworkCore;

class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Você também pode especificar um esquema ao qual a tabela pertence.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
