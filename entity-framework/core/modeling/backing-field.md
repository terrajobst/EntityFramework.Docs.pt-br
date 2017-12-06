---
title: Fazendo campos - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a>Campos de backup

> [!NOTE]  
> Esse recurso é novo no EF Core 1.1.

Os campos de backup permitem EF ler e/ou gravar em um campo em vez de uma propriedade. Isso pode ser útil quando o encapsulamento na classe está sendo usado para restringir o uso de e/ou melhorar a semântica de acesso aos dados pelo código do aplicativo, mas o valor deve ser de leitura de e/ou gravado no banco de dados sem usar essas restrições / aprimoramentos.

## <a name="conventions"></a>Convenções

Por convenção, os campos a seguir serão descobertos como fazendo campos para uma determinada propriedade (listados em ordem de precedência). Campos só são descobertos para propriedades que estão incluídas no modelo. Para obter mais informações no qual as propriedades são incluídas no modelo, consulte [incluindo e excluindo propriedades](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Quando um campo de backup é configurado, EF gravará diretamente a esse campo ao materializar instâncias da entidade do banco de dados (em vez de usar o setter da propriedade). Se precisar de EF ler ou gravar o valor em outras vezes, ele usará a propriedade se possível. Por exemplo, se o EF precisa atualizar o valor de uma propriedade, ele usará o setter da propriedade se um for definido. Se a propriedade é somente leitura, em seguida, ele gravará o campo.

## <a name="data-annotations"></a>Anotações de dados

Fazer campos não pode ser configurado com as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar um campo existente para uma propriedade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Controlar quando o campo é usado

Você pode configurar quando o EF usa o campo ou propriedade. Consulte o [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para as opções com suporte.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campos sem uma propriedade

Você também pode criar uma propriedade conceitual em seu modelo que não tem uma propriedade CLR correspondente na classe de entidade, mas em vez disso, usa um campo para armazenar os dados da entidade. Isso é diferente de [propriedades de sombra](shadow-properties.md), onde os dados são armazenados no controlador de alterações. Isso normalmente seria usado se a classe da entidade usa métodos para valores de get/set.

Você pode dar o nome do campo no EF o `Property(...)` API. Se não houver nenhuma propriedade com o nome fornecido, EF procurará um campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Você também pode optar por dar um nome diferente do nome do campo de propriedade. Esse nome é usado ao criar o modelo, mais notoriamente, ela será usada para o nome da coluna que é mapeado para o banco de dados.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Quando não há nenhuma propriedade na classe de entidade, você pode usar o `EF.Property(...)` método em uma consulta LINQ para referir-se a propriedade que conceitualmente faz parte do modelo.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
