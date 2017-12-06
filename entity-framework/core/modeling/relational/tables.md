---
title: Mapeamento de tabela - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="table-mapping"></a><span data-ttu-id="8cded-102">Mapeamento de tabela</span><span class="sxs-lookup"><span data-stu-id="8cded-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="8cded-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="8cded-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8cded-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="8cded-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8cded-105">Mapeamento de tabela identifica quais dados de tabela devem ser consultados a partir e salvos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8cded-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="8cded-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="8cded-106">Conventions</span></span>

<span data-ttu-id="8cded-107">Por convenção, cada entidade será instalado para mapear para uma tabela com o mesmo nome que o `DbSet<TEntity>` propriedade que expõe a entidade no contexto de derivada.</span><span class="sxs-lookup"><span data-stu-id="8cded-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="8cded-108">Se nenhum `DbSet<TEntity>` está incluído para a entidade especificada, o nome da classe é usado.</span><span class="sxs-lookup"><span data-stu-id="8cded-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8cded-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="8cded-109">Data Annotations</span></span>

<span data-ttu-id="8cded-110">Você pode usar as anotações de dados para configurar a tabela que é mapeado para um tipo.</span><span class="sxs-lookup"><span data-stu-id="8cded-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="8cded-111">Você também pode especificar um esquema que a tabela pertence.</span><span class="sxs-lookup"><span data-stu-id="8cded-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="8cded-112">API fluente</span><span class="sxs-lookup"><span data-stu-id="8cded-112">Fluent API</span></span>

<span data-ttu-id="8cded-113">Você pode usar a API fluente para configurar a tabela que é mapeado para um tipo.</span><span class="sxs-lookup"><span data-stu-id="8cded-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="8cded-114">Você também pode especificar um esquema que a tabela pertence.</span><span class="sxs-lookup"><span data-stu-id="8cded-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
