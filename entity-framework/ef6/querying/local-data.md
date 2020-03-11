---
title: Dados locais-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: efd646348d8a18bbeed2d0a0e708d4d36eb26eac
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417105"
---
# <a name="local-data"></a>Dados locais
A execução de uma consulta LINQ diretamente em um DbSet sempre enviará uma consulta para o banco de dados, mas você pode acessá-los atualmente na memória usando a propriedade DbSet. local. Você também pode acessar as informações extras que o EF está acompanhando sobre suas entidades usando os métodos DbContext. entry e DbContext. ChangeTracker. Entries. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="using-local-to-look-at-local-data"></a>Usando local para examinar dados locais  

A propriedade local de DbSet fornece acesso simples às entidades do conjunto que estão sendo rastreadas no momento pelo contexto e não foram marcadas como excluídas. O acesso à propriedade local nunca faz com que uma consulta seja enviada ao banco de dados. Isso significa que geralmente é usado depois que uma consulta já foi executada. O método de extensão de carregamento pode ser usado para executar uma consulta para que o contexto acompanhe os resultados. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

Se tivéssemos dois Blogs no banco de dados-' blog ADO.NET ' com um blogid de 1 e ' o blog do Visual Studio ' com um blogid de 2-poderíamos esperar a seguinte saída:  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Isso ilustra três pontos:  

- O novo blog "meu novo blog" está incluído na coleção local, embora ainda não tenha sido salvo no banco de dados. Este blog tem uma chave primária de zero porque o banco de dados ainda não gerou uma chave real para a entidade.  
- O ' blog ADO.NET ' não está incluído na coleção local, embora ainda esteja sendo acompanhado pelo contexto. Isso ocorre porque nós o removemos da DbSet, marcando-o como excluído.  
- Quando DbSet é usado para executar uma consulta, o blog marcado para exclusão (blog ADO.NET) é incluído nos resultados e o novo blog (meu novo blog) que ainda não foi salvo no banco de dados não está incluído nos resultados. Isso ocorre porque DbSet está executando uma consulta no banco de dados e os resultados retornados sempre refletem o que está no banco de dados.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Usando local para adicionar e remover entidades do contexto  

A propriedade local em DbSet retorna uma [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) com eventos conectados, de modo que permaneça em sincronia com o conteúdo do contexto. Isso significa que as entidades podem ser adicionadas ou removidas da coleção local ou do DbSet. Isso também significa que as consultas que trazem novas entidades no contexto resultarão na atualização da coleção local com essas entidades. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework")).Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

Supondo que tivemos algumas postagens marcadas com ' Entity-Framework ' e ' asp.net ', a saída pode ser semelhante a esta:  

