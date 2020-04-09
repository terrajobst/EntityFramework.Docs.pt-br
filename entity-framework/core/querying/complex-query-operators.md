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
# <a name="complex-query-operators"></a><span data-ttu-id="5d19c-102">Operadores de consulta complexa</span><span class="sxs-lookup"><span data-stu-id="5d19c-102">Complex Query Operators</span></span>

<span data-ttu-id="5d19c-103">A Linguagem Integrada Query (LINQ) contém muitos operadores complexos, que combinam várias fontes de dados ou fazem processamentos complexos.</span><span class="sxs-lookup"><span data-stu-id="5d19c-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="5d19c-104">Nem todos os operadores LINQ têm traduções adequadas no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="5d19c-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="5d19c-105">Às vezes, uma consulta em um formulário se traduz para o servidor, mas se escrito de forma diferente não se traduz mesmo se o resultado for o mesmo.</span><span class="sxs-lookup"><span data-stu-id="5d19c-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="5d19c-106">Esta página descreve alguns dos operadores complexos e suas variações suportadas.</span><span class="sxs-lookup"><span data-stu-id="5d19c-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="5d19c-107">Em versões futuras, podemos reconhecer mais padrões e adicionar suas traduções correspondentes.</span><span class="sxs-lookup"><span data-stu-id="5d19c-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="5d19c-108">Também é importante ter em mente que o suporte à tradução varia entre os provedores.</span><span class="sxs-lookup"><span data-stu-id="5d19c-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="5d19c-109">Uma consulta específica, que é traduzida no SqlServer, pode não funcionar para bancos de dados SQLite.</span><span class="sxs-lookup"><span data-stu-id="5d19c-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="5d19c-110">Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="5d19c-110">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="5d19c-111">Join</span><span class="sxs-lookup"><span data-stu-id="5d19c-111">Join</span></span>

<span data-ttu-id="5d19c-112">O operador LINQ Join permite conectar duas fontes de dados com base no seletor de chaves para cada fonte, gerando uma tupla de valores quando a chave corresponde.</span><span class="sxs-lookup"><span data-stu-id="5d19c-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="5d19c-113">Naturalmente se traduz `INNER JOIN` em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="5d19c-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="5d19c-114">Enquanto o LINQ Join possui seletores de chaves externos e internos, o banco de dados requer uma única condição de adesão.</span><span class="sxs-lookup"><span data-stu-id="5d19c-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="5d19c-115">Assim, o EF Core gera uma condição de adesão comparando o seletor de tecla externa com o seletor de tecla interna para a igualdade.</span><span class="sxs-lookup"><span data-stu-id="5d19c-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="5d19c-116">Além disso, se os seletores-chave são tipos anônimos, o EF Core gera uma condição de adesão para comparar o componente de igualdade sábio.</span><span class="sxs-lookup"><span data-stu-id="5d19c-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="5d19c-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="5d19c-117">GroupJoin</span></span>

<span data-ttu-id="5d19c-118">O operador LINQ GroupJoin permite conectar duas fontes de dados semelhantes ao Join, mas cria um grupo de valores internos para combinar elementos externos.</span><span class="sxs-lookup"><span data-stu-id="5d19c-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="5d19c-119">A execução de uma consulta como `Blog`  &  `IEnumerable<Post>`o exemplo a seguir gera um resultado de .</span><span class="sxs-lookup"><span data-stu-id="5d19c-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="5d19c-120">Como os bancos de dados (especialmente bancos de dados relacionais) não têm uma maneira de representar uma coleção de objetos do lado do cliente, o GroupJoin não se traduz para o servidor em muitos casos.</span><span class="sxs-lookup"><span data-stu-id="5d19c-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="5d19c-121">Ele exige que você obtenha todos os dados do servidor para fazer o GroupJoin sem um seletor especial (primeira consulta abaixo).</span><span class="sxs-lookup"><span data-stu-id="5d19c-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="5d19c-122">Mas se o seletor estiver limitando a coleta de dados, então buscar todos os dados do servidor pode causar problemas de desempenho (segunda consulta abaixo).</span><span class="sxs-lookup"><span data-stu-id="5d19c-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="5d19c-123">É por isso que o EF Core não traduz groupjoin.</span><span class="sxs-lookup"><span data-stu-id="5d19c-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="5d19c-124">SelectMany</span><span class="sxs-lookup"><span data-stu-id="5d19c-124">SelectMany</span></span>

