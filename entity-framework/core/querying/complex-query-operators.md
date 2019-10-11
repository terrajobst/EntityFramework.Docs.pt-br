---
title: Operadores de consulta complexos-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 350a7fa6a3ee1de16bad4b63e10842f9356a1b60
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186256"
---
# <a name="complex-query-operators"></a><span data-ttu-id="c8fc1-102">Operadores de consulta complexos</span><span class="sxs-lookup"><span data-stu-id="c8fc1-102">Complex Query Operators</span></span>

<span data-ttu-id="c8fc1-103">O LINQ (consulta integrada à linguagem) contém muitos operadores complexos, que combinam várias fontes de dados ou realiza processamento complexo.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="c8fc1-104">Nem todos os operadores LINQ têm traduções adequadas no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="c8fc1-105">Às vezes, uma consulta em um formulário é convertida com o servidor, mas, se escrito em um formulário diferente, não é traduzido mesmo que o resultado seja o mesmo.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="c8fc1-106">Esta página descreve alguns dos operadores complexos e suas variações com suporte.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="c8fc1-107">Em versões futuras, poderemos reconhecer mais padrões e adicionar suas traduções correspondentes.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="c8fc1-108">Também é importante ter em mente que o suporte à tradução varia entre os provedores.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="c8fc1-109">Uma consulta específica, que é traduzida no SqlServer, pode não funcionar para bancos de dados do SQLite.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="c8fc1-110">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-110">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="c8fc1-111">Join</span><span class="sxs-lookup"><span data-stu-id="c8fc1-111">Join</span></span>

<span data-ttu-id="c8fc1-112">O operador LINQ Join permite que você conecte duas fontes de dados com base no seletor de chave para cada fonte, gerando uma tupla de valores quando a chave corresponde.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="c8fc1-113">Naturalmente, ele se traduz em `INNER JOIN` em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="c8fc1-114">Embora a junção LINQ tenha seletores de chave externa e interna, o banco de dados requer uma condição de junção única.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="c8fc1-115">Portanto EF Core gera uma condição de junção comparando o seletor de chave externa ao seletor de chave interna para igualdade.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="c8fc1-116">Além disso, se os seletores de chave forem tipos anônimos, EF Core gerará uma condição de junção para comparar o componente de igualdade.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="c8fc1-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="c8fc1-117">GroupJoin</span></span>

<span data-ttu-id="c8fc1-118">O operador LINQ GroupJoin permite que você conecte duas fontes de dados semelhantes a Join, mas cria um grupo de valores internos para correspondência de elementos externos.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="c8fc1-119">A execução de uma consulta como o exemplo a seguir gera um resultado de `Blog` @ no__t-1 @ no__t-2.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="c8fc1-120">Como os bancos de dados (especialmente bancos de dados relacionais) não têm uma maneira de representar uma coleção de objetos do lado do cliente, o GroupJoin não é convertido para o servidor em muitos casos.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="c8fc1-121">Ele exige que você obtenha todos os dados do servidor para fazer GroupJoin sem um seletor especial (primeira consulta abaixo).</span><span class="sxs-lookup"><span data-stu-id="c8fc1-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="c8fc1-122">Mas se o seletor estiver limitando os dados que estão sendo selecionados, a busca de todos os dados do servidor poderá causar problemas de desempenho (segunda consulta abaixo).</span><span class="sxs-lookup"><span data-stu-id="c8fc1-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="c8fc1-123">É por isso que EF Core não converte GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="c8fc1-124">SelectMany</span><span class="sxs-lookup"><span data-stu-id="c8fc1-124">SelectMany</span></span>

<span data-ttu-id="c8fc1-125">O operador LINQ SelectMany permite que você enumere um seletor de coleção para cada elemento externo e gere tuplas de valores de cada fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="c8fc1-126">De uma forma, é uma junção, mas sem qualquer condição, de modo que todos os elementos externos sejam conectados com um elemento da origem da coleção.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="c8fc1-127">Dependendo de como o seletor de coleção está relacionado à fonte de dados externa, SelectMany pode ser convertido em várias consultas diferentes no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="c8fc1-128">Seletor de coleção não referencia externo</span><span class="sxs-lookup"><span data-stu-id="c8fc1-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="c8fc1-129">Quando o seletor de coleção não faz referência a nada da fonte externa, o resultado é um produto cartesiano de ambas as fontes de dados.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="c8fc1-130">Ele se traduz em `CROSS JOIN` em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="c8fc1-131">O seletor de coleção faz referência a externamente em uma cláusula WHERE</span><span class="sxs-lookup"><span data-stu-id="c8fc1-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="c8fc1-132">Quando o seletor de coleção tem uma cláusula WHERE, que faz referência ao elemento externo, EF Core converte-o em uma junção de banco de dados e usa o predicado como condição de junção.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="c8fc1-133">Normalmente, esse caso surge ao usar a navegação da coleção no elemento externo como o seletor de coleção.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="c8fc1-134">Se a coleção estiver vazia para um elemento externo, nenhum resultado será gerado para esse elemento externo.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="c8fc1-135">Mas se `DefaultIfEmpty` for aplicado no seletor de coleção, o elemento externo será conectado com um valor padrão do elemento interno.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="c8fc1-136">Devido a essa distinção, esse tipo de consulta se traduz em `INNER JOIN` na ausência de `DefaultIfEmpty` e `LEFT JOIN` quando `DefaultIfEmpty` é aplicado.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="c8fc1-137">O seletor de coleção faz referência a externamente em um caso não em que</span><span class="sxs-lookup"><span data-stu-id="c8fc1-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="c8fc1-138">Quando o seletor de coleção referencia o elemento externo, que não está em uma cláusula Where (como o caso acima), ele não é convertido em uma junção de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="c8fc1-139">É por isso que precisamos avaliar o seletor de coleção para cada elemento externo.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="c8fc1-140">Ele se traduz em operações `APPLY` em muitos bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="c8fc1-141">Se a coleção estiver vazia para um elemento externo, nenhum resultado será gerado para esse elemento externo.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="c8fc1-142">Mas se `DefaultIfEmpty` for aplicado no seletor de coleção, o elemento externo será conectado com um valor padrão do elemento interno.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="c8fc1-143">Devido a essa distinção, esse tipo de consulta se traduz em `CROSS APPLY` na ausência de `DefaultIfEmpty` e `OUTER APPLY` quando `DefaultIfEmpty` é aplicado.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="c8fc1-144">Determinados bancos de dados como o SQLite não dão suporte a operadores `APPLY`, portanto, esse tipo de consulta não pode ser traduzido.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="c8fc1-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="c8fc1-145">GroupBy</span></span>

