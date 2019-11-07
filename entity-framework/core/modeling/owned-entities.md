---
title: Tipos de entidade de propriedade-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: a0665bfa27134b8dc3eba854ff3f7b1af4b69217
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655931"
---
# <a name="owned-entity-types"></a><span data-ttu-id="14229-102">Tipos de entidade de propriedade</span><span class="sxs-lookup"><span data-stu-id="14229-102">Owned Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="14229-103">Esse recurso é novo no EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="14229-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="14229-104">EF Core permite que você modele tipos de entidade que só podem aparecer em Propriedades de navegação de outros tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="14229-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="14229-105">Eles são chamados de _tipos de entidade de propriedade_.</span><span class="sxs-lookup"><span data-stu-id="14229-105">These are called _owned entity types_.</span></span> <span data-ttu-id="14229-106">A entidade que contém um tipo de entidade de propriedade é seu _proprietário_.</span><span class="sxs-lookup"><span data-stu-id="14229-106">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="14229-107">As entidades de propriedade são essencialmente uma parte do proprietário e não podem existir sem ela, elas são conceitualmente semelhantes às [agregações](https://martinfowler.com/bliki/DDD_Aggregate.html).</span><span class="sxs-lookup"><span data-stu-id="14229-107">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="14229-108">Configuração explícita</span><span class="sxs-lookup"><span data-stu-id="14229-108">Explicit configuration</span></span>

<span data-ttu-id="14229-109">Tipos de entidade de propriedade nunca são incluídos por EF Core no modelo por convenção.</span><span class="sxs-lookup"><span data-stu-id="14229-109">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="14229-110">Você pode usar o método `OwnsOne` em `OnModelCreating` ou anotar o tipo com `OwnedAttribute` (novo no EF Core 2,1) para configurar o tipo como um tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="14229-110">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="14229-111">Neste exemplo, `StreetAddress` é um tipo sem nenhuma propriedade de identidade.</span><span class="sxs-lookup"><span data-stu-id="14229-111">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="14229-112">Ele é usado como uma propriedade do tipo Ordem para especificar o endereço para entrega para uma ordem específica.</span><span class="sxs-lookup"><span data-stu-id="14229-112">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="14229-113">Podemos usar o `OwnedAttribute` para tratá-lo como uma entidade de propriedade quando referenciado de outro tipo de entidade:</span><span class="sxs-lookup"><span data-stu-id="14229-113">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="14229-114">Também é possível usar o método `OwnsOne` em `OnModelCreating` para especificar que a propriedade `ShippingAddress` é uma entidade de Propriedade do tipo de entidade `Order` e configurar facetas adicionais, se necessário.</span><span class="sxs-lookup"><span data-stu-id="14229-114">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="14229-115">Se a propriedade `ShippingAddress` for particular no tipo `Order`, você poderá usar a versão de cadeia de caracteres do método `OwnsOne`:</span><span class="sxs-lookup"><span data-stu-id="14229-115">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="14229-116">Consulte o [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) para obter mais contexto.</span><span class="sxs-lookup"><span data-stu-id="14229-116">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span>

## <a name="implicit-keys"></a><span data-ttu-id="14229-117">Chaves implícitas</span><span class="sxs-lookup"><span data-stu-id="14229-117">Implicit keys</span></span>

