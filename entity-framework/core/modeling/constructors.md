---
title: Tipos de entidade com construtores - Core de EF
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 38ab0c1c3cd8c490875abf30b8478c99bc58630f
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="0665c-102">Tipos de entidade com construtores</span><span class="sxs-lookup"><span data-stu-id="0665c-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="0665c-103">Esse recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0665c-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="0665c-104">Começando com o EF Core 2.1, agora é possível definir um construtor com parâmetros e ter EF Core chama este construtor ao criar uma instância da entidade.</span><span class="sxs-lookup"><span data-stu-id="0665c-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="0665c-105">Os parâmetros do construtor podem ser associados às propriedades mapeadas, ou para vários tipos de serviços para facilitar a comportamentos como carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="0665c-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="0665c-106">A partir do EF Core 2.1, todos os associação de construtor é por convenção.</span><span class="sxs-lookup"><span data-stu-id="0665c-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="0665c-107">Configuração do construtor específico a ser usado é planejada para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="0665c-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="0665c-108">Associando a propriedades mapeadas</span><span class="sxs-lookup"><span data-stu-id="0665c-108">Binding to mapped properties</span></span>

<span data-ttu-id="0665c-109">Considere um modelo típico de postagem do Blog:</span><span class="sxs-lookup"><span data-stu-id="0665c-109">Consider a typical Blog/Post model:</span></span>

```Csharp
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

<span data-ttu-id="0665c-110">Quando o EF Core cria instâncias desses tipos, como para os resultados de uma consulta, ele primeiro chamar o construtor sem parâmetros padrão e, em seguida, defina cada propriedade para o valor do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0665c-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="0665c-111">No entanto, se EF Core localiza um construtor com parâmetros com nomes de parâmetro e tipos que correspondem às de mapeados propriedades, ele chamará em vez disso, o construtor com parâmetros com valores para as propriedades e não definirá explicitamente cada propriedade.</span><span class="sxs-lookup"><span data-stu-id="0665c-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="0665c-112">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0665c-112">For example:</span></span>

```Csharp
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
<span data-ttu-id="0665c-113">Algumas coisas a observar:</span><span class="sxs-lookup"><span data-stu-id="0665c-113">Some things to note:</span></span>
* <span data-ttu-id="0665c-114">Nem todas as propriedades precisam ter parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="0665c-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="0665c-115">Por exemplo, a propriedade Post.Content não é definida por qualquer parâmetro de construtor, para que Core EF definirá depois de chamar o construtor da maneira normal.</span><span class="sxs-lookup"><span data-stu-id="0665c-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="0665c-116">Os nomes e tipos de parâmetro devem corresponder tipos de propriedade e nomes, exceto que as propriedades podem ser maiusculas e minúsculas de Pascal enquanto os parâmetros são concatenados.</span><span class="sxs-lookup"><span data-stu-id="0665c-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="0665c-117">EF Core não é possível definir propriedades de navegação (como Blog ou postagens acima) usando um construtor.</span><span class="sxs-lookup"><span data-stu-id="0665c-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="0665c-118">O construtor pode ser público, privada, ou qualquer outro tipo de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="0665c-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="0665c-119">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="0665c-119">Read-only properties</span></span>

