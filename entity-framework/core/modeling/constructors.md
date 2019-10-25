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
# <a name="entity-types-with-constructors"></a>Tipos de entidade com construtores

> [!NOTE]  
> Este recurso é novo no EF Core 2.1.

A partir do EF Core 2,1, agora é possível definir um construtor com parâmetros e EF Core chamar esse construtor ao criar uma instância da entidade. Os parâmetros do Construtor podem ser associados a Propriedades mapeadas ou a vários tipos de serviços para facilitar comportamentos como carregamento lento.

> [!NOTE]  
> A partir de EF Core 2,1, toda a associação de construtor é por convenção. A configuração de construtores específicos a serem usados é planejada para uma versão futura.

## <a name="binding-to-mapped-properties"></a>Associação a Propriedades mapeadas

Considere um modelo de blog/postagem típico:

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

Quando EF Core cria instâncias desses tipos, como para os resultados de uma consulta, ele primeiro chamará o construtor padrão sem parâmetros e, em seguida, definirá cada propriedade para o valor do banco de dados. No entanto, se EF Core encontrar um construtor com parâmetros com nomes de parâmetro e tipos que correspondam às propriedades mapeadas, ele chamará o construtor parametrizado com valores para essas propriedades e não definirá cada propriedade explicitamente. Por exemplo:

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

Algumas coisas a serem observadas:

* Nem todas as propriedades precisam ter parâmetros de construtor. Por exemplo, a propriedade post. Content não é definida por nenhum parâmetro de construtor, portanto EF Core a definirá depois de chamar o construtor da maneira normal.
* Os tipos e nomes de parâmetro devem corresponder aos tipos de propriedade e nomes, exceto que as propriedades podem ser Pascal-case enquanto os parâmetros são camel-case.
* EF Core não pode definir propriedades de navegação (como blog ou postagens acima) usando um construtor.
* O construtor pode ser público, privado ou ter qualquer outra acessibilidade. No entanto, os proxies de carregamento lento exigem que o Construtor seja acessível a partir da classe de proxy herdada. Geralmente, isso significa torná-lo público ou protegido.

### <a name="read-only-properties"></a>Propriedades somente leitura

Depois que as propriedades estão sendo definidas por meio do Construtor, é possível fazer sentido tornar algumas delas somente leitura. O EF Core dá suporte a isso, mas há algumas coisas a serem examinadas:

* Propriedades sem setters não são mapeadas por convenção. (Isso tende a mapear propriedades que não devem ser mapeadas, como propriedades computadas.)
* O uso de valores de chave gerados automaticamente requer uma propriedade de chave que é de leitura/gravação, já que o valor da chave precisa ser definido pelo gerador de chave ao inserir novas entidades.

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

EF Core vê uma propriedade com um setter particular como leitura/gravação, o que significa que todas as propriedades são mapeadas como antes e a chave ainda pode ser gerada pelo armazenamento.

Uma alternativa ao uso de setters privados é tornar as propriedades realmente somente leitura e adicionar um mapeamento mais explícito em OnModelCreating. Da mesma forma, algumas propriedades podem ser completamente removidas e substituídas por apenas campos. Por exemplo, considere estes tipos de entidade:

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

Itens a serem observados:

* A chave "Property" agora é um campo. Não é um campo `readonly` para que as chaves geradas pelo repositório possam ser usadas.
* As outras propriedades são propriedades somente leitura definidas somente no construtor.
* Se o valor da chave primária for apenas definido pelo EF ou lido no banco de dados, não haverá necessidade de incluí-lo no construtor. Isso deixa a chave "Propriedade" como um campo simples e torna claro que ela não deve ser definida explicitamente ao criar novos Blogs ou postagens.

> [!NOTE]  
> Esse código resultará no aviso do compilador ' 169 ', indicando que o campo nunca é usado. Isso pode ser ignorado, pois na realidade EF Core está usando o campo de uma maneira extralinguistic.

## <a name="injecting-services"></a>Injetando serviços

EF Core também pode injetar "serviços" no construtor de um tipo de entidade. Por exemplo, o seguinte pode ser injetado:

* `DbContext`-a instância de contexto atual, que também pode ser digitada como seu tipo DbContext derivado
* `ILazyLoader`-o serviço de carregamento lento--consulte a [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes
* `Action<object, string>`-um delegado de carregamento lento--consulte a [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes
* `IEntityType`-os metadados de EF Core associados a esse tipo de entidade

> [!NOTE]  
> A partir de EF Core 2,1, somente os serviços conhecidos pelo EF Core podem ser injetados. O suporte para injetar serviços de aplicativos está sendo considerado para uma versão futura.

Por exemplo, um DbContext injetado pode ser usado para acessar seletivamente o banco de dados para obter informações sobre entidades relacionadas sem carregá-las. No exemplo abaixo, é usado para obter o número de postagens em um blog sem carregar as postagens:

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

Algumas coisas a serem observadas sobre isso:

* O construtor é privado, pois ele só é chamado por EF Core, e há outro construtor público para uso geral.
* O código que usa o serviço injetado (ou seja, o contexto) é defensiva contra `null` para lidar com casos em que EF Core não está criando a instância.
* Como o serviço é armazenado em uma propriedade de leitura/gravação, ele será redefinido quando a entidade for anexada a uma nova instância de contexto.

> [!WARNING]  
> Injetar o DbContext como esse geralmente é considerado um antipadrão, já que ele associa os tipos de entidade diretamente a EF Core. Considere cuidadosamente todas as opções antes de usar a injeção de serviço como esta.
