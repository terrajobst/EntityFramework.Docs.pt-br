---
title: Tipos de entidade com construtores-EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: ddfaa8eebde388a9d3309f21b8891de593077956
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811885"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="d2b65-102">Tipos de entidade com construtores</span><span class="sxs-lookup"><span data-stu-id="d2b65-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="d2b65-103">Este recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d2b65-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="d2b65-104">A partir do EF Core 2,1, agora é possível definir um construtor com parâmetros e EF Core chamar esse construtor ao criar uma instância da entidade.</span><span class="sxs-lookup"><span data-stu-id="d2b65-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="d2b65-105">Os parâmetros do Construtor podem ser associados a Propriedades mapeadas ou a vários tipos de serviços para facilitar comportamentos como carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="d2b65-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="d2b65-106">A partir de EF Core 2,1, toda a associação de construtor é por convenção.</span><span class="sxs-lookup"><span data-stu-id="d2b65-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="d2b65-107">A configuração de construtores específicos a serem usados é planejada para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="d2b65-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="d2b65-108">Associação a Propriedades mapeadas</span><span class="sxs-lookup"><span data-stu-id="d2b65-108">Binding to mapped properties</span></span>

<span data-ttu-id="d2b65-109">Considere um modelo de blog/postagem típico:</span><span class="sxs-lookup"><span data-stu-id="d2b65-109">Consider a typical Blog/Post model:</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="d2b65-110">Quando EF Core cria instâncias desses tipos, como para os resultados de uma consulta, ele primeiro chamará o construtor padrão sem parâmetros e, em seguida, definirá cada propriedade para o valor do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d2b65-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="d2b65-111">No entanto, se EF Core encontrar um construtor com parâmetros com nomes de parâmetro e tipos que correspondam às propriedades mapeadas, ele chamará o construtor parametrizado com valores para essas propriedades e não definirá cada propriedade explicitamente.</span><span class="sxs-lookup"><span data-stu-id="d2b65-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="d2b65-112">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d2b65-112">For example:</span></span>

``` csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="d2b65-113">Algumas coisas a serem observadas:</span><span class="sxs-lookup"><span data-stu-id="d2b65-113">Some things to note:</span></span>

* <span data-ttu-id="d2b65-114">Nem todas as propriedades precisam ter parâmetros de construtor.</span><span class="sxs-lookup"><span data-stu-id="d2b65-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="d2b65-115">Por exemplo, a propriedade post. Content não é definida por nenhum parâmetro de construtor, portanto EF Core a definirá depois de chamar o construtor da maneira normal.</span><span class="sxs-lookup"><span data-stu-id="d2b65-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="d2b65-116">Os tipos e nomes de parâmetro devem corresponder aos tipos de propriedade e nomes, exceto que as propriedades podem ser Pascal-case enquanto os parâmetros são camel-case.</span><span class="sxs-lookup"><span data-stu-id="d2b65-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="d2b65-117">EF Core não pode definir propriedades de navegação (como blog ou postagens acima) usando um construtor.</span><span class="sxs-lookup"><span data-stu-id="d2b65-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="d2b65-118">O construtor pode ser público, privado ou ter qualquer outra acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="d2b65-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="d2b65-119">No entanto, os proxies de carregamento lento exigem que o Construtor seja acessível a partir da classe de proxy herdada.</span><span class="sxs-lookup"><span data-stu-id="d2b65-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="d2b65-120">Geralmente, isso significa torná-lo público ou protegido.</span><span class="sxs-lookup"><span data-stu-id="d2b65-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="d2b65-121">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="d2b65-121">Read-only properties</span></span>

<span data-ttu-id="d2b65-122">Depois que as propriedades estão sendo definidas por meio do Construtor, é possível fazer sentido tornar algumas delas somente leitura.</span><span class="sxs-lookup"><span data-stu-id="d2b65-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="d2b65-123">O EF Core dá suporte a isso, mas há algumas coisas a serem examinadas:</span><span class="sxs-lookup"><span data-stu-id="d2b65-123">EF Core supports this, but there are some things to look out for:</span></span>

* <span data-ttu-id="d2b65-124">Propriedades sem setters não são mapeadas por convenção.</span><span class="sxs-lookup"><span data-stu-id="d2b65-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="d2b65-125">(Isso tende a mapear propriedades que não devem ser mapeadas, como propriedades computadas.)</span><span class="sxs-lookup"><span data-stu-id="d2b65-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="d2b65-126">O uso de valores de chave gerados automaticamente requer uma propriedade de chave que é de leitura/gravação, já que o valor da chave precisa ser definido pelo gerador de chave ao inserir novas entidades.</span><span class="sxs-lookup"><span data-stu-id="d2b65-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="d2b65-127">Uma maneira fácil de evitar essas coisas é usar setters privados.</span><span class="sxs-lookup"><span data-stu-id="d2b65-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="d2b65-128">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d2b65-128">For example:</span></span>

``` csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="d2b65-129">EF Core vê uma propriedade com um setter particular como leitura/gravação, o que significa que todas as propriedades são mapeadas como antes e a chave ainda pode ser gerada pelo armazenamento.</span><span class="sxs-lookup"><span data-stu-id="d2b65-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="d2b65-130">Uma alternativa ao uso de setters privados é tornar as propriedades realmente somente leitura e adicionar um mapeamento mais explícito em OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="d2b65-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="d2b65-131">Da mesma forma, algumas propriedades podem ser completamente removidas e substituídas por apenas campos.</span><span class="sxs-lookup"><span data-stu-id="d2b65-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="d2b65-132">Por exemplo, considere estes tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="d2b65-132">For example, consider these entity types:</span></span>

``` csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="d2b65-133">E essa configuração em OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="d2b65-133">And this configuration in OnModelCreating:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```

<span data-ttu-id="d2b65-134">Itens a serem observados:</span><span class="sxs-lookup"><span data-stu-id="d2b65-134">Things to note:</span></span>

* <span data-ttu-id="d2b65-135">A chave "Property" agora é um campo.</span><span class="sxs-lookup"><span data-stu-id="d2b65-135">The key "property" is now a field.</span></span> <span data-ttu-id="d2b65-136">Não é um campo `readonly` para que as chaves geradas pelo repositório possam ser usadas.</span><span class="sxs-lookup"><span data-stu-id="d2b65-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="d2b65-137">As outras propriedades são propriedades somente leitura definidas somente no construtor.</span><span class="sxs-lookup"><span data-stu-id="d2b65-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="d2b65-138">Se o valor da chave primária for apenas definido pelo EF ou lido no banco de dados, não haverá necessidade de incluí-lo no construtor.</span><span class="sxs-lookup"><span data-stu-id="d2b65-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="d2b65-139">Isso deixa a chave "Propriedade" como um campo simples e torna claro que ela não deve ser definida explicitamente ao criar novos Blogs ou postagens.</span><span class="sxs-lookup"><span data-stu-id="d2b65-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="d2b65-140">Esse código resultará no aviso do compilador ' 169 ', indicando que o campo nunca é usado.</span><span class="sxs-lookup"><span data-stu-id="d2b65-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="d2b65-141">Isso pode ser ignorado, pois na realidade EF Core está usando o campo de uma maneira extralinguistic.</span><span class="sxs-lookup"><span data-stu-id="d2b65-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="d2b65-142">Injetando serviços</span><span class="sxs-lookup"><span data-stu-id="d2b65-142">Injecting services</span></span>

<span data-ttu-id="d2b65-143">EF Core também pode injetar "serviços" no construtor de um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="d2b65-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="d2b65-144">Por exemplo, o seguinte pode ser injetado:</span><span class="sxs-lookup"><span data-stu-id="d2b65-144">For example, the following can be injected:</span></span>

* <span data-ttu-id="d2b65-145">`DbContext`-a instância de contexto atual, que também pode ser digitada como seu tipo DbContext derivado</span><span class="sxs-lookup"><span data-stu-id="d2b65-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="d2b65-146">`ILazyLoader`-o serviço de carregamento lento--consulte a [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes</span><span class="sxs-lookup"><span data-stu-id="d2b65-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="d2b65-147">`Action<object, string>`-um delegado de carregamento lento--consulte a [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes</span><span class="sxs-lookup"><span data-stu-id="d2b65-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="d2b65-148">`IEntityType`-os metadados de EF Core associados a esse tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="d2b65-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="d2b65-149">A partir de EF Core 2,1, somente os serviços conhecidos pelo EF Core podem ser injetados.</span><span class="sxs-lookup"><span data-stu-id="d2b65-149">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="d2b65-150">O suporte para injetar serviços de aplicativos está sendo considerado para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="d2b65-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="d2b65-151">Por exemplo, um DbContext injetado pode ser usado para acessar seletivamente o banco de dados para obter informações sobre entidades relacionadas sem carregá-las.</span><span class="sxs-lookup"><span data-stu-id="d2b65-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="d2b65-152">No exemplo abaixo, é usado para obter o número de postagens em um blog sem carregar as postagens:</span><span class="sxs-lookup"><span data-stu-id="d2b65-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="d2b65-153">Algumas coisas a serem observadas sobre isso:</span><span class="sxs-lookup"><span data-stu-id="d2b65-153">A few things to notice about this:</span></span>

* <span data-ttu-id="d2b65-154">O construtor é privado, pois ele só é chamado por EF Core, e há outro construtor público para uso geral.</span><span class="sxs-lookup"><span data-stu-id="d2b65-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="d2b65-155">O código que usa o serviço injetado (ou seja, o contexto) é defensiva contra `null` para lidar com casos em que EF Core não está criando a instância.</span><span class="sxs-lookup"><span data-stu-id="d2b65-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="d2b65-156">Como o serviço é armazenado em uma propriedade de leitura/gravação, ele será redefinido quando a entidade for anexada a uma nova instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="d2b65-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="d2b65-157">Injetar o DbContext como esse geralmente é considerado um antipadrão, já que ele associa os tipos de entidade diretamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="d2b65-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="d2b65-158">Considere cuidadosamente todas as opções antes de usar a injeção de serviço como esta.</span><span class="sxs-lookup"><span data-stu-id="d2b65-158">Carefully consider all options before using service injection like this.</span></span>
