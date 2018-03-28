---
title: Carregamento de dados - Core EF relacionados ao
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 0d7705e0e5368435536e98d319c853ea8c732643
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="f921e-102">Carregamento de dados relacionados</span><span class="sxs-lookup"><span data-stu-id="f921e-102">Loading Related Data</span></span>

<span data-ttu-id="f921e-103">Entity Framework Core permite que você use as propriedades de navegação em seu modelo para carregar as entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="f921e-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="f921e-104">Há três padrões de S/RM comuns usados para carregar dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="f921e-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="f921e-105">**Carregamento adiantado** significa que os dados relacionados são carregados do banco de dados como parte da consulta inicial.</span><span class="sxs-lookup"><span data-stu-id="f921e-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="f921e-106">**Carregamento explícito** significa que os dados relacionados são explicitamente carregados do banco de dados em um momento posterior.</span><span class="sxs-lookup"><span data-stu-id="f921e-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="f921e-107">**Carregamento preguiçoso** significa que os dados relacionados são transparentemente carregados do banco de dados quando a propriedade de navegação é acessada.</span><span class="sxs-lookup"><span data-stu-id="f921e-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="f921e-108">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="f921e-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="f921e-109">Carregamento adiantado</span><span class="sxs-lookup"><span data-stu-id="f921e-109">Eager loading</span></span>

<span data-ttu-id="f921e-110">Você pode usar o método `Include` para especificar dados relacionados a serem incluídos nos resultados da consulta.</span><span class="sxs-lookup"><span data-stu-id="f921e-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="f921e-111">No exemplo a seguir, os blogs que são retornados nos resultados terão suas propriedades `Posts` preenchidas com as postagens relacionadas.</span><span class="sxs-lookup"><span data-stu-id="f921e-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="f921e-112">O Entity Framework Core corrigirá automaticamente as propriedades de navegação para outras entidades que foram previamente carregadas para a instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="f921e-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="f921e-113">Dessa forma, mesmo se você não incluir explicitamente os dados para uma propriedade de navegação, a propriedade ainda poderá ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f921e-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="f921e-114">Você pode incluir dados relacionados de várias relações em uma única consulta.</span><span class="sxs-lookup"><span data-stu-id="f921e-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="f921e-115">Incluindo vários níveis</span><span class="sxs-lookup"><span data-stu-id="f921e-115">Including multiple levels</span></span>

<span data-ttu-id="f921e-116">Você pode fazer uma busca detalhada por meio de relações para incluir vários níveis de dados relacionados usando o método `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="f921e-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="f921e-117">O exemplo a seguir carrega todos os blogs, suas postagens relacionadas e o autor de cada postagem.</span><span class="sxs-lookup"><span data-stu-id="f921e-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="f921e-118">As versões atuais do Visual Studio oferecem opções de conclusão de código incorreto, que podem causar expressões corretas a serem sinalizadas com erros de sintaxe ao usar o método `ThenInclude` após uma propriedade de navegação da coleção.</span><span class="sxs-lookup"><span data-stu-id="f921e-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="f921e-119">Este é um sintoma de um bug de IntelliSense controlado no https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="f921e-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="f921e-120">É seguro ignorar esses erros de sintaxe artificiais desde que o código esteja correto e possa ser compilado com êxito.</span><span class="sxs-lookup"><span data-stu-id="f921e-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="f921e-121">É possível encadear chamadas múltiplas para `ThenInclude` para continuar incluindo mais níveis de dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="f921e-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="f921e-122">Você pode combinar tudo isso para incluir dados relacionados de vários níveis e várias raízes na mesma consulta.</span><span class="sxs-lookup"><span data-stu-id="f921e-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="f921e-123">Você talvez queira incluir várias entidades relacionadas para uma das entidades que está sendo incluída.</span><span class="sxs-lookup"><span data-stu-id="f921e-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="f921e-124">Por exemplo, ao consultar `Blog`, você inclui `Posts` e deseja incluir o `Author` e `Tags` dos `Posts`.</span><span class="sxs-lookup"><span data-stu-id="f921e-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="f921e-125">Para fazer isso, você precisa especificar cada `Include` a partir da raiz do caminho.</span><span class="sxs-lookup"><span data-stu-id="f921e-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="f921e-126">Por exemplo, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="f921e-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="f921e-127">Isso não significa que você obterá junções redundantes, na maioria dos casos o EF irá consolidar as associações durante a geração de SQL.</span><span class="sxs-lookup"><span data-stu-id="f921e-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="f921e-128">Incluir em tipos derivados</span><span class="sxs-lookup"><span data-stu-id="f921e-128">Include on derived types</span></span>

<span data-ttu-id="f921e-129">Você pode incluir dados relacionados de navegações definidos apenas em um tipo derivado usando `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="f921e-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="f921e-130">Recebe o seguinte modelo:</span><span class="sxs-lookup"><span data-stu-id="f921e-130">Given the following model:</span></span>

