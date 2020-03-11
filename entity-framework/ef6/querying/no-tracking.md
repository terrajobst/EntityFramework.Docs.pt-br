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
# <a name="no-tracking-queries"></a>Consultas sem acompanhamento
Às vezes, você pode querer obter entidades de volta de uma consulta, mas não ter essas entidades rastreadas pelo contexto. Isso pode resultar em melhor desempenho ao consultar um grande número de entidades em cenários somente leitura. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

Um novo método de extensão AsNoTracking permite que qualquer consulta seja executada dessa maneira. Por exemplo:  

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
