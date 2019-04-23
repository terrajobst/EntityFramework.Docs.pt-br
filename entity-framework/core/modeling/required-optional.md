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
# <a name="required-and-optional-properties"></a><span data-ttu-id="ff9c3-102">Propriedades obrigatórias e opcionais</span><span class="sxs-lookup"><span data-stu-id="ff9c3-102">Required and Optional Properties</span></span>

<span data-ttu-id="ff9c3-103">Uma propriedade é considerada opcional se ele for válido para que ele contenha `null`.</span><span class="sxs-lookup"><span data-stu-id="ff9c3-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="ff9c3-104">Se `null` não é um valor válido a ser atribuído a uma propriedade, em seguida, ele é considerado uma propriedade necessária.</span><span class="sxs-lookup"><span data-stu-id="ff9c3-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="ff9c3-105">Convenções</span><span class="sxs-lookup"><span data-stu-id="ff9c3-105">Conventions</span></span>

<span data-ttu-id="ff9c3-106">Por convenção, uma propriedade cujo tipo CLR pode conter nulos será configurada como opcional (`string`, `int?`, `byte[]`, etc.).</span><span class="sxs-lookup"><span data-stu-id="ff9c3-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="ff9c3-107">As propriedades cujo tipo CLR não pode conter nulos serão configuradas conforme necessário (`int`, `decimal`, `bool`, etc.).</span><span class="sxs-lookup"><span data-stu-id="ff9c3-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="ff9c3-108">Uma propriedade cujo tipo CLR não pode conter nulo não pode ser configurada como opcional.</span><span class="sxs-lookup"><span data-stu-id="ff9c3-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="ff9c3-109">A propriedade sempre será considerada necessário pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ff9c3-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ff9c3-110">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="ff9c3-110">Data Annotations</span></span>

<span data-ttu-id="ff9c3-111">Você pode usar anotações de dados para indicar que uma propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="ff9c3-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="ff9c3-112">API fluente</span><span class="sxs-lookup"><span data-stu-id="ff9c3-112">Fluent API</span></span>

<span data-ttu-id="ff9c3-113">Você pode usar a API Fluent para indicar que uma propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="ff9c3-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