<span data-ttu-id="14229-118">Tipos de propriedade configurados com `OwnsOne` ou descobertos por meio de uma navegação de referência sempre têm uma relação um-para-um com o proprietário, portanto, eles não precisam de seus próprios valores de chave, pois os valores de chave estrangeira são exclusivos.</span><span class="sxs-lookup"><span data-stu-id="14229-118">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="14229-119">No exemplo anterior, o tipo de `StreetAddress` não precisa definir uma propriedade de chave.</span><span class="sxs-lookup"><span data-stu-id="14229-119">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="14229-120">Para entender como o EF Core controla esses objetos, é útil saber que uma chave primária é criada como uma [propriedade de sombra](xref:core/modeling/shadow-properties) para o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="14229-120">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="14229-121">O valor da chave de uma instância do tipo de propriedade será igual ao valor da chave da instância do proprietário.</span><span class="sxs-lookup"><span data-stu-id="14229-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="14229-122">Coleções de tipos de propriedade</span><span class="sxs-lookup"><span data-stu-id="14229-122">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="14229-123">Este recurso é novo no EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="14229-123">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="14229-124">Para configurar uma coleção de tipos de propriedade, use `OwnsMany` em `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="14229-124">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="14229-125">Tipos de propriedade precisam de uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="14229-125">Owned types need a primary key.</span></span> <span data-ttu-id="14229-126">Se não houver boas propriedades de candidatos no tipo .NET, EF Core poderá tentar criar uma.</span><span class="sxs-lookup"><span data-stu-id="14229-126">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="14229-127">No entanto, quando tipos de propriedade são definidos por meio de uma coleção, não é suficiente apenas criar uma propriedade Shadow para atuar como a chave estrangeira no proprietário e a chave primária da instância de propriedade, como fazemos para `OwnsOne`: pode haver várias instâncias de tipo de propriedade para cada proprietário e, portanto, a chave do proprietário não é suficiente para fornecer uma identidade exclusiva para cada instância de propriedade.</span><span class="sxs-lookup"><span data-stu-id="14229-127">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="14229-128">As duas soluções mais simples para isso são:</span><span class="sxs-lookup"><span data-stu-id="14229-128">The two most straightforward solutions to this are:</span></span>

- <span data-ttu-id="14229-129">Definir uma chave primária substituta em uma nova propriedade independente da chave estrangeira que aponta para o proprietário.</span><span class="sxs-lookup"><span data-stu-id="14229-129">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="14229-130">Os valores contidos precisariam ser exclusivos em todos os proprietários (por exemplo, se o pai {1} tiver {1}filho, o pai {2} não poderá ter {1}filho), portanto, o valor não terá nenhum significado inerente.</span><span class="sxs-lookup"><span data-stu-id="14229-130">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="14229-131">Como a chave estrangeira não faz parte da chave primária, seus valores podem ser alterados e, portanto, você pode mover um filho de um pai para outro, no entanto, isso geralmente se aplica à semântica agregada.</span><span class="sxs-lookup"><span data-stu-id="14229-131">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="14229-132">Usando a chave estrangeira e uma propriedade adicional como uma chave composta.</span><span class="sxs-lookup"><span data-stu-id="14229-132">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="14229-133">O valor da propriedade adicional agora só precisa ser exclusivo para um determinado pai (portanto, se o {1} pai tiver filho {1,1} o pai {2} ainda poderá ter {2,1}filho).</span><span class="sxs-lookup"><span data-stu-id="14229-133">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="14229-134">Ao tornar a parte da chave estrangeira da chave primária, a relação entre o proprietário e a entidade de propriedade torna-se imutável e reflete melhor a semântica agregada.</span><span class="sxs-lookup"><span data-stu-id="14229-134">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="14229-135">Isso é o que EF Core faz por padrão.</span><span class="sxs-lookup"><span data-stu-id="14229-135">This is what EF Core does by default.</span></span>

<span data-ttu-id="14229-136">Neste exemplo, usaremos a classe `Distributor`:</span><span class="sxs-lookup"><span data-stu-id="14229-136">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="14229-137">Por padrão, a chave primária usada para o tipo de propriedade referenciada por meio da propriedade de navegação `ShippingCenters` será `("DistributorId", "Id")` em que `"DistributorId"` é o FK e `"Id"` é um valor de `int` exclusivo.</span><span class="sxs-lookup"><span data-stu-id="14229-137">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="14229-138">Para configurar uma chamada de CP diferente `HasKey`:</span><span class="sxs-lookup"><span data-stu-id="14229-138">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="14229-139">Antes de EF Core 3,0 `WithOwner()` método não existe, portanto, essa chamada deve ser removida.</span><span class="sxs-lookup"><span data-stu-id="14229-139">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="14229-140">Mapeando tipos de propriedade com divisão de tabela</span><span class="sxs-lookup"><span data-stu-id="14229-140">Mapping owned types with table splitting</span></span>

