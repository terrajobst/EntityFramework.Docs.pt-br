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
# <a name="including--excluding-properties"></a><span data-ttu-id="db9d4-102">Incluir e excluir propriedades</span><span class="sxs-lookup"><span data-stu-id="db9d4-102">Including & Excluding Properties</span></span>

<span data-ttu-id="db9d4-103">Incluindo uma propriedade no modelo significa que o EF tem metadados sobre essa propriedade e tentará ler e gravar valores de/para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="db9d4-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="db9d4-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="db9d4-104">Conventions</span></span>

<span data-ttu-id="db9d4-105">Por convenção, as propriedades públicas com um getter e um setter serão incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="db9d4-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="db9d4-106">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="db9d4-106">Data Annotations</span></span>

<span data-ttu-id="db9d4-107">Você pode usar anotações de dados para excluir uma propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="db9d4-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="db9d4-108">API fluente</span><span class="sxs-lookup"><span data-stu-id="db9d4-108">Fluent API</span></span>

<span data-ttu-id="db9d4-109">Você pode usar a API Fluent para excluir uma propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="db9d4-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
