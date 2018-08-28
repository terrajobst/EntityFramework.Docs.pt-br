---
title: Valores padrão – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 341f243ddddc345bb4236e5c34f814694b71e32a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996246"
---
# <a name="default-values"></a><span data-ttu-id="f942a-102">Valores padrão</span><span class="sxs-lookup"><span data-stu-id="f942a-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="f942a-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="f942a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f942a-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="f942a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f942a-105">O valor padrão de uma coluna é o valor que será inserido se uma nova linha é inserida, mas nenhum valor for especificado para a coluna.</span><span class="sxs-lookup"><span data-stu-id="f942a-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="f942a-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="f942a-106">Conventions</span></span>

<span data-ttu-id="f942a-107">Por convenção, um valor padrão não está configurado.</span><span class="sxs-lookup"><span data-stu-id="f942a-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f942a-108">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="f942a-108">Data Annotations</span></span>

<span data-ttu-id="f942a-109">Você não pode definir um valor padrão usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="f942a-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f942a-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="f942a-110">Fluent API</span></span>

<span data-ttu-id="f942a-111">Você pode usar a API Fluent para especificar o valor padrão para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="f942a-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="f942a-112">Você também pode especificar um fragmento SQL que é usado para calcular o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="f942a-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
