---
title: "Cliente vs. Avaliação do servidor - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="e9831-102">Cliente vs. Avaliação do servidor</span><span class="sxs-lookup"><span data-stu-id="e9831-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="e9831-103">Entity Framework Core dá suporte a partes da consulta que está sendo avaliada no cliente e partes do que está sendo enviada por push para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e9831-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="e9831-104">Cabe o provedor de banco de dados para determinar quais partes da consulta serão avaliadas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e9831-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="e9831-105">Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="e9831-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="e9831-106">Avaliação do cliente</span><span class="sxs-lookup"><span data-stu-id="e9831-106">Client evaluation</span></span>

<span data-ttu-id="e9831-107">No exemplo a seguir, um método auxiliar é usado para padronizar URLs para blogs que são retornados de um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e9831-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="e9831-108">Como o provedor do SQL Server não tem nenhuma informação sobre como esse método é implementado, não é possível para traduzi-lo em SQL.</span><span class="sxs-lookup"><span data-stu-id="e9831-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="e9831-109">Todos os outros aspectos da consulta são avaliados no banco de dados, mas passando retornado `URL` por esse método é executado no cliente.</span><span class="sxs-lookup"><span data-stu-id="e9831-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="disabling-client-evaluation"></a><span data-ttu-id="e9831-110">Desabilitando a avaliação do cliente</span><span class="sxs-lookup"><span data-stu-id="e9831-110">Disabling client evaluation</span></span>

<span data-ttu-id="e9831-111">Durante a avaliação do cliente pode ser muito útil, em alguns casos, isso poderá resultar em um baixo desempenho.</span><span class="sxs-lookup"><span data-stu-id="e9831-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="e9831-112">Considere a consulta a seguir, onde o método auxiliar agora é usado em um filtro.</span><span class="sxs-lookup"><span data-stu-id="e9831-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="e9831-113">Como isso não pode ser executado no banco de dados, todos os dados são extraídos na memória e, em seguida, o filtro é aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="e9831-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="e9831-114">Dependendo da quantidade de dados, e a quantidade de dados é filtrado, isso pode resultar em baixo desempenho.</span><span class="sxs-lookup"><span data-stu-id="e9831-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="e9831-115">Por padrão, o EF Core registrará um aviso quando a avaliação do cliente é executada.</span><span class="sxs-lookup"><span data-stu-id="e9831-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="e9831-116">Consulte [log](../miscellaneous/logging.md) para obter mais informações sobre como exibir a saída de log.</span><span class="sxs-lookup"><span data-stu-id="e9831-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="e9831-117">Você pode alterar o comportamento quando a avaliação do cliente ocorre para gerar ou não faça nada.</span><span class="sxs-lookup"><span data-stu-id="e9831-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="e9831-118">Isso é feito ao configurar as opções para o contexto - normalmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e9831-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
