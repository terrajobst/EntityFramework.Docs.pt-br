---
title: "Controle vs. Consultas de acompanhamento de não - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>Controle vs. Consultas de acompanhamento não

Controles de comportamento de controle se ou não o Entity Framework Core manterá as informações sobre uma instância de entidade em seu controlador de alterações. Se uma entidade é rastreada, qualquer alteração detectada na entidade serão mantidas no banco de dados durante a `SaveChanges()`. Entity Framework Core será correção também o propriedades de navegação entre entidades que são obtidas de um rastreamento de consulta e entidades que foram previamente carregadas para a instância de DbContext.

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) no GitHub.

## <a name="tracking-queries"></a>Consultas de acompanhamento

Por padrão, as consultas que retornam tipos de entidade são controle. Isso significa que você pode fazer alterações às instâncias de entidade e tem essas alterações persistirão por `SaveChanges()`.

No exemplo a seguir, a alteração para a classificação de blogs será detectada e persistentes no banco de dados durante a `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Consultas de acompanhamento não

Nenhuma consulta de rastreamento é úteis quando os resultados são usados em um cenário de somente leitura. Eles são mais rápidos executar porque não há nenhuma necessidade de informações do controle de alterações de configuração.

Você pode trocar uma consulta individual para controle de não ser:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Você também pode alterar o comportamento no nível de instância do contexto de controle padrão:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Nenhuma consulta de controle ainda executar a resolução de identidade dentro da consulta de executar. Se o conjunto de resultados contém a mesma entidade várias vezes, a mesma instância da classe da entidade será retornada para cada ocorrência do conjunto de resultados. No entanto, referências fracas são usadas para controlar as entidades que já foram retornadas. Se um resultado anterior com a mesma identidade sai do escopo e coleta de lixo é executado, você pode receber uma nova instância da entidade. Para obter mais informações, consulte [como funciona a consulta](overview.md).

## <a name="tracking-and-projections"></a>Projeções e controle

Mesmo se o tipo de resultado da consulta não é um tipo de entidade, se o resultado contém tipos de entidade ainda serão rastreados por padrão. Na consulta a seguir, que retorna um tipo anônimo, as instâncias do `Blog` no resultado do conjunto será rastreado.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
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

Se o conjunto de resultados não contém nenhum tipo de entidade, é executado sem rastreamento. Na consulta a seguir, que retorna um tipo anônimo com alguns dos valores da entidade (mas não há instâncias do tipo de entidade real), não há nenhum controle executada.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
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
