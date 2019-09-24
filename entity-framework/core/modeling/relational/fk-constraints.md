---
title: Restrições de chave estrangeira-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197061"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="f4286-102">Restrições Foreign Key</span><span class="sxs-lookup"><span data-stu-id="f4286-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="f4286-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="f4286-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f4286-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="f4286-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f4286-105">Uma restrição FOREIGN KEY é introduzida para cada relação no modelo.</span><span class="sxs-lookup"><span data-stu-id="f4286-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="f4286-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="f4286-106">Conventions</span></span>

<span data-ttu-id="f4286-107">Por convenção, as restrições FOREIGN KEY são `FK_<dependent type name>_<principal type name>_<foreign key property name>`nomeadas.</span><span class="sxs-lookup"><span data-stu-id="f4286-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="f4286-108">Para chaves `<foreign key property name>` estrangeiras compostas se torna uma lista separada por sublinhado de nomes de propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="f4286-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f4286-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="f4286-109">Data Annotations</span></span>

<span data-ttu-id="f4286-110">Nomes de restrição de chave estrangeira não podem ser configurados usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="f4286-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f4286-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="f4286-111">Fluent API</span></span>

<span data-ttu-id="f4286-112">Você pode usar a API Fluent para configurar o nome da restrição de chave estrangeira para uma relação.</span><span class="sxs-lookup"><span data-stu-id="f4286-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
