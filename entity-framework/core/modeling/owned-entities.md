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
# <a name="owned-entity-types"></a>Tipos de entidade de propriedade

>[!NOTE]
> Este recurso é novo no EF Core 2.0.

EF Core permite tipos de entidade de modelo só podem aparecer em Propriedades de navegação de outros tipos de entidade. Esses são chamados de _tipos de entidade de propriedade_. A entidade que contém um tipo de entidade de propriedade é seu _proprietário_.

## <a name="explicit-configuration"></a>Configuração explícita

Propriedade da entidade tipos nunca são incluídos por núcleo EF em modelo por convenção. Você pode usar o `OwnsOne` método `OnModelCreating` ou anotar o tipo com `OwnedAttrbibute` (novo no EF Core 2.1) para configurar o tipo como um tipo de propriedade.

Neste exemplo, StreetAddress é um tipo com nenhuma propriedade de identidade. Ele é usado como uma propriedade do tipo Ordem para especificar o endereço para entrega para uma ordem específica. Em `OnModelCreating`, usamos o `OwnsOne` método para especificar que a propriedade ShippingAddress é uma entidade de propriedade do tipo de ordem.

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

Se a propriedade ShippingAddress é particular no tipo de ordem, você pode usar a versão de cadeia de caracteres do `OwnsOne` método:

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

No EF Core 2.0 e 2.1, somente propriedades de navegação de referência podem apontar para os tipos de propriedade. Não há suporte para coleções de tipos de propriedade. Esses referência de propriedade tipos sempre têm uma relação um para um com o proprietário, portanto, não precisam seus próprios valores de chave. No exemplo anterior, o tipo de StreetAddress não precisa definir uma propriedade de chave.  

Para entender como o EF Core controla esses objetos, é útil pensar que uma chave primária é criada como um [sombra propriedade](xref:core/modeling/shadow-properties) para o tipo de propriedade. O valor da chave de uma instância do tipo de propriedade será o mesmo que o valor da chave de instância do proprietário.      

## <a name="mapping-owned-types-with-table-splitting"></a>Mapeamento de tipos com a divisão da tabela de propriedade

Ao usar bancos de dados relacionais, por convenção, tipos de propriedade são mapeados para a mesma tabela como o proprietário. Isso requer dividir a tabela em duas: algumas colunas serão usadas para armazenar os dados do proprietário e algumas colunas serão usadas para armazenar dados de entidade de propriedade. Este é um recurso comum conhecido como divisão de tabela.

> [!TIP]
> Propriedade tipos armazenados com a divisão de tabela podem ser usado de forma muito similar a tipos complexos como são usados em EF6.

Por convenção, EF Core nomear as colunas de banco de dados para as propriedades do tipo de propriedade de entidade seguindo o padrão _EntityProperty_OwnedEntityProperty_. Portanto, as propriedades de StreetAddress aparecerão na tabela Orders com os nomes ShippingAddress_Street e ShippingAddress_City.

Você pode acrescentar o `HasColumnName` método renomear essas colunas. No caso onde StreetAddress é uma propriedade pública, os mapeamentos seria

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Compartilhando o mesmo tipo de .NET entre os vários tipos de propriedade

Um tipo de entidade de propriedade pode ser do mesmo tipo de .NET como outro tipo de entidade de propriedade, por isso que o tipo de .NET pode não ser suficientes para identificar um tipo de propriedade.

Nesses casos, a propriedade apontando do proprietário para a entidade de propriedade torna-se a _definindo navegação_ do tipo de propriedade de entidade. Da perspectiva do EF Core, a navegação de definição faz parte da identidade do tipo junto com o tipo .NET.   

Por exemplo, na classe a seguir, ShippingAddress e BillingAddress são do mesmo tipo .NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Para entender como o EF Core distinguirá controladas instâncias desses objetos, pode ser útil considerar que a definição de navegação se torna parte da chave de instância junto com o valor da chave do proprietário e o tipo de .NET do tipo de propriedade.

## <a name="nested-owned-types"></a>Tipos aninhados de propriedade

Neste exemplo OrderDetails possui BillingAddress e ShippingAddress, que são os dois tipos de StreetAddress. Em seguida, OrderDetails pertence o tipo de ordem.

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

É possível para a cadeia de `OwnsOne` método em um mapeamento fluente para configurar este modelo:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

É possível obter a mesma coisa usando `OwnedAttribute` OrderDetails e StreetAdress.

Além dos tipos de propriedade aninhados, um tipo de propriedade pode fazer referência a uma entidade normal. No exemplo a seguir, o país for uma entidade normal (ou seja, não é proprietário):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Este recurso define tipos de entidade de propriedade além de tipos complexos no EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Armazenando tipos de propriedade em tabelas separadas

Também ao contrário de tipos complexos de EF6, tipos de propriedade podem ser armazenados em uma tabela separada do proprietário. Para substituir a convenção de que um tipo de propriedade é mapeado para a mesma tabela como o proprietário, você pode simplesmente chamar `ToTable` e forneça um nome de tabela diferente. O exemplo a seguir mapeará OrderDetails e seus dois endereços em uma tabela diferente da ordem:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Consultando os tipos de propriedade

Ao consultar o proprietário, os tipos próprios serão incluídos por padrão. Não é necessário usar o `Include` método, mesmo se os tipos de propriedade são armazenados em uma tabela separada. Com base no modelo descrito antes, a consulta a seguir extrairá ordem, OrderDetails e os dois StreeAddresses corporativos para todos os pedidos pendentes do banco de dados:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Limitações

Aqui estão algumas limitações de tipos de entidade de propriedade. Algumas dessas limitações são fundamentais para o trabalho de tipos como proprietários, mas outras são as restrições de point-in-time que desejamos remover em futuras versões:

### <a name="current-shortcomings"></a>Limitações atuais
- Não há suporte para a herança dos tipos de propriedade
- Tipos de propriedade não podem ser apontados por uma propriedade de navegação de coleção
- Desde que eles usam a tabela dividindo pelo padrão de propriedade tipos também têm as seguintes restrições, a menos que explicitamente mapeados em uma tabela diferente:
   - Eles não podem pertencer a um tipo derivado
   - A propriedade de navegação definição não pode ser definida como null (ou seja, propriedade de tipos na mesma tabela sempre são necessários)

### <a name="by-design-restrictions"></a>Por design, restrições
- Não é possível criar um `DbSet<T>`
- Não é possível chamar `Entity<T>()` com um tipo de propriedade no `ModelBuilder`
