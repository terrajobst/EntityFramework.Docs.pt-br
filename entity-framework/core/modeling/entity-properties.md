---
title: Propriedades da entidade-EF Core
description: Como configurar e mapear Propriedades de entidade usando Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502463"
---
# <a name="entity-properties"></a><span data-ttu-id="e7736-103">Propriedades da Entidade</span><span class="sxs-lookup"><span data-stu-id="e7736-103">Entity Properties</span></span>

<span data-ttu-id="e7736-104">Cada tipo de entidade em seu modelo tem um conjunto de propriedades, que EF Core lerá e gravará no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e7736-104">Each entity type in your model has a set of properties, which EF Core will read and write from the database.</span></span> <span data-ttu-id="e7736-105">Se você estiver usando um banco de dados relacional, as propriedades de entidade serão mapeadas para colunas de tabela.</span><span class="sxs-lookup"><span data-stu-id="e7736-105">If you're using a relational database, entity properties map to table columns.</span></span>

## <a name="included-and-excluded-properties"></a><span data-ttu-id="e7736-106">Propriedades incluídas e excluídas</span><span class="sxs-lookup"><span data-stu-id="e7736-106">Included and excluded properties</span></span>

<span data-ttu-id="e7736-107">Por convenção, todas as propriedades públicas com um getter e um setter serão incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="e7736-107">By convention, all public properties with a getter and a setter will be included in the model.</span></span>

<span data-ttu-id="e7736-108">Propriedades específicas podem ser excluídas da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e7736-108">Specific properties can be excluded as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="e7736-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e7736-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="e7736-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="e7736-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a><span data-ttu-id="e7736-111">Nomes de coluna</span><span class="sxs-lookup"><span data-stu-id="e7736-111">Column names</span></span>

<span data-ttu-id="e7736-112">Por convenção, ao usar um banco de dados relacional, as propriedades de entidade são mapeadas para colunas de tabela com o mesmo nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7736-112">By convention, when using a relational database, entity properties are mapped to table columns having the same name as the property.</span></span>

<span data-ttu-id="e7736-113">Se preferir configurar suas colunas com nomes diferentes, você poderá fazer isso da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e7736-113">If you prefer to configure your columns with different names, you can do so as following:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="e7736-114">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e7736-114">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="e7736-115">API fluente</span><span class="sxs-lookup"><span data-stu-id="e7736-115">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a><span data-ttu-id="e7736-116">Tipos de dados de coluna</span><span class="sxs-lookup"><span data-stu-id="e7736-116">Column data types</span></span>

<span data-ttu-id="e7736-117">Ao usar um banco de dados relacional, o provedor de banco de dados seleciona um tipo de dado com base no tipo .NET da propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7736-117">When using a relational database, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="e7736-118">Ele também leva em conta outros metadados, como o [comprimento máximo](#maximum-length)configurado, se a propriedade faz parte de uma chave primária, etc.</span><span class="sxs-lookup"><span data-stu-id="e7736-118">It also takes into account other metadata, such as the configured [maximum length](#maximum-length), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="e7736-119">Por exemplo, SQL Server mapeia `DateTime` Propriedades para `datetime2(7)` colunas e `string` Propriedades para `nvarchar(max)` colunas (ou `nvarchar(450)` para propriedades que são usadas como uma chave).</span><span class="sxs-lookup"><span data-stu-id="e7736-119">For example, SQL Server maps `DateTime` properties to `datetime2(7)` columns, and `string` properties to `nvarchar(max)` columns (or to `nvarchar(450)` for properties that are used as a key).</span></span>

<span data-ttu-id="e7736-120">Você também pode configurar suas colunas para especificar um tipo de dados exato para uma coluna.</span><span class="sxs-lookup"><span data-stu-id="e7736-120">You can also configure your columns to specify an exact data type for a column.</span></span> <span data-ttu-id="e7736-121">Por exemplo, o código a seguir configura `Url` como uma cadeia de caracteres não-Unicode com comprimento máximo de `200` e `Rating` como decimal com precisão de `5` e escala de `2`:</span><span class="sxs-lookup"><span data-stu-id="e7736-121">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="e7736-122">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e7736-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="e7736-123">API fluente</span><span class="sxs-lookup"><span data-stu-id="e7736-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a><span data-ttu-id="e7736-124">Comprimento máximo</span><span class="sxs-lookup"><span data-stu-id="e7736-124">Maximum length</span></span>

<span data-ttu-id="e7736-125">A configuração de um comprimento máximo fornece uma dica ao provedor de banco de dados sobre o tipo de dado de coluna apropriado a ser escolhido para uma determinada propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7736-125">Configuring a maximum length provides a hint to the database provider about the appropriate column data type to choose for a given property.</span></span> <span data-ttu-id="e7736-126">O comprimento máximo só se aplica a tipos de dados de matriz, como `string` e `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="e7736-126">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]
> <span data-ttu-id="e7736-127">Entity Framework não realiza nenhuma validação de comprimento máximo antes de passar os dados para o provedor.</span><span class="sxs-lookup"><span data-stu-id="e7736-127">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="e7736-128">Cabe ao provedor ou repositório de dados validar se apropriado.</span><span class="sxs-lookup"><span data-stu-id="e7736-128">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="e7736-129">Por exemplo, ao direcionar SQL Server, exceder o comprimento máximo resultará em uma exceção, pois o tipo de dados da coluna subjacente não permitirá que os dados em excesso sejam armazenados.</span><span class="sxs-lookup"><span data-stu-id="e7736-129">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

