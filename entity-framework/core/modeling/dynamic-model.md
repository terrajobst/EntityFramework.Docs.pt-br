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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="1972a-102">Alternando entre vários modelos com o mesmo tipo DbContext</span><span class="sxs-lookup"><span data-stu-id="1972a-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="1972a-103">O modelo criado `OnModelCreating` pode usar uma propriedade no contexto para alterar como o modelo é criado.</span><span class="sxs-lookup"><span data-stu-id="1972a-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="1972a-104">Por exemplo, ele pode ser usado para excluir determinadas propriedades:</span><span class="sxs-lookup"><span data-stu-id="1972a-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="1972a-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="1972a-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="1972a-106">No entanto se você tentou fazer acima sem alterações adicionais você obteria o mesmo modelo toda vez que um novo contexto é criado para qualquer valor de `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="1972a-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="1972a-107">Isso é causado pelo modelo EF usa para melhorar o desempenho invocando somente do mecanismo de cache `OnModelCreating` uma vez e o modelo de cache.</span><span class="sxs-lookup"><span data-stu-id="1972a-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="1972a-108">Por padrão o EF presume que para qualquer contexto determinado tipo de modelo será o mesmo.</span><span class="sxs-lookup"><span data-stu-id="1972a-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="1972a-109">Para fazer isso a implementação padrão de `IModelCacheKeyFactory` retorna uma chave que contém apenas o tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="1972a-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="1972a-110">Para alterar isso, você precisa substituir o `IModelCacheKeyFactory` service.</span><span class="sxs-lookup"><span data-stu-id="1972a-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="1972a-111">A nova implementação deve retornar um objeto que pode ser comparado às outras chaves de modelo usando o `Equals` método que leva em conta todas as variáveis que afetam o modelo:</span><span class="sxs-lookup"><span data-stu-id="1972a-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
