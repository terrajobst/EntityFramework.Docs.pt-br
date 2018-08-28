---
title: Comparar EF Core e EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997118"
---
# <a name="compare-ef-core--ef6"></a>Comparar EF Core e EF6

Há duas versões do Entity Framework: Entity Framework Core e Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

O Entity Framework 6 (EF6) é uma tecnologia de acesso a dados testada e com muitos anos de recursos e estabilização. Foi lançado em 2008 como parte do .NET Framework 3.5 SP1 e do Visual Studio 2008 SP1. Da versão EF4.1 em diante, é enviado como o [pacote EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) – atualmente um dos pacotes mais populares em NuGet.org.

O EF6 continua sendo um produto compatível e continuaremos a observar correções de bug e pequenas melhorias por algum tempo.

## <a name="entity-framework-core"></a>Entity Framework Core

O EF Core (Entity Framework Core) é uma versão leve, extensível e de multiplaforma do Entity Framework. O EF Core apresenta vários aprimoramentos e novos recursos em comparação ao EF6. Ao mesmo tempo, o EF Core é uma nova base de código e não está tão maduro quanto o EF6.

O EF Core mantém a experiência do desenvolvedor do EF6 e a maioria das APIs de nível superior permanece a mesma também, portanto, o EF Core será bastante familiar para quem usou o EF6. Ao mesmo tempo, o EF Core baseia-se em um conjunto completamente novo de componentes centrais. Isso significa que o EF Core não herda automaticamente todos os recursos do EF6. Enquanto alguns desses recursos aparecerão em versões futuras, outros recursos menos usados não serão implementados no EF Core.

O novo núcleo extensível e leve também nos permitiu adicionar alguns recursos ao EF Core que não serão implementados no EF6 (como chaves alternativas, atualizações em lote e avaliação mista de cliente/banco de dados em consultas LINQ).
