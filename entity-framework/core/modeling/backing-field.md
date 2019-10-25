---
title: Fazendo backup de campos-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811884"
---
# <a name="backing-fields"></a><span data-ttu-id="b6b05-102">Campos de backup</span><span class="sxs-lookup"><span data-stu-id="b6b05-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="b6b05-103">Esse recurso é novo no EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="b6b05-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="b6b05-104">Os campos de backup permitem que o EF Leia e/ou grave em um campo em vez de em uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="b6b05-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="b6b05-105">Isso pode ser útil quando o encapsulamento na classe está sendo usado para restringir o uso de e/ou aprimorar a semântica em relação ao acesso aos dados por código do aplicativo, mas o valor deve ser lido e/ou gravado no Database sem usar essas restrições/ aprimoramentos.</span><span class="sxs-lookup"><span data-stu-id="b6b05-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="b6b05-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="b6b05-106">Conventions</span></span>

<span data-ttu-id="b6b05-107">Por convenção, os campos a seguir serão descobertos como campos de apoio para uma determinada propriedade (listada em ordem de precedência).</span><span class="sxs-lookup"><span data-stu-id="b6b05-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="b6b05-108">Os campos são descobertos apenas para propriedades incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="b6b05-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="b6b05-109">Para obter mais informações sobre quais propriedades são incluídas no modelo, consulte [incluindo & excluindo Propriedades](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="b6b05-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="b6b05-110">Quando um campo de backup for configurado, o EF gravará diretamente nesse campo ao materializar instâncias de entidade do banco de dados (em vez de usar o setter de propriedade).</span><span class="sxs-lookup"><span data-stu-id="b6b05-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="b6b05-111">Se o EF precisar ler ou gravar o valor em outros momentos, ele usará a propriedade, se possível.</span><span class="sxs-lookup"><span data-stu-id="b6b05-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="b6b05-112">Por exemplo, se o EF precisar atualizar o valor de uma propriedade, ele usará o setter de propriedade se um for definido.</span><span class="sxs-lookup"><span data-stu-id="b6b05-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="b6b05-113">Se a propriedade for somente leitura, ela será gravada no campo.</span><span class="sxs-lookup"><span data-stu-id="b6b05-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b6b05-114">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="b6b05-114">Data Annotations</span></span>

<span data-ttu-id="b6b05-115">Os campos de backup não podem ser configurados com anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="b6b05-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b6b05-116">API fluente</span><span class="sxs-lookup"><span data-stu-id="b6b05-116">Fluent API</span></span>

<span data-ttu-id="b6b05-117">Você pode usar a API Fluent para configurar um campo de apoio para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="b6b05-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="b6b05-118">Controlando quando o campo é usado</span><span class="sxs-lookup"><span data-stu-id="b6b05-118">Controlling when the field is used</span></span>

<span data-ttu-id="b6b05-119">Você pode configurar quando o EF usa o campo ou a propriedade.</span><span class="sxs-lookup"><span data-stu-id="b6b05-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="b6b05-120">Consulte a [Enumeração PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para obter as opções com suporte.</span><span class="sxs-lookup"><span data-stu-id="b6b05-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="b6b05-121">Campos sem uma propriedade</span><span class="sxs-lookup"><span data-stu-id="b6b05-121">Fields without a property</span></span>

<span data-ttu-id="b6b05-122">Você também pode criar uma propriedade conceitual em seu modelo que não tem uma propriedade CLR correspondente na classe de entidade, mas, em vez disso, usa um campo para armazenar os dados na entidade.</span><span class="sxs-lookup"><span data-stu-id="b6b05-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="b6b05-123">Isso é diferente das [Propriedades de sombra](shadow-properties.md), onde os dados são armazenados no controlador de alterações.</span><span class="sxs-lookup"><span data-stu-id="b6b05-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="b6b05-124">Isso normalmente seria usado se a classe de entidade usa métodos para obter/definir valores.</span><span class="sxs-lookup"><span data-stu-id="b6b05-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="b6b05-125">Você pode dar ao EF o nome do campo na API `Property(...)`.</span><span class="sxs-lookup"><span data-stu-id="b6b05-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="b6b05-126">Se não houver nenhuma propriedade com o nome fornecido, o EF procurará um campo.</span><span class="sxs-lookup"><span data-stu-id="b6b05-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="b6b05-127">Quando não há nenhuma propriedade na classe de entidade, você pode usar o método `EF.Property(...)` em uma consulta LINQ para referir-se à propriedade que é conceitualmente parte do modelo.</span><span class="sxs-lookup"><span data-stu-id="b6b05-127">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
