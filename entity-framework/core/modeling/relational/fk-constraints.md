---
title: "Restrições de chave estrangeira - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
ms.technology: entity-framework-core
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 726f03e2ee4cd3ec851c9a861b75dd12f9203e9c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="c1a2e-102">Restrições FOREIGN KEY</span><span class="sxs-lookup"><span data-stu-id="c1a2e-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="c1a2e-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="c1a2e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c1a2e-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="c1a2e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c1a2e-105">Uma restrição de chave estrangeira é apresentada para cada relação no modelo.</span><span class="sxs-lookup"><span data-stu-id="c1a2e-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="c1a2e-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="c1a2e-106">Conventions</span></span>

<span data-ttu-id="c1a2e-107">Por convenção, as restrições de chave estrangeira são denominadas `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c1a2e-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="c1a2e-108">Para chaves estrangeiras compostas `<foreign key property name>` torna-se uma lista de sublinhado separado dos nomes de propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="c1a2e-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c1a2e-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="c1a2e-109">Data Annotations</span></span>

<span data-ttu-id="c1a2e-110">Nomes de restrição de chave estrangeira não podem ser configurados usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="c1a2e-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c1a2e-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="c1a2e-111">Fluent API</span></span>

<span data-ttu-id="c1a2e-112">Você pode usar a API fluente para configurar o nome de restrição de chave estrangeira para uma relação.</span><span class="sxs-lookup"><span data-stu-id="c1a2e-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
