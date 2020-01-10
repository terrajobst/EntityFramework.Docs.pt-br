---
title: Alternando entre vários modelos com o mesmo tipo de DbContext-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781125"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="4ea9f-102">Alternando entre vários modelos com o mesmo tipo de DbContext</span><span class="sxs-lookup"><span data-stu-id="4ea9f-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="4ea9f-103">O modelo interno `OnModelCreating` pode usar uma propriedade no contexto para alterar a forma como o modelo é criado.</span><span class="sxs-lookup"><span data-stu-id="4ea9f-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="4ea9f-104">Por exemplo, suponha que você quisesse configurar uma entidade de forma diferente com base em alguma propriedade:</span><span class="sxs-lookup"><span data-stu-id="4ea9f-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="4ea9f-105">Infelizmente, esse código não funcionaria como está, pois o EF compila o modelo e executa `OnModelCreating` apenas uma vez, armazenando em cache o resultado por motivos de desempenho.</span><span class="sxs-lookup"><span data-stu-id="4ea9f-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="4ea9f-106">No entanto, você pode conectar-se ao mecanismo de cache de modelo para fazer com que o EF reconheça a propriedade, produzindo modelos diferentes.</span><span class="sxs-lookup"><span data-stu-id="4ea9f-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="4ea9f-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="4ea9f-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="4ea9f-108">O EF usa o `IModelCacheKeyFactory` para gerar chaves de cache para modelos; Por padrão, o EF pressupõe que, para qualquer tipo de contexto específico, o modelo será o mesmo, então a implementação padrão desse serviço retorna uma chave que contém apenas o tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="4ea9f-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="4ea9f-109">Para produzir modelos diferentes do mesmo tipo de contexto, você precisa substituir o serviço `IModelCacheKeyFactory` pela implementação correta; a chave gerada será comparada com outras chaves de modelo usando o método `Equals`, levando em conta todas as variáveis que afetam o modelo:</span><span class="sxs-lookup"><span data-stu-id="4ea9f-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="4ea9f-110">A implementação a seguir leva o `IgnoreIntProperty` em conta ao produzir uma chave de cache de modelo:</span><span class="sxs-lookup"><span data-stu-id="4ea9f-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="4ea9f-111">Por fim, registre suas novas `IModelCacheKeyFactory` na `OnConfiguring`do seu contexto:</span><span class="sxs-lookup"><span data-stu-id="4ea9f-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="4ea9f-112">Consulte o [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) para obter mais contexto.</span><span class="sxs-lookup"><span data-stu-id="4ea9f-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
