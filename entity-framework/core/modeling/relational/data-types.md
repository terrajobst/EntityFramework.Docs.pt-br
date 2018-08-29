---
title: Tipos de dados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993515"
---
# <a name="data-types"></a>Tipos de Dados

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Tipo de dados refere-se para o tipo específico de banco de dados da coluna à qual uma propriedade é mapeada.

## <a name="conventions"></a>Convenções

Por convenção, o provedor de banco de dados seleciona um tipo de dados com base no tipo CLR da propriedade. Ele também leva em consideração outros metadados, como configurado [comprimento máximo](../max-length.md), se a propriedade é parte de uma chave primária, etc.

Por exemplo, o SQL Server usa `datetime2(7)` para `DateTime` propriedades, e `nvarchar(max)` para `string` propriedades (ou `nvarchar(450)` para `string` propriedades que são usadas como uma chave).

## <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para especificar um tipo de dados exato para uma coluna.

Por exemplo o código a seguir configura `Url` como uma cadeia de caracteres com comprimento máximo de não-unicode `200` e `Rating` como decimal com precisão de `5` e dimensionar de `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API fluente

Você também pode usar a API Fluent para especificar os mesmos tipos de dados para as colunas.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
