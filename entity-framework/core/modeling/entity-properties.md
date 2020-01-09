---
title: Propriedades da entidade-EF Core
description: Como configurar e mapear Propriedades de entidade usando Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502463"
---
# <a name="entity-properties"></a>Propriedades da Entidade

Cada tipo de entidade em seu modelo tem um conjunto de propriedades, que EF Core lerá e gravará no banco de dados. Se você estiver usando um banco de dados relacional, as propriedades de entidade serão mapeadas para colunas de tabela.

## <a name="included-and-excluded-properties"></a>Propriedades incluídas e excluídas

Por convenção, todas as propriedades públicas com um getter e um setter serão incluídas no modelo.

Propriedades específicas podem ser excluídas da seguinte maneira:

### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a>Nomes de coluna

Por convenção, ao usar um banco de dados relacional, as propriedades de entidade são mapeadas para colunas de tabela com o mesmo nome da propriedade.

Se preferir configurar suas colunas com nomes diferentes, você poderá fazer isso da seguinte maneira:

### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a>Tipos de dados de coluna

Ao usar um banco de dados relacional, o provedor de banco de dados seleciona um tipo de dado com base no tipo .NET da propriedade. Ele também leva em conta outros metadados, como o [comprimento máximo](#maximum-length)configurado, se a propriedade faz parte de uma chave primária, etc.

Por exemplo, SQL Server mapeia `DateTime` Propriedades para `datetime2(7)` colunas e `string` Propriedades para `nvarchar(max)` colunas (ou `nvarchar(450)` para propriedades que são usadas como uma chave).

Você também pode configurar suas colunas para especificar um tipo de dados exato para uma coluna. Por exemplo, o código a seguir configura `Url` como uma cadeia de caracteres não-Unicode com comprimento máximo de `200` e `Rating` como decimal com precisão de `5` e escala de `2`:

### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a>Comprimento máximo

A configuração de um comprimento máximo fornece uma dica ao provedor de banco de dados sobre o tipo de dado de coluna apropriado a ser escolhido para uma determinada propriedade. O comprimento máximo só se aplica a tipos de dados de matriz, como `string` e `byte[]`.

> [!NOTE]
> Entity Framework não realiza nenhuma validação de comprimento máximo antes de passar os dados para o provedor. Cabe ao provedor ou repositório de dados validar se apropriado. Por exemplo, ao direcionar SQL Server, exceder o comprimento máximo resultará em uma exceção, pois o tipo de dados da coluna subjacente não permitirá que os dados em excesso sejam armazenados.

No exemplo a seguir, a configuração de um comprimento máximo de 500 fará com que uma coluna do tipo `nvarchar(500)` seja criada em SQL Server:

#### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a>Propriedades obrigatórias e opcionais

Uma propriedade será considerada opcional se for válida para que ela contenha `null`. Se `null` não for um valor válido a ser atribuído a uma propriedade, então será considerada uma propriedade necessária. Ao mapear para um esquema de banco de dados relacional, as propriedades obrigatórias são criadas como colunas não anuláveis e as propriedades opcionais são criadas como colunas anuláveis.

### <a name="conventions"></a>Convenções

Por convenção, uma propriedade cujo tipo .NET pode conter NULL será configurada como opcional, enquanto as propriedades cujo tipo .NET não pode conter NULL serão configuradas conforme necessário. Por exemplo, todas as propriedades com tipos de valor do .NET (`int`, `decimal`, `bool`, etc.) são configuradas conforme necessário, e todas as propriedades com tipos de valor do .NET anuláveis (`int?`, `decimal?`, `bool?`, etc.) são configuradas como opcionais.

C#8 introduziu um novo recurso chamado [tipos de referência anulável](/dotnet/csharp/tutorials/nullable-reference-types), que permite que os tipos de referência sejam anotados, indicando se é válido para que eles contenham NULL ou não. Esse recurso está desabilitado por padrão e, se habilitado, ele modifica o comportamento do EF Core da seguinte maneira:

* Se tipos de referência anuláveis estiverem desabilitados (o padrão), todas as propriedades com tipos de referência do .NET serão configuradas como opcionais por convenção (por exemplo, `string`).
* Se os tipos de referência anuláveis estiverem habilitados, as propriedades serão C# configuradas com base na nulidade de seu tipo .net: `string?` será configurado como opcional, enquanto `string` será configurada conforme necessário.

O exemplo a seguir mostra um tipo de entidade com propriedades obrigatórias e opcionais, com o recurso de referência anulável desabilitado (o padrão) e habilitado:

#### <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Sem tipos de referência anuláveis (padrão)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-typestabwith-nrt"></a>[Com tipos de referência anuláveis](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

O uso de tipos de referência anuláveis é recomendado, pois ele flui C# a nulidade expressa em código para o modelo de EF Core e para o banco de dados e dispensa o uso da API fluente ou das anotações de data para expressar o mesmo conceito duas vezes.

> [!NOTE]
> Tenha cuidado ao habilitar tipos de referência anuláveis em um projeto existente: as propriedades do tipo de referência que foram previamente configuradas como opcionais agora serão configuradas conforme necessário, a menos que sejam anotadas explicitamente para permitir valor nulo. Ao gerenciar um esquema de banco de dados relacional, isso pode fazer com que as migrações sejam geradas, alterando a nulidade da coluna do banco de dados.

Para obter mais informações sobre tipos de referência anuláveis e como usá-los com EF Core, [consulte a página de documentação dedicada para esse recurso](xref:core/miscellaneous/nullable-reference-types).

### <a name="explicit-configuration"></a>Configuração explícita

Uma propriedade que seria opcional por convenção pode ser configurada para ser necessária da seguinte maneira:

#### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
