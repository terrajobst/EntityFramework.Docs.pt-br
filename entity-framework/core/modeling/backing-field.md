---
title: Fazendo backup de campos-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124373"
---
# <a name="backing-fields"></a><span data-ttu-id="36d56-102">Campos de backup</span><span class="sxs-lookup"><span data-stu-id="36d56-102">Backing Fields</span></span>

<span data-ttu-id="36d56-103">Os campos de backup permitem que o EF Leia e/ou grave em um campo em vez de em uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="36d56-103">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="36d56-104">Isso pode ser útil quando o encapsulamento na classe está sendo usado para restringir o uso de e/ou aprimorar a semântica em relação ao acesso aos dados por código do aplicativo, mas o valor deve ser lido e/ou gravado no Database sem usar essas restrições/aprimoramentos.</span><span class="sxs-lookup"><span data-stu-id="36d56-104">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="basic-configuration"></a><span data-ttu-id="36d56-105">Configuração básica</span><span class="sxs-lookup"><span data-stu-id="36d56-105">Basic configuration</span></span>

<span data-ttu-id="36d56-106">Por convenção, os campos a seguir serão descobertos como campos de apoio para uma determinada propriedade (listada em ordem de precedência).</span><span class="sxs-lookup"><span data-stu-id="36d56-106">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

<span data-ttu-id="36d56-107">No exemplo a seguir, a propriedade `Url` está configurada para ter `_url` como seu campo de apoio:</span><span class="sxs-lookup"><span data-stu-id="36d56-107">In the following sample, the `Url` property is configured to have `_url` as its backing field:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="36d56-108">Observe que os campos de apoio são descobertos apenas para propriedades incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="36d56-108">Note that backing fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="36d56-109">Para obter mais informações sobre quais propriedades são incluídas no modelo, consulte [incluindo & excluindo Propriedades](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="36d56-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

<span data-ttu-id="36d56-110">Você também pode configurar os campos de backup explicitamente, por exemplo, se o nome do campo não corresponder às convenções acima:</span><span class="sxs-lookup"><span data-stu-id="36d56-110">You can also configure backing fields explicitly, e.g. if the field name doesn't correspond to the above conventions:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a><span data-ttu-id="36d56-111">Acesso de campo e Propriedade</span><span class="sxs-lookup"><span data-stu-id="36d56-111">Field and property access</span></span>

<span data-ttu-id="36d56-112">Por padrão, o EF sempre lerá e gravará no campo de backup-supondo que um tenha sido configurado corretamente-e nunca usará a propriedade.</span><span class="sxs-lookup"><span data-stu-id="36d56-112">By default, EF will always read and write to the backing field - assuming one has been properly configured - and will never use the property.</span></span> <span data-ttu-id="36d56-113">No entanto, o EF também oferece suporte a outros padrões de acesso.</span><span class="sxs-lookup"><span data-stu-id="36d56-113">However, EF also supports other access patterns.</span></span> <span data-ttu-id="36d56-114">Por exemplo, o exemplo a seguir instrui o EF a gravar no campo de backup somente durante a materialização e a usar a propriedade em todos os outros casos:</span><span class="sxs-lookup"><span data-stu-id="36d56-114">For example, the following sample instructs EF to write to the backing field only while materializing, and to use the property in all other cases:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

<span data-ttu-id="36d56-115">Consulte a [Enumeração PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para obter o conjunto completo de opções com suporte.</span><span class="sxs-lookup"><span data-stu-id="36d56-115">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the complete set of supported options.</span></span>

> [!NOTE]
> <span data-ttu-id="36d56-116">Com o EF Core 3,0, o modo de acesso de propriedade padrão mudou de `PreferFieldDuringConstruction` para `PreferField`.</span><span class="sxs-lookup"><span data-stu-id="36d56-116">With EF Core 3.0, the default property access mode changed from `PreferFieldDuringConstruction` to `PreferField`.</span></span>

## <a name="field-only-properties"></a><span data-ttu-id="36d56-117">Propriedades somente de campo</span><span class="sxs-lookup"><span data-stu-id="36d56-117">Field-only properties</span></span>

<span data-ttu-id="36d56-118">Você também pode criar uma propriedade conceitual em seu modelo que não tem uma propriedade CLR correspondente na classe de entidade, mas, em vez disso, usa um campo para armazenar os dados na entidade.</span><span class="sxs-lookup"><span data-stu-id="36d56-118">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="36d56-119">Isso é diferente das [Propriedades de sombra](shadow-properties.md), em que os dados são armazenados no controlador de alterações, em vez de no tipo CLR da entidade.</span><span class="sxs-lookup"><span data-stu-id="36d56-119">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker, rather than in the entity's CLR type.</span></span> <span data-ttu-id="36d56-120">As propriedades somente de campo são normalmente usadas quando a classe de entidade usa métodos em vez de propriedades para obter/definir valores, ou em casos em que os campos não devem ser expostos no modelo de domínio (por exemplo, chaves primárias).</span><span class="sxs-lookup"><span data-stu-id="36d56-120">Field-only properties are commonly used when the entity class uses methods instead of properties to get/set values, or in cases where fields shouldn't be exposed at all in the domain model (e.g. primary keys).</span></span>

<span data-ttu-id="36d56-121">Você pode configurar uma propriedade somente de campo fornecendo um nome na API de `Property(...)`:</span><span class="sxs-lookup"><span data-stu-id="36d56-121">You can configure a field-only property by providing a name in the `Property(...)` API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="36d56-122">O EF tentará localizar uma propriedade CLR com o nome fornecido ou um campo se uma propriedade não for encontrada.</span><span class="sxs-lookup"><span data-stu-id="36d56-122">EF will attempt to find a CLR property with the given name, or a field if a property isn't found.</span></span> <span data-ttu-id="36d56-123">Se nem uma propriedade nem um campo forem encontrados, uma propriedade Shadow será configurada em vez disso.</span><span class="sxs-lookup"><span data-stu-id="36d56-123">If neither a property nor a field are found, a shadow property will be set up instead.</span></span>

<span data-ttu-id="36d56-124">Talvez seja necessário referir-se a uma propriedade somente de campo de consultas LINQ, mas esses campos normalmente são privados.</span><span class="sxs-lookup"><span data-stu-id="36d56-124">You may need to refer to a field-only property from LINQ queries, but such fields are typically private.</span></span> <span data-ttu-id="36d56-125">Você pode usar o método `EF.Property(...)` em uma consulta LINQ para se referir ao campo:</span><span class="sxs-lookup"><span data-stu-id="36d56-125">You can use the `EF.Property(...)` method in a LINQ query to refer to the field:</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
