---
title: Fazendo backup de campos-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124373"
---
# <a name="backing-fields"></a>Campos de backup

Os campos de backup permitem que o EF Leia e/ou grave em um campo em vez de em uma propriedade. Isso pode ser útil quando o encapsulamento na classe está sendo usado para restringir o uso de e/ou aprimorar a semântica em relação ao acesso aos dados por código do aplicativo, mas o valor deve ser lido e/ou gravado no Database sem usar essas restrições/aprimoramentos.

## <a name="basic-configuration"></a>Configuração básica

Por convenção, os campos a seguir serão descobertos como campos de apoio para uma determinada propriedade (listada em ordem de precedência). 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

No exemplo a seguir, a propriedade `Url` está configurada para ter `_url` como seu campo de apoio:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Observe que os campos de apoio são descobertos apenas para propriedades incluídas no modelo. Para obter mais informações sobre quais propriedades são incluídas no modelo, consulte [incluindo & excluindo Propriedades](included-properties.md).

Você também pode configurar os campos de backup explicitamente, por exemplo, se o nome do campo não corresponder às convenções acima:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a>Acesso de campo e Propriedade

Por padrão, o EF sempre lerá e gravará no campo de backup-supondo que um tenha sido configurado corretamente-e nunca usará a propriedade. No entanto, o EF também oferece suporte a outros padrões de acesso. Por exemplo, o exemplo a seguir instrui o EF a gravar no campo de backup somente durante a materialização e a usar a propriedade em todos os outros casos:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

Consulte a [Enumeração PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para obter o conjunto completo de opções com suporte.

> [!NOTE]
> Com o EF Core 3,0, o modo de acesso de propriedade padrão mudou de `PreferFieldDuringConstruction` para `PreferField`.

## <a name="field-only-properties"></a>Propriedades somente de campo

Você também pode criar uma propriedade conceitual em seu modelo que não tem uma propriedade CLR correspondente na classe de entidade, mas, em vez disso, usa um campo para armazenar os dados na entidade. Isso é diferente das [Propriedades de sombra](shadow-properties.md), em que os dados são armazenados no controlador de alterações, em vez de no tipo CLR da entidade. As propriedades somente de campo são normalmente usadas quando a classe de entidade usa métodos em vez de propriedades para obter/definir valores, ou em casos em que os campos não devem ser expostos no modelo de domínio (por exemplo, chaves primárias).

Você pode configurar uma propriedade somente de campo fornecendo um nome na API de `Property(...)`:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

O EF tentará localizar uma propriedade CLR com o nome fornecido ou um campo se uma propriedade não for encontrada. Se nem uma propriedade nem um campo forem encontrados, uma propriedade Shadow será configurada em vez disso.

Talvez seja necessário referir-se a uma propriedade somente de campo de consultas LINQ, mas esses campos normalmente são privados. Você pode usar o método `EF.Property(...)` em uma consulta LINQ para se referir ao campo:

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
