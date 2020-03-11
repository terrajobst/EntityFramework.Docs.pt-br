---
title: Operadores de consulta complexos-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417739"
---
# <a name="complex-query-operators"></a>Operadores de consulta complexa

O LINQ (consulta integrada à linguagem) contém muitos operadores complexos, que combinam várias fontes de dados ou realiza processamento complexo. Nem todos os operadores LINQ têm traduções adequadas no lado do servidor. Às vezes, uma consulta em um formulário é convertida com o servidor, mas, se escrito em um formulário diferente, não é traduzido mesmo que o resultado seja o mesmo. Esta página descreve alguns dos operadores complexos e suas variações com suporte. Em versões futuras, poderemos reconhecer mais padrões e adicionar suas traduções correspondentes. Também é importante ter em mente que o suporte à tradução varia entre os provedores. Uma consulta específica, que é traduzida no SqlServer, pode não funcionar para bancos de dados do SQLite.

> [!TIP]
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="join"></a>Join

O operador LINQ Join permite que você conecte duas fontes de dados com base no seletor de chave para cada fonte, gerando uma tupla de valores quando a chave corresponde. Naturalmente, ele se traduz em `INNER JOIN` em bancos de dados relacionais. Embora a junção LINQ tenha seletores de chave externa e interna, o banco de dados requer uma condição de junção única. Portanto EF Core gera uma condição de junção comparando o seletor de chave externa ao seletor de chave interna para igualdade. Além disso, se os seletores de chave forem tipos anônimos, EF Core gerará uma condição de junção para comparar o componente de igualdade.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>GroupJoin

O operador LINQ GroupJoin permite que você conecte duas fontes de dados semelhantes a Join, mas cria um grupo de valores internos para correspondência de elementos externos. A execução de uma consulta como o exemplo a seguir gera um resultado de `Blog` & `IEnumerable<Post>`. Como os bancos de dados (especialmente bancos de dados relacionais) não têm uma maneira de representar uma coleção de objetos do lado do cliente, o GroupJoin não é convertido para o servidor em muitos casos. Ele exige que você obtenha todos os dados do servidor para fazer GroupJoin sem um seletor especial (primeira consulta abaixo). Mas se o seletor estiver limitando os dados que estão sendo selecionados, a busca de todos os dados do servidor poderá causar problemas de desempenho (segunda consulta abaixo). É por isso que EF Core não converte GroupJoin.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>SelectMany

O operador LINQ SelectMany permite que você enumere um seletor de coleção para cada elemento externo e gere tuplas de valores de cada fonte de dados. De uma forma, é uma junção, mas sem qualquer condição, de modo que todos os elementos externos sejam conectados com um elemento da origem da coleção. Dependendo de como o seletor de coleção está relacionado à fonte de dados externa, SelectMany pode ser convertido em várias consultas diferentes no lado do servidor.

### <a name="collection-selector-doesnt-reference-outer"></a>Seletor de coleção não referencia externo

Quando o seletor de coleção não faz referência a nada da fonte externa, o resultado é um produto cartesiano de ambas as fontes de dados. Ele se traduz em `CROSS JOIN` em bancos de dados relacionais.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>O seletor de coleção faz referência a externamente em uma cláusula WHERE

Quando o seletor de coleção tem uma cláusula WHERE, que faz referência ao elemento externo, EF Core converte-o em uma junção de banco de dados e usa o predicado como condição de junção. Normalmente, esse caso surge ao usar a navegação da coleção no elemento externo como o seletor de coleção. Se a coleção estiver vazia para um elemento externo, nenhum resultado será gerado para esse elemento externo. Mas se `DefaultIfEmpty` for aplicado no seletor de coleção, o elemento externo será conectado com um valor padrão do elemento interno. Devido a essa distinção, esse tipo de consulta se traduz em `INNER JOIN` na ausência de `DefaultIfEmpty` e `LEFT JOIN` quando `DefaultIfEmpty` é aplicado.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>O seletor de coleção faz referência a externamente em um caso não em que

Quando o seletor de coleção referencia o elemento externo, que não está em uma cláusula Where (como o caso acima), ele não é convertido em uma junção de banco de dados. É por isso que precisamos avaliar o seletor de coleção para cada elemento externo. Ele se traduz em operações de `APPLY` em muitos bancos de dados relacionais. Se a coleção estiver vazia para um elemento externo, nenhum resultado será gerado para esse elemento externo. Mas se `DefaultIfEmpty` for aplicado no seletor de coleção, o elemento externo será conectado com um valor padrão do elemento interno. Devido a essa distinção, esse tipo de consulta se traduz em `CROSS APPLY` na ausência de `DefaultIfEmpty` e `OUTER APPLY` quando `DefaultIfEmpty` é aplicado. Determinados bancos de dados como o SQLite não dão suporte a operadores de `APPLY` para que esse tipo de consulta não possa ser traduzido.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a>GroupBy

Operadores GroupBy do LINQ criam um resultado do tipo `IGrouping<TKey, TElement>` onde `TKey` e `TElement` podem ser qualquer tipo arbitrário. Além disso, `IGrouping` implementa `IEnumerable<TElement>`, o que significa que você pode compor sobre ele usando qualquer operador LINQ após o agrupamento. Como nenhuma estrutura de banco de dados pode representar um `IGrouping`, os operadores GroupBy não têm nenhuma tradução na maioria dos casos. Quando um operador de agregação é aplicado a cada grupo, que retorna um escalar, ele pode ser convertido em SQL `GROUP BY` em bancos de dados relacionais. O `GROUP BY` do SQL também é restritivo. Ele exige que você agrupe somente por valores escalares. A projeção só pode conter colunas de chave de agrupamento ou qualquer agregação aplicada em uma coluna. EF Core identifica esse padrão e o traduz para o servidor, como no exemplo a seguir:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core também traduz consultas em que um operador de agregação no agrupamento é exibido em um operador de LINQ WHERE ou OrderBy (ou outra ordenação). Ele usa a cláusula `HAVING` no SQL para a cláusula WHERE. A parte da consulta antes de aplicar o operador GroupBy pode ser qualquer consulta complexa, desde que possa ser convertida no servidor. Além disso, depois de aplicar operadores de agregação em uma consulta de agrupamento para remover agrupamentos da fonte resultante, você pode compor sobre ele como qualquer outra consulta.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

Os operadores de agregação EF Core suporte são os seguintes

- Média
- Contagem
- LongCount
- Max
- Mín
- SUM

## <a name="left-join"></a>Junção à esquerda

Embora a junção à esquerda não seja um operador LINQ, os bancos de dados relacionais têm o conceito de junção à esquerda que é frequentemente usado em consultas. Um padrão específico em consultas LINQ fornece o mesmo resultado que um `LEFT JOIN` no servidor. EF Core identifica tais padrões e gera o `LEFT JOIN` equivalente no lado do servidor. O padrão envolve a criação de um GroupJoin entre as fontes de dados e, em seguida, a mesclagem do agrupamento usando o operador SelectMany com DefaultIfEmpty na origem do agrupamento para corresponder nulo quando o interior não tiver um elemento relacionado. O exemplo a seguir mostra como esse padrão é semelhante e o que ele gera.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

O padrão acima cria uma estrutura complexa na árvore de expressão. Por isso, EF Core exige que você nivelar os resultados de agrupamento do operador GroupJoin em uma etapa imediatamente após o operador. Mesmo que o GroupJoin-DefaultIfEmpty-SelectMany seja usado, mas em um padrão diferente, talvez não o identifiquemos como uma junção à esquerda.
