---
title: Herança (banco de dados relacional) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2d0a2abc554f5f115479f886ca3f9f4f01b80b5b
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452287"
---
# <a name="inheritance-relational-database"></a>Herança (banco de dados relacional)

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Herança no modelo do EF é usada para controlar como a herança em classes de entidade é representada no banco de dados.

> [!NOTE]  
> Atualmente, o tabela por hierarquia (TPH) padrão somente é implementado no EF Core. Outros padrões comuns, como a tabela por tipo (TPT) e tabela-por--tipo concreto (TPC) ainda não estão disponíveis.

## <a name="conventions"></a>Convenções

Por convenção, a herança será mapeada usando o padrão de tabela por hierarquia (TPH). TPH usa uma única tabela para armazenar os dados para todos os tipos na hierarquia. Uma coluna de discriminador é usada para identificar qual tipo cada linha representa.

O EF Core só irá configurar a herança se dois ou mais tipos herdados estão explicitamente incluídos no modelo (consulte [herança](../inheritance.md) para obter mais detalhes).

Abaixo está um exemplo que mostra um cenário simples de herança e os dados armazenados em uma tabela de banco de dados relacional usando o padrão TPH. O *discriminador* coluna identifica qual tipo de *Blog* é armazenada em cada linha.

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

>[!NOTE]
> Colunas de banco de dados são feitas automaticamente que permite valor nulo, conforme necessário ao usar o mapeamento TPH.

## <a name="data-annotations"></a>Anotações de dados

É possível usar as anotações de dados para configurar a herança.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar o nome e tipo de coluna de discriminador e os valores que são usados para identificar cada tipo na hierarquia.

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

## <a name="configuring-the-discriminator-property"></a>Configurando a propriedade de discriminador

Nos exemplos acima, o discriminador é criado como uma [propriedade de sombra](xref:core/modeling/shadow-properties) sobre a entidade básica da hierarquia. Uma vez que ele é uma propriedade no modelo, ele pode ser configurado assim como outras propriedades. Por exemplo, para definir o tamanho máximo quando estiver sendo usado o padrão, o discriminador por convenção:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

O discriminador também pode ser mapeado para uma propriedade CLR real em sua entidade. Por exemplo:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Combinar essas duas coisas é possível mapear o discriminador para uma propriedade real tanto configurá-lo:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
