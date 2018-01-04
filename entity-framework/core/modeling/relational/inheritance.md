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
# <a name="inheritance-relational-database"></a><span data-ttu-id="ba57f-102">Herança (banco de dados relacional)</span><span class="sxs-lookup"><span data-stu-id="ba57f-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="ba57f-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="ba57f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ba57f-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="ba57f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ba57f-105">Herança no modelo EF é usada para controlar como a herança em classes de entidade é representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ba57f-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="ba57f-106">No momento, somente o padrão do tabela por hierarquia (TPH) é implementado no núcleo do EF.</span><span class="sxs-lookup"><span data-stu-id="ba57f-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="ba57f-107">Outros padrões comuns de tabela por tipo (TPT), como e -por-concreto-tipo de tabela (TPC) ainda não estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="ba57f-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="ba57f-108">Convenções</span><span class="sxs-lookup"><span data-stu-id="ba57f-108">Conventions</span></span>

<span data-ttu-id="ba57f-109">Por convenção, a herança será mapeada usando o padrão de tabela por hierarquia (TPH).</span><span class="sxs-lookup"><span data-stu-id="ba57f-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="ba57f-110">TPH usa uma única tabela para armazenar os dados para todos os tipos na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="ba57f-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="ba57f-111">Uma coluna discriminatória é usada para identificar qual tipo de cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="ba57f-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="ba57f-112">EF Core apenas configurar herança se dois ou mais tipos herdados estão explicitamente incluídos no modelo (consulte [herança](../inheritance.md) para obter mais detalhes).</span><span class="sxs-lookup"><span data-stu-id="ba57f-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="ba57f-113">Abaixo está um exemplo que mostra um cenário de herança simples e os dados armazenados em uma tabela de banco de dados relacional usando o padrão TPH.</span><span class="sxs-lookup"><span data-stu-id="ba57f-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="ba57f-114">O *discriminador* coluna identifica o tipo de *Blog* é armazenado em cada linha.</span><span class="sxs-lookup"><span data-stu-id="ba57f-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="ba57f-116">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="ba57f-116">Data Annotations</span></span>

<span data-ttu-id="ba57f-117">Você não pode usar as anotações de dados para configurar a herança.</span><span class="sxs-lookup"><span data-stu-id="ba57f-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ba57f-118">API fluente</span><span class="sxs-lookup"><span data-stu-id="ba57f-118">Fluent API</span></span>

<span data-ttu-id="ba57f-119">Você pode usar a API fluente para configurar o nome e o tipo de coluna discriminadora e os valores que são usados para identificar cada tipo na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="ba57f-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
