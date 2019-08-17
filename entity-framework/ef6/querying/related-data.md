---
title: Carregando entidades relacionadas-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: f40034475ed6659b60ab4317605fd1d802218d69
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565326"
---
# <a name="loading-related-entities"></a>Carregando entidades relacionadas
O Entity Framework dá suporte a três maneiras de carregar dados relacionados-carregamento adiantado, carregamento lento e carregamento explícito. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="eagerly-loading"></a>Carregamento adiantado  

O carregamento adiantado é o processo pelo qual uma consulta para um tipo de entidade também carrega entidades relacionadas como parte da consulta. O carregamento adiantado é obtido pelo uso do método include. Por exemplo, as consultas abaixo carregarão Blogs e todas as postagens relacionadas a cada blog.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts
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

Observe que include é um método de extensão no namespace System. Data. Entity, portanto, verifique se você está usando esse namespace.  

### <a name="eagerly-loading-multiple-levels"></a>Carregamento adiantado de vários níveis  

Também é possível carregar cuidadosamente vários níveis de entidades relacionadas. As consultas a seguir mostram exemplos de como fazer isso para as propriedades de navegação de coleção e referência.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

Observe que atualmente não é possível filtrar quais entidades relacionadas são carregadas. Include sempre trará todas as entidades relacionadas.  

## <a name="lazy-loading"></a>Carregamento lento  

O carregamento lento é o processo pelo qual uma entidade ou coleção de entidades é carregada automaticamente do banco de dados na primeira vez que uma propriedade que se refere à entidade/entidades é acessada. Ao usar tipos de entidade POCO, o carregamento lento é obtido criando instâncias de tipos de proxy derivados e, em seguida, substituindo as propriedades virtuais para adicionar o gancho de carregamento. Por exemplo, ao usar a classe de entidade de blog definida abaixo, as postagens relacionadas serão carregadas na primeira vez que a propriedade de navegação de postagens for acessada:  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Desativar o carregamento lento para serialização  

O carregamento e a serialização lentos não são bem misturados e, se você não tiver cuidado, poderá acabar consultando seu banco de dados inteiro porque o carregamento lento está habilitado. A maioria dos serializadores funciona acessando cada propriedade em uma instância de um tipo. O acesso à propriedade dispara o carregamento lento e, portanto, mais entidades são serializadas. Nessas propriedades de entidades são acessadas e ainda mais entidades são carregadas. É uma boa prática ativar o carregamento lento antes de serializar uma entidade. As seções a seguir mostram como fazer isso.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Desativando o carregamento lento para propriedades de navegação específicas  

O carregamento lento da coleção de postagens pode ser desativado ao tornar a propriedade de postagens não virtual:  

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

O carregamento da coleção de postagens ainda pode ser obtido usando o carregamento adiantado (veja *carregamento adiantado* acima) ou o método Load (Confira *carregamento explícito* abaixo).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Desativar o carregamento lento de todas as entidades  

O carregamento lento pode ser desativado para todas as entidades no contexto, definindo um sinalizador na propriedade de configuração. Por exemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

O carregamento de entidades relacionadas ainda pode ser obtido usando o carregamento adiantado (veja *carregamento* adiantado acima) ou o método Load (Confira *carregamento explícito* abaixo).  

## <a name="explicitly-loading"></a>Carregando explicitamente  

Mesmo com o carregamento lento desabilitado, ainda é possível carregar lentamente as entidades relacionadas, mas isso deve ser feito com uma chamada explícita. Para fazer isso, você usa o método Load na entrada da entidade relacionada. Por exemplo:  

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

Observe que o método de referência deve ser usado quando uma entidade tiver uma propriedade de navegação para outra entidade única. Por outro lado, o método de coleção deve ser usado quando uma entidade tem uma propriedade de navegação para uma coleção de outras entidades.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Aplicando filtros ao carregar explicitamente entidades relacionadas  

O método Query fornece acesso à consulta subjacente que Entity Framework será usada ao carregar entidades relacionadas. Em seguida, você pode usar o LINQ para aplicar filtros à consulta antes de executá-lo com uma chamada para um método de extensão LINQ, como ToList, Load, etc. O método Query pode ser usado com as propriedades de navegação e de referência de coleção, mas é mais útil para coleções em que ele pode ser usado para carregar apenas parte da coleção. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```  

Ao usar o método de consulta, geralmente é melhor desativar o carregamento lento para a propriedade de navegação. Isso ocorre porque caso contrário, toda a coleção pode ser carregada automaticamente pelo mecanismo de carregamento lento antes ou depois da execução da consulta filtrada.  

Observe que, embora a relação possa ser especificada como uma cadeia de caracteres em vez de uma expressão lambda, o IQueryable retornado não é genérico quando uma cadeia de caracteres é usada e, portanto, o método Cast geralmente é necessário antes que qualquer coisa útil possa ser feito com ele.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Usando a consulta para contar entidades relacionadas sem carregá-las  

Às vezes, é útil saber quantas entidades estão relacionadas a outra entidade no banco de dados sem realmente incorrer no custo de carregar todas essas entidades. O método de consulta com o método de contagem LINQ pode ser usado para fazer isso. Por exemplo:  

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
