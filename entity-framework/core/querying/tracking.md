---
title: Consultas com acompanhamento versus Consultas sem acompanhamento – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: d93be5c2b727d8fbaddd103f8f367c699ae80a7c
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921649"
---
# <a name="tracking-vs-no-tracking-queries"></a>Consultas com acompanhamento versus Consultas sem acompanhamento

O comportamento de acompanhamento controla se o Entity Framework Core manterá as informações sobre uma instância de entidade em seu controlador de alterações. Se uma entidade é acompanhada, qualquer alteração detectada na entidade será mantida no banco de dados durante a `SaveChanges()`. Entity Framework Core também corrigirá as propriedades de navegação entre as entidades que são obtidas de um acompanhamento de consulta e entidades que foram previamente carregadas para a instância de DbContext.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="tracking-queries"></a>Consultas com acompanhamento

Por padrão, as consultas que retornam tipos de entidade são de acompanhamento. Isso significa que você pode fazer alterações às instâncias da entidade e fazer essas alterações serem persistidas por `SaveChanges()`.

No exemplo a seguir, a alteração para a classificação de blogs será detectada e persistida no banco de dados durante a `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Consultas sem controle

As consulta sem acompanhamento são úteis quando os resultados são usados em um cenário de somente leitura. Eles são mais rápidos de executar porque não há necessidade de configurar informações de controle de alterações.

Você pode trocar uma consulta individual para ser sem acompanhamento:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Você também pode alterar o comportamento de acompanhamento padrão no nível de instância do contexto:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> As consultas sem controle ainda executam a resolução de identidade dentro da consulta de execução. Se o conjunto de resultados contém a mesma entidade várias vezes, a mesma instância da classe da entidade será retornada para cada ocorrência do conjunto de resultados. No entanto, as referências fracas são usadas para controlar as entidades que já foram retornadas. Se um resultado anterior com a mesma identidade sai do escopo e a coleta de lixo é executada, você poderá receber uma nova instância da entidade. Para saber mais, veja [Como funciona a consulta](overview.md).

## <a name="tracking-and-projections"></a>Acompanhamento e projeções

Mesmo se o tipo de resultado da consulta não for um tipo de entidade, se o resultado contiver tipos de entidade, ele ainda será rastreado por padrão. Na consulta a seguir, que retorna um tipo anônimo, as instâncias do `Blog` no conjunto de resultados será rastreado.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

Se o conjunto de resultados não contiver tipos de entidade, nenhum acompanhamento será executado. Na consulta a seguir, que retorna um tipo anônimo com alguns dos valores da entidade (mas não há instâncias do tipo de entidade real), não há nenhum acompanhamento executado.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