<span data-ttu-id="0665c-120">Depois que as propriedades estão sendo definidas via construtor pode fazer sentido fazer algumas delas somente leitura.</span><span class="sxs-lookup"><span data-stu-id="0665c-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="0665c-121">EF Core dá suporte a isso, mas há algumas coisas para verificar:</span><span class="sxs-lookup"><span data-stu-id="0665c-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="0665c-122">Propriedades sem setters não mapeadas pela convenção.</span><span class="sxs-lookup"><span data-stu-id="0665c-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="0665c-123">(Isso tende mapear propriedades que não devem ser mapeadas, como as propriedades calculadas).</span><span class="sxs-lookup"><span data-stu-id="0665c-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="0665c-124">Usar valores de chave gerados automaticamente requer uma propriedade de chave é leitura / gravação, pois o valor da chave precisa ser definida pelo gerador de chave ao inserir novas entidades.</span><span class="sxs-lookup"><span data-stu-id="0665c-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="0665c-125">Uma maneira fácil de evitar essas coisas é usar setters privadas.</span><span class="sxs-lookup"><span data-stu-id="0665c-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="0665c-126">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0665c-126">For example:</span></span>
```Csharp
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
<span data-ttu-id="0665c-127">Núcleo EF vê uma propriedade com um setter privada como leitura-gravação, o que significa que todas as propriedades são mapeadas como antes e a chave pode ainda ser gerado pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="0665c-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="0665c-128">Uma alternativa ao uso particulares setters é tornar realmente somente leitura, propriedades e adicionar mapeamento mais explícito em OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="0665c-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="0665c-129">Da mesma forma, algumas propriedades podem ser completamente removidas e substituídas por apenas campos.</span><span class="sxs-lookup"><span data-stu-id="0665c-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="0665c-130">Por exemplo, considere esses tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="0665c-130">For example, consider these entity types:</span></span>

```Csharp
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
<span data-ttu-id="0665c-131">E essa configuração no OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="0665c-131">And this configuration in OnModelCreating:</span></span>
```Csharp
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
<span data-ttu-id="0665c-132">Observe o seguinte:</span><span class="sxs-lookup"><span data-stu-id="0665c-132">Things to note:</span></span>
* <span data-ttu-id="0665c-133">A chave "property" agora é um campo.</span><span class="sxs-lookup"><span data-stu-id="0665c-133">The key "property" is now a field.</span></span> <span data-ttu-id="0665c-134">Não é um `readonly` campo para que as chaves geradas pelo repositório podem ser usadas.</span><span class="sxs-lookup"><span data-stu-id="0665c-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="0665c-135">As outras propriedades são propriedades somente leitura definidas somente no construtor.</span><span class="sxs-lookup"><span data-stu-id="0665c-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="0665c-136">Se o valor de chave primária só é definido por EF ou ler do banco de dados, não é necessário para incluí-lo no construtor.</span><span class="sxs-lookup"><span data-stu-id="0665c-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="0665c-137">Isso deixa a chave "property" como um campo simples e deixa claro que ele não deve ser definido explicitamente ao criar novos blogs ou postagens.</span><span class="sxs-lookup"><span data-stu-id="0665c-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="0665c-138">Esse código resultará no compilador aviso '169', indicando que o campo nunca é usado.</span><span class="sxs-lookup"><span data-stu-id="0665c-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="0665c-139">Isso pode ser ignorado já que na verdade EF Core é usando o campo de maneira extralinguistic.</span><span class="sxs-lookup"><span data-stu-id="0665c-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="0665c-140">Injeção de serviços</span><span class="sxs-lookup"><span data-stu-id="0665c-140">Injecting services</span></span>

<span data-ttu-id="0665c-141">EF principal também pode inserir "serviços" no construtor do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="0665c-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="0665c-142">Por exemplo, o seguinte pode ser inserido:</span><span class="sxs-lookup"><span data-stu-id="0665c-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="0665c-143">`DbContext` -a instância do contexto atual, que também pode ser digitada como seu tipo derivado de DbContext</span><span class="sxs-lookup"><span data-stu-id="0665c-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="0665c-144">`ILazyLoader` -o serviço de carregamento lento - consulte o [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes</span><span class="sxs-lookup"><span data-stu-id="0665c-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="0665c-145">`Action<object, string>` -um delegado de carregamento lento - consulte o [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes</span><span class="sxs-lookup"><span data-stu-id="0665c-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="0665c-146">`IEntityType` -os metadados de Core EF associados a este tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="0665c-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="0665c-147">A partir do EF Core 2.1, apenas serviços conhecidos por núcleo de EF podem ser inseridos.</span><span class="sxs-lookup"><span data-stu-id="0665c-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="0665c-148">Suporte para injeção de serviços de aplicativo está sendo considerado para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="0665c-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="0665c-149">Por exemplo, um DbContext injetado pode ser usado para acessar seletivamente o banco de dados para obter informações sobre entidades relacionadas sem carregar todos eles.</span><span class="sxs-lookup"><span data-stu-id="0665c-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="0665c-150">No exemplo a seguir, isso é usado para obter o número de mensagens em um blog sem carregar as postagens:</span><span class="sxs-lookup"><span data-stu-id="0665c-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

```Csharp
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
<span data-ttu-id="0665c-151">Algumas coisas a observar sobre isso:</span><span class="sxs-lookup"><span data-stu-id="0665c-151">A few things to notice about this:</span></span>
* <span data-ttu-id="0665c-152">O construtor é particular, como é apenas nunca chamam por núcleo EF e há outro construtor público para uso geral.</span><span class="sxs-lookup"><span data-stu-id="0665c-152">The constructor is private, since it is only ever call by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="0665c-153">O código usando o serviço injetado (ou seja, o contexto) é defesa contra ele sendo `null` para lidar com casos onde EF Core não é criar a instância.</span><span class="sxs-lookup"><span data-stu-id="0665c-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="0665c-154">Porque o serviço é armazenado em uma propriedade de leitura/gravação, será redefinido quando a entidade é anexada a uma nova instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="0665c-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="0665c-155">Injetar DbContext como isso é geralmente considerado um antidesde padrão de que associa seus tipos de entidade diretamente ao EF Core.</span><span class="sxs-lookup"><span data-stu-id="0665c-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="0665c-156">Considere cuidadosamente todas as opções antes de usar a injeção de serviço assim.</span><span class="sxs-lookup"><span data-stu-id="0665c-156">Carefully consider all options before using service injection like this.</span></span>
