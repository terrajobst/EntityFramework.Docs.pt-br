---
title: Tokens de simultaneidade-EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197457"
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidade

> [!NOTE]
> Esta página documenta como configurar tokens de simultaneidade. Consulte [lidando com conflitos de simultaneidade](../saving/concurrency.md) para obter uma explicação detalhada de como o controle de simultaneidade funciona em EF Core e exemplos de como lidar com conflitos de simultaneidade em seu aplicativo.

Propriedades configuradas como tokens de simultaneidade são usadas para implementar o controle de simultaneidade otimista.

## <a name="conventions"></a>Convenções

Por convenção, as propriedades nunca são configuradas como tokens de simultaneidade.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar uma propriedade como um token de simultaneidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma propriedade como um token de simultaneidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versão do carimbo de data/hora/linha

Um carimbo de data/hora é uma propriedade em que um novo valor é gerado pelo banco de dados toda vez que uma linha é inserida ou atualizada. A propriedade também é tratada como um token de simultaneidade. Isso garante que você receberá uma exceção se outra pessoa tiver modificado uma linha que você está tentando atualizar desde que você consultou os dados.

A forma como isso é feito é o provedor de banco de dados que está sendo usado. Por SQL Server, o carimbo de data/hora geralmente é usado em uma propriedade *byte []* , que será configurada como uma coluna do *doversion* no banco de dados.

### <a name="conventions"></a>Convenções

Por convenção, as propriedades nunca são configuradas como carimbos de data/hora.

### <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para configurar uma propriedade como um carimbo de data/hora.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma propriedade como um carimbo de data/hora.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
