---
title: Comparar EF Core e EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4609ecbc9e24d8a359694d256523c64141b5ff62
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2018
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="fff30-102">Comparar EF Core e EF6</span><span class="sxs-lookup"><span data-stu-id="fff30-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="fff30-103">Há duas versões do Entity Framework: Entity Framework Core e Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="fff30-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="fff30-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="fff30-104">Entity Framework 6</span></span>

<span data-ttu-id="fff30-105">O Entity Framework 6 (EF6) é uma tecnologia de acesso a dados testada e com muitos anos de recursos e estabilização.</span><span class="sxs-lookup"><span data-stu-id="fff30-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="fff30-106">Foi lançado em 2008 como parte do .NET Framework 3.5 SP1 e do Visual Studio 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="fff30-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="fff30-107">Da versão EF4.1 em diante, é enviado como o [pacote EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) – atualmente um dos pacotes mais populares em NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="fff30-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="fff30-108">O EF6 continua sendo um produto compatível e continuaremos a observar correções de bug e pequenas melhorias por algum tempo.</span><span class="sxs-lookup"><span data-stu-id="fff30-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="fff30-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="fff30-109">Entity Framework Core</span></span>

<span data-ttu-id="fff30-110">O EF Core (Entity Framework Core) é uma versão leve, extensível e de multiplaforma do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fff30-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="fff30-111">O EF Core apresenta vários aprimoramentos e novos recursos em comparação ao EF6.</span><span class="sxs-lookup"><span data-stu-id="fff30-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="fff30-112">Ao mesmo tempo, o EF Core é uma nova base de código e não está tão maduro quanto o EF6.</span><span class="sxs-lookup"><span data-stu-id="fff30-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="fff30-113">O EF Core mantém a experiência do desenvolvedor do EF6 e a maioria das APIs de nível superior permanece a mesma também, portanto, o EF Core será bastante familiar para quem usou o EF6.</span><span class="sxs-lookup"><span data-stu-id="fff30-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="fff30-114">Ao mesmo tempo, o EF Core baseia-se em um conjunto completamente novo de componentes centrais.</span><span class="sxs-lookup"><span data-stu-id="fff30-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="fff30-115">Isso significa que o EF Core não herda automaticamente todos os recursos do EF6.</span><span class="sxs-lookup"><span data-stu-id="fff30-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="fff30-116">Enquanto alguns desses recursos aparecerão em versões futuras, outros recursos menos usados não serão implementados no EF Core.</span><span class="sxs-lookup"><span data-stu-id="fff30-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="fff30-117">O novo núcleo extensível e leve também nos permitiu adicionar alguns recursos ao EF Core que não serão implementados no EF6 (como chaves alternativas, atualizações em lote e avaliação mista de cliente/banco de dados em consultas LINQ).</span><span class="sxs-lookup"><span data-stu-id="fff30-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
