---
title: Operadores de consulta complexos - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417739"
---
# <a name="complex-query-operators"></a>Operadores de consulta complexa

A Linguagem Integrada Query (LINQ) contém muitos operadores complexos, que combinam várias fontes de dados ou fazem processamentos complexos. Nem todos os operadores LINQ têm traduções adequadas no lado do servidor. Às vezes, uma consulta em um formulário se traduz para o servidor, mas se escrito de forma diferente não se traduz mesmo se o resultado for o mesmo. Esta página descreve alguns dos operadores complexos e suas variações suportadas. Em versões futuras, podemos reconhecer mais padrões e adicionar suas traduções correspondentes. Também é importante ter em mente que o suporte à tradução varia entre os provedores. Uma consulta específica, que é traduzida no SqlServer, pode não funcionar para bancos de dados SQLite.

> [!TIP]
> Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="join"></a>Join

O operador LINQ Join permite conectar duas fontes de dados com base no seletor de chaves para cada fonte, gerando uma tupla de valores quando a chave corresponde. Naturalmente se traduz `INNER JOIN` em bancos de dados relacionais. Enquanto o LINQ Join possui seletores de chaves externos e internos, o banco de dados requer uma única condição de adesão. Assim, o EF Core gera uma condição de adesão comparando o seletor de tecla externa com o seletor de tecla interna para a igualdade. Além disso, se os seletores-chave são tipos anônimos, o EF Core gera uma condição de adesão para comparar o componente de igualdade sábio.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>GroupJoin

O operador LINQ GroupJoin permite conectar duas fontes de dados semelhantes ao Join, mas cria um grupo de valores internos para combinar elementos externos. A execução de uma consulta como `Blog`  &  `IEnumerable<Post>`o exemplo a seguir gera um resultado de . Como os bancos de dados (especialmente bancos de dados relacionais) não têm uma maneira de representar uma coleção de objetos do lado do cliente, o GroupJoin não se traduz para o servidor em muitos casos. Ele exige que você obtenha todos os dados do servidor para fazer o GroupJoin sem um seletor especial (primeira consulta abaixo). Mas se o seletor estiver limitando a coleta de dados, então buscar todos os dados do servidor pode causar problemas de desempenho (segunda consulta abaixo). É por isso que o EF Core não traduz groupjoin.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>SelectMany

O operador LINQ SelectMany permite que você enumerar um seletor de coleta para cada elemento externo e gerar tuplas de valores de cada fonte de dados. De certa forma, é uma junta, mas sem qualquer condição, então cada elemento externo está conectado com um elemento da fonte de coleta. Dependendo de como o seletor de coleta está relacionado à fonte de dados externo, SelectMany pode se traduzir em várias consultas diferentes no lado do servidor.

### <a name="collection-selector-doesnt-reference-outer"></a>O seletor de coleta não faz referência externa

Quando o seletor de coleta não está fazendo referência a nada da fonte externa, o resultado é um produto cartesiano de ambas as fontes de dados. Traduz-se `CROSS JOIN` em bancos de dados relacionais.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Seletor de coleção faz referência seletor externo em uma cláusula onde

Quando o seletor de coleção tem uma cláusula onde, que faz referência ao elemento externo, então o EF Core o traduz para uma unidade de banco de dados e usa o predicado como condição de adesão. Normalmente, este caso surge ao usar a navegação de coleta no elemento externo como seletor de coleta. Se a coleção estiver vazia para um elemento externo, então nenhum resultado será gerado para esse elemento externo. Mas `DefaultIfEmpty` se for aplicado no seletor de coleta, então o elemento externo será conectado com um valor padrão do elemento interno. Por causa dessa distinção, esse tipo `INNER JOIN` de consulta `DefaultIfEmpty` `LEFT JOIN` se `DefaultIfEmpty` traduz na ausência e quando é aplicada.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>O seletor de coleta faz referências externas em um caso não-onde

Quando o seletor de coleção faz referência ao elemento externo, que não está em uma cláusula onde (como o caso acima), ele não se traduz em uma adesão ao banco de dados. É por isso que precisamos avaliar o seletor de coleta para cada elemento externo. Traduz-se `APPLY` em operações em muitas bases de dados relacionais. Se a coleção estiver vazia para um elemento externo, então nenhum resultado será gerado para esse elemento externo. Mas `DefaultIfEmpty` se for aplicado no seletor de coleta, então o elemento externo será conectado com um valor padrão do elemento interno. Por causa dessa distinção, esse tipo `CROSS APPLY` de consulta `DefaultIfEmpty` `OUTER APPLY` se `DefaultIfEmpty` traduz na ausência e quando é aplicada. Certos bancos de dados como `APPLY` o SQLite não suportam operadores, então esse tipo de consulta pode não ser traduzida.

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

Os operadores linq groupBy `IGrouping<TKey, TElement>` criam `TKey` `TElement` um resultado de tipo onde e pode ser qualquer tipo arbitrário. Além disso, `IGrouping` `IEnumerable<TElement>`implementa, o que significa que você pode compor sobre ele usando qualquer operador LINQ após o agrupamento. Uma vez que nenhuma `IGrouping`estrutura de banco de dados pode representar um , os operadores do GroupBy não têm tradução na maioria dos casos. Quando um operador agregado é aplicado a cada grupo, o que retorna um `GROUP BY` escalar, ele pode ser traduzido para SQL em bancos de dados relacionais. O SQL `GROUP BY` também é restritivo. Exige que você se agrupe apenas por valores escalares. A projeção só pode conter colunas-chave de agrupamento ou qualquer agregado aplicado sobre uma coluna. O EF Core identifica esse padrão e o traduz para o servidor, como no exemplo a seguir:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

O EF Core também traduz consultas em que um operador agregado no agrupamento aparece em um operador LINQ (ou outro pedido). Ele `HAVING` usa cláusula no SQL para a cláusula onde. A parte da consulta antes de aplicar o operador GroupBy pode ser qualquer consulta complexa, desde que possa ser traduzida para servidor. Além disso, uma vez que você aplique operadores agregados em uma consulta de agrupamento para remover agrupamentos da fonte resultante, você pode compor em cima dele como qualquer outra consulta.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

Os suportes agregados do EF Core são os seguintes

- Média
- Contagem
- LongCount
- Max
- Mín
- SUM

## <a name="left-join"></a>Ingressar à esquerda

Embora o Left Join não seja um operador LINQ, os bancos de dados relacionais têm o conceito de Left Join, que é frequentemente usado em consultas. Um padrão específico em consultas LINQ dá `LEFT JOIN` o mesmo resultado que um no servidor. O EF Core identifica tais `LEFT JOIN` padrões e gera o equivalente no lado do servidor. O padrão envolve a criação de um GroupJoin entre ambas as fontes de dados e, em seguida, o achatamento do agrupamento usando o operador SelectMany com DefaultIfEmpty na fonte de agrupamento para corresponder nulo quando o interior não tem um elemento relacionado. O exemplo a seguir mostra como esse padrão se parece e o que ele gera.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

O padrão acima cria uma estrutura complexa na árvore de expressão. Por causa disso, o EF Core exige que você achate os resultados de agrupamento do operador GroupJoin em uma etapa imediatamente após o operador. Mesmo que o GroupJoin-DefaultIfEmpty-SelectMany seja usado, mas em um padrão diferente, podemos não identificá-lo como um Grupo à esquerda.
