---
title: Propriedade tipos de entidade – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: fe7e07b8bd483fb3f9b672ee78ef7541f06a21a4
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058767"
---
# <a name="owned-entity-types"></a><span data-ttu-id="996a1-102">Tipos de entidade própria</span><span class="sxs-lookup"><span data-stu-id="996a1-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="996a1-103">Esse recurso é novo no EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="996a1-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="996a1-104">O EF Core permite que você a tipos de entidade de modelo que só podem aparecer nas propriedades de navegação de outros tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="996a1-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="996a1-105">Eles são chamados _tipos de entidade de propriedade_.</span><span class="sxs-lookup"><span data-stu-id="996a1-105">These are called _owned entity types_.</span></span> <span data-ttu-id="996a1-106">É a entidade que contém um tipo de entidade própria seus _proprietário_.</span><span class="sxs-lookup"><span data-stu-id="996a1-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="996a1-107">Configuração explícita</span><span class="sxs-lookup"><span data-stu-id="996a1-107">Explicit configuration</span></span>

<span data-ttu-id="996a1-108">Propriedade da entidade tipos nunca são incluídos pelo EF Core em modelo por convenção.</span><span class="sxs-lookup"><span data-stu-id="996a1-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="996a1-109">Você pode usar o `OwnsOne` método no `OnModelCreating` ou anotar o tipo com `OwnedAttribute` (novo no EF Core 2.1) para configurar o tipo como um tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="996a1-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="996a1-110">Neste exemplo, `StreetAddress` é um tipo com nenhuma propriedade de identidade.</span><span class="sxs-lookup"><span data-stu-id="996a1-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="996a1-111">Ele é usado como uma propriedade do tipo Ordem para especificar o endereço para entrega para uma ordem específica.</span><span class="sxs-lookup"><span data-stu-id="996a1-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="996a1-112">Podemos usar o `OwnedAttribute` tratá-la como uma entidade de propriedade quando referenciados de outro tipo de entidade:</span><span class="sxs-lookup"><span data-stu-id="996a1-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="996a1-113">Também é possível usar o `OwnsOne` método no `OnModelCreating` para especificar que o `ShippingAddress` propriedade é uma entidade de propriedade do `Order` tipo de entidade e configurar aspectos adicionais, se necessário.</span><span class="sxs-lookup"><span data-stu-id="996a1-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="996a1-114">Se o `ShippingAddress` propriedade é particular na `Order` tipo, você pode usar a versão de cadeia de caracteres do `OwnsOne` método:</span><span class="sxs-lookup"><span data-stu-id="996a1-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="996a1-115">Consulte a [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) para obter mais contexto.</span><span class="sxs-lookup"><span data-stu-id="996a1-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="996a1-116">Chaves implícitas</span><span class="sxs-lookup"><span data-stu-id="996a1-116">Implicit keys</span></span>

