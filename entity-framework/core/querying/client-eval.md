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
# <a name="client-vs-server-evaluation"></a>Cliente vs. Avaliação do servidor

Entity Framework Core dá suporte a partes da consulta que está sendo avaliada no cliente e partes do que está sendo enviada por push para o banco de dados. Cabe o provedor de banco de dados para determinar quais partes da consulta serão avaliadas no banco de dados.

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) no GitHub.

## <a name="client-evaluation"></a>Avaliação do cliente

No exemplo a seguir, um método auxiliar é usado para padronizar URLs para blogs que são retornados de um banco de dados do SQL Server. Como o provedor do SQL Server não tem nenhuma informação sobre como esse método é implementado, não é possível para traduzi-lo em SQL. Todos os outros aspectos da consulta são avaliados no banco de dados, mas passando retornado `URL` por esse método é executado no cliente.

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

## <a name="disabling-client-evaluation"></a>Desabilitando a avaliação do cliente

Durante a avaliação do cliente pode ser muito útil, em alguns casos, isso poderá resultar em um baixo desempenho. Considere a consulta a seguir, onde o método auxiliar agora é usado em um filtro. Como isso não pode ser executado no banco de dados, todos os dados são extraídos na memória e, em seguida, o filtro é aplicado no cliente. Dependendo da quantidade de dados, e a quantidade de dados é filtrado, isso pode resultar em baixo desempenho.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Por padrão, o EF Core registrará um aviso quando a avaliação do cliente é executada. Consulte [log](../miscellaneous/logging.md) para obter mais informações sobre como exibir a saída de log. Você pode alterar o comportamento quando a avaliação do cliente ocorre para gerar ou não faça nada. Isso é feito ao configurar as opções para o contexto - normalmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
