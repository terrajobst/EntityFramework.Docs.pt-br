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
# <a name="alternate-keys"></a><span data-ttu-id="e88c7-102">Chaves alternativas</span><span class="sxs-lookup"><span data-stu-id="e88c7-102">Alternate Keys</span></span>

<span data-ttu-id="e88c7-103">Uma chave alternativa serve como um identificador exclusivo alternativo para cada instância de entidade, além da chave primária.</span><span class="sxs-lookup"><span data-stu-id="e88c7-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="e88c7-104">Chaves alternativas podem ser usadas como o destino de uma relação.</span><span class="sxs-lookup"><span data-stu-id="e88c7-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="e88c7-105">Ao usar um banco de dados relacional, ele é mapeado para o conceito de um índice/restrição exclusivo nas colunas de chave alternativas e uma ou mais restrições Foreign Key que fazem referência às colunas.</span><span class="sxs-lookup"><span data-stu-id="e88c7-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="e88c7-106">Se você quiser apenas impor a exclusividade de uma coluna, então você deseja um índice exclusivo em vez de uma chave alternativa, consulte [índices](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="e88c7-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="e88c7-107">No EF, chaves alternativas fornecem maior funcionalidade do que índices exclusivos, pois eles podem ser usados como o destino de uma chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="e88c7-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="e88c7-108">As chaves alternativas são normalmente introduzidas quando necessário e você não precisa configurá-las manualmente.</span><span class="sxs-lookup"><span data-stu-id="e88c7-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="e88c7-109">Consulte [convenções](#conventions) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="e88c7-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="e88c7-110">Convenções</span><span class="sxs-lookup"><span data-stu-id="e88c7-110">Conventions</span></span>

<span data-ttu-id="e88c7-111">Por convenção, uma chave alternativa é introduzida para você quando você identifica uma propriedade, que não é a chave primária, como o destino de uma relação.</span><span class="sxs-lookup"><span data-stu-id="e88c7-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a><span data-ttu-id="e88c7-112">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e88c7-112">Data Annotations</span></span>

<span data-ttu-id="e88c7-113">Chaves alternativas não podem ser configuradas usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="e88c7-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e88c7-114">API fluente</span><span class="sxs-lookup"><span data-stu-id="e88c7-114">Fluent API</span></span>

<span data-ttu-id="e88c7-115">Você pode usar a API Fluent para configurar uma única propriedade para ser uma chave alternativa.</span><span class="sxs-lookup"><span data-stu-id="e88c7-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

<span data-ttu-id="e88c7-116">Você também pode usar a API Fluent para configurar várias propriedades para serem uma chave alternativa (conhecida como uma chave alternativa composta).</span><span class="sxs-lookup"><span data-stu-id="e88c7-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
