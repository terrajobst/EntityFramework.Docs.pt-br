---
title: Tokens de simultaneidade - Core EF
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2018
ms.locfileid: "29745468"
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidade

> [!NOTE]
> Esta página documenta como configurar tokens de simultaneidade. Consulte [controlando conflitos de simultaneidade](../saving/concurrency.md) para obter uma explicação detalhada de como funciona o controle de simultaneidade no EF Core e exemplos de como lidar com conflitos de simultaneidade em seu aplicativo.

As propriedades configuradas como tokens de simultaneidade são usadas para implementar o controle de simultaneidade otimista.

## <a name="conventions"></a>Convenções

Por convenção, as propriedades nunca são configuradas como tokens de simultaneidade.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar uma propriedade como um token de simultaneidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar uma propriedade como um token de simultaneidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versão de carimbo de hora/linha

Um carimbo de hora é uma propriedade em que um novo valor é gerado pelo banco de dados sempre que uma linha é inserida ou atualizada. A propriedade também é tratada como um token de simultaneidade. Isso garante que você receberá uma exceção se ninguém tiver modificado uma linha que você está tentando atualizar desde consultada para os dados.

Como isso é obtido é até o provedor de banco de dados que está sendo usado. Para o SQL Server, carimbo de hora é geralmente usado em uma *byte []* propriedade, que será de instalação como um *ROWVERSION* coluna no banco de dados.

### <a name="conventions"></a>Convenções

Por convenção, as propriedades nunca são configuradas como os carimbos de hora.

### <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar uma propriedade como um carimbo de hora.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar uma propriedade como um carimbo de hora.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
