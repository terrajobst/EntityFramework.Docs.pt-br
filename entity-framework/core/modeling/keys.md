---
title: Chaves-EF Core
description: Como configurar chaves para tipos de entidade ao usar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416463"
---
# <a name="keys"></a><span data-ttu-id="d01bd-103">simétricas</span><span class="sxs-lookup"><span data-stu-id="d01bd-103">Keys</span></span>

<span data-ttu-id="d01bd-104">Uma chave serve como um identificador exclusivo para cada instância de entidade.</span><span class="sxs-lookup"><span data-stu-id="d01bd-104">A key serves as a unique identifier for each entity instance.</span></span> <span data-ttu-id="d01bd-105">A maioria das entidades no EF tem uma única chave, que é mapeada para o conceito de uma *chave primária* em bancos de dados relacionais (para entidades sem chaves, consulte [entidades inferiores](xref:core/modeling/keyless-entity-types)).</span><span class="sxs-lookup"><span data-stu-id="d01bd-105">Most entities in EF have a single key, which maps to the concept of a *primary key* in relational databases (for entities without keys, see [Keyless entities](xref:core/modeling/keyless-entity-types)).</span></span> <span data-ttu-id="d01bd-106">As entidades podem ter chaves adicionais além da chave primária (consulte [chaves alternativas](#alternate-keys) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="d01bd-106">Entities can have additional keys beyond the primary key (see [Alternate Keys](#alternate-keys) for more information).</span></span>

<span data-ttu-id="d01bd-107">Por convenção, uma propriedade chamada `Id` ou `<type name>Id` será configurada como a chave primária de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="d01bd-107">By convention, a property named `Id` or `<type name>Id` will be configured as the primary key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> <span data-ttu-id="d01bd-108">[Tipos de entidade pertencentes](xref:core/modeling/owned-entities) usam regras diferentes para definir chaves.</span><span class="sxs-lookup"><span data-stu-id="d01bd-108">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

<span data-ttu-id="d01bd-109">Você pode configurar uma única propriedade para ser a chave primária de uma entidade da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="d01bd-109">You can configure a single property to be the primary key of an entity as follows:</span></span>

## <a name="data-annotations"></a>[<span data-ttu-id="d01bd-110">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="d01bd-110">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-api"></a>[<span data-ttu-id="d01bd-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="d01bd-111">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

<span data-ttu-id="d01bd-112">Você também pode configurar várias propriedades para ser a chave de uma entidade – isso é conhecido como uma chave composta.</span><span class="sxs-lookup"><span data-stu-id="d01bd-112">You can also configure multiple properties to be the key of an entity - this is known as a composite key.</span></span> <span data-ttu-id="d01bd-113">As chaves compostas só podem ser configuradas usando a API Fluent; as convenções nunca configurarão uma chave composta e você não poderá usar as anotações de dados para configurar uma.</span><span class="sxs-lookup"><span data-stu-id="d01bd-113">Composite keys can only be configured using the Fluent API; conventions will never setup a composite key, and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a><span data-ttu-id="d01bd-114">Nome da chave primária</span><span class="sxs-lookup"><span data-stu-id="d01bd-114">Primary key name</span></span>

<span data-ttu-id="d01bd-115">Por convenção, em bancos de dados relacionais, as chaves primárias são criadas com o nome `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="d01bd-115">By convention, on relational databases primary keys are created with the name `PK_<type name>`.</span></span> <span data-ttu-id="d01bd-116">Você pode configurar o nome da restrição PRIMARY KEY da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="d01bd-116">You can configure the name of the primary key constraint as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a><span data-ttu-id="d01bd-117">Tipos de chave e valores</span><span class="sxs-lookup"><span data-stu-id="d01bd-117">Key types and values</span></span>

<span data-ttu-id="d01bd-118">Embora EF Core ofereça suporte ao uso de propriedades de qualquer tipo primitivo como chave primária, incluindo `string`, `Guid`, `byte[]` e outros, nem todos os bancos de dados oferecem suporte a todos os tipos como chaves.</span><span class="sxs-lookup"><span data-stu-id="d01bd-118">While EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others, not all databases support all types as keys.</span></span> <span data-ttu-id="d01bd-119">Em alguns casos, os valores de chave podem ser convertidos em um tipo com suporte automaticamente; caso contrário, a conversão deve ser [especificada manualmente](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="d01bd-119">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="d01bd-120">As propriedades de chave sempre devem ter um valor não padrão ao adicionar uma nova entidade ao contexto, mas alguns tipos serão [gerados pelo banco de dados](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="d01bd-120">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="d01bd-121">Nesse caso, o EF tentará gerar um valor temporário quando a entidade for adicionada para fins de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="d01bd-121">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="d01bd-122">Depois que [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) for chamado, o valor temporário será substituído pelo valor gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d01bd-122">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="d01bd-123">Se uma propriedade de chave tiver seu valor gerado pelo banco de dados e um valor não padrão for especificado quando uma entidade for adicionada, o EF irá pressupor que a entidade já existe no banco de dados e tentará atualizá-la em vez de inserir uma nova.</span><span class="sxs-lookup"><span data-stu-id="d01bd-123">If a key property has its value generated by the database and a non-default value is specified when an entity is added, then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="d01bd-124">Para evitar isso, desative a geração de valor ou veja [como especificar valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="d01bd-124">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>

## <a name="alternate-keys"></a><span data-ttu-id="d01bd-125">Chaves alternativas</span><span class="sxs-lookup"><span data-stu-id="d01bd-125">Alternate Keys</span></span>

<span data-ttu-id="d01bd-126">Uma chave alternativa serve como um identificador exclusivo alternativo para cada instância de entidade, além da chave primária; Ele pode ser usado como o destino de uma relação.</span><span class="sxs-lookup"><span data-stu-id="d01bd-126">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key; it can be used as the target of a relationship.</span></span> <span data-ttu-id="d01bd-127">Ao usar um banco de dados relacional, ele é mapeado para o conceito de um índice/restrição exclusivo nas colunas de chave alternativas e uma ou mais restrições Foreign Key que fazem referência às colunas.</span><span class="sxs-lookup"><span data-stu-id="d01bd-127">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]
> <span data-ttu-id="d01bd-128">Se você quiser apenas impor a exclusividade em uma coluna, defina um índice exclusivo em vez de uma chave alternativa (consulte [índices](indexes.md)).</span><span class="sxs-lookup"><span data-stu-id="d01bd-128">If you just want to enforce uniqueness on a column, define a unique index rather than an alternate key (see [Indexes](indexes.md)).</span></span> <span data-ttu-id="d01bd-129">No EF, chaves alternativas são somente leitura e fornecem semântica adicional sobre índices exclusivos, pois eles podem ser usados como o destino de uma chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="d01bd-129">In EF, alternate keys are read-only and provide additional semantics over unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="d01bd-130">As chaves alternativas são normalmente introduzidas quando necessário e você não precisa configurá-las manualmente.</span><span class="sxs-lookup"><span data-stu-id="d01bd-130">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="d01bd-131">Por convenção, uma chave alternativa é introduzida quando você identifica uma propriedade que não é a chave primária como o destino de uma relação.</span><span class="sxs-lookup"><span data-stu-id="d01bd-131">By convention, an alternate key is introduced for you when you identify a property which isn't the primary key as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

<span data-ttu-id="d01bd-132">Você também pode configurar uma única propriedade para ser uma chave alternativa:</span><span class="sxs-lookup"><span data-stu-id="d01bd-132">You can also configure a single property to be an alternate key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

<span data-ttu-id="d01bd-133">Você também pode configurar várias propriedades para ser uma chave alternativa (conhecida como uma chave alternativa composta):</span><span class="sxs-lookup"><span data-stu-id="d01bd-133">You can also configure multiple properties to be an alternate key (known as a composite alternate key):</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

<span data-ttu-id="d01bd-134">Por fim, por convenção, o índice e a restrição que são introduzidos para uma chave alternativa serão nomeados `AK_<type name>_<property name>` (para chaves alternativas compostas `<property name>` se tornará uma lista de nomes de propriedades separada por sublinhado).</span><span class="sxs-lookup"><span data-stu-id="d01bd-134">Finally, by convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>` (for composite alternate keys `<property name>` becomes an underscore separated list of property names).</span></span> <span data-ttu-id="d01bd-135">Você pode configurar o nome do índice da chave alternativa e a restrição UNIQUE:</span><span class="sxs-lookup"><span data-stu-id="d01bd-135">You can configure the name of the alternate key's index and unique constraint:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
