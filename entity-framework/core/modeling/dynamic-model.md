---
title: Alternando entre vários modelos com o mesmo tipo de DbContext – EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994980"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando entre vários modelos com o mesmo tipo de DbContext

O modelo criado `OnModelCreating` poderia usar uma propriedade no contexto para alterar como o modelo é criado. Por exemplo, ele pode ser usado para excluir determinadas propriedades:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
No entanto se você tentou usar o método acima sem alterações adicionais você obteria o mesmo modelo sempre que um novo contexto é criado para qualquer valor de `IgnoreIntProperty`. Isso é causado pelo modelo usa o EF para melhorar o desempenho invocando somente do mecanismo de cache `OnModelCreating` uma vez e o modelo de armazenamento em cache.

Por padrão o EF pressupõe que para qualquer contexto de determinado tipo de modelo será o mesmo. Para fazer isso a implementação padrão de `IModelCacheKeyFactory` retorna uma chave que contém apenas o tipo de contexto. Para alterar isso, é necessário substituir o `IModelCacheKeyFactory` service. A nova implementação precisa retornar um objeto que pode ser comparado para outras chaves de modelo usando o `Equals` método que leva em conta todas as variáveis que afetam o modelo:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
