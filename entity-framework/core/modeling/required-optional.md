---
title: Propriedades obrigatórias e opcionais-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655657"
---
# <a name="required-and-optional-properties"></a>Propriedades obrigatórias e opcionais

Uma propriedade será considerada opcional se for válida para que ela contenha `null`. Se `null` não for um valor válido a ser atribuído a uma propriedade, então será considerada uma propriedade necessária.

Ao mapear para um esquema de banco de dados relacional, as propriedades obrigatórias são criadas como colunas não anuláveis e as propriedades opcionais são criadas como colunas anuláveis.

## <a name="conventions"></a>Convenções

Por convenção, uma propriedade cujo tipo .NET pode conter NULL será configurada como opcional, enquanto as propriedades cujo tipo .NET não pode conter NULL serão configuradas conforme necessário. Por exemplo, todas as propriedades com tipos de valor do .NET (`int`, `decimal`, `bool`, etc.) são configuradas conforme necessário, e todas as propriedades com tipos de valor do .NET anuláveis (`int?`, `decimal?`, `bool?`, etc.) são configuradas como opcionais.

C#8 introduziu um novo recurso chamado [tipos de referência anulável](/dotnet/csharp/tutorials/nullable-reference-types), que permite que os tipos de referência sejam anotados, indicando se é válido para que eles contenham NULL ou não. Esse recurso está desabilitado por padrão e, se habilitado, ele modifica o comportamento do EF Core da seguinte maneira:

* Se tipos de referência anuláveis estiverem desabilitados (o padrão), todas as propriedades com tipos de referência do .NET serão configuradas como opcionais por convenção (por exemplo, `string`).
* Se os tipos de referência anuláveis estiverem habilitados, as propriedades serão C# configuradas com base na nulidade de seu tipo .net: `string?` será configurado como opcional, enquanto `string` será configurada conforme necessário.

O exemplo a seguir mostra um tipo de entidade com propriedades obrigatórias e opcionais, com o recurso de referência anulável desabilitado (o padrão) e habilitado:

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Sem tipos de referência anuláveis (padrão)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[Com tipos de referência anuláveis](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

O uso de tipos de referência anuláveis é recomendado, pois ele flui C# a nulidade expressa em código para o modelo de EF Core e para o banco de dados e dispensa o uso da API fluente ou das anotações de data para expressar o mesmo conceito duas vezes.

> [!NOTE]
> Tenha cuidado ao habilitar tipos de referência anuláveis em um projeto existente: as propriedades do tipo de referência que foram previamente configuradas como opcionais agora serão configuradas conforme necessário, a menos que sejam anotadas explicitamente para permitir valor nulo. Ao gerenciar um esquema de banco de dados relacional, isso pode fazer com que as migrações sejam geradas, alterando a nulidade da coluna do banco de dados.

Para obter mais informações sobre tipos de referência anuláveis e como usá-los com EF Core, [consulte a página de documentação dedicada para esse recurso](xref:core/miscellaneous/nullable-reference-types).

## <a name="configuration"></a>Configuração

Uma propriedade que seria opcional por convenção pode ser configurada para ser necessária da seguinte maneira:

# <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***
