---
title: Propriedades da sombra – EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417408"
---
# <a name="shadow-properties"></a><span data-ttu-id="c88e5-102">Propriedades de sombra</span><span class="sxs-lookup"><span data-stu-id="c88e5-102">Shadow Properties</span></span>

<span data-ttu-id="c88e5-103">Propriedades de sombra são propriedades que não são definidas em sua classe de entidade do .NET, mas são definidas para esse tipo de entidade no modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c88e5-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="c88e5-104">O valor e o estado dessas propriedades são mantidos puramente no controlador de alterações.</span><span class="sxs-lookup"><span data-stu-id="c88e5-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="c88e5-105">As propriedades de sombra são úteis quando há dados no banco que não devem ser expostos nos tipos de entidade mapeada.</span><span class="sxs-lookup"><span data-stu-id="c88e5-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="c88e5-106">Propriedades da sombra da chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="c88e5-106">Foreign key shadow properties</span></span>

<span data-ttu-id="c88e5-107">As propriedades de sombra são usadas com mais frequência para propriedades de chave estrangeira, em que a relação entre duas entidades é representada por um valor de chave estrangeira no banco de dados, mas a relação é gerenciada nos tipos de entidade usando propriedades de navegação entre a entidade digita.</span><span class="sxs-lookup"><span data-stu-id="c88e5-107">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="c88e5-108">Por convenção, o EF introduzirá uma propriedade de sombra quando uma relação for descoberta, mas nenhuma propriedade de chave estrangeira for encontrada na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="c88e5-108">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="c88e5-109">A propriedade será nomeada `<navigation property name><principal key property name>` (a navegação na entidade dependente, que aponta para a entidade principal, é usada para a nomenclatura).</span><span class="sxs-lookup"><span data-stu-id="c88e5-109">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="c88e5-110">Se o nome da propriedade da chave principal incluir o nome da propriedade de navegação, o nome será apenas `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c88e5-110">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="c88e5-111">Se não houver nenhuma propriedade de navegação na entidade dependente, o nome do tipo de entidade de segurança será usado em seu lugar.</span><span class="sxs-lookup"><span data-stu-id="c88e5-111">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="c88e5-112">Por exemplo, a listagem de código a seguir resultará em uma propriedade de sombra `BlogId` introduzida na entidade `Post`:</span><span class="sxs-lookup"><span data-stu-id="c88e5-112">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="c88e5-113">Configurando propriedades de sombra</span><span class="sxs-lookup"><span data-stu-id="c88e5-113">Configuring shadow properties</span></span>

<span data-ttu-id="c88e5-114">Você pode usar a API Fluent para configurar propriedades de sombra.</span><span class="sxs-lookup"><span data-stu-id="c88e5-114">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="c88e5-115">Depois de ter chamado a sobrecarga de cadeia de caracteres de `Property`, você pode encadear qualquer uma das chamadas de configuração para outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="c88e5-115">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="c88e5-116">No exemplo a seguir, como `Blog` não tem nenhuma propriedade CLR chamada `LastUpdated`, uma propriedade Shadow é criada:</span><span class="sxs-lookup"><span data-stu-id="c88e5-116">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="c88e5-117">Se o nome fornecido ao método de `Property` corresponder ao nome de uma propriedade existente (uma propriedade de sombra ou uma definida na classe de entidade), o código configurará essa propriedade existente em vez de introduzir uma nova propriedade de sombra.</span><span class="sxs-lookup"><span data-stu-id="c88e5-117">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="c88e5-118">Acessando Propriedades de sombra</span><span class="sxs-lookup"><span data-stu-id="c88e5-118">Accessing shadow properties</span></span>

<span data-ttu-id="c88e5-119">Os valores de propriedade de sombra podem ser obtidos e alterados por meio da API `ChangeTracker`:</span><span class="sxs-lookup"><span data-stu-id="c88e5-119">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="c88e5-120">As propriedades de sombra podem ser referenciadas em consultas LINQ por meio do método estático `EF.Property`:</span><span class="sxs-lookup"><span data-stu-id="c88e5-120">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
