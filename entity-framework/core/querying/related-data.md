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
# <a name="loading-related-data"></a>Carregamento de dados relacionados

Entity Framework Core permite que você use as propriedades de navegação em seu modelo para carregar as entidades relacionadas. Há três padrões de S/RM comuns usados para carregar dados relacionados.
* **Carregamento adiantado** significa que os dados relacionados são carregados do banco de dados como parte da consulta inicial.
* **Carregamento explícito** significa que os dados relacionados são explicitamente carregados do banco de dados em um momento posterior.
* **Carregamento preguiçoso** significa que os dados relacionados são transparentemente carregados do banco de dados quando a propriedade de navegação é acessada.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="eager-loading"></a>Carregamento adiantado

Você pode usar o método `Include` para especificar dados relacionados a serem incluídos nos resultados da consulta. No exemplo a seguir, os blogs que são retornados nos resultados terão suas propriedades `Posts` preenchidas com as postagens relacionadas.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> O Entity Framework Core corrigirá automaticamente as propriedades de navegação para outras entidades que foram previamente carregadas para a instância de contexto. Dessa forma, mesmo se você não incluir explicitamente os dados para uma propriedade de navegação, a propriedade ainda poderá ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.


Você pode incluir dados relacionados de várias relações em uma única consulta.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Incluindo vários níveis

Você pode fazer uma busca detalhada por meio de relações para incluir vários níveis de dados relacionados usando o método `ThenInclude`. O exemplo a seguir carrega todos os blogs, suas postagens relacionadas e o autor de cada postagem.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> As versões atuais do Visual Studio oferecem opções de conclusão de código incorreto, que podem causar expressões corretas a serem sinalizadas com erros de sintaxe ao usar o método `ThenInclude` após uma propriedade de navegação da coleção. Este é um sintoma de um bug de IntelliSense controlado no https://github.com/dotnet/roslyn/issues/8237. É seguro ignorar esses erros de sintaxe artificiais desde que o código esteja correto e possa ser compilado com êxito. 

É possível encadear chamadas múltiplas para `ThenInclude` para continuar incluindo mais níveis de dados relacionados.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Você pode combinar tudo isso para incluir dados relacionados de vários níveis e várias raízes na mesma consulta.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Você talvez queira incluir várias entidades relacionadas para uma das entidades que está sendo incluída. Por exemplo, ao consultar `Blog`, você inclui `Posts` e deseja incluir o `Author` e `Tags` dos `Posts`. Para fazer isso, você precisa especificar cada `Include` a partir da raiz do caminho. Por exemplo, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`. Isso não significa que você obterá junções redundantes, na maioria dos casos o EF irá consolidar as associações durante a geração de SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Incluir em tipos derivados

Você pode incluir dados relacionados de navegações definidos apenas em um tipo derivado usando `Include` e `ThenInclude`. 

Recebe o seguinte modelo:

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

O conteúdo de navegação de `School` de todas as pessoas que são alunos pode ser carregado ansiosamente usando um número de padrões:

- usando cast
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- usando o operador `as`
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- usando a sobrecarga de `Include` que usa o parâmetro de tipo `string`
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a>Operadores include ignorados

Se você alterar a consulta para que ela não retorne instâncias do tipo de entidade com o qual ela começa, então os operadores include serão ignorados.


No exemplo a seguir, os operadores de inclusão se baseiam no `Blog`, mas em seguida o operador `Select` é usado para alterar a consulta para retornar um tipo anônimo. Nesse caso, os operadores de inclusão não têm efeito.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Por padrão, o EF Core registrará um aviso quando operadores de inclusão são ignorados. Consulte [log](../miscellaneous/logging.md) para obter mais informações sobre como exibir a saída de log. Você pode alterar o comportamento quando um operador include é ignorado para gerar ou não fazer nada. Isso é feito ao configurar as opções para o seu contexto - normalmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Carregamento explícito

> [!NOTE]  
> Esse recurso foi introduzido no EF Core 1.1.

Você pode carregar explicitamente uma propriedade de navegação por meio da API `DbContext.Entry(...)`.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Você pode carregar também explicitamente uma propriedade de navegação executando uma consulta separada que retorna as entidades relacionadas. Se o controle de alterações estiver habilitado, ao carregar uma entidade, o EF Core vai automaticamente definir as propriedades de navegação da entidade carregada recentemente para se referir a qualquer entidade já carregada e definir as propriedades de navegação das entidades já carregadas para se referir às entidades carregadas recentemente.

### <a name="querying-related-entities"></a>Consultando entidades relacionadas

Você também pode obter uma consulta LINQ que representa o conteúdo de uma propriedade de navegação.

Isso permite que você faça coisas como a execução de um operador de agregação sobre as entidades relacionadas sem carregá-los na memória.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Você também pode filtrar quais entidades relacionadas são carregadas na memória.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Carregamento preguiçoso

> [!NOTE]  
> Esse recurso foi introduzido no EF Core 2.1.

A maneira mais simples de usar o carregamento lento é instalando o pacote [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) e o habilitando com uma chamada para `UseLazyLoadingProxies`. Por exemplo:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Ou, ao usar AddDbContext:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
Em seguida, EF Core habilitará o carregamento lento para qualquer propriedade de navegação que pode ser substituída – isto é, ela deve ser `virtual` e estar em uma classe da qual pode ser herdada. Por exemplo, nas entidades a seguir, as propriedades de navegação `Post.Blog` e `Blog.Posts` terão carregamento lento.
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
### <a name="lazy-loading-without-proxies"></a>Carregamento lento sem proxies

Proxies de carregamento lento trabalham inserindo o serviço `ILazyLoader` em uma entidade, conforme descrito em [construtores de tipo de entidade](../modeling/constructors.md). Por exemplo:
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
sso não requer que os tipos de entidade sejam herdados de ou que as propriedades de navegação sejam virtuais e permite que que instâncias da entidade criadas com `new` carreguem lentamente quando conectadas a um contexto. No entanto, ele requer uma referência para o serviço `ILazyLoader`, que associa os tipos de entidade para o assembly principal EF. Para evitar isso, o núcleo EF permite que o método `ILazyLoader.Load` seja inserido como um representante.
 Por exemplo:
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
O código acima usa um método de extensão `Load` para fazer o uso do delegate um pouco mais limpo:
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
> O parâmetro de construtor para o representante de carregamento lento deve ser chamado "lazyLoader". Configuração para usar um nome diferente, que isso é planejado para uma versão futura.

## <a name="related-data-and-serialization"></a>Serialização e dados relacionados

Como o EF Core irá automaticamente corrigir as propriedades de navegação, você pode acabar com ciclos em seu gráfico de objetos. Por exemplo, o carregamento de um blog suas postagens relacionadas resultará em um objeto de blog referenciando uma coleção de postagens. Cada uma dessas postagens terá uma referência de volta para o blog.

Algumas estruturas de serialização não permitem esses ciclos. Por exemplo, Json.NET lançará a exceção a seguir se for encontrado um ciclo.

> Newtonsoft.Json.JsonSerializationException: Self referenciando loop detectado para a propriedade 'Blog' com tipo 'MyApplication.Models.Blog'.

Se você estiver usando o ASP.NET Core, poderá configurar Json.NET para ignorar ciclos que ele encontrar no gráfico de objeto. Isso é feito no método `ConfigureServices(...)` em `Startup.cs`.

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
