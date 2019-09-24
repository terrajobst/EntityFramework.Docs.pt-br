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
# <a name="including--excluding-properties"></a><span data-ttu-id="235bd-102">Incluindo & excluindo Propriedades</span><span class="sxs-lookup"><span data-stu-id="235bd-102">Including & Excluding Properties</span></span>

<span data-ttu-id="235bd-103">A inclusão de uma propriedade no modelo significa que o EF tem metadados sobre essa propriedade e tentará ler e gravar valores de/para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="235bd-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="235bd-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="235bd-104">Conventions</span></span>

<span data-ttu-id="235bd-105">Por convenção, as propriedades públicas com um getter e um setter serão incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="235bd-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="235bd-106">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="235bd-106">Data Annotations</span></span>

<span data-ttu-id="235bd-107">Você pode usar anotações de dados para excluir uma propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="235bd-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="235bd-108">API fluente</span><span class="sxs-lookup"><span data-stu-id="235bd-108">Fluent API</span></span>

<span data-ttu-id="235bd-109">Você pode usar a API fluente para excluir uma propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="235bd-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