```Csharp
    public class SchoolContext : DbContext
    {
        public DbSet<Person> People { get; set; }
        public DbSet<School> Schools { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
        }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Student : Person
    {
        public School School { get; set; }
    }

    public class School
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public List<Student> Students { get; set; }
    }
```

<span data-ttu-id="f921e-131">O conteúdo de navegação de `School` de todas as pessoas que são alunos pode ser carregado ansiosamente usando um número de padrões:</span><span class="sxs-lookup"><span data-stu-id="f921e-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="f921e-132">usando cast</span><span class="sxs-lookup"><span data-stu-id="f921e-132">using cast</span></span>
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- <span data-ttu-id="f921e-133">usando o operador `as`</span><span class="sxs-lookup"><span data-stu-id="f921e-133">using `as` operator</span></span>
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- <span data-ttu-id="f921e-134">usando a sobrecarga de `Include` que usa o parâmetro de tipo `string`</span><span class="sxs-lookup"><span data-stu-id="f921e-134">using overload of `Include` that takes parameter of type `string`</span></span>
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a><span data-ttu-id="f921e-135">Operadores include ignorados</span><span class="sxs-lookup"><span data-stu-id="f921e-135">Ignored includes</span></span>

<span data-ttu-id="f921e-136">Se você alterar a consulta para que ela não retorne instâncias do tipo de entidade com o qual ela começa, então os operadores include serão ignorados.
</span><span class="sxs-lookup"><span data-stu-id="f921e-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="f921e-137">No exemplo a seguir, os operadores de inclusão se baseiam no `Blog`, mas em seguida o operador `Select` é usado para alterar a consulta para retornar um tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="f921e-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="f921e-138">Nesse caso, os operadores de inclusão não têm efeito.</span><span class="sxs-lookup"><span data-stu-id="f921e-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="f921e-139">Por padrão, o EF Core registrará um aviso quando operadores de inclusão são ignorados.</span><span class="sxs-lookup"><span data-stu-id="f921e-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="f921e-140">Consulte [log](../miscellaneous/logging.md) para obter mais informações sobre como exibir a saída de log.</span><span class="sxs-lookup"><span data-stu-id="f921e-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="f921e-141">Você pode alterar o comportamento quando um operador include é ignorado para gerar ou não fazer nada.</span><span class="sxs-lookup"><span data-stu-id="f921e-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="f921e-142">Isso é feito ao configurar as opções para o seu contexto - normalmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f921e-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="f921e-143">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="f921e-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="f921e-144">Esse recurso foi introduzido no EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="f921e-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="f921e-145">Você pode carregar explicitamente uma propriedade de navegação por meio da API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="f921e-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="f921e-146">Você pode carregar também explicitamente uma propriedade de navegação executando uma consulta separada que retorna as entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="f921e-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="f921e-147">Se o controle de alterações está habilitada, ao carregar uma entidade, Core EF automaticamente definir as propriedades de navegação da entidade para se referir a qualquer entidade que já carregada recentemente carregado e definir as propriedades de navegação das entidades já carregado para referir-se a entidade carregado recentemente.</span><span class="sxs-lookup"><span data-stu-id="f921e-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="f921e-148">Consultando entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="f921e-148">Querying related entities</span></span>

