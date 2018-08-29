---
title: Consultas básicas – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: eceac81546b23157611edd530b8b71f71e970c1f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "42447789"
---
# <a name="basic-queries"></a><span data-ttu-id="6c54d-102">Consultas básicas</span><span class="sxs-lookup"><span data-stu-id="6c54d-102">Basic Queries</span></span>

<span data-ttu-id="6c54d-103">Saiba como carregar entidades do banco de dados usando LINQ (Consulta Integrada à Linguagem).</span><span class="sxs-lookup"><span data-stu-id="6c54d-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="6c54d-104">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="6c54d-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="6c54d-105">101 exemplos do LINQ</span><span class="sxs-lookup"><span data-stu-id="6c54d-105">101 LINQ samples</span></span>

<span data-ttu-id="6c54d-106">Esta página mostra alguns exemplos para realizar tarefas comuns com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6c54d-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="6c54d-107">Para ter acesso a um conjunto amplo de exemplos mostrando o que é possível com o LINQ, confira [101 exemplos do LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="6c54d-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="6c54d-108">Como carregar todos os dados</span><span class="sxs-lookup"><span data-stu-id="6c54d-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="6c54d-109">Como carregar uma única entidade</span><span class="sxs-lookup"><span data-stu-id="6c54d-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="6c54d-110">Filtragem</span><span class="sxs-lookup"><span data-stu-id="6c54d-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
