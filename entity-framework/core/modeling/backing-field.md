---
title: Fazendo campos – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994087"
---
# <a name="backing-fields"></a><span data-ttu-id="a818a-102">Campos de suporte</span><span class="sxs-lookup"><span data-stu-id="a818a-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="a818a-103">Esse recurso é novo no EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="a818a-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="a818a-104">Campos de suporte permitem que o EF para leitura e/ou gravar em um campo em vez de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="a818a-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="a818a-105">Isso pode ser útil ao encapsulamento na classe está sendo usado para restringir o uso de e/ou aprimorar a semântica de acesso aos dados pelo código do aplicativo, mas o valor deve ser de leitura de e/ou gravado no banco de dados sem usar essas restrições / aprimoramentos.</span><span class="sxs-lookup"><span data-stu-id="a818a-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="a818a-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="a818a-106">Conventions</span></span>

<span data-ttu-id="a818a-107">Por convenção, os campos a seguir serão descobertos como campos de suporte para uma determinada propriedade (listados em ordem de precedência).</span><span class="sxs-lookup"><span data-stu-id="a818a-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="a818a-108">Campos só são descobertos para propriedades que são incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="a818a-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="a818a-109">Para obter mais informações no qual as propriedades são incluídas no modelo, consulte [incluir e excluir propriedades](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="a818a-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="a818a-110">Quando um campo de backup é configurado, o EF gravará diretamente a esse campo quando materializar as instâncias de entidade do banco de dados (em vez de usar o setter da propriedade).</span><span class="sxs-lookup"><span data-stu-id="a818a-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="a818a-111">Se o EF precisa ler ou gravar o valor em outros momentos, ele usará a propriedade se possível.</span><span class="sxs-lookup"><span data-stu-id="a818a-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="a818a-112">Por exemplo, se o EF precisa para atualizar o valor para uma propriedade, ele usará o setter da propriedade se algum tiver sido definido.</span><span class="sxs-lookup"><span data-stu-id="a818a-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="a818a-113">Se a propriedade é somente leitura, ele gravará para o campo.</span><span class="sxs-lookup"><span data-stu-id="a818a-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a818a-114">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="a818a-114">Data Annotations</span></span>

<span data-ttu-id="a818a-115">Campos de backup não podem ser configurado com anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="a818a-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a818a-116">API fluente</span><span class="sxs-lookup"><span data-stu-id="a818a-116">Fluent API</span></span>

<span data-ttu-id="a818a-117">Você pode usar a API Fluent para configurar um campo de suporte para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="a818a-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="a818a-118">Controlar quando o campo é usado</span><span class="sxs-lookup"><span data-stu-id="a818a-118">Controlling when the field is used</span></span>

<span data-ttu-id="a818a-119">Você pode configurar quando o EF usa o campo ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="a818a-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="a818a-120">Consulte a [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para as opções com suporte.</span><span class="sxs-lookup"><span data-stu-id="a818a-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="a818a-121">Campos sem uma propriedade</span><span class="sxs-lookup"><span data-stu-id="a818a-121">Fields without a property</span></span>

<span data-ttu-id="a818a-122">Você também pode criar uma propriedade conceitual em seu modelo que não tem uma propriedade CLR correspondente na classe de entidade, mas em vez disso, usa um campo para armazenar os dados na entidade.</span><span class="sxs-lookup"><span data-stu-id="a818a-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="a818a-123">Isso é diferente de [propriedades de sombra](shadow-properties.md), onde os dados são armazenados no Rastreador de alteração.</span><span class="sxs-lookup"><span data-stu-id="a818a-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="a818a-124">Isso normalmente seria usado se a classe de entidade usa métodos para obter/definir valores.</span><span class="sxs-lookup"><span data-stu-id="a818a-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="a818a-125">Você pode dar o nome do campo no EF o `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="a818a-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="a818a-126">Se não houver nenhuma propriedade com o nome fornecido, o EF procurará um campo.</span><span class="sxs-lookup"><span data-stu-id="a818a-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="a818a-127">Você também pode optar por dar um nome diferente do nome do campo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="a818a-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="a818a-128">Esse nome é usado ao criar o modelo, mais notavelmente, ele será usado para o nome da coluna que é mapeado para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a818a-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="a818a-129">Quando não há nenhuma propriedade na classe de entidade, você pode usar o `EF.Property(...)` método em uma consulta LINQ para fazer referência à propriedade que é conceitualmente parte do modelo.</span><span class="sxs-lookup"><span data-stu-id="a818a-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
