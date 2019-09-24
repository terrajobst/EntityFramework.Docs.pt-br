---
title: Fazendo backup de campos-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197488"
---
# <a name="backing-fields"></a>Campos de backup

> [!NOTE]  
> Esse recurso é novo no EF Core 1,1.

Os campos de backup permitem que o EF Leia e/ou grave em um campo em vez de em uma propriedade. Isso pode ser útil quando o encapsulamento na classe está sendo usado para restringir o uso de e/ou aprimorar a semântica em relação ao acesso aos dados por código do aplicativo, mas o valor deve ser lido e/ou gravado no Database sem usar essas restrições/ aprimoramentos.

## <a name="conventions"></a>Convenções

Por convenção, os campos a seguir serão descobertos como campos de apoio para uma determinada propriedade (listada em ordem de precedência). Os campos são descobertos apenas para propriedades incluídas no modelo. Para obter mais informações sobre quais propriedades são incluídas no modelo, consulte [incluindo & excluindo Propriedades](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Quando um campo de backup for configurado, o EF gravará diretamente nesse campo ao materializar instâncias de entidade do banco de dados (em vez de usar o setter de propriedade). Se o EF precisar ler ou gravar o valor em outros momentos, ele usará a propriedade, se possível. Por exemplo, se o EF precisar atualizar o valor de uma propriedade, ele usará o setter de propriedade se um for definido. Se a propriedade for somente leitura, ela será gravada no campo.

## <a name="data-annotations"></a>Anotações de dados

Os campos de backup não podem ser configurados com anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar um campo de apoio para uma propriedade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Controlando quando o campo é usado

Você pode configurar quando o EF usa o campo ou a propriedade. Consulte a [Enumeração PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para obter as opções com suporte.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campos sem uma propriedade

Você também pode criar uma propriedade conceitual em seu modelo que não tem uma propriedade CLR correspondente na classe de entidade, mas, em vez disso, usa um campo para armazenar os dados na entidade. Isso é diferente das [Propriedades de sombra](shadow-properties.md), onde os dados são armazenados no controlador de alterações. Isso normalmente seria usado se a classe de entidade usa métodos para obter/definir valores.

Você pode dar ao EF o nome do campo na `Property(...)` API. Se não houver nenhuma propriedade com o nome fornecido, o EF procurará um campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

Você também pode optar por dar um nome à propriedade, diferente do nome do campo. Esse nome é usado ao criar o modelo, mais notavelmente, ele será usado para o nome de coluna que está mapeado no banco de dados.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

Quando não há nenhuma propriedade na classe de entidade, você pode usar o `EF.Property(...)` método em uma consulta LINQ para fazer referência à propriedade que é conceitualmente parte do modelo.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
