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
# <a name="required-and-optional-properties"></a><span data-ttu-id="62a24-102">Propriedades obrigatórias e opcionais</span><span class="sxs-lookup"><span data-stu-id="62a24-102">Required and Optional Properties</span></span>

<span data-ttu-id="62a24-103">Uma propriedade será considerada opcional se for válida para conter `null`.</span><span class="sxs-lookup"><span data-stu-id="62a24-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="62a24-104">Se `null` não for um valor válido a ser atribuído a uma propriedade, será considerada uma propriedade necessária.</span><span class="sxs-lookup"><span data-stu-id="62a24-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="62a24-105">Convenções</span><span class="sxs-lookup"><span data-stu-id="62a24-105">Conventions</span></span>

<span data-ttu-id="62a24-106">Por convenção, uma propriedade cujo tipo .net pode conter NULL será configurada como opcional`string`( `int?`, `byte[]`,, etc.).</span><span class="sxs-lookup"><span data-stu-id="62a24-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="62a24-107">As propriedades cujo tipo CLR não pode conter NULL serão configuradas`int`conforme `decimal`necessário `bool`(,,, etc.).</span><span class="sxs-lookup"><span data-stu-id="62a24-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="62a24-108">Uma propriedade cujo tipo .NET não pode conter NULL não pode ser configurada como opcional.</span><span class="sxs-lookup"><span data-stu-id="62a24-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="62a24-109">A propriedade sempre será considerada exigida pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="62a24-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="62a24-110">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="62a24-110">Data Annotations</span></span>

<span data-ttu-id="62a24-111">Você pode usar anotações de dados para indicar que uma propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="62a24-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="62a24-112">API fluente</span><span class="sxs-lookup"><span data-stu-id="62a24-112">Fluent API</span></span>

<span data-ttu-id="62a24-113">Você pode usar a API fluente para indicar que uma propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="62a24-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