<span data-ttu-id="996a1-117">Configurado com tipos de propriedade `OwnsOne` ou descobertos por meio de uma navegação de referência sempre têm uma relação um para um com o proprietário, portanto eles não precisam seus próprios valores de chave que os valores de chave estrangeiros são exclusivos.</span><span class="sxs-lookup"><span data-stu-id="996a1-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="996a1-118">No exemplo anterior, o `StreetAddress` tipo não precisa definir uma propriedade de chave.</span><span class="sxs-lookup"><span data-stu-id="996a1-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="996a1-119">Para entender como o EF Core controla esses objetos, é útil pensar que uma chave primária é criada como um [propriedade de sombra](xref:core/modeling/shadow-properties) para o tipo próprio.</span><span class="sxs-lookup"><span data-stu-id="996a1-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="996a1-120">O valor da chave de uma instância do tipo próprio será igual ao valor da chave da instância do proprietário.</span><span class="sxs-lookup"><span data-stu-id="996a1-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="996a1-121">Coleções de tipos próprios</span><span class="sxs-lookup"><span data-stu-id="996a1-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="996a1-122">Este recurso é novo no EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="996a1-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="996a1-123">Para configurar uma coleção de tipos próprios `OwnsMany` deve ser usado em `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="996a1-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="996a1-124">No entanto a chave primária não será configurada por convenção, portanto, ela precisa ser especificado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="996a1-124">However the primary key will not be configured by convention, so it need to be specified explicitly.</span></span> <span data-ttu-id="996a1-125">É comum usar uma chave complexa para esses tipos de entidades, incorporando a chave estrangeira para o proprietário e uma propriedade adicional exclusiva que também pode estar no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="996a1-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="996a1-126">Tipos com a divisão de tabela de mapeamento de propriedade</span><span class="sxs-lookup"><span data-stu-id="996a1-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="996a1-127">Ao usar relacional bancos de dados, por referência de convenção de propriedade tipos são mapeados para a mesma tabela como o proprietário.</span><span class="sxs-lookup"><span data-stu-id="996a1-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="996a1-128">Isso exige a dividir a tabela em dois: algumas colunas serão usadas para armazenar os dados do proprietário, e algumas colunas serão usadas para armazenar dados de entidade de propriedade.</span><span class="sxs-lookup"><span data-stu-id="996a1-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="996a1-129">Esse é um recurso comum conhecido como a divisão de tabela.</span><span class="sxs-lookup"><span data-stu-id="996a1-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="996a1-130">Propriedade armazenados com a divisão de tabela de tipos podem ser usada de forma semelhante a como os tipos complexos são usados no EF6.</span><span class="sxs-lookup"><span data-stu-id="996a1-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="996a1-131">Por convenção, o EF Core nomeie as colunas de banco de dados para as propriedades do tipo de entidade própria seguindo o padrão _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="996a1-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="996a1-132">Portanto o `StreetAddress` propriedades serão exibidas na tabela "Pedidos" com os nomes 'ShippingAddress_Street' e 'ShippingAddress_City'.</span><span class="sxs-lookup"><span data-stu-id="996a1-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="996a1-133">Você pode acrescentar o `HasColumnName` método para renomear as colunas:</span><span class="sxs-lookup"><span data-stu-id="996a1-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="996a1-134">Compartilhando o mesmo tipo de .NET entre vários tipos de propriedade</span><span class="sxs-lookup"><span data-stu-id="996a1-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="996a1-135">Um tipo de entidade própria pode ser do mesmo tipo de .NET como outro tipo de entidade de propriedade, portanto, que o tipo do .NET pode não ser suficiente para identificar um tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="996a1-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="996a1-136">Nesses casos, a propriedade apontando do proprietário para a entidade de propriedade se torna a _definir a navegação_ do tipo de entidade de propriedade.</span><span class="sxs-lookup"><span data-stu-id="996a1-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="996a1-137">Da perspectiva do EF Core, a navegação de definição é parte da identidade do tipo juntamente com o tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="996a1-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="996a1-138">Por exemplo, na classe `ShippingAddress` e `BillingAddress` forem do mesmo tipo de .NET, `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="996a1-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="996a1-139">Para compreender como o EF Core distinguirá controladas instâncias desses objetos, pode ser útil pensar que a navegação de definição tornou-se parte da chave da instância junto com o valor da chave do proprietário e o tipo de .NET do tipo próprio.</span><span class="sxs-lookup"><span data-stu-id="996a1-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="996a1-140">Tipos próprios aninhados</span><span class="sxs-lookup"><span data-stu-id="996a1-140">Nested owned types</span></span>

