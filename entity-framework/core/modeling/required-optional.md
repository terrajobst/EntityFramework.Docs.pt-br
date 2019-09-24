---
title: Propriedades obrigatórias e opcionais-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: fd9e96e6f79965e63b07c21217edd004fd5c4d54
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197842"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="995a3-102">Propriedades obrigatórias e opcionais</span><span class="sxs-lookup"><span data-stu-id="995a3-102">Required and Optional Properties</span></span>

<span data-ttu-id="995a3-103">Uma propriedade será considerada opcional se for válida para conter `null`.</span><span class="sxs-lookup"><span data-stu-id="995a3-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="995a3-104">Se `null` não for um valor válido a ser atribuído a uma propriedade, será considerada uma propriedade necessária.</span><span class="sxs-lookup"><span data-stu-id="995a3-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="995a3-105">Ao mapear para um esquema de banco de dados relacional, as propriedades obrigatórias são criadas como colunas não anuláveis e as propriedades opcionais são criadas como colunas anuláveis.</span><span class="sxs-lookup"><span data-stu-id="995a3-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="995a3-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="995a3-106">Conventions</span></span>

<span data-ttu-id="995a3-107">Por convenção, uma propriedade cujo tipo .NET pode conter NULL será configurada como opcional, enquanto as propriedades cujo tipo .NET não pode conter NULL serão configuradas conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="995a3-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="995a3-108">Por exemplo, todas as propriedades com tipos de valor`int`do `decimal`.NET `bool`(,,, etc.) são configuradas como obrigatórias, e todas as propriedades `decimal?`com `bool?`tipos de valor do .net anuláveis (`int?`,,, etc.) são configurado como opcional.</span><span class="sxs-lookup"><span data-stu-id="995a3-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="995a3-109">C#8 introduziu um novo recurso chamado [tipos de referência anulável](/dotnet/csharp/tutorials/nullable-reference-types), que permite que os tipos de referência sejam anotados, indicando se é válido para que eles contenham NULL ou não.</span><span class="sxs-lookup"><span data-stu-id="995a3-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="995a3-110">Esse recurso está desabilitado por padrão e, se habilitado, ele modifica o comportamento do EF Core da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="995a3-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="995a3-111">Se tipos de referência anuláveis estiverem desabilitados (o padrão), todas as propriedades com tipos de referência do .NET serão configuradas como opcionais por convenção (por exemplo `string`,).</span><span class="sxs-lookup"><span data-stu-id="995a3-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="995a3-112">Se os tipos de C# referência anuláveis estiverem habilitados, as propriedades serão configuradas com base na nulidade `string?` de seu tipo .net: serão configuradas como opcionais, enquanto `string` que serão configuradas conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="995a3-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="995a3-113">O exemplo a seguir mostra um tipo de entidade com propriedades obrigatórias e opcionais, com o recurso de referência anulável desabilitado (o padrão) e habilitado:</span><span class="sxs-lookup"><span data-stu-id="995a3-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="995a3-114">Sem tipos de referência anuláveis (padrão)</span><span class="sxs-lookup"><span data-stu-id="995a3-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="995a3-115">Com tipos de referência anuláveis</span><span class="sxs-lookup"><span data-stu-id="995a3-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="995a3-116">O uso de tipos de referência anuláveis é recomendado, pois ele flui C# a nulidade expressa em código para o modelo de EF Core e para o banco de dados e dispensa o uso da API fluente ou das anotações de data para expressar o mesmo conceito duas vezes.</span><span class="sxs-lookup"><span data-stu-id="995a3-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="995a3-117">Tenha cuidado ao habilitar tipos de referência anuláveis em um projeto existente: as propriedades do tipo de referência que foram previamente configuradas como opcionais agora serão configuradas conforme necessário, a menos que sejam anotadas explicitamente para permitir valor nulo.</span><span class="sxs-lookup"><span data-stu-id="995a3-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="995a3-118">Ao gerenciar um esquema de banco de dados relacional, isso pode fazer com que as migrações sejam geradas, alterando a nulidade da coluna do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="995a3-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="995a3-119">Para obter mais informações sobre tipos de referência anuláveis e como usá-los com EF Core, [consulte a página de documentação dedicada para esse recurso](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="995a3-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="995a3-120">Configuração</span><span class="sxs-lookup"><span data-stu-id="995a3-120">Configuration</span></span>

<span data-ttu-id="995a3-121">Uma propriedade que seria opcional por convenção pode ser configurada para ser necessária da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="995a3-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="995a3-122">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="995a3-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="995a3-123">API fluente</span><span class="sxs-lookup"><span data-stu-id="995a3-123">Fluent API</span></span>](#tab/fluent-api) 

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***