<span data-ttu-id="14229-141">Ao usar bancos de dados relacionais, por padrão, os tipos de propriedade são mapeados para a mesma tabela que o proprietário.</span><span class="sxs-lookup"><span data-stu-id="14229-141">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="14229-142">Isso requer a divisão da tabela em duas: algumas colunas serão usadas para armazenar os dados do proprietário e algumas colunas serão usadas para armazenar dados da entidade de propriedade.</span><span class="sxs-lookup"><span data-stu-id="14229-142">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="14229-143">Esse é um recurso comum conhecido como [divisão de tabela](table-splitting.md).</span><span class="sxs-lookup"><span data-stu-id="14229-143">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="14229-144">Por padrão, EF Core nomeará as colunas do banco de dados para as propriedades do tipo de entidade de propriedade seguindo o padrão _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="14229-144">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="14229-145">Portanto, as propriedades de `StreetAddress` serão exibidas na tabela ' Orders ' com os nomes ' ShippingAddress_Street ' e ' ShippingAddress_City '.</span><span class="sxs-lookup"><span data-stu-id="14229-145">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="14229-146">Você pode usar o método `HasColumnName` para renomear essas colunas:</span><span class="sxs-lookup"><span data-stu-id="14229-146">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="14229-147">Compartilhando o mesmo tipo .NET entre vários tipos de propriedade</span><span class="sxs-lookup"><span data-stu-id="14229-147">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="14229-148">Um tipo de entidade de propriedade pode ser do mesmo tipo .NET que outro tipo de entidade de propriedade, portanto, o tipo .NET pode não ser suficiente para identificar um tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="14229-148">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="14229-149">Nesses casos, a propriedade que aponta do proprietário para a entidade de propriedade se torna a _definição da navegação_ do tipo de entidade de propriedade.</span><span class="sxs-lookup"><span data-stu-id="14229-149">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="14229-150">Da perspectiva do EF Core, a definição da navegação faz parte da identidade do tipo junto com o tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="14229-150">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>

<span data-ttu-id="14229-151">Por exemplo, na classe a seguir `ShippingAddress` e `BillingAddress` são do mesmo tipo .NET, `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="14229-151">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="14229-152">Para entender como EF Core irá distinguir as instâncias rastreadas desses objetos, pode ser útil imaginar que a definição de navegação se tornou parte da chave da instância, junto com o valor da chave do proprietário e o tipo de .NET do tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="14229-152">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="14229-153">Tipos de propriedade aninhada</span><span class="sxs-lookup"><span data-stu-id="14229-153">Nested owned types</span></span>

<span data-ttu-id="14229-154">Neste exemplo `OrderDetails` possui `BillingAddress` e `ShippingAddress`, que são ambos os tipos de `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="14229-154">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="14229-155">Então `OrderDetails` pertence ao tipo `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="14229-155">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="14229-156">Além dos tipos de propriedade aninhados, um tipo de propriedade pode fazer referência a uma entidade regular, pode ser o proprietário ou uma entidade diferente, desde que a entidade de propriedade esteja no lado dependente.</span><span class="sxs-lookup"><span data-stu-id="14229-156">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="14229-157">Esse recurso define tipos de entidade pertencentes de tipos complexos em EF6.</span><span class="sxs-lookup"><span data-stu-id="14229-157">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="14229-158">É possível encadear o método `OwnsOne` em uma chamada fluente para configurar esse modelo:</span><span class="sxs-lookup"><span data-stu-id="14229-158">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="14229-159">Observe a chamada `WithOwner` usada para configurar a propriedade de navegação apontando de volta ao proprietário.</span><span class="sxs-lookup"><span data-stu-id="14229-159">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span>

<span data-ttu-id="14229-160">É possível obter o resultado usando `OwnedAttribute` em `OrderDetails` e `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="14229-160">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="14229-161">Armazenando tipos de propriedade em tabelas separadas</span><span class="sxs-lookup"><span data-stu-id="14229-161">Storing owned types in separate tables</span></span>

