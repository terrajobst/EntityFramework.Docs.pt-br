---
title: Tipos de dados - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053496"
---
# <a name="data-types"></a>Tipos de dados

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).

Tipo de dados refere-se para o tipo específico de banco de dados da coluna à qual uma propriedade está mapeada.

## <a name="conventions"></a>Convenções

Por convenção, o provedor de banco de dados seleciona um tipo de dados com base no tipo CLR da propriedade. Ele também leva em conta outros metadados, como configurado [comprimento máximo](../max-length.md), se a propriedade é parte de uma chave primária, etc.

Por exemplo, o SQL Server usa `datetime2(7)` para `DateTime` propriedades, e `nvarchar(max)` para `string` propriedades (ou `nvarchar(450)` para `string` propriedades que são usadas como uma chave).

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para especificar um tipo de dados exato de uma coluna.

Por exemplo de código a seguir configura `Url` como uma cadeia de caracteres não unicode com um comprimento máximo de `200` e `Rating` como decimal com precisão de `5` e a escala de `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API fluente

Você também pode usar a API fluente para especificar os mesmos tipos de dados para as colunas.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
