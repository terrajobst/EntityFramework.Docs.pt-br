---
title: Propriedades obrigatórios/opcionais – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929804"
---
# <a name="required-and-optional-properties"></a>Propriedades obrigatórias e opcionais

Uma propriedade é considerada opcional se ele for válido para que ele contenha `null`. Se `null` não é um valor válido a ser atribuído a uma propriedade, em seguida, ele é considerado uma propriedade necessária.

## <a name="conventions"></a>Convenções

Por convenção, uma propriedade cujo tipo CLR pode conter nulos será configurada como opcional (`string`, `int?`, `byte[]`, etc.). As propriedades cujo tipo CLR não pode conter nulos serão configuradas conforme necessário (`int`, `decimal`, `bool`, etc.).

> [!NOTE]  
> Uma propriedade cujo tipo CLR não pode conter nulo não pode ser configurada como opcional. A propriedade sempre será considerada necessário pelo Entity Framework.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para indicar que uma propriedade é necessária.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para indicar que uma propriedade é necessária.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

