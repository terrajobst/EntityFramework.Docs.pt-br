---
title: Chaves (primárias)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655944"
---
# <a name="keys-primary"></a><span data-ttu-id="523c8-102">Chaves (primárias)</span><span class="sxs-lookup"><span data-stu-id="523c8-102">Keys (primary)</span></span>

<span data-ttu-id="523c8-103">Uma chave serve como o identificador exclusivo primário para cada instância de entidade.</span><span class="sxs-lookup"><span data-stu-id="523c8-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="523c8-104">Ao usar um banco de dados relacional, isso é mapeado para o conceito de uma *chave primária*.</span><span class="sxs-lookup"><span data-stu-id="523c8-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="523c8-105">Você também pode configurar um identificador exclusivo que não seja a chave primária (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="523c8-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="523c8-106">Um dos métodos a seguir pode ser usado para configurar/criar uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="523c8-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="523c8-107">Convenções</span><span class="sxs-lookup"><span data-stu-id="523c8-107">Conventions</span></span>

<span data-ttu-id="523c8-108">Por convenção, uma propriedade chamada `Id` ou `<type name>Id` será configurada como a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="523c8-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a><span data-ttu-id="523c8-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="523c8-109">Data Annotations</span></span>

<span data-ttu-id="523c8-110">Você pode usar as anotações de dados para configurar uma única propriedade para ser a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="523c8-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="523c8-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="523c8-111">Fluent API</span></span>

<span data-ttu-id="523c8-112">Você pode usar a API Fluent para configurar uma única propriedade para ser a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="523c8-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="523c8-113">Você também pode usar a API fluente para configurar várias propriedades para ser a chave de uma entidade (conhecida como chave composta).</span><span class="sxs-lookup"><span data-stu-id="523c8-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="523c8-114">As chaves compostas só podem ser configuradas usando a API fluente – as convenções nunca instalarão uma chave composta e você não poderá usar as anotações de dados para configurar uma.</span><span class="sxs-lookup"><span data-stu-id="523c8-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
