---
title: Fazendo campos – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994087"
---
# <a name="backing-fields"></a>Campos de suporte

> [!NOTE]  
> Esse recurso é novo no EF Core 1.1.

Campos de suporte permitem que o EF para leitura e/ou gravar em um campo em vez de uma propriedade. Isso pode ser útil ao encapsulamento na classe está sendo usado para restringir o uso de e/ou aprimorar a semântica de acesso aos dados pelo código do aplicativo, mas o valor deve ser de leitura de e/ou gravado no banco de dados sem usar essas restrições / aprimoramentos.

## <a name="conventions"></a>Convenções

Por convenção, os campos a seguir serão descobertos como campos de suporte para uma determinada propriedade (listados em ordem de precedência). Campos só são descobertos para propriedades que são incluídas no modelo. Para obter mais informações no qual as propriedades são incluídas no modelo, consulte [incluir e excluir propriedades](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Quando um campo de backup é configurado, o EF gravará diretamente a esse campo quando materializar as instâncias de entidade do banco de dados (em vez de usar o setter da propriedade). Se o EF precisa ler ou gravar o valor em outros momentos, ele usará a propriedade se possível. Por exemplo, se o EF precisa para atualizar o valor para uma propriedade, ele usará o setter da propriedade se algum tiver sido definido. Se a propriedade é somente leitura, ele gravará para o campo.

## <a name="data-annotations"></a>Anotações de dados

Campos de backup não podem ser configurado com anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar um campo de suporte para uma propriedade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Controlar quando o campo é usado

Você pode configurar quando o EF usa o campo ou propriedade. Consulte a [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para as opções com suporte.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campos sem uma propriedade

Você também pode criar uma propriedade conceitual em seu modelo que não tem uma propriedade CLR correspondente na classe de entidade, mas em vez disso, usa um campo para armazenar os dados na entidade. Isso é diferente de [propriedades de sombra](shadow-properties.md), onde os dados são armazenados no Rastreador de alteração. Isso normalmente seria usado se a classe de entidade usa métodos para obter/definir valores.

Você pode dar o nome do campo no EF o `Property(...)` API. Se não houver nenhuma propriedade com o nome fornecido, o EF procurará um campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Você também pode optar por dar um nome diferente do nome do campo de propriedade. Esse nome é usado ao criar o modelo, mais notavelmente, ele será usado para o nome da coluna que é mapeado para o banco de dados.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Quando não há nenhuma propriedade na classe de entidade, você pode usar o `EF.Property(...)` método em uma consulta LINQ para fazer referência à propriedade que é conceitualmente parte do modelo.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
