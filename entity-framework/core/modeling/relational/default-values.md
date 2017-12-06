---
title: "Valores padrão - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="default-values"></a><span data-ttu-id="e46e3-102">Valores padrão</span><span class="sxs-lookup"><span data-stu-id="e46e3-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="e46e3-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="e46e3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e46e3-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="e46e3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e46e3-105">O valor padrão de uma coluna é o valor que será inserido se uma nova linha é inserida, mas nenhum valor for especificado para a coluna.</span><span class="sxs-lookup"><span data-stu-id="e46e3-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="e46e3-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="e46e3-106">Conventions</span></span>

<span data-ttu-id="e46e3-107">Por convenção, um valor padrão não está configurado.</span><span class="sxs-lookup"><span data-stu-id="e46e3-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e46e3-108">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e46e3-108">Data Annotations</span></span>

<span data-ttu-id="e46e3-109">Você não pode definir um valor padrão usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="e46e3-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e46e3-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="e46e3-110">Fluent API</span></span>

<span data-ttu-id="e46e3-111">Você pode usar a API fluente para especificar o valor padrão para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="e46e3-111">You can use the Fluent API to specify the default value for a property.</span></span>

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

<span data-ttu-id="e46e3-112">Você também pode especificar um fragmento SQL que é usado para calcular o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="e46e3-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

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
