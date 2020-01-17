---
title: Tokens de simultaneidade-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: bfeb611f222f7195fe22d920b452b40cc4addf90
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124360"
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidade

> [!NOTE]
> Esta página documenta como configurar tokens de simultaneidade. Consulte [lidando com conflitos de simultaneidade](../saving/concurrency.md) para obter uma explicação detalhada de como o controle de simultaneidade funciona em EF Core e exemplos de como lidar com conflitos de simultaneidade em seu aplicativo.

Propriedades configuradas como tokens de simultaneidade são usadas para implementar o controle de simultaneidade otimista.

## <a name="configuration"></a>Configuração do

### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Carimbo de data/hora/versão do

Um timestamp/rowgroup é uma propriedade para a qual um novo valor é gerado automaticamente pelo banco de dados toda vez que uma linha é inserida ou atualizada. A propriedade também é tratada como um token de simultaneidade, garantindo que você receba uma exceção se uma linha que está sendo atualizada foi alterada desde que você a consultou. Os detalhes precisos dependem do provedor de banco de dados que está sendo usado; por SQL Server, uma propriedade *byte []* geralmente é usada, que será configurada *como uma coluna de série* do banco de dados.

Você pode configurar uma propriedade para ser um carimbo de data/hora/uma versão da seguinte maneira:

### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs?name=Timestamp&highlight=9,17)]

***
