---
title: Tipos de entidade com construtores – EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 1b36197465fb9a6571a306d36eb1e9d885a5399e
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152459"
---
# <a name="entity-types-with-constructors"></a>Tipos de entidade com construtores

> [!NOTE]  
> Este recurso é novo no EF Core 2.1.

Começando com o EF Core 2.1, agora é possível definir um construtor com parâmetros e fazer com que o EF Core chamar esse construtor, ao criar uma instância da entidade. Os parâmetros do construtor podem ser associados a propriedades mapeadas ou para vários tipos de serviços para facilitar a comportamentos, como carregamento lento.

> [!NOTE]  
> A partir do EF Core 2.1, todos os associação de construtor é por convenção. Configuração de um construtor específico para usar está planejada para uma versão futura.

## <a name="binding-to-mapped-properties"></a>Associando a propriedades mapeadas

Considere um modelo típico de postagem do Blog:

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

Quando o EF Core cria instâncias desses tipos, como para os resultados de uma consulta, ele primeiro chamar o construtor padrão sem parâmetros e, em seguida, defina cada propriedade para o valor do banco de dados. No entanto, se o EF Core encontra um construtor com parâmetros com nomes de parâmetro e tipos que correspondem da mapeados propriedades, ele chamará em vez disso, o construtor com parâmetros com valores para essas propriedades e não definirá explicitamente cada propriedade. Por exemplo:

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
Algumas coisas a observar:
* Nem todas as propriedades precisam ter os parâmetros do construtor. Por exemplo, a propriedade de Post.Content não é definida por qualquer parâmetro de construtor, portanto, o EF Core defini-lo depois de chamar o construtor da maneira normal.
* Os nomes e tipos de parâmetro devem corresponder ao tipos de propriedade e nomes, exceto que as propriedades podem ser padrão Pascal-case enquanto os parâmetros estão em camel case.
* O EF Core não é possível definir propriedades de navegação (por exemplo, Blog ou postagens acima) usando um construtor.
* O construtor pode ser público, privada ou ter outros recursos de acessibilidade.

### <a name="read-only-properties"></a>Propriedades somente leitura

Depois que as propriedades estão sendo definidas por meio do construtor pode fazer sentido fazer alguns deles somente leitura. O EF Core dá suporte a isso, mas há algumas coisas para verificar:
* As propriedades sem setters não são mapeadas por convenção. (Isso tende a mapear as propriedades que não devem ser mapeadas, como as propriedades computadas.)
* Usar valores de chave geradas automaticamente requer uma propriedade de chave é leitura / gravação, uma vez que o valor da chave precisa ser definido pelo gerador de chave ao inserir novas entidades.

Uma maneira fácil de evitar essas coisas é usar setters privados. Por exemplo:
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
O EF Core vê uma propriedade com um setter privado como leitura / gravação, o que significa que todas as propriedades são mapeadas como antes e a chave pode ainda ser geradas pelo repositório.

É uma alternativa ao uso de setters privados tornar as propriedades realmente somente leitura e adicionar mapeamento mais explícito em OnModelCreating. Da mesma forma, algumas propriedades podem ser completamente removidas e substituídas pelo somente os campos. Por exemplo, considere estes tipos de entidade:

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
E essa configuração em OnModelCreating:
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
Coisas a observar:
* A chave "property" agora é um campo. Não é um `readonly` campo para que as chaves geradas pelo repositório podem ser usadas.
* As outras propriedades são propriedades somente leitura definidas somente no construtor.
* Se o valor de chave primária só é definido pelo EF ou ler do banco de dados, não é necessário incluí-lo no construtor. Isso deixa a chave "property" como um campo simples e deixa claro que ele não deve ser definido explicitamente ao criar novos blogs ou postagens.

> [!NOTE]  
> Esse código resulta em aviso '169', indicando que o campo nunca é usado do compilador. Isso pode ser ignorado, pois, na realidade o EF Core é usando o campo de uma maneira extralinguistic.

## <a name="injecting-services"></a>Injeção de serviços

O EF Core também pode injetar "serviços" no construtor do tipo de entidade. Por exemplo, a seguir pode ser injetado:
* `DbContext` -a instância de contexto atual, que também pode ser digitada como seu tipo derivado de DbContext
* `ILazyLoader` -o serviço de carregamento lento, consulte o [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes
* `Action<object, string>` -um delegado de carregamento lento – consulte a [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes
* `IEntityType` – EF Core metadados associados com esse tipo de entidade

> [!NOTE]  
> A partir do EF Core 2.1, somente os serviços conhecidos pelo EF Core podem ser injetados. Suporte para injeção de serviços de aplicativo está sendo considerado para uma versão futura.

Por exemplo, um DbContext injetado pode ser usado para acessar seletivamente o banco de dados para obter informações sobre entidades relacionadas sem carregar todos eles. No exemplo a seguir, isso é usado para obter o número de postagens em um blog sem carregar as postagens de:

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
Algumas coisas a observar sobre isso:
* O construtor é particular, uma vez que ele só é chamado pelo EF Core, e há outro construtor público para uso geral.
* O código usando o serviço injetado (ou seja, o contexto) é defensivo em relação a ela que está sendo `null` para lidar com casos em que o EF Core não é criar a instância.
* Porque o serviço é armazenado em uma propriedade de leitura/gravação, será redefinido quando a entidade é anexada a uma nova instância de contexto.

> [!WARNING]  
> Injetar o DbContext como esta é geralmente considerado um antipadrão desde que seus tipos de entidade associa diretamente para o EF Core. Considere cuidadosamente todas as opções antes de usar a injeção de serviço como esse.
