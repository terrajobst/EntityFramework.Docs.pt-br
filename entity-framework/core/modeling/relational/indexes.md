---
title: "Índices - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a>Índices

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).

Um índice em um banco de dados relacional é mapeado para o mesmo conceito que um índice no núcleo do Entity Framework.

## <a name="conventions"></a>Convenções

Por convenção, os índices são denominados `IX_<type name>_<property name>`. Para índices compostos `<property name>` torna-se uma lista de sublinhado separado dos nomes de propriedade.

## <a name="data-annotations"></a>Anotações de dados

Índices não podem ser configurados usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o nome de um índice.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
