---
title: Chaves (primárias)-EF Core
description: Como configurar chaves para tipos de entidade ao usar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824612"
---
# <a name="keys-primary"></a><span data-ttu-id="e53af-103">Chaves (primárias)</span><span class="sxs-lookup"><span data-stu-id="e53af-103">Keys (primary)</span></span>

<span data-ttu-id="e53af-104">Uma chave serve como o identificador exclusivo primário para cada instância de entidade.</span><span class="sxs-lookup"><span data-stu-id="e53af-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="e53af-105">Ao usar um banco de dados relacional, isso é mapeado para o conceito de uma *chave primária*.</span><span class="sxs-lookup"><span data-stu-id="e53af-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="e53af-106">Você também pode configurar um identificador exclusivo que não seja a chave primária (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="e53af-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="e53af-107">Um dos métodos a seguir pode ser usado para configurar/criar uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="e53af-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="e53af-108">Convenções</span><span class="sxs-lookup"><span data-stu-id="e53af-108">Conventions</span></span>

<span data-ttu-id="e53af-109">Por padrão, uma propriedade chamada `Id` ou `<type name>Id` será configurada como a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="e53af-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="e53af-110">[Tipos de entidade pertencentes](xref:core/modeling/owned-entities) usam regras diferentes para definir chaves.</span><span class="sxs-lookup"><span data-stu-id="e53af-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e53af-111">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e53af-111">Data Annotations</span></span>

<span data-ttu-id="e53af-112">Você pode usar as anotações de dados para configurar uma única propriedade para ser a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="e53af-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="e53af-113">API fluente</span><span class="sxs-lookup"><span data-stu-id="e53af-113">Fluent API</span></span>

<span data-ttu-id="e53af-114">Você pode usar a API Fluent para configurar uma única propriedade para ser a chave de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="e53af-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="e53af-115">Você também pode usar a API fluente para configurar várias propriedades para ser a chave de uma entidade (conhecida como chave composta).</span><span class="sxs-lookup"><span data-stu-id="e53af-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="e53af-116">As chaves compostas só podem ser configuradas usando a API fluente – as convenções nunca instalarão uma chave composta e você não poderá usar as anotações de dados para configurar uma.</span><span class="sxs-lookup"><span data-stu-id="e53af-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="e53af-117">Tipos de chave e valores</span><span class="sxs-lookup"><span data-stu-id="e53af-117">Key types and values</span></span>

<span data-ttu-id="e53af-118">O EF Core dá suporte ao uso de propriedades de qualquer tipo primitivo como chave primária, incluindo `string`, `Guid`, `byte[]` e outros.</span><span class="sxs-lookup"><span data-stu-id="e53af-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="e53af-119">Mas nem todos os bancos de dados dão suporte a eles.</span><span class="sxs-lookup"><span data-stu-id="e53af-119">But not all databases support them.</span></span> <span data-ttu-id="e53af-120">Em alguns casos, os valores de chave podem ser convertidos em um tipo com suporte automaticamente; caso contrário, a conversão deve ser [especificada manualmente](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="e53af-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="e53af-121">As propriedades de chave sempre devem ter um valor não padrão ao adicionar uma nova entidade ao contexto, mas alguns tipos serão [gerados pelo banco de dados](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="e53af-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="e53af-122">Nesse caso, o EF tentará gerar um valor temporário quando a entidade for adicionada para fins de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="e53af-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="e53af-123">Depois que [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) for chamado, o valor temporário será substituído pelo valor gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e53af-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="e53af-124">Se uma propriedade de chave tiver um valor gerado pelo banco de dados e um valor não padrão for especificado quando uma entidade for adicionada, o EF irá pressupor que a entidade já existe no banco de dados e tentará atualizá-la em vez de inserir uma nova.</span><span class="sxs-lookup"><span data-stu-id="e53af-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="e53af-125">Para evitar isso, desative a geração de valor ou veja [como especificar valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="e53af-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>