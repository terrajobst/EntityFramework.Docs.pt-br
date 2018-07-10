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
# <a name="owned-entity-types"></a>Tipos de entidade própria

>[!NOTE]
> Esse recurso é novo no EF Core 2.0.

O EF Core permite que você a tipos de entidade de modelo que só podem aparecer nas propriedades de navegação de outros tipos de entidade. Eles são chamados _tipos de entidade de propriedade_. É a entidade que contém um tipo de entidade própria seus _proprietário_.

## <a name="explicit-configuration"></a>Configuração explícita

Propriedade da entidade tipos nunca são incluídos pelo EF Core em modelo por convenção. Você pode usar o `OwnsOne` método no `OnModelCreating` ou anotar o tipo com `OwnedAttribute` (novo no EF Core 2.1) para configurar o tipo como um tipo de propriedade.

Neste exemplo, StreetAddress é um tipo com nenhuma propriedade de identidade. Ele é usado como uma propriedade do tipo Ordem para especificar o endereço para entrega para uma ordem específica. Na `OnModelCreating`, usamos o `OwnsOne` método para especificar que a propriedade ShippingAddress é uma entidade de propriedade do tipo de ordem.

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

Se a propriedade ShippingAddress é particular no tipo de ordem, você pode usar a versão de cadeia de caracteres da `OwnsOne` método:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

Neste exemplo, usamos o `OwnedAttribute` para alcançar o mesmo objetivo:

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

## <a name="implicit-keys"></a>Chaves implícitas

No EF Core 2.0 e 2.1, somente as propriedades de navegação de referência podem apontar para tipos próprios. Não há suporte para coleções de tipos próprios. Essas referência de propriedade tipos sempre têm uma relação um para um com o proprietário, portanto eles não precisam seus próprios valores de chave. No exemplo anterior, o tipo de StreetAddress não precisa definir uma propriedade de chave.  

Para entender como o EF Core controla esses objetos, é útil pensar que uma chave primária é criada como um [propriedade de sombra](xref:core/modeling/shadow-properties) para o tipo próprio. O valor da chave de uma instância do tipo próprio será igual ao valor da chave da instância do proprietário.      

## <a name="mapping-owned-types-with-table-splitting"></a>Tipos com a divisão de tabela de mapeamento de propriedade

Ao usar bancos de dados relacionais, por convenção, tipos de propriedade são mapeados para a mesma tabela como o proprietário. Isso exige a dividir a tabela em dois: algumas colunas serão usadas para armazenar os dados do proprietário, e algumas colunas serão usadas para armazenar dados de entidade de propriedade. Esse é um recurso comum conhecido como a divisão de tabela.

> [!TIP]
> Propriedade armazenados com a divisão de tabela de tipos podem ser usados de maneira muito semelhante a como os tipos complexos são usados no EF6.

Por convenção, o EF Core nomeie as colunas de banco de dados para as propriedades do tipo de entidade própria seguindo o padrão _EntityProperty_OwnedEntityProperty_. Portanto, as propriedades de StreetAddress aparecerão na tabela de pedidos com os nomes ShippingAddress_Street e ShippingAddress_City.

Você pode acrescentar o `HasColumnName` método para renomear as colunas. No caso em que StreetAddress é uma propriedade pública, os mapeamentos seriam

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Compartilhando o mesmo tipo de .NET entre vários tipos de propriedade

Um tipo de entidade própria pode ser do mesmo tipo de .NET como outro tipo de entidade de propriedade, portanto, que o tipo do .NET pode não ser suficiente para identificar um tipo de propriedade.

Nesses casos, a propriedade apontando do proprietário para a entidade de propriedade se torna a _definir a navegação_ do tipo de entidade de propriedade. Da perspectiva do EF Core, a navegação de definição é parte da identidade do tipo juntamente com o tipo .NET.   

Por exemplo, a seguinte classe, ShippingAddress e BillingAddress são do mesmo tipo de .NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Para compreender como o EF Core distinguirá controladas instâncias desses objetos, pode ser útil pensar que a navegação de definição tornou-se parte da chave da instância junto com o valor da chave do proprietário e o tipo de .NET do tipo próprio.

## <a name="nested-owned-types"></a>Tipos próprios aninhados

Neste exemplo OrderDetails possui BillingAddress e ShippingAddress, que são os dois tipos de StreetAddress. Em seguida, OrderDetails pertence ao tipo Order.

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

É possível encadear o `OwnsOne` método em um mapeamento fluente para configurar este modelo:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

É possível obter a mesma coisa usando `OwnedAttribute` em OrderDetails e StreetAdress.

Além dos tipos próprios aninhados, um tipo próprio pode fazer referência a uma entidade regular. No exemplo a seguir, o país é uma entidade normal (ou seja, não é proprietário):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Esse recurso define tipos de entidade própria além dos tipos complexos no EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Armazenando tipos próprios em tabelas separadas

Também diferentemente dos tipos complexos do EF6, tipos de propriedade podem ser armazenados em uma tabela separada do proprietário. Para substituir a convenção que mapeia um tipo de propriedade para a mesma tabela como o proprietário, você pode simplesmente chamar `ToTable` e forneça um nome de tabela diferente. O exemplo a seguir será mapeada OrderDetails e seus dois endereços em uma tabela diferente da ordem:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Consultando tipos próprios

Ao consultar o proprietário, os tipos próprios serão incluídos por padrão. Não é necessário usar o `Include` método, mesmo se os tipos próprios são armazenados em uma tabela separada. A consulta a seguir com base no modelo descrito antes, efetuará pull ordem, OrderDetails e os dois StreeAddresses propriedade para todos os pedidos pendentes do banco de dados:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Limitações

Algumas dessas limitações são fundamentais para o trabalho de tipos de entidade como propriedade, mas outras são restrições que podemos pode ser capazes de ser removido em futuras versões:

### <a name="shortcomings-in-previous-versions"></a>Limitações nas versões anteriores
- No EF Core 2.0, de propriedade navegações para tipos de entidade não podem ser declarados em tipos de entidade derivada, a menos que as entidades de propriedade estejam explicitamente mapeadas em uma tabela diferente da hierarquia de proprietário. Essa limitação foi removida no EF Core 2.1
 
### <a name="current-shortcomings"></a>Limitações atuais
- Não há suporte para tipos de entidade de propriedade de hierarquias de herança que incluem
- Tipos de entidade de propriedade não podem ser apontados por uma propriedade de navegação de coleção (somente a referência navegações têm suporte no momento)
- Navegações para tipos de entidade não podem ser nulos, a menos que eles estejam explicitamente mapeados em uma tabela diferente do proprietário de propriedade 
- Instâncias de tipos de entidade de propriedade não podem ser compartilhadas por vários proprietários (esse é um cenário bem conhecido para objetos de valor não pode ser implementado usando tipos de entidade de propriedade)

### <a name="by-design-restrictions"></a>Restrições de design
- Não é possível criar um `DbSet<T>`
- Você não pode chamar `Entity<T>()` com um tipo de propriedade em `ModelBuilder`
