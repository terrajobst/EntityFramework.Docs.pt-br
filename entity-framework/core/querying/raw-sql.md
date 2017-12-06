---
title: Consultas SQL bruto - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="37cfc-102">Consultas SQL bruto</span><span class="sxs-lookup"><span data-stu-id="37cfc-102">Raw SQL Queries</span></span>

<span data-ttu-id="37cfc-103">Entity Framework Core permite que a lista suspensa brutas consultas SQL ao trabalhar com um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="37cfc-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="37cfc-104">Isso pode ser útil se a consulta que você deseja executar não pode ser expressada usando LINQ, ou se usando uma consulta LINQ está resultando em ineficiente SQL sendo enviado para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="37cfc-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="37cfc-105">Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="37cfc-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="37cfc-106">Limitações</span><span class="sxs-lookup"><span data-stu-id="37cfc-106">Limitations</span></span>

<span data-ttu-id="37cfc-107">Há algumas limitações a serem consideradas ao usar consultas SQL brutas:</span><span class="sxs-lookup"><span data-stu-id="37cfc-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="37cfc-108">Consultas SQL só podem ser usadas para retornar os tipos de entidade que fazem parte do seu modelo.</span><span class="sxs-lookup"><span data-stu-id="37cfc-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="37cfc-109">Há um aprimoramento na nossa lista de pendências para [habilitar retornando tipos de ad hoc de consultas SQL brutas](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="37cfc-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="37cfc-110">A consulta SQL deve retornar dados de todas as propriedades do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="37cfc-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="37cfc-111">Os nomes de coluna no conjunto de resultados devem corresponder aos nomes de coluna mapeados para propriedades.</span><span class="sxs-lookup"><span data-stu-id="37cfc-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="37cfc-112">Observe que isso é diferente de EF6 onde o mapeamento de coluna/propriedade foi ignorado para consultas SQL brutas e tinham nomes que correspondam aos nomes de propriedade de coluna do conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="37cfc-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="37cfc-113">A consulta SQL não pode conter dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="37cfc-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="37cfc-114">No entanto, em muitos casos você pode compor sobre a consulta usando o `Include` operador para retornar dados relacionados (consulte [incluindo dados relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="37cfc-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="37cfc-115">Consultas SQL brutas básicas</span><span class="sxs-lookup"><span data-stu-id="37cfc-115">Basic raw SQL queries</span></span>

<span data-ttu-id="37cfc-116">Você pode usar o *FromSql* método de extensão para iniciar uma consulta LINQ com base em uma consulta SQL bruta.</span><span class="sxs-lookup"><span data-stu-id="37cfc-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="37cfc-117">Consultas SQL brutas podem ser usadas para executar um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="37cfc-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="37cfc-118">Passando parâmetros</span><span class="sxs-lookup"><span data-stu-id="37cfc-118">Passing parameters</span></span>

<span data-ttu-id="37cfc-119">Assim como acontece com qualquer API que aceita SQL, é importante parametrizar qualquer entrada do usuário para proteger contra um ataque de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="37cfc-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="37cfc-120">Você pode incluir espaços reservados de parâmetros na cadeia de caracteres de consulta SQL e, em seguida, fornece valores de parâmetros como argumentos adicionais.</span><span class="sxs-lookup"><span data-stu-id="37cfc-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="37cfc-121">Quaisquer valores de parâmetro que você fornecer serão automaticamente convertidos para um `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="37cfc-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="37cfc-122">O exemplo a seguir passa um único parâmetro para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="37cfc-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="37cfc-123">Enquanto isso pode parecer como `String.Format` sintaxe, o valor fornecido é encapsulado em um parâmetro e o nome de parâmetro gerado inserida onde o `{0}` espaço reservado foi especificado.</span><span class="sxs-lookup"><span data-stu-id="37cfc-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="37cfc-124">Essa é a mesma consulta, mas usando a sintaxe de interpolação de cadeia de caracteres, que tem suporte no EF Core 2.0 e posterior:</span><span class="sxs-lookup"><span data-stu-id="37cfc-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="37cfc-125">Você também pode criar um DbParameter e fornecê-lo como um valor de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="37cfc-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="37cfc-126">Isso permite que você use parâmetros nomeados na cadeia de caracteres de consulta SQL</span><span class="sxs-lookup"><span data-stu-id="37cfc-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="37cfc-127">Compor com LINQ</span><span class="sxs-lookup"><span data-stu-id="37cfc-127">Composing with LINQ</span></span>

<span data-ttu-id="37cfc-128">Se a consulta SQL pode ser composta no banco de dados, você pode compor sobre a consulta SQL bruta inicial usando operadores LINQ.</span><span class="sxs-lookup"><span data-stu-id="37cfc-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="37cfc-129">Consultas SQL que podem ser compostas em ser com o `SELECT` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="37cfc-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="37cfc-130">O exemplo a seguir usa uma consulta SQL bruta que seleciona a partir de uma função com valor de tabela (TVF) e, em seguida, compõe usando LINQ para executar a filtragem e classificação.</span><span class="sxs-lookup"><span data-stu-id="37cfc-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="37cfc-131">Incluindo dados relacionados</span><span class="sxs-lookup"><span data-stu-id="37cfc-131">Including related data</span></span>

<span data-ttu-id="37cfc-132">Compondo operadores LINQ pode ser usado para incluir dados relacionados na consulta.</span><span class="sxs-lookup"><span data-stu-id="37cfc-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="37cfc-133">**Sempre use a parametrização para consultas SQL brutas:** APIs que aceitam um SQL bruto da cadeia de caracteres como `FromSql` e `ExecuteSqlCommand` permitir valores facilmente ser passados como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="37cfc-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="37cfc-134">Além de validar a entrada do usuário, sempre use parametrização para os valores usados em um consulta SQL bruto/comando.</span><span class="sxs-lookup"><span data-stu-id="37cfc-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="37cfc-135">Se você estiver usando a concatenação de cadeia de caracteres para criar dinamicamente a qualquer parte da cadeia de caracteres de consulta, você será responsável por validar qualquer entrada para proteger contra ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="37cfc-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
