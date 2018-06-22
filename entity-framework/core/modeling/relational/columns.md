---
title: Mapeamento de coluna - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052896"
---
# <a name="column-mapping"></a><span data-ttu-id="5a26e-102">Mapeamento de coluna</span><span class="sxs-lookup"><span data-stu-id="5a26e-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="5a26e-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="5a26e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5a26e-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="5a26e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5a26e-105">Mapeamento de coluna identifica quais dados da coluna devem ser consultados a partir e salvos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5a26e-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="5a26e-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="5a26e-106">Conventions</span></span>

<span data-ttu-id="5a26e-107">Por convenção, cada propriedade será instalado para mapear para uma coluna com o mesmo nome que a propriedade.</span><span class="sxs-lookup"><span data-stu-id="5a26e-107">By convention, each property will be setup to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5a26e-108">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="5a26e-108">Data Annotations</span></span>

<span data-ttu-id="5a26e-109">Você pode usar as anotações de dados para configurar a coluna à qual uma propriedade está mapeada.</span><span class="sxs-lookup"><span data-stu-id="5a26e-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="5a26e-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="5a26e-110">Fluent API</span></span>

<span data-ttu-id="5a26e-111">Você pode usar a API fluente para configurar a coluna à qual uma propriedade está mapeada.</span><span class="sxs-lookup"><span data-stu-id="5a26e-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
