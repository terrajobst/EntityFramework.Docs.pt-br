---
title: Carregando relacionados entidades - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 091493f60c732573ca63adb8c65bc28606453d34
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995919"
---
# <a name="loading-related-entities"></a><span data-ttu-id="c6534-102">Carregando entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="c6534-102">Loading Related Entities</span></span>
<span data-ttu-id="c6534-103">Entity Framework oferece suporte a três maneiras de carregar dados relacionados – carregamento rápido, carregamento lento e o carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="c6534-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="c6534-104">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="c6534-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="c6534-105">Carregar avidamente</span><span class="sxs-lookup"><span data-stu-id="c6534-105">Eagerly Loading</span></span>  

<span data-ttu-id="c6534-106">O carregamento adiantado é o processo pelo qual uma consulta para um tipo de entidade também carrega entidades relacionadas como parte da consulta.</span><span class="sxs-lookup"><span data-stu-id="c6534-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="c6534-107">O carregamento adiantado é obtido por meio do método Include.</span><span class="sxs-lookup"><span data-stu-id="c6534-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="c6534-108">Por exemplo, as consultas abaixo carregará blogs e todas as postagens relacionadas a cada blog.</span><span class="sxs-lookup"><span data-stu-id="c6534-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

<span data-ttu-id="c6534-109">Observe que a inclusão é um método de extensão no namespace System, então, certifique-se de você estiver usando o namespace.</span><span class="sxs-lookup"><span data-stu-id="c6534-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="c6534-110">Carregar avidamente vários níveis</span><span class="sxs-lookup"><span data-stu-id="c6534-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="c6534-111">Também é possível carregar avidamente vários níveis de entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c6534-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="c6534-112">Consultas a seguir mostram exemplos de como fazer isso para coleta e propriedades de navegação de referência.</span><span class="sxs-lookup"><span data-stu-id="c6534-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="c6534-113">Observe que não é possível filtrar quais entidades relacionadas são carregadas no momento.</span><span class="sxs-lookup"><span data-stu-id="c6534-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="c6534-114">Incluir irá sempre colocar em entidades todas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c6534-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="c6534-115">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="c6534-115">Lazy Loading</span></span>  

<span data-ttu-id="c6534-116">Carregamento lento é o processo pelo qual uma entidade ou uma coleção de entidades é carregada automaticamente do banco de dados na primeira vez que uma propriedade referindo-se às entidades/entidade é acessada.</span><span class="sxs-lookup"><span data-stu-id="c6534-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="c6534-117">Ao usar tipos de entidade POCO, carregamento lento é obtido pela criação de instâncias de tipos de proxy derivada e, em seguida, substituindo propriedades virtuais para adicionar o gancho do carregamento.</span><span class="sxs-lookup"><span data-stu-id="c6534-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="c6534-118">Por exemplo, ao usar a classe de entidade do Blog definida abaixo, as postagens relacionadas serão carregadas na primeira vez em que a propriedade de navegação de postagens é acessada:</span><span class="sxs-lookup"><span data-stu-id="c6534-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

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

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="c6534-119">Ativar o carregamento lento desativado para serialização</span><span class="sxs-lookup"><span data-stu-id="c6534-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="c6534-120">Serialização e o carregamento lento não se misturam bem e se você não tiver cuidado pode acabar consultar seu banco de dados inteiro só porque o carregamento lento está habilitado.</span><span class="sxs-lookup"><span data-stu-id="c6534-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="c6534-121">A maioria dos serializadores funcionam acessando cada propriedade em uma instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="c6534-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="c6534-122">Acesso à propriedade dispara o carregamento lento, portanto, mais entidades são serializadas.</span><span class="sxs-lookup"><span data-stu-id="c6534-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="c6534-123">Nessas entidades propriedades são acessadas e até mesmo mais entidades são carregadas.</span><span class="sxs-lookup"><span data-stu-id="c6534-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="c6534-124">É uma boa prática para ativar o carregamento antes de você serializar uma entidade lento.</span><span class="sxs-lookup"><span data-stu-id="c6534-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="c6534-125">As seções a seguir mostram como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="c6534-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="c6534-126">Como desativar o carregamento lento para propriedades de navegação específicos</span><span class="sxs-lookup"><span data-stu-id="c6534-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="c6534-127">Carregamento lento da coleção de postagens pode ser desativado, tornando a propriedade de postagens não virtual:</span><span class="sxs-lookup"><span data-stu-id="c6534-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

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