<span data-ttu-id="f921e-149">Você também pode obter uma consulta LINQ que representa o conteúdo de uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="f921e-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="f921e-150">Isso permite que você faça coisas como a execução de um operador de agregação sobre as entidades relacionadas sem carregá-los na memória.</span><span class="sxs-lookup"><span data-stu-id="f921e-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="f921e-151">Você também pode filtrar quais entidades relacionadas são carregadas na memória.</span><span class="sxs-lookup"><span data-stu-id="f921e-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="f921e-152">Carregamento preguiçoso</span><span class="sxs-lookup"><span data-stu-id="f921e-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="f921e-153">Esse recurso foi introduzido no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f921e-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="f921e-154">A maneira mais simples de usar o carregamento lento é instalando o pacote [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) e o habilitando com uma chamada para `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="f921e-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="f921e-155">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f921e-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="f921e-156">Ou, ao usar AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="f921e-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="f921e-157">Em seguida, EF Core habilitará o carregamento lento para qualquer propriedade de navegação que pode ser substituída – isto é, ela deve ser `virtual` e estar em uma classe da qual pode ser herdada.</span><span class="sxs-lookup"><span data-stu-id="f921e-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="f921e-158">Por exemplo, nas entidades a seguir, as propriedades de navegação `Post.Blog` e `Blog.Posts` terão carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="f921e-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="f921e-159">Carregamento lento sem proxies</span><span class="sxs-lookup"><span data-stu-id="f921e-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="f921e-160">Proxies de carregamento lento trabalham inserindo o serviço `ILazyLoader` em uma entidade, conforme descrito em [construtores de tipo de entidade](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="f921e-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="f921e-161">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f921e-161">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="f921e-162">sso não requer que os tipos de entidade sejam herdados de ou que as propriedades de navegação sejam virtuais e permite que que instâncias da entidade criadas com `new` carreguem lentamente quando conectadas a um contexto.</span><span class="sxs-lookup"><span data-stu-id="f921e-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="f921e-163">No entanto, ele requer uma referência para o serviço `ILazyLoader`, que associa os tipos de entidade para o assembly principal EF.</span><span class="sxs-lookup"><span data-stu-id="f921e-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="f921e-164">Para evitar isso, o núcleo EF permite que o método `ILazyLoader.Load` seja inserido como um representante.
</span><span class="sxs-lookup"><span data-stu-id="f921e-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="f921e-165">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f921e-165">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="f921e-166">O código acima usa um método de extensão `Load` para fazer o uso do delegate um pouco mais limpo:</span><span class="sxs-lookup"><span data-stu-id="f921e-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="f921e-167">O parâmetro de construtor para o representante de carregamento lento deve ser chamado "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="f921e-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="f921e-168">Configuração para usar um nome diferente, que isso é planejado para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="f921e-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="f921e-169">Serialização e dados relacionados</span><span class="sxs-lookup"><span data-stu-id="f921e-169">Related data and serialization</span></span>

<span data-ttu-id="f921e-170">Como o EF Core irá automaticamente corrigir as propriedades de navegação, você pode acabar com ciclos em seu gráfico de objetos.</span><span class="sxs-lookup"><span data-stu-id="f921e-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="f921e-171">Por exemplo, o carregamento de um blog suas postagens relacionadas resultará em um objeto de blog referenciando uma coleção de postagens.</span><span class="sxs-lookup"><span data-stu-id="f921e-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="f921e-172">Cada uma dessas postagens terá uma referência de volta para o blog.</span><span class="sxs-lookup"><span data-stu-id="f921e-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="f921e-173">Algumas estruturas de serialização não permitem esses ciclos.</span><span class="sxs-lookup"><span data-stu-id="f921e-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="f921e-174">Por exemplo, Json.NET lançará a exceção a seguir se for encontrado um ciclo.</span><span class="sxs-lookup"><span data-stu-id="f921e-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="f921e-175">Newtonsoft.Json.JsonSerializationException: Self referenciando loop detectado para a propriedade 'Blog' com tipo 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="f921e-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="f921e-176">Se você estiver usando o ASP.NET Core, poderá configurar Json.NET para ignorar ciclos que ele encontrar no gráfico de objeto.</span><span class="sxs-lookup"><span data-stu-id="f921e-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="f921e-177">Isso é feito no método `ConfigureServices(...)` em `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="f921e-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
