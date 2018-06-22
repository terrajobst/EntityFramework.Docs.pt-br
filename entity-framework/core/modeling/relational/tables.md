---
title: Mapeamento de tabela - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052726"
---
# <a name="table-mapping"></a>Mapeamento de tabela

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).

Mapeamento de tabela identifica quais dados de tabela devem ser consultados a partir e salvos no banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, cada entidade será instalado para mapear para uma tabela com o mesmo nome que o `DbSet<TEntity>` propriedade que expõe a entidade no contexto de derivada. Se nenhum `DbSet<TEntity>` está incluído para a entidade especificada, o nome da classe é usado.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar a tabela que é mapeado para um tipo.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Você também pode especificar um esquema que a tabela pertence.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar a tabela que é mapeado para um tipo.

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
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

Você também pode especificar um esquema que a tabela pertence.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
