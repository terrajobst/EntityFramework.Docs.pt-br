---
title: Consultas SQL brutas – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 343162596780e6146b57f73a38221701009cd855
ms.sourcegitcommit: 85d17524d8e022f933cde7fc848313f57dfd3eb8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55760503"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="a0fec-102">Consultas SQL brutas</span><span class="sxs-lookup"><span data-stu-id="a0fec-102">Raw SQL Queries</span></span>

<span data-ttu-id="a0fec-103">O Entity Framework Core permite acessar uma lista suspensa de consultas SQL brutas ao trabalhar com um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="a0fec-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="a0fec-104">Isso pode ser útil quando a consulta que você deseja realizar não pode ser expressa usando o LINQ ou quando o uso de uma consulta do LINQ resulta em consultas SQL ineficientes.</span><span class="sxs-lookup"><span data-stu-id="a0fec-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="a0fec-105">Consultas SQL brutas podem retornar tipos de entidade ou, a partir do EF Core 2.1, [tipos de consulta](xref:core/modeling/query-types) que fazem parte do seu modelo.</span><span class="sxs-lookup"><span data-stu-id="a0fec-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="a0fec-106">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="a0fec-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="a0fec-107">Consultas SQL brutas básicas</span><span class="sxs-lookup"><span data-stu-id="a0fec-107">Basic raw SQL queries</span></span>

<span data-ttu-id="a0fec-108">Você pode usar o método de extensão *FromSql* para iniciar uma consulta LINQ com base em uma consulta SQL bruta.</span><span class="sxs-lookup"><span data-stu-id="a0fec-108">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="a0fec-109">As consultas SQL brutas podem ser usadas para executar um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="a0fec-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="a0fec-110">Passando parâmetros</span><span class="sxs-lookup"><span data-stu-id="a0fec-110">Passing parameters</span></span>

