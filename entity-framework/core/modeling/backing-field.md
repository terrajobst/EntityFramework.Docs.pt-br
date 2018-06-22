---
title: Fazendo campos - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053456"
---
# <a name="backing-fields"></a><span data-ttu-id="35a87-102">Campos de backup</span><span class="sxs-lookup"><span data-stu-id="35a87-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="35a87-103">Esse recurso é novo no EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="35a87-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="35a87-104">Os campos de backup permitem EF ler e/ou gravar em um campo em vez de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="35a87-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="35a87-105">Isso pode ser útil quando o encapsulamento na classe está sendo usado para restringir o uso de e/ou melhorar a semântica de acesso aos dados pelo código do aplicativo, mas o valor deve ser de leitura de e/ou gravado no banco de dados sem usar essas restrições / aprimoramentos.</span><span class="sxs-lookup"><span data-stu-id="35a87-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="35a87-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="35a87-106">Conventions</span></span>

<span data-ttu-id="35a87-107">Por convenção, os campos a seguir serão descobertos como fazendo campos para uma determinada propriedade (listados em ordem de precedência).</span><span class="sxs-lookup"><span data-stu-id="35a87-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="35a87-108">Campos só são descobertos para propriedades que estão incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="35a87-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="35a87-109">Para obter mais informações no qual as propriedades são incluídas no modelo, consulte [incluindo e excluindo propriedades](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="35a87-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="35a87-110">Quando um campo de backup é configurado, EF gravará diretamente a esse campo ao materializar instâncias da entidade do banco de dados (em vez de usar o setter da propriedade).</span><span class="sxs-lookup"><span data-stu-id="35a87-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="35a87-111">Se precisar de EF ler ou gravar o valor em outras vezes, ele usará a propriedade se possível.</span><span class="sxs-lookup"><span data-stu-id="35a87-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="35a87-112">Por exemplo, se o EF precisa atualizar o valor de uma propriedade, ele usará o setter da propriedade se um for definido.</span><span class="sxs-lookup"><span data-stu-id="35a87-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="35a87-113">Se a propriedade é somente leitura, em seguida, ele gravará o campo.</span><span class="sxs-lookup"><span data-stu-id="35a87-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="35a87-114">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="35a87-114">Data Annotations</span></span>

<span data-ttu-id="35a87-115">Fazer campos não pode ser configurado com as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="35a87-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="35a87-116">API fluente</span><span class="sxs-lookup"><span data-stu-id="35a87-116">Fluent API</span></span>

<span data-ttu-id="35a87-117">Você pode usar a API fluente para configurar um campo existente para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="35a87-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="35a87-118">Controlar quando o campo é usado</span><span class="sxs-lookup"><span data-stu-id="35a87-118">Controlling when the field is used</span></span>

<span data-ttu-id="35a87-119">Você pode configurar quando o EF usa o campo ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="35a87-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="35a87-120">Consulte o [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para as opções com suporte.</span><span class="sxs-lookup"><span data-stu-id="35a87-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="35a87-121">Campos sem uma propriedade</span><span class="sxs-lookup"><span data-stu-id="35a87-121">Fields without a property</span></span>

<span data-ttu-id="35a87-122">Você também pode criar uma propriedade conceitual em seu modelo que não tem uma propriedade CLR correspondente na classe de entidade, mas em vez disso, usa um campo para armazenar os dados da entidade.</span><span class="sxs-lookup"><span data-stu-id="35a87-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="35a87-123">Isso é diferente de [propriedades de sombra](shadow-properties.md), onde os dados são armazenados no controlador de alterações.</span><span class="sxs-lookup"><span data-stu-id="35a87-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="35a87-124">Isso normalmente seria usado se a classe da entidade usa métodos para valores de get/set.</span><span class="sxs-lookup"><span data-stu-id="35a87-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="35a87-125">Você pode dar o nome do campo no EF o `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="35a87-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="35a87-126">Se não houver nenhuma propriedade com o nome fornecido, EF procurará um campo.</span><span class="sxs-lookup"><span data-stu-id="35a87-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="35a87-127">Você também pode optar por dar um nome diferente do nome do campo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="35a87-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="35a87-128">Esse nome é usado ao criar o modelo, mais notoriamente, ela será usada para o nome da coluna que é mapeado para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="35a87-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="35a87-129">Quando não há nenhuma propriedade na classe de entidade, você pode usar o `EF.Property(...)` método em uma consulta LINQ para referir-se a propriedade que conceitualmente faz parte do modelo.</span><span class="sxs-lookup"><span data-stu-id="35a87-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