<span data-ttu-id="996a1-141">Neste exemplo `OrderDetails` possui `BillingAddress` e `ShippingAddress`, que são ambos `StreetAddress` tipos.</span><span class="sxs-lookup"><span data-stu-id="996a1-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="996a1-142">Então `OrderDetails` pertence ao tipo `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="996a1-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="996a1-143">Além dos tipos próprios aninhados, um tipo próprio pode fazer referência a uma entidade regular, ele pode ser o proprietário ou uma entidade diferente, desde que a entidade de propriedade é dependente do lado.</span><span class="sxs-lookup"><span data-stu-id="996a1-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="996a1-144">Esse recurso define tipos de entidade própria além dos tipos complexos no EF6.</span><span class="sxs-lookup"><span data-stu-id="996a1-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="996a1-145">É possível encadear o `OwnsOne` método em uma chamada fluente para configurar este modelo:</span><span class="sxs-lookup"><span data-stu-id="996a1-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="996a1-146">Também é possível obter a mesma coisa usando `OwnedAttribute` em ambos `OrderDetails` e `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="996a1-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="996a1-147">Armazenando tipos próprios em tabelas separadas</span><span class="sxs-lookup"><span data-stu-id="996a1-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="996a1-148">Também diferentemente dos tipos complexos do EF6, tipos de propriedade podem ser armazenados em uma tabela separada do proprietário.</span><span class="sxs-lookup"><span data-stu-id="996a1-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="996a1-149">Para substituir a convenção que mapeia um tipo de propriedade para a mesma tabela como o proprietário, você pode simplesmente chamar `ToTable` e forneça um nome de tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="996a1-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="996a1-150">O exemplo a seguir serão mapeados `OrderDetails` e seus dois endereços para uma tabela separada do `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="996a1-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="996a1-151">Consultando tipos próprios</span><span class="sxs-lookup"><span data-stu-id="996a1-151">Querying owned types</span></span>

<span data-ttu-id="996a1-152">Ao consultar o proprietário, os tipos próprios serão incluídos por padrão.</span><span class="sxs-lookup"><span data-stu-id="996a1-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="996a1-153">Não é necessário usar o `Include` método, mesmo se os tipos próprios são armazenados em uma tabela separada.</span><span class="sxs-lookup"><span data-stu-id="996a1-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="996a1-154">Com base no modelo descrito antes, a consulta a seguir obterá `Order`, `OrderDetails` e duas de propriedade `StreetAddresses` do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="996a1-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="996a1-155">Limitações</span><span class="sxs-lookup"><span data-stu-id="996a1-155">Limitations</span></span>

<span data-ttu-id="996a1-156">Algumas dessas limitações são fundamentais para o trabalho de tipos de entidade como propriedade, mas outras são restrições que podemos pode ser capazes de ser removido em futuras versões:</span><span class="sxs-lookup"><span data-stu-id="996a1-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="996a1-157">Restrições de design</span><span class="sxs-lookup"><span data-stu-id="996a1-157">By-design restrictions</span></span>
- <span data-ttu-id="996a1-158">Não é possível criar um `DbSet<T>` para um tipo de propriedade</span><span class="sxs-lookup"><span data-stu-id="996a1-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="996a1-159">Você não pode chamar `Entity<T>()` com um tipo de propriedade em `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="996a1-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="996a1-160">Limitações atuais</span><span class="sxs-lookup"><span data-stu-id="996a1-160">Current shortcomings</span></span>
- <span data-ttu-id="996a1-161">Não há suporte para tipos de entidade de propriedade de hierarquias de herança que incluem</span><span class="sxs-lookup"><span data-stu-id="996a1-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="996a1-162">Propriedade de navegações de referência para tipos de entidade não podem ser nulos, a menos que eles estejam explicitamente mapeados em uma tabela diferente do proprietário</span><span class="sxs-lookup"><span data-stu-id="996a1-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="996a1-163">Instâncias de tipos de entidade de propriedade não podem ser compartilhadas por vários proprietários (esse é um cenário bem conhecido para objetos de valor não pode ser implementado usando tipos de entidade de propriedade)</span><span class="sxs-lookup"><span data-stu-id="996a1-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="996a1-164">Limitações nas versões anteriores</span><span class="sxs-lookup"><span data-stu-id="996a1-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="996a1-165">No EF Core 2.0, de propriedade navegações para tipos de entidade não podem ser declarados em tipos de entidade derivada, a menos que as entidades de propriedade estejam explicitamente mapeadas em uma tabela diferente da hierarquia de proprietário.</span><span class="sxs-lookup"><span data-stu-id="996a1-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="996a1-166">Essa limitação foi removida no EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="996a1-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="996a1-167">No EF Core 2.0 e 2.1 somente a referência navegações para tipos próprios tinham suporte.</span><span class="sxs-lookup"><span data-stu-id="996a1-167">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="996a1-168">Essa limitação foi removida no EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="996a1-168">This limitation has been removed in EF Core 2.2</span></span>
