---
title: Divisão de tabela-EF Core
description: Como configurar a divisão de tabela usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: de24f8903af79ebd7f68e6b74288257883c1fa8d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417394"
---
# <a name="table-splitting"></a>Divisão de tabela

EF Core permite mapear duas ou mais entidades para uma única linha. Isso é chamado de _divisão de tabela_ ou compartilhamento de _tabela_.

## <a name="configuration"></a>Configuração

Para usar a divisão de tabela, os tipos de entidade precisam ser mapeados para a mesma tabela, ter as chaves primárias mapeadas para as mesmas colunas e pelo menos uma relação configurada entre a chave primária de um tipo de entidade e outra na mesma tabela.

Um cenário comum para divisão de tabela é usar apenas um subconjunto das colunas na tabela para maior desempenho ou encapsulamento.

Neste exemplo `Order` representa um subconjunto de `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Além da configuração necessária, chamamos `Property(o => o.Status).HasColumnName("Status")` para mapear `DetailedOrder.Status` para a mesma coluna que `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> Consulte o [projeto de exemplo completo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obter mais contexto.

## <a name="usage"></a>Uso

Salvar e consultar entidades usando divisão de tabela é feito da mesma maneira que outras entidades:

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a>Entidade dependente opcional

> [!NOTE]
> Esse recurso foi introduzido no EF Core 3,0.

Se todas as colunas usadas por uma entidade dependente forem `NULL` no banco de dados, nenhuma instância para ela será criada quando consultada. Isso permite modelar uma entidade dependente opcional, em que a propriedade relationship no principal seria nula. Observe que isso também aconteceria que todas as propriedades do dependente fossem opcionais e definidas como `null`, o que pode não ser esperado.

## <a name="concurrency-tokens"></a>Tokens de simultaneidade

Se qualquer um dos tipos de entidade que compartilham uma tabela tiver um token de simultaneidade, ele também deverá ser incluído em todos os outros tipos de entidade. Isso é necessário para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.

Para evitar a exposição do token de simultaneidade ao código de consumo, é possível a propriedade criar um como [sombra](xref:core/modeling/shadow-properties):

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