```console
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

Isso ilustra três pontos:  

- A nova postagem ' novidades no EF ' que foi adicionada à coleção local é controlada pelo contexto no estado adicionado. Portanto, ele será inserido no banco de dados quando SaveChanges for chamado.  
- A postagem que foi removida da coleção local (guia de iniciantes do EF) agora está marcada como excluída no contexto. Portanto, ele será excluído do banco de dados quando SaveChanges for chamado.  
- A postagem adicional (guia de iniciantes da ASP.NET) carregada no contexto com a segunda consulta é adicionada automaticamente à coleção local.  

Uma coisa final a ser observada sobre local é que, como é um desempenho de ObservableCollection não é ótimo para grandes números de entidades. Portanto, se você estiver lidando com milhares de entidades em seu contexto, talvez não seja aconselhável usar local.  

## <a name="using-local-for-wpf-data-binding"></a>Usando local para associação de dados do WPF  

A propriedade local em DbSet pode ser usada diretamente para associação de dados em um aplicativo do WPF porque é uma instância de ObservableCollection. Conforme descrito nas seções anteriores, isso significa que ela permanecerá automaticamente em sincronia com o conteúdo do contexto e o conteúdo do contexto permanecerá automaticamente em sincronia com ele. Observe que você precisa preencher previamente a coleção local com os dados para que haja qualquer coisa a ser associada, pois o local nunca causa uma consulta de banco de dado.  

Este não é um local apropriado para um exemplo completo de ligação de dados do WPF, mas os elementos principais são:  

- Configurar uma origem de associação  
- Associá-lo à propriedade local do seu conjunto  
- Preencha o local usando uma consulta ao banco de dados.  

## <a name="wpf-binding-to-navigation-properties"></a>Associação do WPF a propriedades de navegação  

Se você estiver fazendo uma ligação de dados mestre/detalhes, talvez queira associar a exibição de detalhes a uma propriedade de navegação de uma de suas entidades. Uma maneira fácil de fazer esse trabalho é usar uma ObservableCollection para a propriedade de navegação. Por exemplo:  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Usando local para limpar entidades no SaveChanges  

Na maioria dos casos, as entidades removidas de uma propriedade de navegação não serão marcadas automaticamente como excluídas no contexto. Por exemplo, se você remover um objeto post da coleção blog. Posts, essa postagem não será excluída automaticamente quando SaveChanges for chamado. Se você precisar que ele seja excluído, talvez seja necessário encontrar essas entidades pendente e marcá-las como excluídas antes de chamar SaveChanges ou como parte de um SaveChanges substituído. Por exemplo:  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

O código acima usa a coleção local para localizar todas as postagens e marca qualquer que não tenha uma referência de blog como excluída. A chamada ToList é necessária porque, caso contrário, a coleção será modificada pela chamada de remoção enquanto ela estiver sendo enumerada. Na maioria das outras situações, você pode consultar diretamente a propriedade local sem usar o ToList primeiro.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Usando o local e tobindlist para Windows Forms Associação de dados  

Windows Forms não dá suporte à vinculação de dados de fidelidade completa usando ObservableCollection diretamente. No entanto, você ainda pode usar a propriedade local DbSet para a vinculação de dados para obter todos os benefícios descritos nas seções anteriores. Isso é obtido por meio do método de extensão tobindlist que cria uma implementação [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) apoiada pela ObservableCollection local.  

Esse não é um lugar apropriado para um exemplo completo de associação de dados Windows Forms, mas os elementos principais são:  

- Configurar uma origem de associação de objeto  
- Associe-o à propriedade local do conjunto usando local. toassocialist ()  
- Popular local usando uma consulta ao banco de dados  

## <a name="getting-detailed-information-about-tracked-entities"></a>Obtendo informações detalhadas sobre entidades rastreadas  

Muitos dos exemplos nesta série usam o método de entrada para retornar uma instância de DbEntityEntry para uma entidade. Esse objeto de entrada atua como o ponto de partida para coletar informações sobre a entidade, como seu estado atual, bem como para executar operações na entidade, como carregar explicitamente uma entidade relacionada.  

Os métodos de entradas retornam objetos DbEntityEntry para muitas ou todas as entidades que estão sendo rastreadas pelo contexto. Isso permite que você reúna informações ou execute operações em várias entidades, em vez de apenas uma única entrada. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

Você notará que estamos introduzindo uma classe Author e Reader no exemplo – ambas as classes implementam a interface IPerson.  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

Vamos supor que tenhamos os seguintes dados no banco de dado:

Blog com blogid = 1 e Name = ' ADO.NET blog '  
Blog com blogid = 2 e Name = ' o blog do Visual Studio '  
Blog com blogid = 3 e Name = ' .NET Framework blog '  
Autor com AuthorId = 1 e Name = ' Joe Bloggs '  
Leitor com Readerid = 1 e Name = ' John Doe '  

A saída da execução do código seria:  

```console
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

Estes exemplos ilustram vários pontos:  

- Os métodos de entradas retornam entradas para entidades em todos os Estados, incluindo excluído. Compare isso com o local que exclui as entidades excluídas.  
- As entradas para todos os tipos de entidade são retornadas quando o método de entradas não genéricas é usado. Quando o método de entradas genéricas é usado, as entradas são retornadas somente para entidades que são instâncias do tipo genérico. Isso foi usado acima para obter entradas para todos os Blogs. Ele também foi usado para obter entradas para todas as entidades que implementam IPerson. Isso demonstra que o tipo genérico não precisa ser um tipo de entidade real.  
- LINQ to Objects pode ser usado para filtrar os resultados retornados. Isso foi usado acima para localizar entidades de qualquer tipo, desde que elas sejam modificadas.  

Observe que as instâncias de DbEntityEntry sempre contêm uma entidade não nula. Entradas de relação e entradas de stub não são representadas como instâncias de DbEntityEntry para que não haja necessidade de filtrar por elas.
