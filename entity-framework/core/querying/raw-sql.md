---
title: Consultas SQL brutas – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 29b7e20e875bf791a88a92636c1df4bc4e31656b
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163207"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="70fc1-102">Consultas SQL brutas</span><span class="sxs-lookup"><span data-stu-id="70fc1-102">Raw SQL Queries</span></span>

<span data-ttu-id="70fc1-103">O Entity Framework Core permite acessar uma lista suspensa de consultas SQL brutas ao trabalhar com um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="70fc1-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="70fc1-104">Isso pode ser útil se a consulta que você deseja realizar não puder ser expressa usando o LINQ ou se o uso de uma consulta do LINQ estiver resultando no envio de um SQL ineficiente para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="70fc1-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="70fc1-105">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="70fc1-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="70fc1-106">Limitações</span><span class="sxs-lookup"><span data-stu-id="70fc1-106">Limitations</span></span>

<span data-ttu-id="70fc1-107">Algumas limitações deve ser consideradas ao usar consultas SQL brutas:</span><span class="sxs-lookup"><span data-stu-id="70fc1-107">There are a few limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="70fc1-108">As consultas SQL só podem ser usadas para retornar tipos de entidade que fazem parte do seu modelo.</span><span class="sxs-lookup"><span data-stu-id="70fc1-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="70fc1-109">Há um aprimoramento na sua lista de pendências para [habilitar o retorno de tipos de ad hoc de consultas SQL brutas](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="70fc1-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="70fc1-110">A consulta SQL deve retornar dados para todas as propriedades da entidade ou tipo de consulta.</span><span class="sxs-lookup"><span data-stu-id="70fc1-110">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="70fc1-111">Os nomes das colunas no conjunto de resultados devem corresponder aos nomes das colunas para as quais as propriedades são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="70fc1-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="70fc1-112">Observe que isso é diferente do EF6, onde o mapeamento de coluna/propriedade foi ignorado para consultas SQL brutas e os nomes das colunas do conjunto de resultados tinham que corresponder aos nomes das propriedades.</span><span class="sxs-lookup"><span data-stu-id="70fc1-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="70fc1-113">A consulta SQL não pode conter dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="70fc1-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="70fc1-114">No entanto, em muitos casos é possível combinar com base na consulta usando o operador `Include` para retornar dados relacionados (confira [Como incluir dados relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="70fc1-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="70fc1-115">As instruções `SELECT` aprovadas para este método devem normalmente ser combináveis: se o EF Core precisar avaliar operadores de consulta adicionais no servidor (por exemplo, para traduzir operadores LINQ aplicados após `FromSql`), o SQL fornecido será tratado como uma subconsulta.</span><span class="sxs-lookup"><span data-stu-id="70fc1-115">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (e.g. to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="70fc1-116">Isso significa que o SQL aprovado não deve conter nenhum caractere ou opção não válida em uma subconsulta, como:</span><span class="sxs-lookup"><span data-stu-id="70fc1-116">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="70fc1-117">um ponto-e-vírgula à direita</span><span class="sxs-lookup"><span data-stu-id="70fc1-117">a trailing semicolon</span></span>
  * <span data-ttu-id="70fc1-118">No SQL Server, uma dica a nível de consulta à direita, por exemplo, `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="70fc1-118">On SQL Server, a trailing query-level hint, e.g. `OPTION (HASH JOIN)`</span></span>
  * <span data-ttu-id="70fc1-119">No SQL Server, um cláusula `ORDER BY` não é acompanhada de `TOP 100 PERCENT` na cláusula `SELECT`</span><span class="sxs-lookup"><span data-stu-id="70fc1-119">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="70fc1-120">As instruções SQL, diferentes de `SELECT`, são reconhecidas automaticamente como não combináveis.</span><span class="sxs-lookup"><span data-stu-id="70fc1-120">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="70fc1-121">Como consequência, os resultados completos de procedimentos armazenados são sempre retornados ao cliente e os operadores LINQ aplicados após `FromSql` são avaliados na memória.</span><span class="sxs-lookup"><span data-stu-id="70fc1-121">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span> 

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="70fc1-122">Consultas SQL brutas básicas</span><span class="sxs-lookup"><span data-stu-id="70fc1-122">Basic raw SQL queries</span></span>

<span data-ttu-id="70fc1-123">Você pode usar o método de extensão *FromSql* para iniciar uma consulta LINQ com base em uma consulta SQL bruta.</span><span class="sxs-lookup"><span data-stu-id="70fc1-123">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="70fc1-124">As consultas SQL brutas podem ser usadas para executar um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="70fc1-124">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="70fc1-125">Passando parâmetros</span><span class="sxs-lookup"><span data-stu-id="70fc1-125">Passing parameters</span></span>

<span data-ttu-id="70fc1-126">Assim como acontece com qualquer API que aceita SQL, é importante parametrizar qualquer entrada do usuário para proteger contra um ataque de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="70fc1-126">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="70fc1-127">Você pode incluir espaços reservados de parâmetros na cadeia de caracteres de consulta SQL e fornecer os valores de parâmetros como argumentos adicionais.</span><span class="sxs-lookup"><span data-stu-id="70fc1-127">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="70fc1-128">Qualquer valor de parâmetro fornecido será automaticamente convertido em um `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="70fc1-128">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="70fc1-129">O exemplo a seguir aprova um único parâmetro para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="70fc1-129">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="70fc1-130">Embora isso possa parecer a sintaxe `String.Format`, o valor fornecido é encapsulado em um parâmetro e o nome de parâmetro gerado é inserido onde o espaço reservado `{0}` foi especificado.</span><span class="sxs-lookup"><span data-stu-id="70fc1-130">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="70fc1-131">Essa é a mesma consulta, mas usando a sintaxe de interpolação de cadeia de caracteres, que tem suporte no EF Core 2.0 e posterior:</span><span class="sxs-lookup"><span data-stu-id="70fc1-131">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="70fc1-132">Você também pode construir um DbParameter e fornecê-lo como valor de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="70fc1-132">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="70fc1-133">Isso permite usar parâmetros nomeados na cadeia de caracteres de consulta SQL</span><span class="sxs-lookup"><span data-stu-id="70fc1-133">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="70fc1-134">Como compor com o LINQ</span><span class="sxs-lookup"><span data-stu-id="70fc1-134">Composing with LINQ</span></span>

<span data-ttu-id="70fc1-135">Se a consulta SQL puder ser composta no banco de dados, é possível compor com base na consulta SQL bruta inicial usando os operadores LINQ.</span><span class="sxs-lookup"><span data-stu-id="70fc1-135">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="70fc1-136">As consultas SQL que podem ser compostas com a palavra-chave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="70fc1-136">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="70fc1-137">O exemplo a seguir usa uma consulta SQL bruta que é selecionada de uma Função com Valor de Tabela (TVF) e compõe com base nela usando o LINQ para filtrar e classificar.</span><span class="sxs-lookup"><span data-stu-id="70fc1-137">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="70fc1-138">Como incluir dados relacionados</span><span class="sxs-lookup"><span data-stu-id="70fc1-138">Including related data</span></span>

<span data-ttu-id="70fc1-139">A composição com operadores LINQ pode ser usada para incluir dados relacionados na consulta.</span><span class="sxs-lookup"><span data-stu-id="70fc1-139">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="70fc1-140">**Sempre use a parametrização para consultas SQL brutas:** as APIs que aceitam uma cadeia de caracteres SQL, como `FromSql` e `ExecuteSqlCommand`, permitem que os valores sejam facilmente aprovados como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70fc1-140">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="70fc1-141">Além de validar a entrada do usuário, sempre use a parametrização para os valores usados em um comando/consulta SQL bruta.</span><span class="sxs-lookup"><span data-stu-id="70fc1-141">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="70fc1-142">Se você estiver usando a concatenação de cadeias de caracteres para criar de forma dinâmica qualquer parte da cadeia de caracteres de consulta, você é responsável por validar qualquer entrada para proteger-se de ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="70fc1-142">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
