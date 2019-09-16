---
title: Carregando entidades relacionadas-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: c359d8d32a88049213fd5e98e99fe49d7e3121a3
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005484"
---
# <a name="loading-related-entities"></a><span data-ttu-id="ba2e0-102">Carregando entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="ba2e0-102">Loading Related Entities</span></span>

<span data-ttu-id="ba2e0-103">O Entity Framework dá suporte a três maneiras de carregar dados relacionados-carregamento adiantado, carregamento lento e carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="ba2e0-104">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>

## <a name="eagerly-loading"></a><span data-ttu-id="ba2e0-105">Carregamento adiantado</span><span class="sxs-lookup"><span data-stu-id="ba2e0-105">Eagerly Loading</span></span>

<span data-ttu-id="ba2e0-106">O carregamento adiantado é o processo pelo qual uma consulta para um tipo de entidade também carrega entidades relacionadas como parte da consulta.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="ba2e0-107">O carregamento adiantado é obtido pelo uso do método include.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="ba2e0-108">Por exemplo, as consultas abaixo carregarão Blogs e todas as postagens relacionadas a cada blog.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts.
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts
    // using a string to specify the relationship.
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts
    // using a string to specify the relationship.
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```

> [!NOTE]
> <span data-ttu-id="ba2e0-109">Include é um método de extensão no namespace System. Data. Entity, portanto, verifique se você está usando esse namespace.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-109">Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="ba2e0-110">Carregamento adiantado de vários níveis</span><span class="sxs-lookup"><span data-stu-id="ba2e0-110">Eagerly loading multiple levels</span></span>

<span data-ttu-id="ba2e0-111">Também é possível carregar cuidadosamente vários níveis de entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="ba2e0-112">As consultas a seguir mostram exemplos de como fazer isso para as propriedades de navegação de coleção e referência.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar.
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships.
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships.
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

> [!NOTE]
> <span data-ttu-id="ba2e0-113">No momento, não é possível filtrar quais entidades relacionadas são carregadas.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-113">It is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="ba2e0-114">Include sempre trará todas as entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="ba2e0-115">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="ba2e0-115">Lazy Loading</span></span>

<span data-ttu-id="ba2e0-116">O carregamento lento é o processo pelo qual uma entidade ou coleção de entidades é carregada automaticamente do banco de dados na primeira vez que uma propriedade que se refere à entidade/entidades é acessada.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="ba2e0-117">Ao usar tipos de entidade POCO, o carregamento lento é obtido criando instâncias de tipos de proxy derivados e, em seguida, substituindo as propriedades virtuais para adicionar o gancho de carregamento.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="ba2e0-118">Por exemplo, ao usar a classe de entidade de blog definida abaixo, as postagens relacionadas serão carregadas na primeira vez que a propriedade de navegação de postagens for acessada:</span><span class="sxs-lookup"><span data-stu-id="ba2e0-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }
    public string Tags { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="ba2e0-119">Desativar o carregamento lento para serialização</span><span class="sxs-lookup"><span data-stu-id="ba2e0-119">Turn lazy loading off for serialization</span></span>

<span data-ttu-id="ba2e0-120">O carregamento e a serialização lentos não são bem misturados e, se você não tiver cuidado, poderá acabar consultando seu banco de dados inteiro porque o carregamento lento está habilitado.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="ba2e0-121">A maioria dos serializadores funciona acessando cada propriedade em uma instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="ba2e0-122">O acesso à propriedade dispara o carregamento lento e, portanto, mais entidades são serializadas.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="ba2e0-123">Nessas propriedades de entidades são acessadas e ainda mais entidades são carregadas.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="ba2e0-124">É uma boa prática ativar o carregamento lento antes de serializar uma entidade.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="ba2e0-125">As seções a seguir mostram como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-125">The following sections show how to do this.</span></span>

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="ba2e0-126">Desativando o carregamento lento para propriedades de navegação específicas</span><span class="sxs-lookup"><span data-stu-id="ba2e0-126">Turning off lazy loading for specific navigation properties</span></span>

<span data-ttu-id="ba2e0-127">O carregamento lento da coleção de postagens pode ser desativado ao tornar a propriedade de postagens não virtual:</span><span class="sxs-lookup"><span data-stu-id="ba2e0-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }
    public string Tags { get; set; }

    public ICollection<Post> Posts { get; set; }
}
```