<span data-ttu-id="c6534-128">Carregando das postagens de coleção pode ainda ser obtida usando o carregamento adiantado (consulte *carregar avidamente* acima) ou o método Load (veja *carregar explicitamente* abaixo).</span><span class="sxs-lookup"><span data-stu-id="c6534-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="c6534-129">Desativar carregamento lento para todas as entidades</span><span class="sxs-lookup"><span data-stu-id="c6534-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="c6534-130">Carregamento lento pode ser desativado para todas as entidades no contexto, definindo um sinalizador na propriedade de configuração.</span><span class="sxs-lookup"><span data-stu-id="c6534-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="c6534-131">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c6534-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="c6534-132">Carregamento de entidades relacionadas ainda pode ser obtido usando o carregamento adiantado (consulte *carregar avidamente* acima) ou o método Load (veja *carregar explicitamente* abaixo).</span><span class="sxs-lookup"><span data-stu-id="c6534-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="c6534-133">Carregar explicitamente</span><span class="sxs-lookup"><span data-stu-id="c6534-133">Explicitly Loading</span></span>  

<span data-ttu-id="c6534-134">Mesmo com carregamento lento desativado ainda é possível carregar lentamente entidades relacionadas, mas ele deve ser feito com uma chamada explícita.</span><span class="sxs-lookup"><span data-stu-id="c6534-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="c6534-135">Para fazer isso você pode usar o método Load na entrada da entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="c6534-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="c6534-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c6534-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="c6534-137">Observe que o método de referência deve ser usado quando uma entidade tem uma propriedade de navegação para outra entidade única.</span><span class="sxs-lookup"><span data-stu-id="c6534-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="c6534-138">Por outro lado, o método de coleção deve ser usado quando uma entidade tem uma propriedade de navegação para uma coleção de outras entidades.</span><span class="sxs-lookup"><span data-stu-id="c6534-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="c6534-139">Aplicação de filtros ao carregar explicitamente entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="c6534-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="c6534-140">O método de consulta fornece acesso a consulta subjacente que o Entity Framework usará ao carregar entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c6534-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="c6534-141">Em seguida, você pode usar o LINQ para aplicar filtros à consulta antes de executá-lo com uma chamada para um método de extensão LINQ como ToList, carga, etc. O método de consulta pode ser usado com a referência e a coleção de propriedades de navegação, mas é mais útil para coleções em que ele pode ser usado para carregar apenas parte da coleção.</span><span class="sxs-lookup"><span data-stu-id="c6534-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="c6534-142">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c6534-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

<span data-ttu-id="c6534-143">Ao usar o método de consulta é geralmente melhor desativar carregamento lento para a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="c6534-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="c6534-144">Isso ocorre porque, caso contrário, toda a coleção pode ficar carregada automaticamente com o mecanismo de carregamento lento antes ou depois que a consulta filtrada é executada.</span><span class="sxs-lookup"><span data-stu-id="c6534-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="c6534-145">Observe que enquanto a relação pode ser especificada como uma cadeia de caracteres em vez de uma expressão lambda, o IQueryable retornado não é genérico quando uma cadeia de caracteres é usada e, portanto, o método de conversão geralmente é necessária antes que alguma coisa útil pode ser feita com ele.</span><span class="sxs-lookup"><span data-stu-id="c6534-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="c6534-146">Usando a consulta para contar as entidades relacionadas sem carregá-los</span><span class="sxs-lookup"><span data-stu-id="c6534-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="c6534-147">Às vezes é útil saber quantas entidades relacionadas a outra entidade no banco de dados sem realmente incorrer no custo de carregamento de todas as entidades.</span><span class="sxs-lookup"><span data-stu-id="c6534-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="c6534-148">O método de consulta com o método de contagem de LINQ pode ser usado para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="c6534-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="c6534-149">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c6534-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
