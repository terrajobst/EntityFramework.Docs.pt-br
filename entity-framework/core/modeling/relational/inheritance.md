---
title: "Herança (banco de dados relacional) - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2017
---
# <a name="inheritance-relational-database"></a>Herança (banco de dados relacional)

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Herança no modelo EF é usada para controlar como a herança em classes de entidade é representada no banco de dados.

> [!NOTE]  
> No momento, somente o padrão do tabela por hierarquia (TPH) é implementado no núcleo do EF. Outros padrões comuns de tabela por tipo (TPT), como e -por-concreto-tipo de tabela (TPC) ainda não estão disponíveis.

## <a name="conventions"></a>Convenções

Por convenção, a herança será mapeada usando o padrão de tabela por hierarquia (TPH). TPH usa uma única tabela para armazenar os dados para todos os tipos na hierarquia. Uma coluna discriminatória é usada para identificar qual tipo de cada linha representa.

EF Core apenas configurar herança se dois ou mais tipos herdados estão explicitamente incluídos no modelo (consulte [herança](../inheritance.md) para obter mais detalhes).

Abaixo está um exemplo que mostra um cenário de herança simples e os dados armazenados em uma tabela de banco de dados relacional usando o padrão TPH. O *discriminador* coluna identifica o tipo de *Blog* é armazenado em cada linha.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![imagem](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a>Anotações de dados

Você não pode usar as anotações de dados para configurar a herança.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o nome e o tipo de coluna discriminadora e os valores que são usados para identificar cada tipo na hierarquia.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```
