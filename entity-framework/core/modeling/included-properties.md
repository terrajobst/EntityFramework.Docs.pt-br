---
title: Incluir e excluir propriedades – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929817"
---
# <a name="including--excluding-properties"></a>Incluir e excluir propriedades

Incluindo uma propriedade no modelo significa que o EF tem metadados sobre essa propriedade e tentará ler e gravar valores de/para o banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, as propriedades públicas com um getter e um setter serão incluídas no modelo.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para excluir uma propriedade do modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para excluir uma propriedade do modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
