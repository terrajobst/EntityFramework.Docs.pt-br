---
title: Propriedade tipos de entidade – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333792"
---
# <a name="owned-entity-types"></a>Tipos de entidade própria

>[!NOTE]
> Esse recurso é novo no EF Core 2.0.

O EF Core permite que você a tipos de entidade de modelo que só podem aparecer nas propriedades de navegação de outros tipos de entidade. Eles são chamados _tipos de entidade de propriedade_. É a entidade que contém um tipo de entidade própria seus _proprietário_.

## <a name="explicit-configuration"></a>Configuração explícita

Propriedade da entidade tipos nunca são incluídos pelo EF Core em modelo por convenção. Você pode usar o `OwnsOne` método no `OnModelCreating` ou anotar o tipo com `OwnedAttribute` (novo no EF Core 2.1) para configurar o tipo como um tipo de propriedade.

Neste exemplo, `StreetAddress` é um tipo com nenhuma propriedade de identidade. Ele é usado como uma propriedade do tipo Ordem para especificar o endereço para entrega para uma ordem específica.

Podemos usar o `OwnedAttribute` tratá-la como uma entidade de propriedade quando referenciados de outro tipo de entidade:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Também é possível usar o `OwnsOne` método no `OnModelCreating` para especificar que o `ShippingAddress` propriedade é uma entidade de propriedade do `Order` tipo de entidade e configurar aspectos adicionais, se necessário.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Se o `ShippingAddress` propriedade é particular na `Order` tipo, você pode usar a versão de cadeia de caracteres do `OwnsOne` método:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Consulte a [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) para obter mais contexto. 

## <a name="implicit-keys"></a>Chaves implícitas

Configurado com tipos de propriedade `OwnsOne` ou descobertos por meio de uma navegação de referência sempre têm uma relação um para um com o proprietário, portanto eles não precisam seus próprios valores de chave que os valores de chave estrangeiros são exclusivos. No exemplo anterior, o `StreetAddress` tipo não precisa definir uma propriedade de chave.  

Para entender como o EF Core controla esses objetos, é útil pensar que uma chave primária é criada como um [propriedade de sombra](xref:core/modeling/shadow-properties) para o tipo próprio. O valor da chave de uma instância do tipo próprio será igual ao valor da chave da instância do proprietário.

## <a name="collections-of-owned-types"></a>Coleções de tipos próprios

>[!NOTE]
> Este recurso é novo no EF Core 2.2.

Para configurar uma coleção de tipos próprios `OwnsMany` deve ser usado em `OnModelCreating`. No entanto a chave primária não será configurada por convenção, portanto, ele precisa ser especificado explicitamente. É comum usar uma chave complexa para esses tipos de entidades, incorporando a chave estrangeira para o proprietário e uma propriedade adicional exclusiva que também pode estar no estado de sombra:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>Tipos com a divisão de tabela de mapeamento de propriedade

Ao usar relacional bancos de dados, por referência de convenção de propriedade tipos são mapeados para a mesma tabela como o proprietário. Isso exige a dividir a tabela em dois: algumas colunas serão usadas para armazenar os dados do proprietário, e algumas colunas serão usadas para armazenar dados de entidade de propriedade. Esse é um recurso comum conhecido como a divisão de tabela.

> [!TIP]
> Propriedade armazenados com a divisão de tabela de tipos podem ser usada de forma semelhante a como os tipos complexos são usados no EF6.

Por convenção, o EF Core nomeie as colunas de banco de dados para as propriedades do tipo de entidade própria seguindo o padrão _Navigation_OwnedEntityProperty_. Portanto o `StreetAddress` propriedades serão exibidas na tabela "Pedidos" com os nomes 'ShippingAddress_Street' e 'ShippingAddress_City'.

Você pode acrescentar o `HasColumnName` método para renomear as colunas:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Compartilhando o mesmo tipo de .NET entre vários tipos de propriedade

Um tipo de entidade própria pode ser do mesmo tipo de .NET como outro tipo de entidade de propriedade, portanto, que o tipo do .NET pode não ser suficiente para identificar um tipo de propriedade.

Nesses casos, a propriedade apontando do proprietário para a entidade de propriedade se torna a _definir a navegação_ do tipo de entidade de propriedade. Da perspectiva do EF Core, a navegação de definição é parte da identidade do tipo juntamente com o tipo .NET.   

Por exemplo, na classe `ShippingAddress` e `BillingAddress` forem do mesmo tipo de .NET, `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Para compreender como o EF Core distinguirá controladas instâncias desses objetos, pode ser útil pensar que a navegação de definição tornou-se parte da chave da instância junto com o valor da chave do proprietário e o tipo de .NET do tipo próprio.

## <a name="nested-owned-types"></a>Tipos próprios aninhados

Neste exemplo `OrderDetails` possui `BillingAddress` e `ShippingAddress`, que são ambos `StreetAddress` tipos. Então `OrderDetails` pertence ao tipo `DetailedOrder`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Além dos tipos próprios aninhados, um tipo próprio pode fazer referência a uma entidade regular, ele pode ser o proprietário ou uma entidade diferente, desde que a entidade de propriedade é dependente do lado. Esse recurso define tipos de entidade própria além dos tipos complexos no EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

É possível encadear o `OwnsOne` método em uma chamada fluente para configurar este modelo:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Também é possível obter a mesma coisa usando `OwnedAttribute` em ambos `OrderDetails` e `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Armazenando tipos próprios em tabelas separadas

Também diferentemente dos tipos complexos do EF6, tipos de propriedade podem ser armazenados em uma tabela separada do proprietário. Para substituir a convenção que mapeia um tipo de propriedade para a mesma tabela como o proprietário, você pode simplesmente chamar `ToTable` e forneça um nome de tabela diferente. O exemplo a seguir serão mapeados `OrderDetails` e seus dois endereços para uma tabela separada do `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Consultando tipos próprios

Ao consultar o proprietário, os tipos próprios serão incluídos por padrão. Não é necessário usar o `Include` método, mesmo se os tipos próprios são armazenados em uma tabela separada. Com base no modelo descrito antes, a consulta a seguir obterá `Order`, `OrderDetails` e duas de propriedade `StreetAddresses` do banco de dados:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Limitações

Algumas dessas limitações são fundamentais para o trabalho de tipos de entidade como propriedade, mas outras são restrições que podemos pode ser capazes de ser removido em futuras versões:

### <a name="by-design-restrictions"></a>Restrições de design
- Não é possível criar um `DbSet<T>` para um tipo de propriedade
- Você não pode chamar `Entity<T>()` com um tipo de propriedade em `ModelBuilder`

### <a name="current-shortcomings"></a>Limitações atuais
- Não há suporte para tipos de entidade de propriedade de hierarquias de herança que incluem
- Propriedade de navegações de referência para tipos de entidade não podem ser nulos, a menos que eles estejam explicitamente mapeados em uma tabela diferente do proprietário
- Instâncias de tipos de entidade de propriedade não podem ser compartilhadas por vários proprietários (esse é um cenário bem conhecido para objetos de valor não pode ser implementado usando tipos de entidade de propriedade)

### <a name="shortcomings-in-previous-versions"></a>Limitações nas versões anteriores
- No EF Core 2.0, de propriedade navegações para tipos de entidade não podem ser declarados em tipos de entidade derivada, a menos que as entidades de propriedade estejam explicitamente mapeadas em uma tabela diferente da hierarquia de proprietário. Essa limitação foi removida no EF Core 2.1
- No EF Core 2.0 e 2.1 somente a referência navegações para tipos próprios tinham suporte. Essa limitação foi removida no EF Core 2.2
