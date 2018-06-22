---
title: Alternando entre vários modelos com o mesmo tipo DbContext - Core de EF
author: AndriySvyryd
ms.author: divega
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/dynamic-model
ms.openlocfilehash: 57136802001124ebf9ae7682e33f8dc7826fc2b0
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678716"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando entre vários modelos com o mesmo tipo DbContext

O modelo criado `OnModelCreating` pode usar uma propriedade no contexto para alterar como o modelo é criado. Por exemplo, ele pode ser usado para excluir determinadas propriedades:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
No entanto se você tentou fazer acima sem alterações adicionais você obteria o mesmo modelo toda vez que um novo contexto é criado para qualquer valor de `IgnoreIntProperty`. Isso é causado pelo modelo EF usa para melhorar o desempenho invocando somente do mecanismo de cache `OnModelCreating` uma vez e o modelo de cache.

Por padrão o EF presume que para qualquer contexto determinado tipo de modelo será o mesmo. Para fazer isso a implementação padrão de `IModelCacheKeyFactory` retorna uma chave que contém apenas o tipo de contexto. Para alterar isso, você precisa substituir o `IModelCacheKeyFactory` service. A nova implementação deve retornar um objeto que pode ser comparado às outras chaves de modelo usando o `Equals` método que leva em conta todas as variáveis que afetam o modelo:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
