---
title: Marcas de consulta – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: 3a4d516cab5836c659e42d825c4f1bf89355d671
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688739"
---
# <a name="query-tags"></a>Marcas de consulta
> [!NOTE]
> Este recurso é novo no EF Core 2.2.

Esse recurso ajuda a correlacionar consultas LINQ no código com consultas SQL geradas capturadas nos logs.
Anote uma consulta LINQ usando o novo método `TagWith()`: 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Essa consulta LINQ é convertida para a seguinte instrução SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

É possível chamar `TagWith()` várias vezes na mesma consulta.
As marcas de consulta são cumulativas.
Por exemplo, considerando os seguintes métodos:

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

A seguinte consulta:   

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

É convertida para:

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Também é possível usar cadeias multilinha como marcas de consulta.
Por exemplo:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Isso produz o seguinte SQL:

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a>Limitações conhecidas
**As marcas de consulta não são parametrizáveis:** o EF Core sempre trata as marcas de consulta na consulta LINQ como literais de cadeia de caracteres que estão incluídas no SQL gerado.
Não são permitidas consultas compiladas que usam marcas de consulta como parâmetros.