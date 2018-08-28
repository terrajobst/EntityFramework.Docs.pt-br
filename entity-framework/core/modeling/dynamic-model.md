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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="1b229-102">Alternando entre vários modelos com o mesmo tipo de DbContext</span><span class="sxs-lookup"><span data-stu-id="1b229-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="1b229-103">O modelo criado `OnModelCreating` poderia usar uma propriedade no contexto para alterar como o modelo é criado.</span><span class="sxs-lookup"><span data-stu-id="1b229-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="1b229-104">Por exemplo, ele pode ser usado para excluir determinadas propriedades:</span><span class="sxs-lookup"><span data-stu-id="1b229-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="1b229-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="1b229-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="1b229-106">No entanto se você tentou usar o método acima sem alterações adicionais você obteria o mesmo modelo sempre que um novo contexto é criado para qualquer valor de `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="1b229-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="1b229-107">Isso é causado pelo modelo usa o EF para melhorar o desempenho invocando somente do mecanismo de cache `OnModelCreating` uma vez e o modelo de armazenamento em cache.</span><span class="sxs-lookup"><span data-stu-id="1b229-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="1b229-108">Por padrão o EF pressupõe que para qualquer contexto de determinado tipo de modelo será o mesmo.</span><span class="sxs-lookup"><span data-stu-id="1b229-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="1b229-109">Para fazer isso a implementação padrão de `IModelCacheKeyFactory` retorna uma chave que contém apenas o tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="1b229-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="1b229-110">Para alterar isso, é necessário substituir o `IModelCacheKeyFactory` service.</span><span class="sxs-lookup"><span data-stu-id="1b229-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="1b229-111">A nova implementação precisa retornar um objeto que pode ser comparado para outras chaves de modelo usando o `Equals` método que leva em conta todas as variáveis que afetam o modelo:</span><span class="sxs-lookup"><span data-stu-id="1b229-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