<span data-ttu-id="ba2e0-128">O carregamento da coleção de postagens ainda pode ser obtido usando o carregamento adiantado (veja *carregamento adiantado* acima) ou o método Load (Confira *carregamento explícito* abaixo).</span><span class="sxs-lookup"><span data-stu-id="ba2e0-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="ba2e0-129">Desativar o carregamento lento de todas as entidades</span><span class="sxs-lookup"><span data-stu-id="ba2e0-129">Turn off lazy loading for all entities</span></span>

<span data-ttu-id="ba2e0-130">O carregamento lento pode ser desativado para todas as entidades no contexto, definindo um sinalizador na propriedade de configuração.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="ba2e0-131">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ba2e0-131">For example:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```

<span data-ttu-id="ba2e0-132">O carregamento de entidades relacionadas ainda pode ser obtido usando o carregamento adiantado (veja *carregamento* adiantado acima) ou o método Load (Confira *carregamento explícito* abaixo).</span><span class="sxs-lookup"><span data-stu-id="ba2e0-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

## <a name="explicitly-loading"></a><span data-ttu-id="ba2e0-133">Carregando explicitamente</span><span class="sxs-lookup"><span data-stu-id="ba2e0-133">Explicitly Loading</span></span>

<span data-ttu-id="ba2e0-134">Mesmo com o carregamento lento desabilitado, ainda é possível carregar lentamente as entidades relacionadas, mas isso deve ser feito com uma chamada explícita.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="ba2e0-135">Para fazer isso, você usa o método Load na entrada da entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="ba2e0-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ba2e0-136">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post.
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string.
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog.
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog).Collection("Posts").Load();
}
```

> [!NOTE]
> <span data-ttu-id="ba2e0-137">O método de referência deve ser usado quando uma entidade tiver uma propriedade de navegação para outra entidade única.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-137">The Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="ba2e0-138">Por outro lado, o método de coleção deve ser usado quando uma entidade tem uma propriedade de navegação para uma coleção de outras entidades.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="ba2e0-139">Aplicando filtros ao carregar explicitamente entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="ba2e0-139">Applying filters when explicitly loading related entities</span></span>

<span data-ttu-id="ba2e0-140">O método Query fornece acesso à consulta subjacente que Entity Framework será usada ao carregar entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="ba2e0-141">Em seguida, você pode usar o LINQ para aplicar filtros à consulta antes de executá-lo com uma chamada para um método de extensão LINQ, como ToList, Load, etc. O método Query pode ser usado com as propriedades de navegação e de referência de coleção, mas é mais útil para coleções em que ele pode ser usado para carregar apenas parte da coleção.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="ba2e0-142">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ba2e0-142">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog.
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```

<span data-ttu-id="ba2e0-143">Ao usar o método de consulta, geralmente é melhor desativar o carregamento lento para a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="ba2e0-144">Isso ocorre porque caso contrário, toda a coleção pode ser carregada automaticamente pelo mecanismo de carregamento lento antes ou depois da execução da consulta filtrada.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>

> [!NOTE]
> <span data-ttu-id="ba2e0-145">Embora a relação possa ser especificada como uma cadeia de caracteres em vez de uma expressão lambda, o IQueryable retornado não é genérico quando uma cadeia de caracteres é usada e, portanto, o método Cast geralmente é necessário antes que qualquer coisa útil possa ser feito com ele.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-145">While the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="ba2e0-146">Usando a consulta para contar entidades relacionadas sem carregá-las</span><span class="sxs-lookup"><span data-stu-id="ba2e0-146">Using Query to count related entities without loading them</span></span>

<span data-ttu-id="ba2e0-147">Às vezes, é útil saber quantas entidades estão relacionadas a outra entidade no banco de dados sem realmente incorrer no custo de carregar todas essas entidades.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="ba2e0-148">O método de consulta com o método de contagem LINQ pode ser usado para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="ba2e0-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="ba2e0-149">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ba2e0-149">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has.
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```
