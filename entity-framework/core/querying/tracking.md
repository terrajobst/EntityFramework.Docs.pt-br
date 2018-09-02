---
title: Consultas com acompanhamento versus Consultas sem acompanhamento – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 985adc795f379199a3bacc985843f32f3168cf64
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993349"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="de9ce-102">Consultas com acompanhamento versus Consultas sem acompanhamento</span><span class="sxs-lookup"><span data-stu-id="de9ce-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="de9ce-103">O comportamento de acompanhamento controla se o Entity Framework Core manterá as informações sobre uma instância de entidade em seu controlador de alterações.</span><span class="sxs-lookup"><span data-stu-id="de9ce-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="de9ce-104">Se uma entidade é acompanhada, qualquer alteração detectada na entidade será mantida no banco de dados durante a `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="de9ce-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="de9ce-105">Entity Framework Core também corrigirá as propriedades de navegação entre as entidades que são obtidas de um acompanhamento de consulta e entidades que foram previamente carregadas para a instância de DbContext.</span><span class="sxs-lookup"><span data-stu-id="de9ce-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="de9ce-106">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="de9ce-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="de9ce-107">Consultas com acompanhamento</span><span class="sxs-lookup"><span data-stu-id="de9ce-107">Tracking queries</span></span>

<span data-ttu-id="de9ce-108">Por padrão, as consultas que retornam tipos de entidade são de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="de9ce-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="de9ce-109">Isso significa que você pode fazer alterações às instâncias da entidade e fazer essas alterações serem persistidas por `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="de9ce-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="de9ce-110">No exemplo a seguir, a alteração para a classificação de blogs será detectada e persistida no banco de dados durante a `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="de9ce-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="de9ce-111">Consultas sem controle</span><span class="sxs-lookup"><span data-stu-id="de9ce-111">No-tracking queries</span></span>

<span data-ttu-id="de9ce-112">As consulta sem acompanhamento são úteis quando os resultados são usados em um cenário de somente leitura.</span><span class="sxs-lookup"><span data-stu-id="de9ce-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="de9ce-113">Eles são mais rápidos de executar porque não há necessidade de configurar informações de controle de alterações.</span><span class="sxs-lookup"><span data-stu-id="de9ce-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="de9ce-114">Você pode trocar uma consulta individual para ser sem acompanhamento:</span><span class="sxs-lookup"><span data-stu-id="de9ce-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="de9ce-115">Você também pode alterar o comportamento de acompanhamento padrão no nível de instância do contexto:</span><span class="sxs-lookup"><span data-stu-id="de9ce-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="de9ce-116">As consultas sem acompanhamento ainda executam a resolução de identidade dentro da consulta de execução.</span><span class="sxs-lookup"><span data-stu-id="de9ce-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="de9ce-117">Se o conjunto de resultados contém a mesma entidade várias vezes, a mesma instância da classe da entidade será retornada para cada ocorrência do conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="de9ce-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="de9ce-118">No entanto, as referências fracas são usadas para controlar as entidades que já foram retornadas.</span><span class="sxs-lookup"><span data-stu-id="de9ce-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="de9ce-119">Se um resultado anterior com a mesma identidade sai do escopo e a coleta de lixo é executada, você poderá receber uma nova instância da entidade.</span><span class="sxs-lookup"><span data-stu-id="de9ce-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="de9ce-120">Para saber mais, veja [Como funciona a consulta](overview.md).</span><span class="sxs-lookup"><span data-stu-id="de9ce-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="de9ce-121">Acompanhamento e projeções</span><span class="sxs-lookup"><span data-stu-id="de9ce-121">Tracking and projections</span></span>

<span data-ttu-id="de9ce-122">Mesmo se o tipo de resultado da consulta não for um tipo de entidade, se o resultado contiver tipos de entidade, ele ainda será rastreado por padrão.</span><span class="sxs-lookup"><span data-stu-id="de9ce-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="de9ce-123">Na consulta a seguir, que retorna um tipo anônimo, as instâncias do `Blog` no conjunto de resultados será rastreado.</span><span class="sxs-lookup"><span data-stu-id="de9ce-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="de9ce-124">Se o conjunto de resultados não contiver tipos de entidade, nenhum acompanhamento será executado.</span><span class="sxs-lookup"><span data-stu-id="de9ce-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="de9ce-125">Na consulta a seguir, que retorna um tipo anônimo com alguns dos valores da entidade (mas não há instâncias do tipo de entidade real), não há nenhum acompanhamento executado.</span><span class="sxs-lookup"><span data-stu-id="de9ce-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
