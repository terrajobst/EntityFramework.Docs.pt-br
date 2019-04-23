---
title: Chaves (primárias) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929823"
---
# <a name="keys-primary"></a>Chaves (primárias)

Uma chave serve como o principal identificador exclusivo para cada instância de entidade. Ao usar um banco de dados relacional mapeia para o conceito de um *chave primária*. Você também pode configurar um identificador exclusivo que não é a chave primária (consulte [chaves alternativas](alternate-keys.md) para obter mais informações). 

Um dos métodos a seguir pode ser usado para instalação/criação de uma chave primária.

## <a name="conventions"></a>Convenções

Por convenção, uma propriedade chamada `Id` ou `<type name>Id` serão configurados como a chave de uma entidade.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para configurar uma única propriedade para ser a chave de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma única propriedade para ser a chave de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

Você também pode usar a API Fluent para configurar várias propriedades para ser a chave de uma entidade (conhecida como uma chave composta). Chaves compostas só podem ser configuradas usando a API Fluent - convenções nunca irá configurar uma chave composta, e você não pode usar anotações de dados para configurar um.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
