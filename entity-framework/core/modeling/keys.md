---
title: Chaves (primárias)-EF Core
description: Como configurar chaves para tipos de entidade ao usar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824612"
---
# <a name="keys-primary"></a>Chaves (primárias)

Uma chave serve como o identificador exclusivo primário para cada instância de entidade. Ao usar um banco de dados relacional, isso é mapeado para o conceito de uma *chave primária*. Você também pode configurar um identificador exclusivo que não seja a chave primária (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).

Um dos métodos a seguir pode ser usado para configurar/criar uma chave primária.

## <a name="conventions"></a>Convenções

Por padrão, uma propriedade chamada `Id` ou `<type name>Id` será configurada como a chave de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> [Tipos de entidade pertencentes](xref:core/modeling/owned-entities) usam regras diferentes para definir chaves.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar uma única propriedade para ser a chave de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma única propriedade para ser a chave de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Você também pode usar a API fluente para configurar várias propriedades para ser a chave de uma entidade (conhecida como chave composta). As chaves compostas só podem ser configuradas usando a API fluente – as convenções nunca instalarão uma chave composta e você não poderá usar as anotações de dados para configurar uma.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a>Tipos de chave e valores

O EF Core dá suporte ao uso de propriedades de qualquer tipo primitivo como chave primária, incluindo `string`, `Guid`, `byte[]` e outros. Mas nem todos os bancos de dados dão suporte a eles. Em alguns casos, os valores de chave podem ser convertidos em um tipo com suporte automaticamente; caso contrário, a conversão deve ser [especificada manualmente](xref:core/modeling/value-conversions).

As propriedades de chave sempre devem ter um valor não padrão ao adicionar uma nova entidade ao contexto, mas alguns tipos serão [gerados pelo banco de dados](xref:core/modeling/generated-properties). Nesse caso, o EF tentará gerar um valor temporário quando a entidade for adicionada para fins de acompanhamento. Depois que [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) for chamado, o valor temporário será substituído pelo valor gerado pelo banco de dados.

> [!Important]
> Se uma propriedade de chave tiver um valor gerado pelo banco de dados e um valor não padrão for especificado quando uma entidade for adicionada, o EF irá pressupor que a entidade já existe no banco de dados e tentará atualizá-la em vez de inserir uma nova. Para evitar isso, desative a geração de valor ou veja [como especificar valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).