<span data-ttu-id="5d19c-125">O operador LINQ SelectMany permite que você enumerar um seletor de coleta para cada elemento externo e gerar tuplas de valores de cada fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="5d19c-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="5d19c-126">De certa forma, é uma junta, mas sem qualquer condição, então cada elemento externo está conectado com um elemento da fonte de coleta.</span><span class="sxs-lookup"><span data-stu-id="5d19c-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="5d19c-127">Dependendo de como o seletor de coleta está relacionado à fonte de dados externo, SelectMany pode se traduzir em várias consultas diferentes no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="5d19c-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="5d19c-128">O seletor de coleta não faz referência externa</span><span class="sxs-lookup"><span data-stu-id="5d19c-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="5d19c-129">Quando o seletor de coleta não está fazendo referência a nada da fonte externa, o resultado é um produto cartesiano de ambas as fontes de dados.</span><span class="sxs-lookup"><span data-stu-id="5d19c-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="5d19c-130">Traduz-se `CROSS JOIN` em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="5d19c-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="5d19c-131">Seletor de coleção faz referência seletor externo em uma cláusula onde</span><span class="sxs-lookup"><span data-stu-id="5d19c-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="5d19c-132">Quando o seletor de coleção tem uma cláusula onde, que faz referência ao elemento externo, então o EF Core o traduz para uma unidade de banco de dados e usa o predicado como condição de adesão.</span><span class="sxs-lookup"><span data-stu-id="5d19c-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="5d19c-133">Normalmente, este caso surge ao usar a navegação de coleta no elemento externo como seletor de coleta.</span><span class="sxs-lookup"><span data-stu-id="5d19c-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="5d19c-134">Se a coleção estiver vazia para um elemento externo, então nenhum resultado será gerado para esse elemento externo.</span><span class="sxs-lookup"><span data-stu-id="5d19c-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="5d19c-135">Mas `DefaultIfEmpty` se for aplicado no seletor de coleta, então o elemento externo será conectado com um valor padrão do elemento interno.</span><span class="sxs-lookup"><span data-stu-id="5d19c-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="5d19c-136">Por causa dessa distinção, esse tipo `INNER JOIN` de consulta `DefaultIfEmpty` `LEFT JOIN` se `DefaultIfEmpty` traduz na ausência e quando é aplicada.</span><span class="sxs-lookup"><span data-stu-id="5d19c-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="5d19c-137">O seletor de coleta faz referências externas em um caso não-onde</span><span class="sxs-lookup"><span data-stu-id="5d19c-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="5d19c-138">Quando o seletor de coleção faz referência ao elemento externo, que não está em uma cláusula onde (como o caso acima), ele não se traduz em uma adesão ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5d19c-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="5d19c-139">É por isso que precisamos avaliar o seletor de coleta para cada elemento externo.</span><span class="sxs-lookup"><span data-stu-id="5d19c-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="5d19c-140">Traduz-se `APPLY` em operações em muitas bases de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="5d19c-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="5d19c-141">Se a coleção estiver vazia para um elemento externo, então nenhum resultado será gerado para esse elemento externo.</span><span class="sxs-lookup"><span data-stu-id="5d19c-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="5d19c-142">Mas `DefaultIfEmpty` se for aplicado no seletor de coleta, então o elemento externo será conectado com um valor padrão do elemento interno.</span><span class="sxs-lookup"><span data-stu-id="5d19c-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="5d19c-143">Por causa dessa distinção, esse tipo `CROSS APPLY` de consulta `DefaultIfEmpty` `OUTER APPLY` se `DefaultIfEmpty` traduz na ausência e quando é aplicada.</span><span class="sxs-lookup"><span data-stu-id="5d19c-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="5d19c-144">Certos bancos de dados como `APPLY` o SQLite não suportam operadores, então esse tipo de consulta pode não ser traduzida.</span><span class="sxs-lookup"><span data-stu-id="5d19c-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="5d19c-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="5d19c-145">GroupBy</span></span>

