---
title: O método Load - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
caps.latest.revision: 3
ms.openlocfilehash: 83af79220b52de6e3063868fd9bdac56867d49cb
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119831"
---
# <a name="the-load-method"></a>O método Load
Há vários cenários em que você talvez queira carregar entidades do banco de dados no contexto sem imediatamente fazer alguma coisa com essas entidades. Um bom exemplo disso é carregar entidades para vinculação de dados, conforme descrito em [dados locais](~/ef6/querying/local-data.md). Uma maneira comum de fazer isso é escrever uma consulta LINQ e, em seguida, chamar ToList nela, apenas para descartar imediatamente a lista criada. O método de extensão de carga funciona como ToList, exceto que evita completamente a criação da lista.  

As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

Aqui estão dois exemplos de uso de carga. A primeira é obtida por um aplicativo de associação de dados de formulários do Windows em que a carga é usada para consultar entidades antes da associação à coleção local, conforme descrito em [dados locais](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

A segundo exemplo mostra como usar carga para carregar uma coleção filtrada de entidades relacionadas, conforme descrito em [Carregando entidades relacionadas](~/ef6/querying/related-data.md):  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
