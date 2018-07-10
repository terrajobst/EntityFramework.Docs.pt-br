---
title: Propriedade tipos de entidade – EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 768429b857b09c1974f4ade31b5bbb6b1c7e15c3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911870"
---
# <a name="owned-entity-types"></a><span data-ttu-id="e7adb-102">Tipos de entidade própria</span><span class="sxs-lookup"><span data-stu-id="e7adb-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="e7adb-103">Esse recurso é novo no EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e7adb-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="e7adb-104">O EF Core permite que você a tipos de entidade de modelo que só podem aparecer nas propriedades de navegação de outros tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="e7adb-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="e7adb-105">Eles são chamados _tipos de entidade de propriedade_.</span><span class="sxs-lookup"><span data-stu-id="e7adb-105">These are called _owned entity types_.</span></span> <span data-ttu-id="e7adb-106">É a entidade que contém um tipo de entidade própria seus _proprietário_.</span><span class="sxs-lookup"><span data-stu-id="e7adb-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="e7adb-107">Configuração explícita</span><span class="sxs-lookup"><span data-stu-id="e7adb-107">Explicit configuration</span></span>

<span data-ttu-id="e7adb-108">Propriedade da entidade tipos nunca são incluídos pelo EF Core em modelo por convenção.</span><span class="sxs-lookup"><span data-stu-id="e7adb-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="e7adb-109">Você pode usar o `OwnsOne` método no `OnModelCreating` ou anotar o tipo com `OwnedAttribute` (novo no EF Core 2.1) para configurar o tipo como um tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7adb-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="e7adb-110">Neste exemplo, StreetAddress é um tipo com nenhuma propriedade de identidade.</span><span class="sxs-lookup"><span data-stu-id="e7adb-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="e7adb-111">Ele é usado como uma propriedade do tipo Ordem para especificar o endereço para entrega para uma ordem específica.</span><span class="sxs-lookup"><span data-stu-id="e7adb-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="e7adb-112">Na `OnModelCreating`, usamos o `OwnsOne` método para especificar que a propriedade ShippingAddress é uma entidade de propriedade do tipo de ordem.</span><span class="sxs-lookup"><span data-stu-id="e7adb-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="e7adb-113">Se a propriedade ShippingAddress é particular no tipo de ordem, você pode usar a versão de cadeia de caracteres da `OwnsOne` método:</span><span class="sxs-lookup"><span data-stu-id="e7adb-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="e7adb-114">Neste exemplo, usamos o `OwnedAttribute` para alcançar o mesmo objetivo:</span><span class="sxs-lookup"><span data-stu-id="e7adb-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="e7adb-115">Chaves implícitas</span><span class="sxs-lookup"><span data-stu-id="e7adb-115">Implicit keys</span></span>

