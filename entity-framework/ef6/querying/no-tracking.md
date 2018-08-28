---
title: Consultas sem controle - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994234"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="1ca3a-102">Consultas sem acompanhamento</span><span class="sxs-lookup"><span data-stu-id="1ca3a-102">No-Tracking Queries</span></span>
<span data-ttu-id="1ca3a-103">Às vezes, você talvez queira obter entidades de uma consulta, mas não tem essas entidades a ser acompanhado pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="1ca3a-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="1ca3a-104">Isso pode resultar em melhor desempenho ao consultar para um grande número de entidades em cenários somente leitura.</span><span class="sxs-lookup"><span data-stu-id="1ca3a-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="1ca3a-105">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="1ca3a-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="1ca3a-106">Um novo método de extensão AsNoTracking permite que qualquer consulta ser executada dessa forma.</span><span class="sxs-lookup"><span data-stu-id="1ca3a-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="1ca3a-107">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1ca3a-107">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
