---
title: Tipos de entidade com construtores - Core de EF
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 8cea624c295f99ef54cb8b4758642eade03c235e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="entity-types-with-constructors"></a>Tipos de entidade com construtores

> [!NOTE]  
> Esse recurso é novo no EF Core 2.1.

Começando com o EF Core 2.1, agora é possível definir um construtor com parâmetros e ter EF Core chama este construtor ao criar uma instância da entidade. Os parâmetros do construtor podem ser associados às propriedades mapeadas, ou para vários tipos de serviços para facilitar a comportamentos como carregamento lento.

> [!NOTE]  
> A partir do EF Core 2.1, todos os associação de construtor é por convenção. Configuração do construtor específico a ser usado é planejada para uma versão futura.

## <a name="binding-to-mapped-properties"></a>Associando a propriedades mapeadas

Considere um modelo típico de postagem do Blog:

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

Quando o EF Core cria instâncias desses tipos, como para os resultados de uma consulta, ele primeiro chamar o construtor sem parâmetros padrão e, em seguida, defina cada propriedade para o valor do banco de dados. No entanto, se EF Core localiza um construtor com parâmetros com nomes de parâmetro e tipos que correspondem às de mapeados propriedades, ele chamará em vez disso, o construtor com parâmetros com valores para as propriedades e não definirá explicitamente cada propriedade. Por exemplo:

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
Algumas coisas a observar:
* Nem todas as propriedades precisam ter parâmetros do construtor. Por exemplo, a propriedade Post.Content não é definida por qualquer parâmetro de construtor, para que Core EF definirá depois de chamar o construtor da maneira normal.
* Os nomes e tipos de parâmetro devem corresponder tipos de propriedade e nomes, exceto que as propriedades podem ser maiusculas e minúsculas de Pascal enquanto os parâmetros são concatenados.
* EF Core não é possível definir propriedades de navegação (como Blog ou postagens acima) usando um construtor.
* O construtor pode ser público, privada, ou qualquer outro tipo de acessibilidade.

### <a name="read-only-properties"></a>Propriedades somente leitura

Depois que as propriedades estão sendo definidas via construtor pode fazer sentido fazer algumas delas somente leitura. EF Core dá suporte a isso, mas há algumas coisas para verificar:
* Propriedades sem setters não mapeadas pela convenção. (Isso tende mapear propriedades que não devem ser mapeadas, como as propriedades calculadas).
* Usar valores de chave gerados automaticamente requer uma propriedade de chave é leitura / gravação, pois o valor da chave precisa ser definida pelo gerador de chave ao inserir novas entidades.

Uma maneira fácil de evitar essas coisas é usar setters privadas. Por exemplo:
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
Núcleo EF vê uma propriedade com um setter privada como leitura-gravação, o que significa que todas as propriedades são mapeadas como antes e a chave pode ainda ser gerado pelo repositório.

Uma alternativa ao uso particulares setters é tornar realmente somente leitura, propriedades e adicionar mapeamento mais explícito em OnModelCreating. Da mesma forma, algumas propriedades podem ser completamente removidas e substituídas por apenas campos. Por exemplo, considere esses tipos de entidade:

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
E essa configuração no OnModelCreating:
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
Observe o seguinte:
* A chave "property" agora é um campo. Não é um `readonly` campo para que as chaves geradas pelo repositório podem ser usadas. 
* As outras propriedades são propriedades somente leitura definidas somente no construtor.
* Se o valor de chave primária só é definido por EF ou ler do banco de dados, não é necessário para incluí-lo no construtor. Isso deixa a chave "property" como um campo simples e deixa claro que ele não deve ser definido explicitamente ao criar novos blogs ou postagens.

> [!NOTE]  
> Esse código resultará no compilador aviso '169', indicando que o campo nunca é usado. Isso pode ser ignorado já que na verdade EF Core é usando o campo de maneira extralinguistic.

## <a name="injecting-services"></a>Injeção de serviços

EF principal também pode inserir "serviços" no construtor do tipo de entidade. Por exemplo, o seguinte pode ser inserido:
* `DbContext` -a instância do contexto atual, que também pode ser digitada como seu tipo derivado de DbContext
* `ILazyLoader` -o serviço de carregamento lento - consulte o [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes
* `Action<object, string>` -um delegado de carregamento lento - consulte o [documentação de carregamento lento](../querying/related-data.md) para obter mais detalhes
* `IEntityType` -os metadados de Core EF associados a este tipo de entidade

> [!NOTE]  
> A partir do EF Core 2.1, apenas serviços conhecidos por núcleo de EF podem ser inseridos. Suporte para injeção de serviços de aplicativo está sendo considerado para uma versão futura.

Por exemplo, um DbContext injetado pode ser usado para acessar seletivamente o banco de dados para obter informações sobre entidades relacionadas sem carregar todos eles. No exemplo a seguir, isso é usado para obter o número de mensagens em um blog sem carregar as postagens:

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
Algumas coisas a observar sobre isso:
* O construtor é privado, uma vez que ele só é chamado por núcleo EF e há outro construtor público para uso geral.
* O código usando o serviço injetado (ou seja, o contexto) é defesa contra ele sendo `null` para lidar com casos onde EF Core não é criar a instância.
* Porque o serviço é armazenado em uma propriedade de leitura/gravação, será redefinido quando a entidade é anexada a uma nova instância de contexto.

> [!WARNING]  
> Injetar DbContext como isso é geralmente considerado um antidesde padrão de que associa seus tipos de entidade diretamente ao EF Core. Considere cuidadosamente todas as opções antes de usar a injeção de serviço assim.
