---
title: Consultas básicas – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052546"
---
# <a name="basic-queries"></a><span data-ttu-id="b4474-102">Consultas básicas</span><span class="sxs-lookup"><span data-stu-id="b4474-102">Basic Queries</span></span>

<span data-ttu-id="b4474-103">Saiba como carregar entidades do banco de dados usando LINQ (Consulta Integrada à Linguagem).</span><span class="sxs-lookup"><span data-stu-id="b4474-103">Learn how to load entities from the database using Language Integrate Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="b4474-104">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="b4474-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="b4474-105">101 exemplos do LINQ</span><span class="sxs-lookup"><span data-stu-id="b4474-105">101 LINQ samples</span></span>

<span data-ttu-id="b4474-106">Esta página mostra alguns exemplos para realizar tarefas comuns com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b4474-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="b4474-107">Para ter acesso a um conjunto amplo de exemplos mostrando o que é possível com o LINQ, confira [101 exemplos do LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="b4474-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="b4474-108">Como carregar todos os dados</span><span class="sxs-lookup"><span data-stu-id="b4474-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="b4474-109">Como carregar uma única entidade</span><span class="sxs-lookup"><span data-stu-id="b4474-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="b4474-110">Filtragem</span><span class="sxs-lookup"><span data-stu-id="b4474-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
