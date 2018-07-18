---
title: Carregando relacionados entidades - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
caps.latest.revision: 3
ms.openlocfilehash: e7adc9aea11a7a8e9b87b4f9e9120aa7316588db
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119832"
---
# <a name="loading-related-entities"></a>Carregando entidades relacionadas
Entity Framework oferece suporte a três maneiras de carregar dados relacionados – carregamento rápido, carregamento lento e o carregamento explícito. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="eagerly-loading"></a>Carregar avidamente  

O carregamento adiantado é o processo pelo qual uma consulta para um tipo de entidade também carrega entidades relacionadas como parte da consulta. O carregamento adiantado é obtido por meio do método Include. Por exemplo, as consultas abaixo carregará blogs e todas as postagens relacionadas a cada blog.  

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

Observe que a inclusão é um método de extensão no namespace System, então, certifique-se de você estiver usando o namespace.  

### <a name="eagerly-loading-multiple-levels"></a>Carregar avidamente vários níveis  

Também é possível carregar avidamente vários níveis de entidades relacionadas. Consultas a seguir mostram exemplos de como fazer isso para coleta e propriedades de navegação de referência.  

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

Observe que não é possível filtrar quais entidades relacionadas são carregadas no momento. Incluir irá sempre colocar em entidades todas relacionadas.  

## <a name="lazy-loading"></a>Carregamento lento  

Carregamento lento é o processo pelo qual uma entidade ou uma coleção de entidades é carregada automaticamente do banco de dados na primeira vez que uma propriedade referindo-se às entidades/entidade é acessada. Ao usar tipos de entidade POCO, carregamento lento é obtido pela criação de instâncias de tipos de proxy derivada e, em seguida, substituindo propriedades virtuais para adicionar o gancho do carregamento. Por exemplo, ao usar a classe de entidade do Blog definida abaixo, as postagens relacionadas serão carregadas na primeira vez em que a propriedade de navegação de postagens é acessada:  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Ativar o carregamento lento desativado para serialização  

Serialização e o carregamento lento não se misturam bem e se você não tiver cuidado pode acabar consultar seu banco de dados inteiro só porque o carregamento lento está habilitado. A maioria dos serializadores funcionam acessando cada propriedade em uma instância de um tipo. Acesso à propriedade dispara o carregamento lento, portanto, mais entidades são serializadas. Nessas entidades propriedades são acessadas e até mesmo mais entidades são carregadas. É uma boa prática para ativar o carregamento antes de você serializar uma entidade lento. As seções a seguir mostram como fazer isso.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Como desativar o carregamento lento para propriedades de navegação específicos  

Carregamento lento da coleção de postagens pode ser desativado, tornando a propriedade de postagens não virtual:  

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

Carregando das postagens de coleção pode ainda ser obtida usando o carregamento adiantado (consulte *carregar avidamente* acima) ou o método Load (veja *carregar explicitamente* abaixo).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Desativar carregamento lento para todas as entidades  

Carregamento lento pode ser desativado para todas as entidades no contexto, definindo um sinalizador na propriedade de configuração. Por exemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Carregamento de entidades relacionadas ainda pode ser obtido usando o carregamento adiantado (consulte *carregar avidamente* acima) ou o método Load (veja *carregar explicitamente* abaixo).  

## <a name="explicitly-loading"></a>Carregar explicitamente  

Mesmo com carregamento lento desativado ainda é possível carregar lentamente entidades relacionadas, mas ele deve ser feito com uma chamada explícita. Para fazer isso você pode usar o método Load na entrada da entidade relacionada. Por exemplo:  

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

Observe que o método de referência deve ser usado quando uma entidade tem uma propriedade de navegação para outra entidade única. Por outro lado, o método de coleção deve ser usado quando uma entidade tem uma propriedade de navegação para uma coleção de outras entidades.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Aplicação de filtros ao carregar explicitamente entidades relacionadas  

O método de consulta fornece acesso a consulta subjacente que o Entity Framework usará ao carregar entidades relacionadas. Em seguida, você pode usar o LINQ para aplicar filtros à consulta antes de executá-lo com uma chamada para um método de extensão LINQ como ToList, carga, etc. O método de consulta pode ser usado com a referência e a coleção de propriedades de navegação, mas é mais útil para coleções em que ele pode ser usado para carregar apenas parte da coleção. Por exemplo:  

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

Ao usar o método de consulta é geralmente melhor desativar carregamento lento para a propriedade de navegação. Isso ocorre porque, caso contrário, toda a coleção pode ficar carregada automaticamente com o mecanismo de carregamento lento antes ou depois que a consulta filtrada é executada.  

Observe que enquanto a relação pode ser especificada como uma cadeia de caracteres em vez de uma expressão lambda, o IQueryable retornado não é genérico quando uma cadeia de caracteres é usada e, portanto, o método de conversão geralmente é necessária antes que alguma coisa útil pode ser feita com ele.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Usando a consulta para contar as entidades relacionadas sem carregá-los  

Às vezes é útil saber quantas entidades relacionadas a outra entidade no banco de dados sem realmente incorrer no custo de carregamento de todas as entidades. O método de consulta com o método de contagem de LINQ pode ser usado para fazer isso. Por exemplo:  

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
