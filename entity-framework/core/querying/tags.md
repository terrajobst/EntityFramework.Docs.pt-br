---
title: Marcas de consulta – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417886"
---
# <a name="query-tags"></a><span data-ttu-id="6b56e-102">Marcas de consulta</span><span class="sxs-lookup"><span data-stu-id="6b56e-102">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="6b56e-103">Este recurso é novo no EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="6b56e-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="6b56e-104">Esse recurso ajuda a correlacionar consultas LINQ no código com consultas SQL geradas capturadas nos logs.</span><span class="sxs-lookup"><span data-stu-id="6b56e-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="6b56e-105">Anote uma consulta LINQ usando o novo método `TagWith()`:</span><span class="sxs-lookup"><span data-stu-id="6b56e-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="6b56e-106">Essa consulta LINQ é convertida para a seguinte instrução SQL:</span><span class="sxs-lookup"><span data-stu-id="6b56e-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="6b56e-107">É possível chamar `TagWith()` várias vezes na mesma consulta.</span><span class="sxs-lookup"><span data-stu-id="6b56e-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="6b56e-108">As marcas de consulta são cumulativas.</span><span class="sxs-lookup"><span data-stu-id="6b56e-108">Query tags are cumulative.</span></span>
<span data-ttu-id="6b56e-109">Por exemplo, considerando os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="6b56e-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="6b56e-110">A seguinte consulta:</span><span class="sxs-lookup"><span data-stu-id="6b56e-110">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="6b56e-111">É convertida para:</span><span class="sxs-lookup"><span data-stu-id="6b56e-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="6b56e-112">Também é possível usar cadeias multilinha como marcas de consulta.</span><span class="sxs-lookup"><span data-stu-id="6b56e-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="6b56e-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6b56e-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="6b56e-114">Isso produz o seguinte SQL:</span><span class="sxs-lookup"><span data-stu-id="6b56e-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="6b56e-115">Limitações conhecidas</span><span class="sxs-lookup"><span data-stu-id="6b56e-115">Known limitations</span></span>

<span data-ttu-id="6b56e-116">**As marcas de consulta não são parametrizáveis:** o EF Core sempre trata as marcas de consulta na consulta LINQ como literais de cadeia de caracteres que estão incluídas no SQL gerado.</span><span class="sxs-lookup"><span data-stu-id="6b56e-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="6b56e-117">Não são permitidas consultas compiladas que usam marcas de consulta como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="6b56e-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>