<span data-ttu-id="c8fc1-146">Operadores GroupBy do LINQ criam um resultado do tipo `IGrouping<TKey, TElement>` em que `TKey` e `TElement` podem ser de qualquer tipo arbitrário.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="c8fc1-147">Além disso, `IGrouping` implementa `IEnumerable<TElement>`, o que significa que você pode compor sobre ele usando qualquer operador LINQ após o agrupamento.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="c8fc1-148">Como nenhuma estrutura de banco de dados pode representar um `IGrouping`, os operadores GroupBy não têm nenhuma tradução na maioria dos casos.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="c8fc1-149">Quando um operador de agregação é aplicado a cada grupo, que retorna um escalar, ele pode ser convertido em SQL `GROUP BY` em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="c8fc1-150">O SQL `GROUP BY` também é restritivo.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="c8fc1-151">Ele exige que você agrupe somente por valores escalares.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="c8fc1-152">A projeção só pode conter colunas de chave de agrupamento ou qualquer agregação aplicada em uma coluna.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="c8fc1-153">EF Core identifica esse padrão e o traduz para o servidor, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="c8fc1-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="c8fc1-154">EF Core também traduz consultas em que um operador de agregação no agrupamento é exibido em um operador de LINQ WHERE ou OrderBy (ou outra ordenação).</span><span class="sxs-lookup"><span data-stu-id="c8fc1-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="c8fc1-155">Ele usa a cláusula `HAVING` no SQL para a cláusula WHERE.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="c8fc1-156">A parte da consulta antes de aplicar o operador GroupBy pode ser qualquer consulta complexa, desde que possa ser convertida no servidor.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="c8fc1-157">Além disso, depois de aplicar operadores de agregação em uma consulta de agrupamento para remover agrupamentos da fonte resultante, você pode compor sobre ele como qualquer outra consulta.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="c8fc1-158">Os operadores de agregação EF Core suporte são os seguintes</span><span class="sxs-lookup"><span data-stu-id="c8fc1-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="c8fc1-159">Average</span><span class="sxs-lookup"><span data-stu-id="c8fc1-159">Average</span></span>
- <span data-ttu-id="c8fc1-160">Contagem</span><span class="sxs-lookup"><span data-stu-id="c8fc1-160">Count</span></span>
- <span data-ttu-id="c8fc1-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="c8fc1-161">LongCount</span></span>
- <span data-ttu-id="c8fc1-162">Máx.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-162">Max</span></span>
- <span data-ttu-id="c8fc1-163">Mín.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-163">Min</span></span>
- <span data-ttu-id="c8fc1-164">Sum</span><span class="sxs-lookup"><span data-stu-id="c8fc1-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="c8fc1-165">Junção à esquerda</span><span class="sxs-lookup"><span data-stu-id="c8fc1-165">Left Join</span></span>

<span data-ttu-id="c8fc1-166">Embora a junção à esquerda não seja um operador LINQ, os bancos de dados relacionais têm o conceito de junção à esquerda que é frequentemente usado em consultas.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="c8fc1-167">Um padrão específico em consultas LINQ fornece o mesmo resultado que um `LEFT JOIN` no servidor.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="c8fc1-168">EF Core identifica tais padrões e gera o equivalente `LEFT JOIN` no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="c8fc1-169">O padrão envolve a criação de um GroupJoin entre as fontes de dados e, em seguida, a mesclagem do agrupamento usando o operador SelectMany com DefaultIfEmpty na origem do agrupamento para corresponder nulo quando o interior não tiver um elemento relacionado.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="c8fc1-170">O exemplo a seguir mostra como esse padrão é semelhante e o que ele gera.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="c8fc1-171">O padrão acima cria uma estrutura complexa na árvore de expressão.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="c8fc1-172">Por isso, EF Core exige que você nivelar os resultados de agrupamento do operador GroupJoin em uma etapa imediatamente após o operador.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="c8fc1-173">Mesmo que o GroupJoin-DefaultIfEmpty-SelectMany seja usado, mas em um padrão diferente, talvez não o identifiquemos como uma junção à esquerda.</span><span class="sxs-lookup"><span data-stu-id="c8fc1-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
