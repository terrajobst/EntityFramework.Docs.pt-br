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
# <a name="no-tracking-queries"></a>Consultas sem acompanhamento
Às vezes, você talvez queira obter entidades de uma consulta, mas não tem essas entidades a ser acompanhado pelo contexto. Isso pode resultar em melhor desempenho ao consultar para um grande número de entidades em cenários somente leitura. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

Um novo método de extensão AsNoTracking permite que qualquer consulta ser executada dessa forma. Por exemplo:  

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
