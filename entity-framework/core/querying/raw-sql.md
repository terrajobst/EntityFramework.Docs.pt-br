---
title: Consultas SQL brutas – EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417666"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="f111e-102">Consultas SQL brutas</span><span class="sxs-lookup"><span data-stu-id="f111e-102">Raw SQL Queries</span></span>

<span data-ttu-id="f111e-103">O Entity Framework Core permite acessar uma lista suspensa de consultas SQL brutas ao trabalhar com um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="f111e-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="f111e-104">Consultas SQL brutas são úteis se a consulta que você deseja não pode ser expressa usando LINQ.</span><span class="sxs-lookup"><span data-stu-id="f111e-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="f111e-105">As consultas SQL não processadas também são usadas se o uso de uma consulta LINQ resultar em uma consulta SQL ineficiente.</span><span class="sxs-lookup"><span data-stu-id="f111e-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="f111e-106">Consultas SQL brutas podem retornar tipos de entidade regulares ou [tipos de entidade sem](xref:core/modeling/keyless-entity-types) formatação que fazem parte do seu modelo.</span><span class="sxs-lookup"><span data-stu-id="f111e-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="f111e-107">Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="f111e-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="f111e-108">Consultas SQL brutas básicas</span><span class="sxs-lookup"><span data-stu-id="f111e-108">Basic raw SQL queries</span></span>

<span data-ttu-id="f111e-109">Você pode usar o método de extensão `FromSqlRaw` para iniciar uma consulta LINQ com base em uma consulta SQL bruta.</span><span class="sxs-lookup"><span data-stu-id="f111e-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="f111e-110">`FromSqlRaw` só pode ser usada em raízes de consulta, que está diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="f111e-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="f111e-111">As consultas SQL brutas podem ser usadas para executar um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="f111e-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="f111e-112">Passagem de parâmetros</span><span class="sxs-lookup"><span data-stu-id="f111e-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="f111e-113">**Sempre usar parametrização para consultas SQL brutas**</span><span class="sxs-lookup"><span data-stu-id="f111e-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="f111e-114">Ao introduzir qualquer valor fornecido pelo usuário em uma consulta SQL bruta, deve-se ter cuidado para evitar ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="f111e-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="f111e-115">Além de validar que esses valores não contêm caracteres inválidos, sempre use a parametrização que envia os valores separados do texto SQL.</span><span class="sxs-lookup"><span data-stu-id="f111e-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="f111e-116">Em particular, nunca passe uma cadeia de caracteres concatenada ou interpolada (`$""`) com valores fornecidos pelo usuário não validados em `FromSqlRaw` ou `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="f111e-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="f111e-117">Os métodos `FromSqlInterpolated` e `ExecuteSqlInterpolated` permitem usar a sintaxe de interpolação de cadeia de caracteres de forma a proteger contra ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="f111e-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="f111e-118">O exemplo a seguir passa um único parâmetro para um procedimento armazenado, incluindo um espaço reservado de parâmetro na cadeia de caracteres de consulta SQL e fornecendo um argumento adicional.</span><span class="sxs-lookup"><span data-stu-id="f111e-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="f111e-119">Embora essa sintaxe possa parecer com `String.Format` sintaxe, o valor fornecido é encapsulado em um `DbParameter` e o nome de parâmetro gerado inserido onde o espaço reservado `{0}` foi especificado.</span><span class="sxs-lookup"><span data-stu-id="f111e-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="f111e-120">`FromSqlInterpolated` é semelhante a `FromSqlRaw` mas permite que você use a sintaxe de interpolação de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="f111e-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="f111e-121">Assim como `FromSqlRaw`, `FromSqlInterpolated` só pode ser usada em raízes de consulta.</span><span class="sxs-lookup"><span data-stu-id="f111e-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="f111e-122">Assim como no exemplo anterior, o valor é convertido em um `DbParameter` e não é vulnerável à injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="f111e-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="f111e-123">Antes da versão 3,0, `FromSqlRaw` e `FromSqlInterpolated` eram duas sobrecargas nomeadas `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="f111e-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="f111e-124">Para obter mais informações, consulte a [seção versões anteriores](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="f111e-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="f111e-125">Você também pode construir um DbParameter e fornecê-lo como valor de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="f111e-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="f111e-126">Como um espaço reservado de parâmetro SQL regular é usado, em vez de um espaço reservado de cadeia de caracteres, `FromSqlRaw` pode ser usada com segurança:</span><span class="sxs-lookup"><span data-stu-id="f111e-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="f111e-127">`FromSqlRaw` permite que você use parâmetros nomeados na cadeia de caracteres de consulta SQL, o que é útil quando um procedimento armazenado tem parâmetros opcionais:</span><span class="sxs-lookup"><span data-stu-id="f111e-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="f111e-128">Como compor com o LINQ</span><span class="sxs-lookup"><span data-stu-id="f111e-128">Composing with LINQ</span></span>

<span data-ttu-id="f111e-129">Você pode compor na parte superior da consulta SQL bruta inicial usando operadores LINQ.</span><span class="sxs-lookup"><span data-stu-id="f111e-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="f111e-130">EF Core irá tratá-lo como subconsulta e redigir sobre ele no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f111e-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="f111e-131">O exemplo a seguir usa uma consulta SQL bruta que seleciona de uma função com valor de tabela (TVF).</span><span class="sxs-lookup"><span data-stu-id="f111e-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="f111e-132">E, em seguida, compõe-o usando o LINQ para fazer filtragem e classificação.</span><span class="sxs-lookup"><span data-stu-id="f111e-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="f111e-133">A consulta acima gera o seguinte SQL:</span><span class="sxs-lookup"><span data-stu-id="f111e-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="f111e-134">Como incluir dados relacionados</span><span class="sxs-lookup"><span data-stu-id="f111e-134">Including related data</span></span>

<span data-ttu-id="f111e-135">O método `Include` pode ser usado para incluir dados relacionados, assim como em qualquer outra consulta LINQ:</span><span class="sxs-lookup"><span data-stu-id="f111e-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="f111e-136">A composição com o LINQ exige que sua consulta SQL bruta seja combinável, pois EF Core tratará o SQL fornecido como uma subconsulta.</span><span class="sxs-lookup"><span data-stu-id="f111e-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="f111e-137">As consultas SQL que podem ser compostas começam com a palavra-chave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="f111e-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="f111e-138">Além disso, o SQL Passed não deve conter nenhum caractere ou opção que não seja válida em uma subconsulta, como:</span><span class="sxs-lookup"><span data-stu-id="f111e-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="f111e-139">Um ponto e vírgula à direita</span><span class="sxs-lookup"><span data-stu-id="f111e-139">A trailing semicolon</span></span>
- <span data-ttu-id="f111e-140">No SQL Server, uma dica a nível de consulta à direita (por exemplo, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="f111e-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="f111e-141">No SQL Server, uma cláusula `ORDER BY` que não é usada com `OFFSET 0` ou `TOP 100 PERCENT` na cláusula `SELECT`</span><span class="sxs-lookup"><span data-stu-id="f111e-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="f111e-142">SQL Server não permite a composição de chamadas de procedimento armazenado, portanto, qualquer tentativa de aplicar operadores de consulta adicionais a tal chamada resultará em um SQL inválido.</span><span class="sxs-lookup"><span data-stu-id="f111e-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="f111e-143">Use o método `AsEnumerable` ou `AsAsyncEnumerable` logo após `FromSqlRaw` métodos ou `FromSqlInterpolated` para garantir que o EF Core não tente compor um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="f111e-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="f111e-144">Controle de Alterações</span><span class="sxs-lookup"><span data-stu-id="f111e-144">Change Tracking</span></span>

<span data-ttu-id="f111e-145">As consultas que usam os métodos `FromSqlRaw` ou `FromSqlInterpolated` seguem exatamente as mesmas regras de controle de alterações que qualquer outra consulta LINQ no EF Core.</span><span class="sxs-lookup"><span data-stu-id="f111e-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="f111e-146">Por exemplo, se a consulta projetar tipos de entidade, os resultados serão controlados por padrão.</span><span class="sxs-lookup"><span data-stu-id="f111e-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="f111e-147">O exemplo a seguir usa uma consulta SQL bruta que seleciona de uma função com valor de tabela (TVF) e desabilita o controle de alterações com a chamada para `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="f111e-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="f111e-148">Limitações</span><span class="sxs-lookup"><span data-stu-id="f111e-148">Limitations</span></span>

