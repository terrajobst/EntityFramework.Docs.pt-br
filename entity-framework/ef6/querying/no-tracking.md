---
title: Consultas sem controle - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
caps.latest.revision: 3
ms.openlocfilehash: 8310f2dab9e7ed9197a8c3e875e47e4f7b72d279
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119826"
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
