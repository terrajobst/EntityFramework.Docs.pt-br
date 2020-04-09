---
title: Como carregar dados relacionados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 915aaa41beb495a046f2d6260e9c3b174d5f3031
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417673"
---
# <a name="loading-related-data"></a>Como carregar dados relacionados

O Entity Framework Core permite que você use as propriedades de navegação em seu modelo para carregar as entidades relacionadas. Há três padrões de O/RM comuns usados para carregar os dados relacionados.

* **Carregamento adiantado** significa que os dados relacionados são carregados do banco de dados como parte da consulta inicial.
* **Carregamento explícito** significa que os dados relacionados são explicitamente carregados do banco de dados em um momento posterior.
* **Carregamento lento** significa que os dados relacionados são carregados de modo transparente do banco de dados quando a propriedade de navegação é acessada.

> [!TIP]  
> Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="eager-loading"></a>Carregamento adiantado

Você pode usar o método `Include` para especificar os dados relacionados a serem incluídos nos resultados da consulta. No exemplo a seguir, os blogs que são retornados nos resultados terão suas propriedades `Posts` preenchidas com as postagens relacionadas.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> O Entity Framework Core corrigirá automaticamente as propriedades de navegação para outras entidades que forem carregadas anteriormente na instância do contexto. Então, mesmo se você não incluir de forma explícita os dados para a propriedade de navegação, a propriedade ainda pode ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.

Você pode incluir dados relacionados de várias relações em uma única consulta.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Incluindo vários níveis

Você pode fazer uma busca detalhada por meio de relações para incluir vários níveis de dados relacionados usando o método `ThenInclude`. O exemplo a seguir carrega todos os blogs, suas postagens relacionadas e o autor de cada postagem.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

É possível encadear chamadas múltiplas a `ThenInclude` para continuar incluindo outros níveis de dados relacionados.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Você pode combinar tudo isso para incluir dados relacionados de vários níveis e várias raízes na mesma consulta.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

Você talvez queira incluir várias entidades relacionadas para uma das entidades que está sendo incluída. Por exemplo, ao consultar os `Blogs`, você inclui os `Posts` e, em seguida, o `Author` e as `Tags` dos `Posts`. Para fazer isso, você precisará especificar cada uma para incluir o caminho a partir da raiz. Por exemplo, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`. Isso não significa que você obterá junções redundantes. Na maioria dos casos, o EF consolidará as junções ao gerar o SQL.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> Desde a versão 3.0.0, cada `Include` uma fará com que um JOIN adicional seja adicionado às consultas SQL produzidas por provedores relacionais, enquanto as versões anteriores geraram consultas SQL adicionais. Isso pode alterar significativamente o desempenho de suas consultas, para melhor ou pior. Em particular, as consultas LINQ com `Include` um número extremamente alto de operadores podem precisar ser divididas em várias consultas LINQ separadas, a fim de evitar o problema de explosão cartesiana.

### <a name="include-on-derived-types"></a>Incluir para tipos derivados

Você pode incluir dados relacionados de navegações definidas apenas em um tipo derivado usando `Include` e `ThenInclude`.

Com o seguinte modelo:

```csharp
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

Conteúdo da navegação em `School` de todas as pessoas que são alunos pode ser carregada de maneira adiantada usando um número de padrões:

* usando conversão

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* usando o operador `as`

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* usando a sobrecarga de `Include` que usa o parâmetro de tipo `string`

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a>Carregamento explícito

Você pode carregar explicitamente uma propriedade de navegação pela API `DbContext.Entry(...)`.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Você também pode carregar explicitamente uma propriedade de navegação executando uma consulta separada que retorna as entidades relacionadas. Se o controle de alterações estiver habilitado, ao carregar uma entidade, o EF Core automaticamente definirá as propriedades de navegação da entidade recém-carregada para se referirem a alguma entidade já carregada e definir as propriedades de navegação das entidades já carregadas para se referirem à entidade recém-carregada.

### <a name="querying-related-entities"></a>Como consultar entidades relacionadas

Você também pode obter uma consulta LINQ que representa o conteúdo de uma propriedade de navegação.

Isso permite que você faça coisas como executar um operador de agregação sobre as entidades relacionadas sem carregá-las na memória.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Você também pode filtrar quais entidades relacionadas são carregadas na memória.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Carregamento lento

A maneira mais simples para usar o carregamento lento é instalando o pacote [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) e habilitá-lo com uma chamada para `UseLazyLoadingProxies`. Por exemplo:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

Ou ao usar AddDbContext:

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

O EF Core, em seguida, habilitará o carregamento lento para qualquer propriedade de navegação que pode ser substituída – ou seja, deverá ser `virtual` e em uma classe que pode ser herdada. Por exemplo, nas entidades a seguir, as propriedades de navegação `Post.Blog` e `Blog.Posts` serão de carregamento lento.

```csharp
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

Os proxies de carregamento lento funcionam inserindo o serviço `ILazyLoader` em uma entidade, conforme descrito em [Construtores de tipo de entidade](../modeling/constructors.md). Por exemplo:

```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

Isso não exige que os tipos de entidade sejam herdados de ou as propriedades de navegação sejam virtuais e permitam que as instâncias da entidade criadas com `new` sejam carregados de maneira lenta assim que forem conectadas a um contexto. No entanto, isso exige referência ao serviço `ILazyLoader`, que é definido no pacote [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/). Este pacote contém um conjunto mínimo de tipos, portanto, há pouco impacto em depender dele. No entanto, para evitar completamente a dependência de todos os pacotes do EF Core em tipos de entidade, é possível injetar o método `ILazyLoader.Load` como um delegado. Por exemplo:

```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

O código acima usa um método de extensão `Load` para deixar o representante um pouco mais limpo:

```csharp
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
> O parâmetro de construtor para o representante de carregamento lento deve ser chamado de "lazyLoader". A configuração para usar um nome diferente é planejada para uma versão futura.

## <a name="related-data-and-serialization"></a>Serialização e dados relacionados

Como o EF Core automaticamente corrigirá as propriedades de navegação, você poderá acabar com ciclos em seu gráfico de objeto. Por exemplo, o carregamento de um blog e suas postagens relacionadas resultará em um objeto de blog que referencia uma coleção de postagens. Cada uma dessas postagens terá uma referência de volta para o blog.

Algumas estruturas de serialização não permitem esses ciclos. Por exemplo, Json.NET gerará a exceção a seguir se for encontrado um ciclo.

> Newtonsoft.Json.JsonSerializationException: loop autorreferenciado detectado para a propriedade 'Blog' com tipo 'MyApplication.Models.Blog'.

Se você estiver usando o ASP.NET Core, poderá configurar o Json.NET para ignorar os ciclos que encontrar no gráfico de objeto. Isso é feito no método `ConfigureServices(...)` no `Startup.cs`.

```csharp
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

Outra alternativa é decorar uma das propriedades de navegação com o atributo `[JsonIgnore]`, que instrui o Json.NET para não percorrer essa propriedade de navegação durante a serialização.
