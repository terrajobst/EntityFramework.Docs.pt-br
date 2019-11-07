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
# <a name="owned-entity-types"></a>Tipos de entidade de propriedade

> [!NOTE]
> Esse recurso é novo no EF Core 2,0.

EF Core permite que você modele tipos de entidade que só podem aparecer em Propriedades de navegação de outros tipos de entidade. Eles são chamados de _tipos de entidade de propriedade_. A entidade que contém um tipo de entidade de propriedade é seu _proprietário_.

As entidades de propriedade são essencialmente uma parte do proprietário e não podem existir sem ela, elas são conceitualmente semelhantes às [agregações](https://martinfowler.com/bliki/DDD_Aggregate.html).

## <a name="explicit-configuration"></a>Configuração explícita

Tipos de entidade de propriedade nunca são incluídos por EF Core no modelo por convenção. Você pode usar o método `OwnsOne` em `OnModelCreating` ou anotar o tipo com `OwnedAttribute` (novo no EF Core 2,1) para configurar o tipo como um tipo de propriedade.

Neste exemplo, `StreetAddress` é um tipo sem nenhuma propriedade de identidade. Ele é usado como uma propriedade do tipo Ordem para especificar o endereço para entrega para uma ordem específica.

Podemos usar o `OwnedAttribute` para tratá-lo como uma entidade de propriedade quando referenciado de outro tipo de entidade:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Também é possível usar o método `OwnsOne` em `OnModelCreating` para especificar que a propriedade `ShippingAddress` é uma entidade de Propriedade do tipo de entidade `Order` e configurar facetas adicionais, se necessário.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Se a propriedade `ShippingAddress` for particular no tipo `Order`, você poderá usar a versão de cadeia de caracteres do método `OwnsOne`:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Consulte o [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) para obter mais contexto.

## <a name="implicit-keys"></a>Chaves implícitas

Tipos de propriedade configurados com `OwnsOne` ou descobertos por meio de uma navegação de referência sempre têm uma relação um-para-um com o proprietário, portanto, eles não precisam de seus próprios valores de chave, pois os valores de chave estrangeira são exclusivos. No exemplo anterior, o tipo de `StreetAddress` não precisa definir uma propriedade de chave.  

Para entender como o EF Core controla esses objetos, é útil saber que uma chave primária é criada como uma [propriedade de sombra](xref:core/modeling/shadow-properties) para o tipo de propriedade. O valor da chave de uma instância do tipo de propriedade será igual ao valor da chave da instância do proprietário.

## <a name="collections-of-owned-types"></a>Coleções de tipos de propriedade

> [!NOTE]
> Este recurso é novo no EF Core 2.2.

Para configurar uma coleção de tipos de propriedade, use `OwnsMany` em `OnModelCreating`.

Tipos de propriedade precisam de uma chave primária. Se não houver boas propriedades de candidatos no tipo .NET, EF Core poderá tentar criar uma. No entanto, quando tipos de propriedade são definidos por meio de uma coleção, não é suficiente apenas criar uma propriedade Shadow para atuar como a chave estrangeira no proprietário e a chave primária da instância de propriedade, como fazemos para `OwnsOne`: pode haver várias instâncias de tipo de propriedade para cada proprietário e, portanto, a chave do proprietário não é suficiente para fornecer uma identidade exclusiva para cada instância de propriedade.

As duas soluções mais simples para isso são:

- Definir uma chave primária substituta em uma nova propriedade independente da chave estrangeira que aponta para o proprietário. Os valores contidos precisariam ser exclusivos em todos os proprietários (por exemplo, se o pai {1} tiver {1}filho, o pai {2} não poderá ter {1}filho), portanto, o valor não terá nenhum significado inerente. Como a chave estrangeira não faz parte da chave primária, seus valores podem ser alterados e, portanto, você pode mover um filho de um pai para outro, no entanto, isso geralmente se aplica à semântica agregada.
- Usando a chave estrangeira e uma propriedade adicional como uma chave composta. O valor da propriedade adicional agora só precisa ser exclusivo para um determinado pai (portanto, se o {1} pai tiver filho {1,1} o pai {2} ainda poderá ter {2,1}filho). Ao tornar a parte da chave estrangeira da chave primária, a relação entre o proprietário e a entidade de propriedade torna-se imutável e reflete melhor a semântica agregada. Isso é o que EF Core faz por padrão.

Neste exemplo, usaremos a classe `Distributor`:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Por padrão, a chave primária usada para o tipo de propriedade referenciada por meio da propriedade de navegação `ShippingCenters` será `("DistributorId", "Id")` em que `"DistributorId"` é o FK e `"Id"` é um valor de `int` exclusivo.

Para configurar uma chamada de CP diferente `HasKey`:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> Antes de EF Core 3,0 `WithOwner()` método não existe, portanto, essa chamada deve ser removida.

## <a name="mapping-owned-types-with-table-splitting"></a>Mapeando tipos de propriedade com divisão de tabela

Ao usar bancos de dados relacionais, por padrão, os tipos de propriedade são mapeados para a mesma tabela que o proprietário. Isso requer a divisão da tabela em duas: algumas colunas serão usadas para armazenar os dados do proprietário e algumas colunas serão usadas para armazenar dados da entidade de propriedade. Esse é um recurso comum conhecido como [divisão de tabela](table-splitting.md).

Por padrão, EF Core nomeará as colunas do banco de dados para as propriedades do tipo de entidade de propriedade seguindo o padrão _Navigation_OwnedEntityProperty_. Portanto, as propriedades de `StreetAddress` serão exibidas na tabela ' Orders ' com os nomes ' ShippingAddress_Street ' e ' ShippingAddress_City '.

Você pode usar o método `HasColumnName` para renomear essas colunas:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Compartilhando o mesmo tipo .NET entre vários tipos de propriedade

Um tipo de entidade de propriedade pode ser do mesmo tipo .NET que outro tipo de entidade de propriedade, portanto, o tipo .NET pode não ser suficiente para identificar um tipo de propriedade.

Nesses casos, a propriedade que aponta do proprietário para a entidade de propriedade se torna a _definição da navegação_ do tipo de entidade de propriedade. Da perspectiva do EF Core, a definição da navegação faz parte da identidade do tipo junto com o tipo .NET.

Por exemplo, na classe a seguir `ShippingAddress` e `BillingAddress` são do mesmo tipo .NET, `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Para entender como EF Core irá distinguir as instâncias rastreadas desses objetos, pode ser útil imaginar que a definição de navegação se tornou parte da chave da instância, junto com o valor da chave do proprietário e o tipo de .NET do tipo de propriedade.

## <a name="nested-owned-types"></a>Tipos de propriedade aninhada

Neste exemplo `OrderDetails` possui `BillingAddress` e `ShippingAddress`, que são ambos os tipos de `StreetAddress`. Então `OrderDetails` pertence ao tipo `DetailedOrder`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Além dos tipos de propriedade aninhados, um tipo de propriedade pode fazer referência a uma entidade regular, pode ser o proprietário ou uma entidade diferente, desde que a entidade de propriedade esteja no lado dependente. Esse recurso define tipos de entidade pertencentes de tipos complexos em EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

É possível encadear o método `OwnsOne` em uma chamada fluente para configurar esse modelo:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Observe a chamada `WithOwner` usada para configurar a propriedade de navegação apontando de volta ao proprietário.

É possível obter o resultado usando `OwnedAttribute` em `OrderDetails` e `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Armazenando tipos de propriedade em tabelas separadas

Além disso, ao contrário dos tipos complexos EF6, tipos de propriedade podem ser armazenados em uma tabela separada do proprietário. Para substituir a Convenção que mapeia um tipo de propriedade para a mesma tabela que o proprietário, você pode simplesmente chamar `ToTable` e fornecer um nome de tabela diferente. O exemplo a seguir mapeará `OrderDetails` e seus dois endereços para uma tabela separada de `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Consultando tipos de propriedade

Ao consultar o proprietário, os tipos próprios serão incluídos por padrão. Não é necessário usar o método `Include`, mesmo que os tipos de propriedade sejam armazenados em uma tabela separada. Com base no modelo descrito anteriormente, a consulta a seguir obterá `Order`, `OrderDetails` e as duas `StreetAddresses` de Propriedade do banco de dados:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Limitações

Algumas dessas limitações são fundamentais para a forma como os tipos de entidade de propriedade funcionam, mas algumas outras são restrições que podem ser capazes de remover em versões futuras:

### <a name="by-design-restrictions"></a>Restrições por design

- Não é possível criar um `DbSet<T>` para um tipo de propriedade
- Você não pode chamar `Entity<T>()` com um tipo de propriedade em `ModelBuilder`

### <a name="current-shortcomings"></a>Deficiências atuais

- Não há suporte para hierarquias de herança que incluem tipos de entidade de propriedade
- Navegações de referência para tipos de entidade pertencentes não podem ser nulas, a menos que sejam explicitamente mapeados para uma tabela separada do proprietário
- Instâncias de tipos de entidade de propriedade não podem ser compartilhadas por vários proprietários (este é um cenário bem conhecido para objetos de valor que não podem ser implementados usando tipos de entidade de propriedade)

### <a name="shortcomings-in-previous-versions"></a>Limitações em versões anteriores

- No EF Core 2,0, navegações para tipos de entidade de propriedade não podem ser declaradas em tipos de entidade derivadas, a menos que as entidades de propriedade sejam explicitamente mapeadas para uma tabela separada da hierarquia de proprietário. Essa limitação foi removida no EF Core 2,1
- No EF Core 2,0 e 2,1, somente navegações de referência a tipos de propriedade eram compatíveis. Essa limitação foi removida no EF Core 2,2
