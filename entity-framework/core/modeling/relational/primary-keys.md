---
title: "Chaves primárias - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="primary-keys"></a>Chaves primárias

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).

Uma restrição de chave primária é introduzida para a chave de cada tipo de entidade.

## <a name="conventions"></a>Convenções

Por convenção, a chave primária no banco de dados será nomeada `PK_<type name>`.

## <a name="data-annotations"></a>Anotações de dados

Nenhum aspectos específicos do banco de dados relacional de uma chave primária podem ser configurados usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o nome da restrição de chave primária no banco de dados.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
