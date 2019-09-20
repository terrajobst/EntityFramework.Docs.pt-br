---
title: Propriedades obrigatórias/opcionais-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149146"
---
# <a name="required-and-optional-properties"></a>Propriedades obrigatórias e opcionais

Uma propriedade será considerada opcional se for válida para conter `null`. Se `null` não for um valor válido a ser atribuído a uma propriedade, será considerada uma propriedade necessária.

## <a name="conventions"></a>Convenções

Por convenção, uma propriedade cujo tipo .net pode conter NULL será configurada como opcional`string`( `int?`, `byte[]`,, etc.). As propriedades cujo tipo CLR não pode conter NULL serão configuradas`int`conforme `decimal`necessário `bool`(,,, etc.).

> [!NOTE]  
> Uma propriedade cujo tipo .NET não pode conter NULL não pode ser configurada como opcional. A propriedade sempre será considerada exigida pelo Entity Framework.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para indicar que uma propriedade é necessária.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para indicar que uma propriedade é necessária.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

