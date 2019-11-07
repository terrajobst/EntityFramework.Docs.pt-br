---
title: Chaves alternativas-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655774"
---
# <a name="alternate-keys"></a>Chaves alternativas

Uma chave alternativa serve como um identificador exclusivo alternativo para cada instância de entidade, além da chave primária. Chaves alternativas podem ser usadas como o destino de uma relação. Ao usar um banco de dados relacional, ele é mapeado para o conceito de um índice/restrição exclusivo nas colunas de chave alternativas e uma ou mais restrições Foreign Key que fazem referência às colunas.

> [!TIP]  
> Se você quiser apenas impor a exclusividade de uma coluna, então você deseja um índice exclusivo em vez de uma chave alternativa, consulte [índices](indexes.md). No EF, chaves alternativas fornecem maior funcionalidade do que índices exclusivos, pois eles podem ser usados como o destino de uma chave estrangeira.

As chaves alternativas são normalmente introduzidas quando necessário e você não precisa configurá-las manualmente. Consulte [convenções](#conventions) para obter mais detalhes.

## <a name="conventions"></a>Convenções

Por convenção, uma chave alternativa é introduzida para você quando você identifica uma propriedade, que não é a chave primária, como o destino de uma relação.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a>Anotações de dados

Chaves alternativas não podem ser configuradas usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma única propriedade para ser uma chave alternativa.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

Você também pode usar a API Fluent para configurar várias propriedades para serem uma chave alternativa (conhecida como uma chave alternativa composta).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
