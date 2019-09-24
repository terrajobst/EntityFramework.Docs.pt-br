---
title: Tipos de dados-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 26664ebe18abcdeaa2b9c8dc23a6410204f53c8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197183"
---
# <a name="data-types"></a>Tipos de Dados

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

O tipo de dados refere-se ao tipo específico de Database da coluna para a qual uma propriedade é mapeada.

## <a name="conventions"></a>Convenções

Por convenção, o provedor de banco de dados seleciona um tipo de dado com base no tipo .NET da propriedade. Ele também leva em conta outros metadados, como o [comprimento máximo](../max-length.md)configurado, se a propriedade faz parte de uma chave primária, etc.

Por exemplo, SQL Server usa `datetime2(7)` para `DateTime` Propriedades e `nvarchar(max)` para `string` Propriedades (ou `nvarchar(450)` para `string` propriedades que são usadas como uma chave).

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para especificar um tipo de dados exato para uma coluna.

Por `Url` exemplo, o código a seguir configura como uma cadeia de caracteres não-Unicode com `200` comprimento máximo `Rating` e como decimal com precisão `5` e escala de `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API fluente

Você também pode usar a API fluente para especificar os mesmos tipos de dados para as colunas.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DataType.cs?name=Model&highlight=9-10)]