<span data-ttu-id="e7736-130">No exemplo a seguir, a configuração de um comprimento máximo de 500 fará com que uma coluna do tipo `nvarchar(500)` seja criada em SQL Server:</span><span class="sxs-lookup"><span data-stu-id="e7736-130">In the following example, configuring a maximum length of 500 will cause a column of type `nvarchar(500)` to be created on SQL Server:</span></span>

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="e7736-131">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e7736-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="e7736-132">API fluente</span><span class="sxs-lookup"><span data-stu-id="e7736-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a><span data-ttu-id="e7736-133">Propriedades obrigatórias e opcionais</span><span class="sxs-lookup"><span data-stu-id="e7736-133">Required and optional properties</span></span>

<span data-ttu-id="e7736-134">Uma propriedade será considerada opcional se for válida para que ela contenha `null`.</span><span class="sxs-lookup"><span data-stu-id="e7736-134">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="e7736-135">Se `null` não for um valor válido a ser atribuído a uma propriedade, então será considerada uma propriedade necessária.</span><span class="sxs-lookup"><span data-stu-id="e7736-135">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span> <span data-ttu-id="e7736-136">Ao mapear para um esquema de banco de dados relacional, as propriedades obrigatórias são criadas como colunas não anuláveis e as propriedades opcionais são criadas como colunas anuláveis.</span><span class="sxs-lookup"><span data-stu-id="e7736-136">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

### <a name="conventions"></a><span data-ttu-id="e7736-137">Convenções</span><span class="sxs-lookup"><span data-stu-id="e7736-137">Conventions</span></span>

<span data-ttu-id="e7736-138">Por convenção, uma propriedade cujo tipo .NET pode conter NULL será configurada como opcional, enquanto as propriedades cujo tipo .NET não pode conter NULL serão configuradas conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="e7736-138">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="e7736-139">Por exemplo, todas as propriedades com tipos de valor do .NET (`int`, `decimal`, `bool`, etc.) são configuradas conforme necessário, e todas as propriedades com tipos de valor do .NET anuláveis (`int?`, `decimal?`, `bool?`, etc.) são configuradas como opcionais.</span><span class="sxs-lookup"><span data-stu-id="e7736-139">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="e7736-140">C#8 introduziu um novo recurso chamado [tipos de referência anulável](/dotnet/csharp/tutorials/nullable-reference-types), que permite que os tipos de referência sejam anotados, indicando se é válido para que eles contenham NULL ou não.</span><span class="sxs-lookup"><span data-stu-id="e7736-140">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="e7736-141">Esse recurso está desabilitado por padrão e, se habilitado, ele modifica o comportamento do EF Core da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e7736-141">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="e7736-142">Se tipos de referência anuláveis estiverem desabilitados (o padrão), todas as propriedades com tipos de referência do .NET serão configuradas como opcionais por convenção (por exemplo, `string`).</span><span class="sxs-lookup"><span data-stu-id="e7736-142">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="e7736-143">Se os tipos de referência anuláveis estiverem habilitados, as propriedades serão C# configuradas com base na nulidade de seu tipo .net: `string?` será configurado como opcional, enquanto `string` será configurada conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="e7736-143">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="e7736-144">O exemplo a seguir mostra um tipo de entidade com propriedades obrigatórias e opcionais, com o recurso de referência anulável desabilitado (o padrão) e habilitado:</span><span class="sxs-lookup"><span data-stu-id="e7736-144">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

#### <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="e7736-145">Sem tipos de referência anuláveis (padrão)</span><span class="sxs-lookup"><span data-stu-id="e7736-145">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="e7736-146">Com tipos de referência anuláveis</span><span class="sxs-lookup"><span data-stu-id="e7736-146">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="e7736-147">O uso de tipos de referência anuláveis é recomendado, pois ele flui C# a nulidade expressa em código para o modelo de EF Core e para o banco de dados e dispensa o uso da API fluente ou das anotações de data para expressar o mesmo conceito duas vezes.</span><span class="sxs-lookup"><span data-stu-id="e7736-147">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="e7736-148">Tenha cuidado ao habilitar tipos de referência anuláveis em um projeto existente: as propriedades do tipo de referência que foram previamente configuradas como opcionais agora serão configuradas conforme necessário, a menos que sejam anotadas explicitamente para permitir valor nulo.</span><span class="sxs-lookup"><span data-stu-id="e7736-148">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="e7736-149">Ao gerenciar um esquema de banco de dados relacional, isso pode fazer com que as migrações sejam geradas, alterando a nulidade da coluna do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e7736-149">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="e7736-150">Para obter mais informações sobre tipos de referência anuláveis e como usá-los com EF Core, [consulte a página de documentação dedicada para esse recurso](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="e7736-150">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

### <a name="explicit-configuration"></a><span data-ttu-id="e7736-151">Configuração explícita</span><span class="sxs-lookup"><span data-stu-id="e7736-151">Explicit configuration</span></span>

<span data-ttu-id="e7736-152">Uma propriedade que seria opcional por convenção pode ser configurada para ser necessária da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e7736-152">A property that would be optional by convention can be configured to be required as follows:</span></span>

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="e7736-153">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e7736-153">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="e7736-154">API fluente</span><span class="sxs-lookup"><span data-stu-id="e7736-154">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
