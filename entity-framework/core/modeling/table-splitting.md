---
title: Tabela dividindo - o EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562579"
---
# <a name="table-splitting"></a>Divisão de tabela

>[!NOTE]
> Esse recurso é novo no EF Core 2.0.

O EF Core permite mapear dois ou mais entidades para uma única linha. Isso é chamado _divisão de tabela_ ou _compartilhamento de tabela_.

## <a name="configuration"></a>Configuração

Para usar os tipos de entidade precisam ser mapeados para a mesma tabela de divisão de tabela, ter as chaves primárias mapeadas para as mesmas colunas e pelo menos uma relação configurado entre a chave primária do tipo de uma entidade e outra na mesma tabela.

Um cenário comum para a divisão de tabela está usando apenas um subconjunto das colunas na tabela para maior desempenho ou encapsulamento.

Neste exemplo `Order` representa um subconjunto de `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Além da configuração necessária, chamamos `HasBaseType((string)null)` para evitar o mapeamento `DetailedOrder` na mesma hierarquia como `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

Consulte a [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obter mais contexto.

## <a name="usage"></a>Uso

Salvando e consultar entidades usando a divisão de tabela são feitas da mesma forma que outras entidades, a única diferença é que todas as entidades de uma linha de compartilhamento devem ser controladas para a inserção.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Tokens de simultaneidade

Se qualquer um dos tipos de entidade, compartilhamento de uma tabela tiver um token de simultaneidade, em seguida, ela deve ser incluída em todos os outros tipos de entidade para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.

Para evitar expô-lo para o código de consumo, é possível criar um no estado de sombra.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]