<span data-ttu-id="14229-162">Além disso, ao contrário dos tipos complexos EF6, tipos de propriedade podem ser armazenados em uma tabela separada do proprietário.</span><span class="sxs-lookup"><span data-stu-id="14229-162">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="14229-163">Para substituir a Convenção que mapeia um tipo de propriedade para a mesma tabela que o proprietário, você pode simplesmente chamar `ToTable` e fornecer um nome de tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="14229-163">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="14229-164">O exemplo a seguir mapeará `OrderDetails` e seus dois endereços para uma tabela separada de `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="14229-164">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="14229-165">Consultando tipos de propriedade</span><span class="sxs-lookup"><span data-stu-id="14229-165">Querying owned types</span></span>

<span data-ttu-id="14229-166">Ao consultar o proprietário, os tipos próprios serão incluídos por padrão.</span><span class="sxs-lookup"><span data-stu-id="14229-166">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="14229-167">Não é necessário usar o método `Include`, mesmo que os tipos de propriedade sejam armazenados em uma tabela separada.</span><span class="sxs-lookup"><span data-stu-id="14229-167">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="14229-168">Com base no modelo descrito anteriormente, a consulta a seguir obterá `Order`, `OrderDetails` e as duas `StreetAddresses` de Propriedade do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="14229-168">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="14229-169">Limitações</span><span class="sxs-lookup"><span data-stu-id="14229-169">Limitations</span></span>

<span data-ttu-id="14229-170">Algumas dessas limitações são fundamentais para a forma como os tipos de entidade de propriedade funcionam, mas algumas outras são restrições que podem ser capazes de remover em versões futuras:</span><span class="sxs-lookup"><span data-stu-id="14229-170">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="14229-171">Restrições por design</span><span class="sxs-lookup"><span data-stu-id="14229-171">By-design restrictions</span></span>

- <span data-ttu-id="14229-172">Não é possível criar um `DbSet<T>` para um tipo de propriedade</span><span class="sxs-lookup"><span data-stu-id="14229-172">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="14229-173">Você não pode chamar `Entity<T>()` com um tipo de propriedade em `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="14229-173">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="14229-174">Deficiências atuais</span><span class="sxs-lookup"><span data-stu-id="14229-174">Current shortcomings</span></span>

- <span data-ttu-id="14229-175">Não há suporte para hierarquias de herança que incluem tipos de entidade de propriedade</span><span class="sxs-lookup"><span data-stu-id="14229-175">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="14229-176">Navegações de referência para tipos de entidade pertencentes não podem ser nulas, a menos que sejam explicitamente mapeados para uma tabela separada do proprietário</span><span class="sxs-lookup"><span data-stu-id="14229-176">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="14229-177">Instâncias de tipos de entidade de propriedade não podem ser compartilhadas por vários proprietários (este é um cenário bem conhecido para objetos de valor que não podem ser implementados usando tipos de entidade de propriedade)</span><span class="sxs-lookup"><span data-stu-id="14229-177">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="14229-178">Limitações em versões anteriores</span><span class="sxs-lookup"><span data-stu-id="14229-178">Shortcomings in previous versions</span></span>

- <span data-ttu-id="14229-179">No EF Core 2,0, navegações para tipos de entidade de propriedade não podem ser declaradas em tipos de entidade derivadas, a menos que as entidades de propriedade sejam explicitamente mapeadas para uma tabela separada da hierarquia de proprietário.</span><span class="sxs-lookup"><span data-stu-id="14229-179">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="14229-180">Essa limitação foi removida no EF Core 2,1</span><span class="sxs-lookup"><span data-stu-id="14229-180">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="14229-181">No EF Core 2,0 e 2,1, somente navegações de referência a tipos de propriedade eram compatíveis.</span><span class="sxs-lookup"><span data-stu-id="14229-181">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="14229-182">Essa limitação foi removida no EF Core 2,2</span><span class="sxs-lookup"><span data-stu-id="14229-182">This limitation has been removed in EF Core 2.2</span></span>
