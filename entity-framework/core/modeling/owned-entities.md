---
title: Propriedade tipos de entidade - Core EF
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="e8143-102">Tipos de entidade de propriedade</span><span class="sxs-lookup"><span data-stu-id="e8143-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="e8143-103">Este recurso é novo no EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e8143-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="e8143-104">EF Core permite tipos de entidade de modelo só podem aparecer em Propriedades de navegação de outros tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="e8143-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="e8143-105">Esses são chamados de _tipos de entidade de propriedade_.</span><span class="sxs-lookup"><span data-stu-id="e8143-105">These are called _owned entity types_.</span></span> <span data-ttu-id="e8143-106">A entidade que contém um tipo de entidade de propriedade é seu _proprietário_.</span><span class="sxs-lookup"><span data-stu-id="e8143-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="e8143-107">Configuração explícita</span><span class="sxs-lookup"><span data-stu-id="e8143-107">Explicit configuration</span></span>

<span data-ttu-id="e8143-108">Propriedade da entidade tipos nunca são incluídos por núcleo EF em modelo por convenção.</span><span class="sxs-lookup"><span data-stu-id="e8143-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="e8143-109">Você pode usar o `OwnsOne` método `OnModelCreating` ou anotar o tipo com `OwnedAttrbibute` (novo no EF Core 2.1) para configurar o tipo como um tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8143-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttrbibute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="e8143-110">Neste exemplo, StreetAddress é um tipo com nenhuma propriedade de identidade.</span><span class="sxs-lookup"><span data-stu-id="e8143-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="e8143-111">Ele é usado como uma propriedade do tipo Ordem para especificar o endereço para entrega para uma ordem específica.</span><span class="sxs-lookup"><span data-stu-id="e8143-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="e8143-112">Em `OnModelCreating`, usamos o `OwnsOne` método para especificar que a propriedade ShippingAddress é uma entidade de propriedade do tipo de ordem.</span><span class="sxs-lookup"><span data-stu-id="e8143-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

<span data-ttu-id="e8143-113">Se a propriedade ShippingAddress é particular no tipo de ordem, você pode usar a versão de cadeia de caracteres do `OwnsOne` método:</span><span class="sxs-lookup"><span data-stu-id="e8143-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="e8143-114">Neste exemplo, usamos o `OwnedAttribute` para alcançar o mesmo objetivo:</span><span class="sxs-lookup"><span data-stu-id="e8143-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a><span data-ttu-id="e8143-115">Chaves implícitas</span><span class="sxs-lookup"><span data-stu-id="e8143-115">Implicit keys</span></span>

<span data-ttu-id="e8143-116">No EF Core 2.0 e 2.1, somente propriedades de navegação de referência podem apontar para os tipos de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8143-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="e8143-117">Não há suporte para coleções de tipos de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8143-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="e8143-118">Esses referência de propriedade tipos sempre têm uma relação um para um com o proprietário, portanto, não precisam seus próprios valores de chave.</span><span class="sxs-lookup"><span data-stu-id="e8143-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="e8143-119">No exemplo anterior, o tipo de StreetAddress não precisa definir uma propriedade de chave.</span><span class="sxs-lookup"><span data-stu-id="e8143-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="e8143-120">Para entender como o EF Core controla esses objetos, é útil pensar que uma chave primária é criada como um [sombra propriedade](xref:core/modeling/shadow-properties) para o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8143-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="e8143-121">O valor da chave de uma instância do tipo de propriedade será o mesmo que o valor da chave de instância do proprietário.</span><span class="sxs-lookup"><span data-stu-id="e8143-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="e8143-122">Mapeamento de tipos com a divisão da tabela de propriedade</span><span class="sxs-lookup"><span data-stu-id="e8143-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="e8143-123">Ao usar bancos de dados relacionais, por convenção, tipos de propriedade são mapeados para a mesma tabela como o proprietário.</span><span class="sxs-lookup"><span data-stu-id="e8143-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="e8143-124">Isso requer dividir a tabela em duas: algumas colunas serão usadas para armazenar os dados do proprietário e algumas colunas serão usadas para armazenar dados de entidade de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8143-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="e8143-125">Este é um recurso comum conhecido como divisão de tabela.</span><span class="sxs-lookup"><span data-stu-id="e8143-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="e8143-126">Propriedade tipos armazenados com a divisão de tabela podem ser usado de forma muito similar a tipos complexos como são usados em EF6.</span><span class="sxs-lookup"><span data-stu-id="e8143-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="e8143-127">Por convenção, EF Core nomear as colunas de banco de dados para as propriedades do tipo de propriedade de entidade seguindo o padrão _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="e8143-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="e8143-128">Portanto, as propriedades de StreetAddress aparecerão na tabela Orders com os nomes ShippingAddress_Street e ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="e8143-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="e8143-129">Você pode acrescentar o `HasColumnName` método renomear essas colunas.</span><span class="sxs-lookup"><span data-stu-id="e8143-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="e8143-130">No caso onde StreetAddress é uma propriedade pública, os mapeamentos seria</span><span class="sxs-lookup"><span data-stu-id="e8143-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="e8143-131">Compartilhando o mesmo tipo de .NET entre os vários tipos de propriedade</span><span class="sxs-lookup"><span data-stu-id="e8143-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="e8143-132">Um tipo de entidade de propriedade pode ser do mesmo tipo de .NET como outro tipo de entidade de propriedade, por isso que o tipo de .NET pode não ser suficientes para identificar um tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8143-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="e8143-133">Nesses casos, a propriedade apontando do proprietário para a entidade de propriedade torna-se a _definindo navegação_ do tipo de propriedade de entidade.</span><span class="sxs-lookup"><span data-stu-id="e8143-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="e8143-134">Da perspectiva do EF Core, a navegação de definição faz parte da identidade do tipo junto com o tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="e8143-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="e8143-135">Por exemplo, na classe a seguir, ShippingAddress e BillingAddress são do mesmo tipo .NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="e8143-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="e8143-136">Para entender como o EF Core distinguirá controladas instâncias desses objetos, pode ser útil considerar que a definição de navegação se torna parte da chave de instância junto com o valor da chave do proprietário e o tipo de .NET do tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8143-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="e8143-137">Tipos aninhados de propriedade</span><span class="sxs-lookup"><span data-stu-id="e8143-137">Nested owned types</span></span>

