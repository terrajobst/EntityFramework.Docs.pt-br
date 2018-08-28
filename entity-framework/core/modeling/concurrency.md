---
title: Tokens de simultaneidade – EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994220"
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidade

> [!NOTE]
> Esta página documenta como configurar os tokens de simultaneidade. Ver [tratamento de conflitos de simultaneidade](../saving/concurrency.md) para obter uma explicação detalhada de como funciona o controle de simultaneidade no EF Core e exemplos de como lidar com conflitos de simultaneidade em seu aplicativo.

As propriedades configuradas como tokens de simultaneidade são usadas para implementar o controle de simultaneidade otimista.

## <a name="conventions"></a>Convenções

Por convenção, as propriedades nunca são configuradas como tokens de simultaneidade.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar uma propriedade como um token de simultaneidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma propriedade como um token de simultaneidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versão de linha/carimbo de hora

Um carimbo de hora é uma propriedade em que um novo valor é gerado pelo banco de dados sempre que uma linha é inserida ou atualizada. A propriedade também é tratada como um token de simultaneidade. Isso garante que você receberá uma exceção se outra pessoa tiver modificado uma linha que você está tentando atualizar, pois consultada para os dados.

Como isso é feito é responsabilidade do provedor de banco de dados que está sendo usado. Para o SQL Server, o carimbo de hora geralmente é usado em uma *byte []* propriedade, que será de instalação como um *ROWVERSION* coluna no banco de dados.

### <a name="conventions"></a>Convenções

Por convenção, as propriedades nunca são configuradas como carimbos de hora.

### <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para configurar uma propriedade como um carimbo de hora.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar uma propriedade como um carimbo de hora.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
