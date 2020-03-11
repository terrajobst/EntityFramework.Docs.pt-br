---
title: O método Load-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417119"
---
# <a name="the-load-method"></a><span data-ttu-id="a8da1-102">O método Load</span><span class="sxs-lookup"><span data-stu-id="a8da1-102">The Load Method</span></span>
<span data-ttu-id="a8da1-103">Há vários cenários em que você pode querer carregar entidades do banco de dados no contexto sem fazer imediatamente nada com essas entidades.</span><span class="sxs-lookup"><span data-stu-id="a8da1-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="a8da1-104">Um bom exemplo disso é carregar entidades para vinculação de dados, conforme descrito em [dados locais](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="a8da1-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="a8da1-105">Uma maneira comum de fazer isso é escrever uma consulta LINQ e, em seguida, chamar ToList nela, apenas para descartar imediatamente a lista criada.</span><span class="sxs-lookup"><span data-stu-id="a8da1-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="a8da1-106">O método de extensão de carga funciona exatamente como ToList, exceto que evita a criação da lista totalmente.</span><span class="sxs-lookup"><span data-stu-id="a8da1-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="a8da1-107">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="a8da1-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="a8da1-108">Aqui estão dois exemplos de como usar Load.</span><span class="sxs-lookup"><span data-stu-id="a8da1-108">Here are two examples of using Load.</span></span> <span data-ttu-id="a8da1-109">O primeiro é obtido de um aplicativo de associação de dados Windows Forms em que a carga é usada para consultar entidades antes de fazer a ligação com a coleção local, conforme descrito em [dados locais](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="a8da1-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="a8da1-110">O segundo exemplo mostra o uso de Load para carregar uma coleção filtrada de entidades relacionadas, conforme descrito em [carregando entidades relacionadas](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="a8da1-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
