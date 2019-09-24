---
title: Incluindo & excluindo Propriedades – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197416"
---
# <a name="including--excluding-properties"></a>Incluindo & excluindo Propriedades

A inclusão de uma propriedade no modelo significa que o EF tem metadados sobre essa propriedade e tentará ler e gravar valores de/para o banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, as propriedades públicas com um getter e um setter serão incluídas no modelo.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para excluir uma propriedade do modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para excluir uma propriedade do modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
