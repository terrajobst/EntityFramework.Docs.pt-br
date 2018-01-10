---
title: Consultas SQL bruto - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 79894c7b9fd9e40cdf14460abf5d872ee2f4b9f0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="59d2c-102">Consultas SQL bruto</span><span class="sxs-lookup"><span data-stu-id="59d2c-102">Raw SQL Queries</span></span>

<span data-ttu-id="59d2c-103">Entity Framework Core permite que a lista suspensa brutas consultas SQL ao trabalhar com um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="59d2c-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="59d2c-104">Isso pode ser útil se a consulta que você deseja executar não pode ser expressada usando LINQ, ou se usando uma consulta LINQ está resultando em ineficiente SQL sendo enviado para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="59d2c-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="59d2c-105">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="59d2c-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="59d2c-106">Limitações</span><span class="sxs-lookup"><span data-stu-id="59d2c-106">Limitations</span></span>

<span data-ttu-id="59d2c-107">Há algumas limitações a serem consideradas ao usar consultas SQL brutas:</span><span class="sxs-lookup"><span data-stu-id="59d2c-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="59d2c-108">Consultas SQL só podem ser usadas para retornar os tipos de entidade que fazem parte do seu modelo.</span><span class="sxs-lookup"><span data-stu-id="59d2c-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="59d2c-109">Há um aprimoramento na nossa lista de pendências para [habilitar retornando tipos de ad hoc de consultas SQL brutas](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="59d2c-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="59d2c-110">A consulta SQL deve retornar dados de todas as propriedades do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="59d2c-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="59d2c-111">Os nomes de coluna no conjunto de resultados devem corresponder aos nomes de coluna mapeados para propriedades.</span><span class="sxs-lookup"><span data-stu-id="59d2c-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="59d2c-112">Observe que isso é diferente de EF6 onde o mapeamento de coluna/propriedade foi ignorado para consultas SQL brutas e tinham nomes que correspondam aos nomes de propriedade de coluna do conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="59d2c-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="59d2c-113">A consulta SQL não pode conter dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="59d2c-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="59d2c-114">No entanto, em muitos casos você pode compor sobre a consulta usando o `Include` operador para retornar dados relacionados (consulte [incluindo dados relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="59d2c-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="59d2c-115">`SELECT`instruções passadas para este método devem normalmente ser combináveis: núcleo de EF se precisa avaliar os operadores de consulta adicionais no servidor (por exemplo, converter operadores LINQ aplicadas após `FromSql`), SQL fornecido será tratado como uma subconsulta.</span><span class="sxs-lookup"><span data-stu-id="59d2c-115">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (e.g. to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="59d2c-116">Isso significa que o SQL passado não deve conter todos os caracteres ou opções que não são válidas em uma subconsulta, tais como:</span><span class="sxs-lookup"><span data-stu-id="59d2c-116">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="59d2c-117">um ponto e vírgula à direita</span><span class="sxs-lookup"><span data-stu-id="59d2c-117">a trailing semicolon</span></span>
  * <span data-ttu-id="59d2c-118">No SQL Server, um nível de consulta à direita dica, por exemplo`OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="59d2c-118">On SQL Server, a trailing query-level hint, e.g. `OPTION (HASH JOIN)`</span></span>
  * <span data-ttu-id="59d2c-119">No SQL Server, um `ORDER BY` cláusula não é acompanhada de `TOP 100 PERCENT` no `SELECT` cláusula</span><span class="sxs-lookup"><span data-stu-id="59d2c-119">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="59d2c-120">Instruções SQL que `SELECT` são reconhecidos automaticamente como não combinável.</span><span class="sxs-lookup"><span data-stu-id="59d2c-120">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="59d2c-121">Como consequência, os resultados completos de procedimentos armazenados sempre são retornados ao cliente e os operadores LINQ aplicadas após `FromSql` são avaliadas na memória.</span><span class="sxs-lookup"><span data-stu-id="59d2c-121">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span> 

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="59d2c-122">Consultas SQL brutas básicas</span><span class="sxs-lookup"><span data-stu-id="59d2c-122">Basic raw SQL queries</span></span>

<span data-ttu-id="59d2c-123">Você pode usar o *FromSql* método de extensão para iniciar uma consulta LINQ com base em uma consulta SQL bruta.</span><span class="sxs-lookup"><span data-stu-id="59d2c-123">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="59d2c-124">Consultas SQL brutas podem ser usadas para executar um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="59d2c-124">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="59d2c-125">Passando parâmetros</span><span class="sxs-lookup"><span data-stu-id="59d2c-125">Passing parameters</span></span>

<span data-ttu-id="59d2c-126">Assim como acontece com qualquer API que aceita SQL, é importante parametrizar qualquer entrada do usuário para proteger contra um ataque de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="59d2c-126">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="59d2c-127">Você pode incluir espaços reservados de parâmetros na cadeia de caracteres de consulta SQL e, em seguida, fornece valores de parâmetros como argumentos adicionais.</span><span class="sxs-lookup"><span data-stu-id="59d2c-127">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="59d2c-128">Quaisquer valores de parâmetro que você fornecer serão automaticamente convertidos para um `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="59d2c-128">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="59d2c-129">O exemplo a seguir passa um único parâmetro para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="59d2c-129">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="59d2c-130">Enquanto isso pode parecer como `String.Format` sintaxe, o valor fornecido é encapsulado em um parâmetro e o nome de parâmetro gerado inserida onde o `{0}` espaço reservado foi especificado.</span><span class="sxs-lookup"><span data-stu-id="59d2c-130">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="59d2c-131">Essa é a mesma consulta, mas usando a sintaxe de interpolação de cadeia de caracteres, que tem suporte no EF Core 2.0 e posterior:</span><span class="sxs-lookup"><span data-stu-id="59d2c-131">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="59d2c-132">Você também pode criar um DbParameter e fornecê-lo como um valor de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="59d2c-132">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="59d2c-133">Isso permite que você use parâmetros nomeados na cadeia de caracteres de consulta SQL</span><span class="sxs-lookup"><span data-stu-id="59d2c-133">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="59d2c-134">Compor com LINQ</span><span class="sxs-lookup"><span data-stu-id="59d2c-134">Composing with LINQ</span></span>

<span data-ttu-id="59d2c-135">Se a consulta SQL pode ser composta no banco de dados, você pode compor sobre a consulta SQL bruta inicial usando operadores LINQ.</span><span class="sxs-lookup"><span data-stu-id="59d2c-135">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="59d2c-136">Consultas SQL que podem ser compostas em ser com o `SELECT` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="59d2c-136">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="59d2c-137">O exemplo a seguir usa uma consulta SQL bruta que seleciona a partir de uma função com valor de tabela (TVF) e, em seguida, compõe usando LINQ para executar a filtragem e classificação.</span><span class="sxs-lookup"><span data-stu-id="59d2c-137">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="59d2c-138">Incluindo dados relacionados</span><span class="sxs-lookup"><span data-stu-id="59d2c-138">Including related data</span></span>

<span data-ttu-id="59d2c-139">Compondo operadores LINQ pode ser usado para incluir dados relacionados na consulta.</span><span class="sxs-lookup"><span data-stu-id="59d2c-139">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="59d2c-140">**Sempre use a parametrização para consultas SQL brutas:** APIs que aceitam um SQL bruto da cadeia de caracteres como `FromSql` e `ExecuteSqlCommand` permitir valores facilmente ser passados como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="59d2c-140">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="59d2c-141">Além de validar a entrada do usuário, sempre use parametrização para os valores usados em um consulta SQL bruto/comando.</span><span class="sxs-lookup"><span data-stu-id="59d2c-141">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="59d2c-142">Se você estiver usando a concatenação de cadeia de caracteres para criar dinamicamente a qualquer parte da cadeia de caracteres de consulta, você será responsável por validar qualquer entrada para proteger contra ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="59d2c-142">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
