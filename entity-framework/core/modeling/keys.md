---
title: Chaves (primárias)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655944"
---
# <a name="keys-primary"></a>Chaves (primárias)

Uma chave serve como o identificador exclusivo primário para cada instância de entidade. Ao usar um banco de dados relacional, isso é mapeado para o conceito de uma *chave primária*. Você também pode configurar um identificador exclusivo que não seja a chave primária (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).

Um dos métodos a seguir pode ser usado para configurar/criar uma chave primária.

## <a name="conventions"></a>Convenções

Por convenção, uma propriedade chamada `Id` ou `<type name>Id` será configurada como a chave de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar uma única propriedade para ser a chave de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma única propriedade para ser a chave de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Você também pode usar a API fluente para configurar várias propriedades para ser a chave de uma entidade (conhecida como chave composta). As chaves compostas só podem ser configuradas usando a API fluente – as convenções nunca instalarão uma chave composta e você não poderá usar as anotações de dados para configurar uma.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