<span data-ttu-id="e7adb-116">No EF Core 2.0 e 2.1, somente as propriedades de navegação de referência podem apontar para tipos próprios.</span><span class="sxs-lookup"><span data-stu-id="e7adb-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="e7adb-117">Não há suporte para coleções de tipos próprios.</span><span class="sxs-lookup"><span data-stu-id="e7adb-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="e7adb-118">Essas referência de propriedade tipos sempre têm uma relação um para um com o proprietário, portanto eles não precisam seus próprios valores de chave.</span><span class="sxs-lookup"><span data-stu-id="e7adb-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="e7adb-119">No exemplo anterior, o tipo de StreetAddress não precisa definir uma propriedade de chave.</span><span class="sxs-lookup"><span data-stu-id="e7adb-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="e7adb-120">Para entender como o EF Core controla esses objetos, é útil pensar que uma chave primária é criada como um [propriedade de sombra](xref:core/modeling/shadow-properties) para o tipo próprio.</span><span class="sxs-lookup"><span data-stu-id="e7adb-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="e7adb-121">O valor da chave de uma instância do tipo próprio será igual ao valor da chave da instância do proprietário.</span><span class="sxs-lookup"><span data-stu-id="e7adb-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="e7adb-122">Tipos com a divisão de tabela de mapeamento de propriedade</span><span class="sxs-lookup"><span data-stu-id="e7adb-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="e7adb-123">Ao usar bancos de dados relacionais, por convenção, tipos de propriedade são mapeados para a mesma tabela como o proprietário.</span><span class="sxs-lookup"><span data-stu-id="e7adb-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="e7adb-124">Isso exige a dividir a tabela em dois: algumas colunas serão usadas para armazenar os dados do proprietário, e algumas colunas serão usadas para armazenar dados de entidade de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7adb-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="e7adb-125">Esse é um recurso comum conhecido como a divisão de tabela.</span><span class="sxs-lookup"><span data-stu-id="e7adb-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="e7adb-126">Propriedade armazenados com a divisão de tabela de tipos podem ser usados de maneira muito semelhante a como os tipos complexos são usados no EF6.</span><span class="sxs-lookup"><span data-stu-id="e7adb-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="e7adb-127">Por convenção, o EF Core nomeie as colunas de banco de dados para as propriedades do tipo de entidade própria seguindo o padrão _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="e7adb-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="e7adb-128">Portanto, as propriedades de StreetAddress aparecerão na tabela de pedidos com os nomes ShippingAddress_Street e ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="e7adb-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="e7adb-129">Você pode acrescentar o `HasColumnName` método para renomear as colunas.</span><span class="sxs-lookup"><span data-stu-id="e7adb-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="e7adb-130">No caso em que StreetAddress é uma propriedade pública, os mapeamentos seriam</span><span class="sxs-lookup"><span data-stu-id="e7adb-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="e7adb-131">Compartilhando o mesmo tipo de .NET entre vários tipos de propriedade</span><span class="sxs-lookup"><span data-stu-id="e7adb-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="e7adb-132">Um tipo de entidade própria pode ser do mesmo tipo de .NET como outro tipo de entidade de propriedade, portanto, que o tipo do .NET pode não ser suficiente para identificar um tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7adb-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="e7adb-133">Nesses casos, a propriedade apontando do proprietário para a entidade de propriedade se torna a _definir a navegação_ do tipo de entidade de propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7adb-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="e7adb-134">Da perspectiva do EF Core, a navegação de definição é parte da identidade do tipo juntamente com o tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="e7adb-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="e7adb-135">Por exemplo, a seguinte classe, ShippingAddress e BillingAddress são do mesmo tipo de .NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="e7adb-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="e7adb-136">Para compreender como o EF Core distinguirá controladas instâncias desses objetos, pode ser útil pensar que a navegação de definição tornou-se parte da chave da instância junto com o valor da chave do proprietário e o tipo de .NET do tipo próprio.</span><span class="sxs-lookup"><span data-stu-id="e7adb-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="e7adb-137">Tipos próprios aninhados</span><span class="sxs-lookup"><span data-stu-id="e7adb-137">Nested owned types</span></span>

<span data-ttu-id="e7adb-138">Neste exemplo OrderDetails possui BillingAddress e ShippingAddress, que são os dois tipos de StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="e7adb-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="e7adb-139">Em seguida, OrderDetails pertence ao tipo Order.</span><span class="sxs-lookup"><span data-stu-id="e7adb-139">Then OrderDetails is owned by the Order type.</span></span>

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

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="e7adb-140">É possível encadear o `OwnsOne` método em um mapeamento fluente para configurar este modelo:</span><span class="sxs-lookup"><span data-stu-id="e7adb-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="e7adb-141">É possível obter a mesma coisa usando `OwnedAttribute` em OrderDetails e StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="e7adb-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="e7adb-142">Além dos tipos próprios aninhados, um tipo próprio pode fazer referência a uma entidade regular.</span><span class="sxs-lookup"><span data-stu-id="e7adb-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="e7adb-143">No exemplo a seguir, o país é uma entidade normal (ou seja, não é proprietário):</span><span class="sxs-lookup"><span data-stu-id="e7adb-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="e7adb-144">Esse recurso define tipos de entidade própria além dos tipos complexos no EF6.</span><span class="sxs-lookup"><span data-stu-id="e7adb-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="e7adb-145">Armazenando tipos próprios em tabelas separadas</span><span class="sxs-lookup"><span data-stu-id="e7adb-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="e7adb-146">Também diferentemente dos tipos complexos do EF6, tipos de propriedade podem ser armazenados em uma tabela separada do proprietário.</span><span class="sxs-lookup"><span data-stu-id="e7adb-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="e7adb-147">Para substituir a convenção que mapeia um tipo de propriedade para a mesma tabela como o proprietário, você pode simplesmente chamar `ToTable` e forneça um nome de tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="e7adb-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="e7adb-148">O exemplo a seguir será mapeada OrderDetails e seus dois endereços em uma tabela diferente da ordem:</span><span class="sxs-lookup"><span data-stu-id="e7adb-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="e7adb-149">Consultando tipos próprios</span><span class="sxs-lookup"><span data-stu-id="e7adb-149">Querying owned types</span></span>