<span data-ttu-id="5d19c-146">Os operadores linq groupBy `IGrouping<TKey, TElement>` criam `TKey` `TElement` um resultado de tipo onde e pode ser qualquer tipo arbitrário.</span><span class="sxs-lookup"><span data-stu-id="5d19c-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="5d19c-147">Além disso, `IGrouping` `IEnumerable<TElement>`implementa, o que significa que você pode compor sobre ele usando qualquer operador LINQ após o agrupamento.</span><span class="sxs-lookup"><span data-stu-id="5d19c-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="5d19c-148">Uma vez que nenhuma `IGrouping`estrutura de banco de dados pode representar um , os operadores do GroupBy não têm tradução na maioria dos casos.</span><span class="sxs-lookup"><span data-stu-id="5d19c-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="5d19c-149">Quando um operador agregado é aplicado a cada grupo, o que retorna um `GROUP BY` escalar, ele pode ser traduzido para SQL em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="5d19c-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="5d19c-150">O SQL `GROUP BY` também é restritivo.</span><span class="sxs-lookup"><span data-stu-id="5d19c-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="5d19c-151">Exige que você se agrupe apenas por valores escalares.</span><span class="sxs-lookup"><span data-stu-id="5d19c-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="5d19c-152">A projeção só pode conter colunas-chave de agrupamento ou qualquer agregado aplicado sobre uma coluna.</span><span class="sxs-lookup"><span data-stu-id="5d19c-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="5d19c-153">O EF Core identifica esse padrão e o traduz para o servidor, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="5d19c-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="5d19c-154">O EF Core também traduz consultas em que um operador agregado no agrupamento aparece em um operador LINQ (ou outro pedido).</span><span class="sxs-lookup"><span data-stu-id="5d19c-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="5d19c-155">Ele `HAVING` usa cláusula no SQL para a cláusula onde.</span><span class="sxs-lookup"><span data-stu-id="5d19c-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="5d19c-156">A parte da consulta antes de aplicar o operador GroupBy pode ser qualquer consulta complexa, desde que possa ser traduzida para servidor.</span><span class="sxs-lookup"><span data-stu-id="5d19c-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="5d19c-157">Além disso, uma vez que você aplique operadores agregados em uma consulta de agrupamento para remover agrupamentos da fonte resultante, você pode compor em cima dele como qualquer outra consulta.</span><span class="sxs-lookup"><span data-stu-id="5d19c-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="5d19c-158">Os suportes agregados do EF Core são os seguintes</span><span class="sxs-lookup"><span data-stu-id="5d19c-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="5d19c-159">Média</span><span class="sxs-lookup"><span data-stu-id="5d19c-159">Average</span></span>
- <span data-ttu-id="5d19c-160">Contagem</span><span class="sxs-lookup"><span data-stu-id="5d19c-160">Count</span></span>
- <span data-ttu-id="5d19c-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="5d19c-161">LongCount</span></span>
- <span data-ttu-id="5d19c-162">Max</span><span class="sxs-lookup"><span data-stu-id="5d19c-162">Max</span></span>
- <span data-ttu-id="5d19c-163">Mín</span><span class="sxs-lookup"><span data-stu-id="5d19c-163">Min</span></span>
- <span data-ttu-id="5d19c-164">SUM</span><span class="sxs-lookup"><span data-stu-id="5d19c-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="5d19c-165">Ingressar à esquerda</span><span class="sxs-lookup"><span data-stu-id="5d19c-165">Left Join</span></span>

<span data-ttu-id="5d19c-166">Embora o Left Join não seja um operador LINQ, os bancos de dados relacionais têm o conceito de Left Join, que é frequentemente usado em consultas.</span><span class="sxs-lookup"><span data-stu-id="5d19c-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="5d19c-167">Um padrão específico em consultas LINQ dá `LEFT JOIN` o mesmo resultado que um no servidor.</span><span class="sxs-lookup"><span data-stu-id="5d19c-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="5d19c-168">O EF Core identifica tais `LEFT JOIN` padrões e gera o equivalente no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="5d19c-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="5d19c-169">O padrão envolve a criação de um GroupJoin entre ambas as fontes de dados e, em seguida, o achatamento do agrupamento usando o operador SelectMany com DefaultIfEmpty na fonte de agrupamento para corresponder nulo quando o interior não tem um elemento relacionado.</span><span class="sxs-lookup"><span data-stu-id="5d19c-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="5d19c-170">O exemplo a seguir mostra como esse padrão se parece e o que ele gera.</span><span class="sxs-lookup"><span data-stu-id="5d19c-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="5d19c-171">O padrão acima cria uma estrutura complexa na árvore de expressão.</span><span class="sxs-lookup"><span data-stu-id="5d19c-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="5d19c-172">Por causa disso, o EF Core exige que você achate os resultados de agrupamento do operador GroupJoin em uma etapa imediatamente após o operador.</span><span class="sxs-lookup"><span data-stu-id="5d19c-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="5d19c-173">Mesmo que o GroupJoin-DefaultIfEmpty-SelectMany seja usado, mas em um padrão diferente, podemos não identificá-lo como um Grupo à esquerda.</span><span class="sxs-lookup"><span data-stu-id="5d19c-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
