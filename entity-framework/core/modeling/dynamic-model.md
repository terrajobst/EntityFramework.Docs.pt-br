---
title: Alternando entre vários modelos com o mesmo tipo de DbContext-EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655720"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando entre vários modelos com o mesmo tipo de DbContext

O modelo interno `OnModelCreating` pode usar uma propriedade no contexto para alterar a forma como o modelo é criado. Por exemplo, ele poderia ser usado para excluir uma determinada propriedade:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

No entanto, se você tentou fazer o anterior sem alterações adicionais, obteria o mesmo modelo sempre que um novo contexto for criado para qualquer valor de `IgnoreIntProperty`. Isso é causado pelo mecanismo de cache de modelo que o EF usa para melhorar o desempenho apenas invocando `OnModelCreating` uma vez e armazenando em cache o modelo.

Por padrão, o EF pressupõe que, para qualquer tipo de contexto específico, o modelo será o mesmo. Para fazer isso, a implementação padrão de `IModelCacheKeyFactory` retorna uma chave que contém apenas o tipo de contexto. Para alterar isso, você precisa substituir o serviço `IModelCacheKeyFactory`. A nova implementação precisa retornar um objeto que pode ser comparado a outras chaves de modelo usando o método `Equals` que leva em conta todas as variáveis que afetam o modelo:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
