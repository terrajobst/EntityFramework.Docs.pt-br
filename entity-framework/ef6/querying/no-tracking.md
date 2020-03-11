---
title: Consultas sem controle-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417098"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="f7d8f-102">Consultas sem acompanhamento</span><span class="sxs-lookup"><span data-stu-id="f7d8f-102">No-Tracking Queries</span></span>
<span data-ttu-id="f7d8f-103">Às vezes, você pode querer obter entidades de volta de uma consulta, mas não ter essas entidades rastreadas pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="f7d8f-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="f7d8f-104">Isso pode resultar em melhor desempenho ao consultar um grande número de entidades em cenários somente leitura.</span><span class="sxs-lookup"><span data-stu-id="f7d8f-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="f7d8f-105">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="f7d8f-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="f7d8f-106">Um novo método de extensão AsNoTracking permite que qualquer consulta seja executada dessa maneira.</span><span class="sxs-lookup"><span data-stu-id="f7d8f-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="f7d8f-107">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f7d8f-107">For example:</span></span>  

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
