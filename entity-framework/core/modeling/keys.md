---
title: Chaves-EF Core
description: Como configurar chaves para tipos de entidade ao usar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416463"
---
# <a name="keys"></a>simétricas

Uma chave serve como um identificador exclusivo para cada instância de entidade. A maioria das entidades no EF tem uma única chave, que é mapeada para o conceito de uma *chave primária* em bancos de dados relacionais (para entidades sem chaves, consulte [entidades inferiores](xref:core/modeling/keyless-entity-types)). As entidades podem ter chaves adicionais além da chave primária (consulte [chaves alternativas](#alternate-keys) para obter mais informações).

Por convenção, uma propriedade chamada `Id` ou `<type name>Id` será configurada como a chave primária de uma entidade.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> [Tipos de entidade pertencentes](xref:core/modeling/owned-entities) usam regras diferentes para definir chaves.

Você pode configurar uma única propriedade para ser a chave primária de uma entidade da seguinte maneira:

## <a name="data-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

Você também pode configurar várias propriedades para ser a chave de uma entidade – isso é conhecido como uma chave composta. As chaves compostas só podem ser configuradas usando a API Fluent; as convenções nunca configurarão uma chave composta e você não poderá usar as anotações de dados para configurar uma.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a>Nome da chave primária

Por convenção, em bancos de dados relacionais, as chaves primárias são criadas com o nome `PK_<type name>`. Você pode configurar o nome da restrição PRIMARY KEY da seguinte maneira:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a>Tipos de chave e valores

Embora EF Core ofereça suporte ao uso de propriedades de qualquer tipo primitivo como chave primária, incluindo `string`, `Guid`, `byte[]` e outros, nem todos os bancos de dados oferecem suporte a todos os tipos como chaves. Em alguns casos, os valores de chave podem ser convertidos em um tipo com suporte automaticamente; caso contrário, a conversão deve ser [especificada manualmente](xref:core/modeling/value-conversions).

As propriedades de chave sempre devem ter um valor não padrão ao adicionar uma nova entidade ao contexto, mas alguns tipos serão [gerados pelo banco de dados](xref:core/modeling/generated-properties). Nesse caso, o EF tentará gerar um valor temporário quando a entidade for adicionada para fins de acompanhamento. Depois que [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) for chamado, o valor temporário será substituído pelo valor gerado pelo banco de dados.

> [!Important]
> Se uma propriedade de chave tiver seu valor gerado pelo banco de dados e um valor não padrão for especificado quando uma entidade for adicionada, o EF irá pressupor que a entidade já existe no banco de dados e tentará atualizá-la em vez de inserir uma nova. Para evitar isso, desative a geração de valor ou veja [como especificar valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).

## <a name="alternate-keys"></a>Chaves alternativas

Uma chave alternativa serve como um identificador exclusivo alternativo para cada instância de entidade, além da chave primária; Ele pode ser usado como o destino de uma relação. Ao usar um banco de dados relacional, ele é mapeado para o conceito de um índice/restrição exclusivo nas colunas de chave alternativas e uma ou mais restrições Foreign Key que fazem referência às colunas.

> [!TIP]
> Se você quiser apenas impor a exclusividade em uma coluna, defina um índice exclusivo em vez de uma chave alternativa (consulte [índices](indexes.md)). No EF, chaves alternativas são somente leitura e fornecem semântica adicional sobre índices exclusivos, pois eles podem ser usados como o destino de uma chave estrangeira.

As chaves alternativas são normalmente introduzidas quando necessário e você não precisa configurá-las manualmente. Por convenção, uma chave alternativa é introduzida quando você identifica uma propriedade que não é a chave primária como o destino de uma relação.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

Você também pode configurar uma única propriedade para ser uma chave alternativa:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

Você também pode configurar várias propriedades para ser uma chave alternativa (conhecida como uma chave alternativa composta):

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

Por fim, por convenção, o índice e a restrição que são introduzidos para uma chave alternativa serão nomeados `AK_<type name>_<property name>` (para chaves alternativas compostas `<property name>` se tornará uma lista de nomes de propriedades separada por sublinhado). Você pode configurar o nome do índice da chave alternativa e a restrição UNIQUE:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
