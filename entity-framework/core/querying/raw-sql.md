---
title: Consultas SQL brutas – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813590"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="bb377-102">Consultas SQL brutas</span><span class="sxs-lookup"><span data-stu-id="bb377-102">Raw SQL Queries</span></span>

<span data-ttu-id="bb377-103">O Entity Framework Core permite acessar uma lista suspensa de consultas SQL brutas ao trabalhar com um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="bb377-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="bb377-104">Isso pode ser útil se a consulta que você deseja executar não pode ser expressa usando LINQ ou se usar uma consulta LINQ resulta em uma consulta SQL ineficiente.</span><span class="sxs-lookup"><span data-stu-id="bb377-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="bb377-105">Consultas SQL brutas podem retornar tipos de entidade regulares ou [tipos de entidade sem](xref:core/modeling/keyless-entity-types) formatação que fazem parte do seu modelo.</span><span class="sxs-lookup"><span data-stu-id="bb377-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="bb377-106">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="bb377-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="bb377-107">Consultas SQL brutas básicas</span><span class="sxs-lookup"><span data-stu-id="bb377-107">Basic raw SQL queries</span></span>

<span data-ttu-id="bb377-108">Você pode usar o `FromSqlRaw` método de extensão para iniciar uma consulta LINQ com base em uma consulta SQL bruta.</span><span class="sxs-lookup"><span data-stu-id="bb377-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="bb377-109">As consultas SQL brutas podem ser usadas para executar um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="bb377-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="bb377-110">Passando parâmetros</span><span class="sxs-lookup"><span data-stu-id="bb377-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="bb377-111">**Sempre usar parametrização para consultas SQL brutas**</span><span class="sxs-lookup"><span data-stu-id="bb377-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="bb377-112">Ao introduzir qualquer valor fornecido pelo usuário em uma consulta SQL bruta, deve-se ter cuidado para evitar ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="bb377-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="bb377-113">Além de validar que esses valores não contêm caracteres inválidos, sempre use a parametrização que envia os valores separados do texto SQL.</span><span class="sxs-lookup"><span data-stu-id="bb377-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="bb377-114">Em particular, nunca passe uma cadeia de caracteres concatenada ou interpolada (`$""`) com valores fornecidos pelo usuário não validados no ou `ExecuteSqlRaw`no `FromSqlRaw` .</span><span class="sxs-lookup"><span data-stu-id="bb377-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="bb377-115">Os `FromSqlInterpolated` métodos `ExecuteSqlInterpolated` e permitem usar a sintaxe de interpolação de cadeia de caracteres de forma a proteger contra ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="bb377-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="bb377-116">O exemplo a seguir passa um único parâmetro para um procedimento armazenado, incluindo um espaço reservado de parâmetro na cadeia de caracteres de consulta SQL e fornecendo um argumento adicional.</span><span class="sxs-lookup"><span data-stu-id="bb377-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="bb377-117">Embora isso possa parecer `String.Format` com a sintaxe, o valor fornecido é encapsulado em um `DbParameter` e o nome de parâmetro gerado `{0}` inserido onde o espaço reservado foi especificado.</span><span class="sxs-lookup"><span data-stu-id="bb377-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="bb377-118">Como alternativa ao `FromSqlRaw`, você pode usar `FromSqlInterpolated` o que permite o uso seguro da interpolação de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="bb377-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="bb377-119">Assim como no exemplo anterior, o valor é convertido em `DbParameter` a e, portanto, não é vulnerável à injeção de SQL:</span><span class="sxs-lookup"><span data-stu-id="bb377-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="bb377-120">Antes da versão 3,0, `FromSqlRaw` e `FromSqlInterpolated` foram duas sobrecargas nomeadas `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="bb377-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="bb377-121">Consulte a [seção versões anteriores](#previous-versions) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="bb377-121">See the [previous versions section](#previous-versions) for more details.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="bb377-122">Você também pode construir um DbParameter e fornecê-lo como valor de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="bb377-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="bb377-123">Como um espaço reservado de parâmetro SQL regular é usado, em vez de um `FromSqlRaw` espaço reservado de cadeia de caracteres, pode ser usado com segurança:</span><span class="sxs-lookup"><span data-stu-id="bb377-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="bb377-124">Isso permite que você use parâmetros nomeados na cadeia de consulta SQL, o que é útil quando um procedimento armazenado tem parâmetros opcionais:</span><span class="sxs-lookup"><span data-stu-id="bb377-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="bb377-125">Como compor com o LINQ</span><span class="sxs-lookup"><span data-stu-id="bb377-125">Composing with LINQ</span></span>

<span data-ttu-id="bb377-126">Se a consulta SQL puder ser composta no banco de dados, é possível compor com base na consulta SQL bruta inicial usando os operadores LINQ.</span><span class="sxs-lookup"><span data-stu-id="bb377-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="bb377-127">As consultas SQL que podem ser compostas começam com a palavra-chave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="bb377-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="bb377-128">O exemplo a seguir usa uma consulta SQL bruta que é selecionada de uma Função com Valor de Tabela (TVF) e compõe com base nela usando o LINQ para filtrar e classificar.</span><span class="sxs-lookup"><span data-stu-id="bb377-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="bb377-129">Isso produzirá a seguinte consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="bb377-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="bb377-130">Controle de Alterações</span><span class="sxs-lookup"><span data-stu-id="bb377-130">Change Tracking</span></span>

<span data-ttu-id="bb377-131">As consultas que usam `FromSql` os métodos seguem exatamente as mesmas regras de controle de alterações que qualquer outra consulta LINQ no EF Core.</span><span class="sxs-lookup"><span data-stu-id="bb377-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="bb377-132">Por exemplo, se a consulta projetar tipos de entidade, os resultados serão controlados por padrão.</span><span class="sxs-lookup"><span data-stu-id="bb377-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="bb377-133">O exemplo a seguir usa uma consulta SQL bruta que seleciona de uma função com valor de tabela (TVF) e desabilita o controle de alterações com a chamada `AsNoTracking`para:</span><span class="sxs-lookup"><span data-stu-id="bb377-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="bb377-134">Como incluir dados relacionados</span><span class="sxs-lookup"><span data-stu-id="bb377-134">Including related data</span></span>

<span data-ttu-id="bb377-135">O método `Include` pode ser usado para incluir dados relacionados, assim como em qualquer outra consulta LINQ:</span><span class="sxs-lookup"><span data-stu-id="bb377-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="bb377-136">Observe que isso requer que sua consulta SQL bruta seja combinável; em especial, não funcionará com chamadas de procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="bb377-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="bb377-137">Consulte as observações sobre a capacidade de composição em [limitações](#limitations)).</span><span class="sxs-lookup"><span data-stu-id="bb377-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="bb377-138">Limitações</span><span class="sxs-lookup"><span data-stu-id="bb377-138">Limitations</span></span>

<span data-ttu-id="bb377-139">Algumas limitações deve ser consideradas ao usar consultas SQL brutas:</span><span class="sxs-lookup"><span data-stu-id="bb377-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="bb377-140">A consulta SQL deve retornar dados para todas as propriedades do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="bb377-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="bb377-141">Os nomes das colunas no conjunto de resultados devem corresponder aos nomes das colunas para as quais as propriedades são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="bb377-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="bb377-142">Observe que isso é diferente do EF6, onde o mapeamento de coluna/propriedade foi ignorado para consultas SQL brutas e os nomes das colunas do conjunto de resultados tinham que corresponder aos nomes das propriedades.</span><span class="sxs-lookup"><span data-stu-id="bb377-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="bb377-143">A consulta SQL não pode conter dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="bb377-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="bb377-144">No entanto, em muitos casos é possível combinar com base na consulta usando o operador `Include` para retornar dados relacionados (confira [Como incluir dados relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="bb377-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="bb377-145">Instruções `SELECT` aprovadas para este método devem normalmente ser combináveis: Se EF Core precisar avaliar operadores de consulta adicionais no servidor (por exemplo, para converter operadores LINQ aplicados após `FromSql` métodos), o SQL fornecido será tratado como uma subconsulta.</span><span class="sxs-lookup"><span data-stu-id="bb377-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="bb377-146">Isso significa que o SQL aprovado não deve conter nenhum caractere ou opção não válida em uma subconsulta, como:</span><span class="sxs-lookup"><span data-stu-id="bb377-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="bb377-147">um ponto e vírgula à direita</span><span class="sxs-lookup"><span data-stu-id="bb377-147">A trailing semicolon</span></span>
  * <span data-ttu-id="bb377-148">No SQL Server, uma dica a nível de consulta à direita (por exemplo, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="bb377-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="bb377-149">No SQL Server, uma cláusula `ORDER BY` não é acompanhada de `OFFSET 0` OR `TOP 100 PERCENT` na cláusula `SELECT`</span><span class="sxs-lookup"><span data-stu-id="bb377-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="bb377-150">Observe que SQL Server não permite a composição de chamadas de procedimento armazenado, portanto, qualquer tentativa de aplicar operadores de consulta adicionais a tal chamada resultará em um SQL inválido.</span><span class="sxs-lookup"><span data-stu-id="bb377-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="bb377-151">Os operadores de consulta podem ser `AsEnumerable()` introduzidos depois para avaliação do cliente.</span><span class="sxs-lookup"><span data-stu-id="bb377-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

## <a name="previous-versions"></a><span data-ttu-id="bb377-152">Versões anteriores</span><span class="sxs-lookup"><span data-stu-id="bb377-152">Previous versions</span></span>

<span data-ttu-id="bb377-153">EF core versão 2,2 e anterior tinham duas sobrecargas chamadas `FromSql` que se compararam da mesma maneira que as mais `FromSqlRaw` recentes `FromSqlInterpolated`e.</span><span class="sxs-lookup"><span data-stu-id="bb377-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="bb377-154">Isso tornou muito fácil chamar acidentalmente o método de cadeia de caracteres bruto quando a intenção era chamar o método de cadeia de caracteres interpolado e o contrário.</span><span class="sxs-lookup"><span data-stu-id="bb377-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="bb377-155">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="bb377-155">This could result in queries not being parameterized when they should have been.</span></span>
