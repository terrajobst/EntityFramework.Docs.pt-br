---
title: Novidades no EF Core 2.2 – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656198"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="7fafc-102">Novos recursos no EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="7fafc-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="7fafc-103">Suporte a dados espaciais</span><span class="sxs-lookup"><span data-stu-id="7fafc-103">Spatial data support</span></span>

<span data-ttu-id="7fafc-104">Os dados espaciais podem ser usados para representar o local físico e a forma de objetos.</span><span class="sxs-lookup"><span data-stu-id="7fafc-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="7fafc-105">Muitos bancos de dados podem armazenar, indexar e consultar dados espaciais nativamente.</span><span class="sxs-lookup"><span data-stu-id="7fafc-105">Many databases can natively store, index, and query spatial data.</span></span>
<span data-ttu-id="7fafc-106">Cenários comuns incluem consultas para objetos em uma determinada distância e testar se um polígono contém um determinado local.</span><span class="sxs-lookup"><span data-stu-id="7fafc-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="7fafc-107">Agora o EF Core 2.2 permite trabalhar com dados espaciais de vários bancos de dados usando tipos de biblioteca de [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).</span><span class="sxs-lookup"><span data-stu-id="7fafc-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="7fafc-108">O suporte a dados espaciais é implementado como uma série de pacotes de extensão específica do provedor.</span><span class="sxs-lookup"><span data-stu-id="7fafc-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="7fafc-109">Cada um desses pacotes contribui com mapeamentos de métodos e tipos NTS e as funções e tipos espaciais correspondentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7fafc-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="7fafc-110">Essas extensões de provedor agora estão disponíveis para [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) e [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (do [projeto Npgsql](https://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="7fafc-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="7fafc-111">Os tipos espaciais podem ser usados diretamente com o [provedor na memória do EF Core](xref:core/providers/in-memory/index) sem extensões adicionais.</span><span class="sxs-lookup"><span data-stu-id="7fafc-111">Spatial types can be used directly with the [EF Core in-memory provider](xref:core/providers/in-memory/index) without additional extensions.</span></span>

<span data-ttu-id="7fafc-112">Depois de instalar a extensão do provedor, você pode adicionar propriedades de tipos com suporte às suas entidades.</span><span class="sxs-lookup"><span data-stu-id="7fafc-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="7fafc-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7fafc-113">For example:</span></span>

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
```

<span data-ttu-id="7fafc-114">Você pode usar a persistência de entidades com dados espaciais:</span><span class="sxs-lookup"><span data-stu-id="7fafc-114">You can then persist entities with spatial data:</span></span>

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```

<span data-ttu-id="7fafc-115">E você pode executar consultas de banco de dados com base em operações e dados espaciais:</span><span class="sxs-lookup"><span data-stu-id="7fafc-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="7fafc-116">Para saber mais sobre esse recurso, confira a [documentação de tipos espaciais](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="7fafc-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span>

## <a name="collections-of-owned-entities"></a><span data-ttu-id="7fafc-117">Coleções de entidades de propriedade</span><span class="sxs-lookup"><span data-stu-id="7fafc-117">Collections of owned entities</span></span>

<span data-ttu-id="7fafc-118">O EF Core 2.0 adicionou a capacidade de modelar propriedade em associações de um para um.</span><span class="sxs-lookup"><span data-stu-id="7fafc-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="7fafc-119">O EF Core 2.2 amplia a capacidade de expressar a propriedade para associações de um para muitos.</span><span class="sxs-lookup"><span data-stu-id="7fafc-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="7fafc-120">A propriedade ajuda a restringir como as entidades são usadas.</span><span class="sxs-lookup"><span data-stu-id="7fafc-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="7fafc-121">Por exemplo, entidades de propriedade:</span><span class="sxs-lookup"><span data-stu-id="7fafc-121">For example, owned entities:</span></span>

- <span data-ttu-id="7fafc-122">Só podem aparecer nas propriedades de navegação de outros tipos de entidades.</span><span class="sxs-lookup"><span data-stu-id="7fafc-122">Can only ever appear on navigation properties of other entity types.</span></span>
- <span data-ttu-id="7fafc-123">São carregadas automaticamente e só podem ser acompanhadas por um DbContext junto com seu proprietário.</span><span class="sxs-lookup"><span data-stu-id="7fafc-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="7fafc-124">Em bancos de dados relacionais, as coleções de propriedade são mapeadas em tabelas separadas do proprietário, assim como as associações regulares de um para muitos.</span><span class="sxs-lookup"><span data-stu-id="7fafc-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="7fafc-125">Mas, em bancos de dados orientado a documentos, planejamos manter juntas as entidades de propriedade (em referências ou coleções de propriedade) dentro do mesmo documento que o proprietário.</span><span class="sxs-lookup"><span data-stu-id="7fafc-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="7fafc-126">Você pode usar o recurso chamando a nova API OwnsMany():</span><span class="sxs-lookup"><span data-stu-id="7fafc-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="7fafc-127">Para saber mais, confira a [documentação atualizada de entidades de propriedade](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="7fafc-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="7fafc-128">Marcas de consulta</span><span class="sxs-lookup"><span data-stu-id="7fafc-128">Query tags</span></span>

<span data-ttu-id="7fafc-129">Esse recurso simplifica a correlação de consultas LINQ no código com as consultas SQL geradas capturadas nos logs.</span><span class="sxs-lookup"><span data-stu-id="7fafc-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="7fafc-130">Para aproveitar as marcas de consulta, você deve anotar uma consulta LINQ usando o novo método TagWith().</span><span class="sxs-lookup"><span data-stu-id="7fafc-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="7fafc-131">Usando a consulta espacial do exemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="7fafc-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="7fafc-132">Essa consulta LINQ produzirá a seguinte saída SQL:</span><span class="sxs-lookup"><span data-stu-id="7fafc-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="7fafc-133">Para saber mais, confira a [documentação de marcas de consulta](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="7fafc-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span>