<span data-ttu-id="e7adb-150">Ao consultar o proprietário, os tipos próprios serão incluídos por padrão.</span><span class="sxs-lookup"><span data-stu-id="e7adb-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="e7adb-151">Não é necessário usar o `Include` método, mesmo se os tipos próprios são armazenados em uma tabela separada.</span><span class="sxs-lookup"><span data-stu-id="e7adb-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="e7adb-152">A consulta a seguir com base no modelo descrito antes, efetuará pull ordem, OrderDetails e os dois StreeAddresses propriedade para todos os pedidos pendentes do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="e7adb-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="e7adb-153">Limitações</span><span class="sxs-lookup"><span data-stu-id="e7adb-153">Limitations</span></span>

<span data-ttu-id="e7adb-154">Algumas dessas limitações são fundamentais para o trabalho de tipos de entidade como propriedade, mas outras são restrições que podemos pode ser capazes de ser removido em futuras versões:</span><span class="sxs-lookup"><span data-stu-id="e7adb-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="e7adb-155">Limitações nas versões anteriores</span><span class="sxs-lookup"><span data-stu-id="e7adb-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="e7adb-156">No EF Core 2.0, de propriedade navegações para tipos de entidade não podem ser declarados em tipos de entidade derivada, a menos que as entidades de propriedade estejam explicitamente mapeadas em uma tabela diferente da hierarquia de proprietário.</span><span class="sxs-lookup"><span data-stu-id="e7adb-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="e7adb-157">Essa limitação foi removida no EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="e7adb-157">This limitation has been removed in EF Core 2.1</span></span>
 
### <a name="current-shortcomings"></a><span data-ttu-id="e7adb-158">Limitações atuais</span><span class="sxs-lookup"><span data-stu-id="e7adb-158">Current shortcomings</span></span>
- <span data-ttu-id="e7adb-159">Não há suporte para tipos de entidade de propriedade de hierarquias de herança que incluem</span><span class="sxs-lookup"><span data-stu-id="e7adb-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="e7adb-160">Tipos de entidade de propriedade não podem ser apontados por uma propriedade de navegação de coleção (somente a referência navegações têm suporte no momento)</span><span class="sxs-lookup"><span data-stu-id="e7adb-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="e7adb-161">Navegações para tipos de entidade não podem ser nulos, a menos que eles estejam explicitamente mapeados em uma tabela diferente do proprietário de propriedade</span><span class="sxs-lookup"><span data-stu-id="e7adb-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span> 
- <span data-ttu-id="e7adb-162">Instâncias de tipos de entidade de propriedade não podem ser compartilhadas por vários proprietários (esse é um cenário bem conhecido para objetos de valor não pode ser implementado usando tipos de entidade de propriedade)</span><span class="sxs-lookup"><span data-stu-id="e7adb-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="e7adb-163">Restrições de design</span><span class="sxs-lookup"><span data-stu-id="e7adb-163">By-design restrictions</span></span>
- <span data-ttu-id="e7adb-164">Não é possível criar um `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="e7adb-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="e7adb-165">Você não pode chamar `Entity<T>()` com um tipo de propriedade em `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="e7adb-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
