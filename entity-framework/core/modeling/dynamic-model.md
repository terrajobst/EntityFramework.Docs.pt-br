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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="8ca75-102">Alternando entre vários modelos com o mesmo tipo de DbContext</span><span class="sxs-lookup"><span data-stu-id="8ca75-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="8ca75-103">O modelo interno `OnModelCreating` pode usar uma propriedade no contexto para alterar a forma como o modelo é criado.</span><span class="sxs-lookup"><span data-stu-id="8ca75-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="8ca75-104">Por exemplo, ele poderia ser usado para excluir uma determinada propriedade:</span><span class="sxs-lookup"><span data-stu-id="8ca75-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="8ca75-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="8ca75-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="8ca75-106">No entanto, se você tentou fazer o anterior sem alterações adicionais, obteria o mesmo modelo sempre que um novo contexto for criado para qualquer valor de `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="8ca75-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="8ca75-107">Isso é causado pelo mecanismo de cache de modelo que o EF usa para melhorar o desempenho apenas invocando `OnModelCreating` uma vez e armazenando em cache o modelo.</span><span class="sxs-lookup"><span data-stu-id="8ca75-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="8ca75-108">Por padrão, o EF pressupõe que, para qualquer tipo de contexto específico, o modelo será o mesmo.</span><span class="sxs-lookup"><span data-stu-id="8ca75-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="8ca75-109">Para fazer isso, a implementação padrão de `IModelCacheKeyFactory` retorna uma chave que contém apenas o tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="8ca75-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="8ca75-110">Para alterar isso, você precisa substituir o serviço `IModelCacheKeyFactory`.</span><span class="sxs-lookup"><span data-stu-id="8ca75-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="8ca75-111">A nova implementação precisa retornar um objeto que pode ser comparado a outras chaves de modelo usando o método `Equals` que leva em conta todas as variáveis que afetam o modelo:</span><span class="sxs-lookup"><span data-stu-id="8ca75-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
