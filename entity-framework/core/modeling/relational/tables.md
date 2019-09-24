---
title: Mapeamento de tabela-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196960"
---
# <a name="table-mapping"></a><span data-ttu-id="d8bc8-102">Mapeamento de tabela</span><span class="sxs-lookup"><span data-stu-id="d8bc8-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="d8bc8-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="d8bc8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d8bc8-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="d8bc8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d8bc8-105">O mapeamento de tabela identifica quais dados de tabela devem ser consultados e salvos no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="d8bc8-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d8bc8-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="d8bc8-106">Conventions</span></span>

<span data-ttu-id="d8bc8-107">Por convenção, cada entidade será configurada para ser mapeada para uma tabela com o mesmo nome que a propriedade `DbSet<TEntity>` que expõe a entidade no contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="d8bc8-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="d8bc8-108">Se não `DbSet<TEntity>` for incluído para a entidade especificada, o nome da classe será usado.</span><span class="sxs-lookup"><span data-stu-id="d8bc8-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d8bc8-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="d8bc8-109">Data Annotations</span></span>

<span data-ttu-id="d8bc8-110">Você pode usar as anotações de dados para configurar a tabela para a qual um tipo é mapeado.</span><span class="sxs-lookup"><span data-stu-id="d8bc8-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="d8bc8-111">Você também pode especificar um esquema ao qual a tabela pertence.</span><span class="sxs-lookup"><span data-stu-id="d8bc8-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="d8bc8-112">API fluente</span><span class="sxs-lookup"><span data-stu-id="d8bc8-112">Fluent API</span></span>

<span data-ttu-id="d8bc8-113">Você pode usar a API Fluent para configurar a tabela para a qual um tipo é mapeado.</span><span class="sxs-lookup"><span data-stu-id="d8bc8-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="d8bc8-114">Você também pode especificar um esquema ao qual a tabela pertence.</span><span class="sxs-lookup"><span data-stu-id="d8bc8-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