<span data-ttu-id="a0fec-111">Assim como acontece com qualquer API que aceita SQL, é importante parametrizar qualquer entrada do usuário para proteger contra um ataque de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="a0fec-111">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="a0fec-112">Você pode incluir espaços reservados de parâmetros na cadeia de caracteres de consulta SQL e fornecer os valores de parâmetros como argumentos adicionais.</span><span class="sxs-lookup"><span data-stu-id="a0fec-112">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="a0fec-113">Qualquer valor de parâmetro fornecido será automaticamente convertido em um `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="a0fec-113">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="a0fec-114">O exemplo a seguir aprova um único parâmetro para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="a0fec-114">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="a0fec-115">Embora isso possa parecer a sintaxe `String.Format`, o valor fornecido é encapsulado em um parâmetro e o nome de parâmetro gerado é inserido onde o espaço reservado `{0}` foi especificado.</span><span class="sxs-lookup"><span data-stu-id="a0fec-115">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="a0fec-116">Essa é a mesma consulta, mas usando a sintaxe de interpolação de cadeia de caracteres, que tem suporte no EF Core 2.0 e posterior:</span><span class="sxs-lookup"><span data-stu-id="a0fec-116">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="a0fec-117">Você também pode construir um DbParameter e fornecê-lo como valor de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="a0fec-117">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="a0fec-118">Isso permite usar parâmetros nomeados na cadeia de caracteres de consulta SQL.</span><span class="sxs-lookup"><span data-stu-id="a0fec-118">This allows you to use named parameters in the SQL query string.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="a0fec-119">Como compor com o LINQ</span><span class="sxs-lookup"><span data-stu-id="a0fec-119">Composing with LINQ</span></span>

<span data-ttu-id="a0fec-120">Se a consulta SQL puder ser composta no banco de dados, é possível compor com base na consulta SQL bruta inicial usando os operadores LINQ.</span><span class="sxs-lookup"><span data-stu-id="a0fec-120">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="a0fec-121">As consultas SQL que podem ser compostas começam com a palavra-chave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="a0fec-121">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="a0fec-122">O exemplo a seguir usa uma consulta SQL bruta que é selecionada de uma Função com Valor de Tabela (TVF) e compõe com base nela usando o LINQ para filtrar e classificar.</span><span class="sxs-lookup"><span data-stu-id="a0fec-122">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a><span data-ttu-id="a0fec-123">Controle de Alterações</span><span class="sxs-lookup"><span data-stu-id="a0fec-123">Change Tracking</span></span>

<span data-ttu-id="a0fec-124">As consultas que usam o `FromSql()` seguem exatamente as mesmas regras de controle de alterações que qualquer outra consulta LINQ no EF Core.</span><span class="sxs-lookup"><span data-stu-id="a0fec-124">Queries that use the `FromSql()` follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="a0fec-125">Por exemplo, se a consulta projetar tipos de entidade, os resultados serão controlados por padrão.</span><span class="sxs-lookup"><span data-stu-id="a0fec-125">For example, if the query projects entity types, the results will be tracked by default.</span></span>  

<span data-ttu-id="a0fec-126">O exemplo a seguir usa uma consulta SQL bruta que seleciona em uma TVF (função com valor de tabela) e, em seguida, desabilita o controle de alterações com a chamada para .AsNoTracking():</span><span class="sxs-lookup"><span data-stu-id="a0fec-126">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with teh call to .AsNoTracking():</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="a0fec-127">Como incluir dados relacionados</span><span class="sxs-lookup"><span data-stu-id="a0fec-127">Including related data</span></span>

<span data-ttu-id="a0fec-128">O método `Include()` pode ser usado para incluir dados relacionados, assim como em qualquer outra consulta LINQ:</span><span class="sxs-lookup"><span data-stu-id="a0fec-128">The `Include()` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a><span data-ttu-id="a0fec-129">Limitações</span><span class="sxs-lookup"><span data-stu-id="a0fec-129">Limitations</span></span>

<span data-ttu-id="a0fec-130">Algumas limitações deve ser consideradas ao usar consultas SQL brutas:</span><span class="sxs-lookup"><span data-stu-id="a0fec-130">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="a0fec-131">A consulta SQL deve retornar dados para todas as propriedades da entidade ou tipo de consulta.</span><span class="sxs-lookup"><span data-stu-id="a0fec-131">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="a0fec-132">Os nomes das colunas no conjunto de resultados devem corresponder aos nomes das colunas para as quais as propriedades são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="a0fec-132">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="a0fec-133">Observe que isso é diferente do EF6, onde o mapeamento de coluna/propriedade foi ignorado para consultas SQL brutas e os nomes das colunas do conjunto de resultados tinham que corresponder aos nomes das propriedades.</span><span class="sxs-lookup"><span data-stu-id="a0fec-133">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="a0fec-134">A consulta SQL não pode conter dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="a0fec-134">The SQL query cannot contain related data.</span></span> <span data-ttu-id="a0fec-135">No entanto, em muitos casos é possível combinar com base na consulta usando o operador `Include` para retornar dados relacionados (confira [Como incluir dados relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="a0fec-135">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="a0fec-136">Instruções `SELECT` aprovadas para este método devem normalmente ser combináveis: se o EF Core precisar avaliar operadores de consulta adicionais no servidor (por exemplo, para traduzir operadores LINQ aplicados após `FromSql`), o SQL fornecido será tratado como uma consulta aninhada.</span><span class="sxs-lookup"><span data-stu-id="a0fec-136">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="a0fec-137">Isso significa que o SQL aprovado não deve conter nenhum caractere ou opção não válida em uma subconsulta, como:</span><span class="sxs-lookup"><span data-stu-id="a0fec-137">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="a0fec-138">um ponto-e-vírgula à direita</span><span class="sxs-lookup"><span data-stu-id="a0fec-138">a trailing semicolon</span></span>
  * <span data-ttu-id="a0fec-139">No SQL Server, uma dica a nível de consulta à direita (por exemplo, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="a0fec-139">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="a0fec-140">No SQL Server, um cláusula `ORDER BY` não é acompanhada de `TOP 100 PERCENT` na cláusula `SELECT`</span><span class="sxs-lookup"><span data-stu-id="a0fec-140">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="a0fec-141">As instruções SQL, diferentes de `SELECT`, são reconhecidas automaticamente como não combináveis.</span><span class="sxs-lookup"><span data-stu-id="a0fec-141">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="a0fec-142">Como consequência, os resultados completos de procedimentos armazenados são sempre retornados ao cliente e os operadores LINQ aplicados após `FromSql` são avaliados na memória.</span><span class="sxs-lookup"><span data-stu-id="a0fec-142">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

> [!WARNING]  
> <span data-ttu-id="a0fec-143">**Sempre use a parametrização para consultas SQL brutas:** as APIs que aceitam uma cadeia de caracteres SQL, como `FromSql` e `ExecuteSqlCommand`, permitem que os valores sejam facilmente aprovados como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="a0fec-143">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="a0fec-144">Além de validar a entrada do usuário, sempre use a parametrização para os valores usados em um comando/consulta SQL bruta.</span><span class="sxs-lookup"><span data-stu-id="a0fec-144">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="a0fec-145">Se você estiver usando a concatenação de cadeias de caracteres para criar de forma dinâmica qualquer parte da cadeia de caracteres de consulta, você é responsável por validar qualquer entrada para proteger-se de ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="a0fec-145">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
