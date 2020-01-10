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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando entre vários modelos com o mesmo tipo de DbContext

O modelo interno `OnModelCreating` pode usar uma propriedade no contexto para alterar a forma como o modelo é criado. Por exemplo, suponha que você quisesse configurar uma entidade de forma diferente com base em alguma propriedade:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

Infelizmente, esse código não funcionaria como está, pois o EF compila o modelo e executa `OnModelCreating` apenas uma vez, armazenando em cache o resultado por motivos de desempenho. No entanto, você pode conectar-se ao mecanismo de cache de modelo para fazer com que o EF reconheça a propriedade, produzindo modelos diferentes.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

O EF usa o `IModelCacheKeyFactory` para gerar chaves de cache para modelos; Por padrão, o EF pressupõe que, para qualquer tipo de contexto específico, o modelo será o mesmo, então a implementação padrão desse serviço retorna uma chave que contém apenas o tipo de contexto. Para produzir modelos diferentes do mesmo tipo de contexto, você precisa substituir o serviço `IModelCacheKeyFactory` pela implementação correta; a chave gerada será comparada com outras chaves de modelo usando o método `Equals`, levando em conta todas as variáveis que afetam o modelo:

A implementação a seguir leva o `IgnoreIntProperty` em conta ao produzir uma chave de cache de modelo:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Por fim, registre suas novas `IModelCacheKeyFactory` na `OnConfiguring`do seu contexto:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

Consulte o [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) para obter mais contexto.