<span data-ttu-id="f111e-149">Algumas limitações deve ser consideradas ao usar consultas SQL brutas:</span><span class="sxs-lookup"><span data-stu-id="f111e-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="f111e-150">A consulta SQL deve retornar dados para todas as propriedades do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="f111e-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="f111e-151">Os nomes das colunas no conjunto de resultados devem corresponder aos nomes das colunas para as quais as propriedades são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="f111e-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="f111e-152">Observe que esse comportamento é diferente de EF6.</span><span class="sxs-lookup"><span data-stu-id="f111e-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="f111e-153">EF6 ignorou a propriedade para o mapeamento de coluna para consultas SQL brutas e nomes de coluna de conjunto de resultados tinham que corresponder aos nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="f111e-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="f111e-154">A consulta SQL não pode conter dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="f111e-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="f111e-155">No entanto, em muitos casos é possível combinar com base na consulta usando o operador `Include` para retornar dados relacionados (confira [Como incluir dados relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="f111e-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="f111e-156">Versões anteriores</span><span class="sxs-lookup"><span data-stu-id="f111e-156">Previous versions</span></span>

<span data-ttu-id="f111e-157">EF Core versão 2,2 e anterior tinham duas sobrecargas de método chamada `FromSql`, que se compararam da mesma maneira que as `FromSqlRaw` e `FromSqlInterpolated`mais recentes.</span><span class="sxs-lookup"><span data-stu-id="f111e-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="f111e-158">Era fácil chamar acidentalmente o método de cadeia de caracteres bruto quando a intenção era chamar o método de cadeia de caracteres interpolado e o contrário.</span><span class="sxs-lookup"><span data-stu-id="f111e-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="f111e-159">Chamar a sobrecarga errada acidentalmente pode resultar em consultas que não estão sendo parametrizadas quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="f111e-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