<span data-ttu-id="e8143-138">Neste exemplo OrderDetails possui BillingAddress e ShippingAddress, que são os dois tipos de StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="e8143-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="e8143-139">Em seguida, OrderDetails pertence o tipo de ordem.</span><span class="sxs-lookup"><span data-stu-id="e8143-139">Then OrderDetails is owned by the Order type.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="e8143-140">É possível para a cadeia de `OwnsOne` método em um mapeamento fluente para configurar este modelo:</span><span class="sxs-lookup"><span data-stu-id="e8143-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="e8143-141">É possível obter a mesma coisa usando `OwnedAttribute` OrderDetails e StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="e8143-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="e8143-142">Além dos tipos de propriedade aninhados, um tipo de propriedade pode fazer referência a uma entidade normal.</span><span class="sxs-lookup"><span data-stu-id="e8143-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="e8143-143">No exemplo a seguir, o país for uma entidade normal (ou seja, não é proprietário):</span><span class="sxs-lookup"><span data-stu-id="e8143-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="e8143-144">Este recurso define tipos de entidade de propriedade além de tipos complexos no EF6.</span><span class="sxs-lookup"><span data-stu-id="e8143-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="e8143-145">Armazenando tipos de propriedade em tabelas separadas</span><span class="sxs-lookup"><span data-stu-id="e8143-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="e8143-146">Também ao contrário de tipos complexos de EF6, tipos de propriedade podem ser armazenados em uma tabela separada do proprietário.</span><span class="sxs-lookup"><span data-stu-id="e8143-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="e8143-147">Para substituir a convenção de que um tipo de propriedade é mapeado para a mesma tabela como o proprietário, você pode simplesmente chamar `ToTable` e forneça um nome de tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="e8143-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="e8143-148">O exemplo a seguir mapeará OrderDetails e seus dois endereços em uma tabela diferente da ordem:</span><span class="sxs-lookup"><span data-stu-id="e8143-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="e8143-149">Consultando os tipos de propriedade</span><span class="sxs-lookup"><span data-stu-id="e8143-149">Querying owned types</span></span>

<span data-ttu-id="e8143-150">Ao consultar o proprietário, os tipos próprios serão incluídos por padrão.</span><span class="sxs-lookup"><span data-stu-id="e8143-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="e8143-151">Não é necessário usar o `Include` método, mesmo se os tipos de propriedade são armazenados em uma tabela separada.</span><span class="sxs-lookup"><span data-stu-id="e8143-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="e8143-152">Com base no modelo descrito antes, a consulta a seguir extrairá ordem, OrderDetails e os dois StreeAddresses corporativos para todos os pedidos pendentes do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="e8143-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="e8143-153">Limitações</span><span class="sxs-lookup"><span data-stu-id="e8143-153">Limitations</span></span>

<span data-ttu-id="e8143-154">Aqui estão algumas limitações de tipos de entidade de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8143-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="e8143-155">Algumas dessas limitações são fundamentais para o trabalho de tipos como proprietários, mas outras são as restrições de point-in-time que desejamos remover em futuras versões:</span><span class="sxs-lookup"><span data-stu-id="e8143-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="e8143-156">Limitações atuais</span><span class="sxs-lookup"><span data-stu-id="e8143-156">Current shortcomings</span></span>
- <span data-ttu-id="e8143-157">Não há suporte para a herança dos tipos de propriedade</span><span class="sxs-lookup"><span data-stu-id="e8143-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="e8143-158">Tipos de propriedade não podem ser apontados por uma propriedade de navegação de coleção</span><span class="sxs-lookup"><span data-stu-id="e8143-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="e8143-159">Desde que eles usam a tabela dividindo pelo padrão de propriedade tipos também têm as seguintes restrições, a menos que explicitamente mapeados em uma tabela diferente:</span><span class="sxs-lookup"><span data-stu-id="e8143-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="e8143-160">Eles não podem pertencer a um tipo derivado</span><span class="sxs-lookup"><span data-stu-id="e8143-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="e8143-161">A propriedade de navegação definição não pode ser definida como null (ou seja, propriedade de tipos na mesma tabela sempre são necessários)</span><span class="sxs-lookup"><span data-stu-id="e8143-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="e8143-162">Por design, restrições</span><span class="sxs-lookup"><span data-stu-id="e8143-162">By-design restrictions</span></span>
- <span data-ttu-id="e8143-163">Não é possível criar um `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="e8143-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="e8143-164">Não é possível chamar `Entity<T>()` com um tipo de propriedade no `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="e8143-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
