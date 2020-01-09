---
title: Índices-EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502130"
---
# <a name="indexes"></a>Índices

Os índices são um conceito comum entre vários armazenamentos de dados. Embora sua implementação no armazenamento de dados possa variar, elas são usadas para fazer pesquisas com base em uma coluna (ou conjunto de colunas) mais eficiente.

Não é possível criar índices usando anotações de dados. Você pode usar a API fluente para especificar um índice em uma única coluna da seguinte maneira:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

Você também pode especificar um índice em mais de uma coluna:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> Por convenção, um índice é criado em cada propriedade (ou conjunto de propriedades) que são usados como uma chave estrangeira.
>
> EF Core só dá suporte a um índice por conjunto distinto de propriedades. Se você usar a API fluente para configurar um índice em um conjunto de propriedades que já tem um índice definido, seja por convenção ou configuração anterior, você alterará a definição desse índice. Isso será útil se você quiser configurar ainda mais um índice que foi criado por convenção.

## <a name="index-uniqueness"></a>Exclusividade do índice

Por padrão, os índices não são exclusivos: várias linhas têm permissão para ter os mesmos valores para o conjunto de colunas do índice. Você pode fazer um índice exclusivo da seguinte maneira:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

A tentativa de inserir mais de uma entidade com os mesmos valores para o conjunto de colunas do índice fará com que uma exceção seja gerada.

## <a name="index-name"></a>Nome do índice

Por convenção, os índices criados em um banco de dados relacional são nomeados `IX_<type name>_<property name>`. Para índices compostos, `<property name>` se torna uma lista de nomes de propriedade separados por sublinhado.

Você pode usar a API fluente para definir o nome do índice criado no banco de dados:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a>Filtro de índice

Alguns bancos de dados relacionais permitem que você especifique um índice filtrado ou parcial. Isso permite que você indexe apenas um subconjunto dos valores de uma coluna, reduzindo o tamanho do índice e melhorando o desempenho e o uso do espaço em disco. Para obter mais informações sobre SQL Server índices filtrados, [consulte a documentação](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).

Você pode usar a API fluente para especificar um filtro em um índice, fornecido como uma expressão SQL:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

Ao usar o provedor de SQL Server o EF adiciona um filtro de `'IS NOT NULL'` para todas as colunas anuláveis que fazem parte de um índice exclusivo. Para substituir essa convenção, você pode fornecer um valor `null`.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a>Colunas incluídas

Alguns bancos de dados relacionais permitem que você configure um conjunto de colunas que são incluídas no índice, mas que não fazem parte de sua "chave". Isso pode melhorar significativamente o desempenho da consulta quando todas as colunas na consulta são incluídas no índice como colunas de chave ou não chave, pois a tabela em si não precisa ser acessada. Para obter mais informações sobre SQL Server colunas incluídas, [consulte a documentação](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).

No exemplo a seguir, a coluna `Url` é parte da chave de índice, portanto, qualquer filtragem de consulta nessa coluna pode usar o índice. Mas, além disso, as consultas que acessam apenas as colunas `Title` e `PublishedOn` não precisarão acessar a tabela e serão executadas com mais eficiência:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
