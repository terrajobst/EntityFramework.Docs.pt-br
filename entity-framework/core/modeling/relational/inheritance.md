---
title: Herança (banco de dados relacional)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 381d1878007bb78b359eb49649f4356f1e5eb04a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655638"
---
# <a name="inheritance-relational-database"></a>Herança (banco de dados relacional)

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

A herança no modelo do EF é usada para controlar como a herança nas classes de entidade é representada no banco de dados.

> [!NOTE]  
> Atualmente, somente o padrão de tabela por hierarquia (TPH) é implementado em EF Core. Outros padrões comuns, como tabela por tipo (TPT) e tipo de tabela por concreto (TPC), ainda não estão disponíveis.

## <a name="conventions"></a>Convenções

Por convenção, a herança será mapeada usando o padrão de tabela por hierarquia (TPH). TPH usa uma única tabela para armazenar os dados de todos os tipos na hierarquia. Uma coluna discriminadora é usada para identificar o tipo que cada linha representa.

EF Core só configurará a herança se dois ou mais tipos herdados forem explicitamente incluídos no modelo (consulte [herança](../inheritance.md) para obter mais detalhes).

Veja abaixo um exemplo que mostra um cenário de herança simples e os dados armazenados em uma tabela de banco de dados relacional usando o padrão TPH. A coluna *discriminador* identifica qual tipo de *blog* é armazenado em cada linha.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![imagem](_static/inheritance-tph-data.png)

>[!NOTE]
> As colunas de banco de dados são automaticamente tornadas anuláveis conforme necessário ao usar o mapeamento TPH.

## <a name="data-annotations"></a>Anotações de dados

Você não pode usar anotações de dados para configurar a herança.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o nome e o tipo da coluna discriminadora e os valores que são usados para identificar cada tipo na hierarquia.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Configurando a propriedade discriminador

Nos exemplos acima, o discriminador é criado como uma [Propriedade Shadow](xref:core/modeling/shadow-properties) na entidade base da hierarquia. Como é uma propriedade no modelo, ela pode ser configurada assim como outras propriedades. Por exemplo, para definir o comprimento máximo quando o discriminador padrão por convenção está sendo usado:

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

Combinando essas duas coisas em conjunto, é possível mapear o discriminador para uma propriedade real e configurá-lo:

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
