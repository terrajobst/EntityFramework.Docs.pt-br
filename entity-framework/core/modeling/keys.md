---
title: Chaves (primárias)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197276"
---
# <a name="keys-primary"></a><span data-ttu-id="9da88-102">Chaves (primárias)</span><span class="sxs-lookup"><span data-stu-id="9da88-102">Keys (primary)</span></span>

<span data-ttu-id="9da88-103">Uma chave serve como o identificador exclusivo primário para cada instância de entidade.</span><span class="sxs-lookup"><span data-stu-id="9da88-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="9da88-104">Ao usar um banco de dados relacional, isso é mapeado para o conceito de uma *chave primária*.</span><span class="sxs-lookup"><span data-stu-id="9da88-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="9da88-105">Você também pode configurar um identificador exclusivo que não seja a chave primária (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="9da88-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="9da88-106">Um dos métodos a seguir pode ser usado para configurar/criar uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="9da88-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="9da88-107">Convenções</span><span class="sxs-lookup"><span data-stu-id="9da88-107">Conventions</span></span>

<span data-ttu-id="9da88-108">Por convenção, uma propriedade chamada `Id` ou `<type name>Id` será configurada como a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="9da88-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="9da88-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="9da88-109">Data Annotations</span></span>

<span data-ttu-id="9da88-110">Você pode usar as anotações de dados para configurar uma única propriedade para ser a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="9da88-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="9da88-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="9da88-111">Fluent API</span></span>

<span data-ttu-id="9da88-112">Você pode usar a API Fluent para configurar uma única propriedade para ser a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="9da88-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="9da88-113">Você também pode usar a API fluente para configurar várias propriedades para ser a chave de uma entidade (conhecida como chave composta).</span><span class="sxs-lookup"><span data-stu-id="9da88-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="9da88-114">As chaves compostas só podem ser configuradas usando a API fluente – as convenções nunca instalarão uma chave composta e você não poderá usar as anotações de dados para configurar uma.</span><span class="sxs-lookup"><span data-stu-id="9da88-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
