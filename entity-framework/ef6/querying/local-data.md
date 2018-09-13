---
title: Dados locais - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: 400b9e1337edac1b9fa4f0ec9e1384ca58aa2fbc
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490448"
---
# <a name="local-data"></a>Dados locais
Executando uma consulta LINQ diretamente em um DbSet sempre enviar uma consulta ao banco de dados, mas você pode acessar os dados que estão atualmente na memória usando a propriedade DbSet. Você também pode acessar as informações extras para acompanhar o EF sobre suas entidades usando os métodos Entry e DbContext.ChangeTracker.Entries. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="using-local-to-look-at-local-data"></a>Usando o Local para examinar dados locais  

A propriedade Local do DbSet fornece acesso simples às entidades do conjunto que atualmente estão sendo controladas pelo contexto e não foram marcadas como excluídas. Acessando a propriedade Local nunca faz com que uma consulta a ser enviado ao banco de dados. Isso significa que ele normalmente é usado depois que uma consulta já foi executada. O método de extensão de carga pode ser usado para executar uma consulta para que o contexto controla os resultados. Por exemplo:  

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

Se tivéssemos dois blogs no banco de dados - 'ADO.NET Blog' com um BlogId 1 - e 'Blog do Visual Studio' com um BlogId de 2 nós poderia esperar a saída a seguir:  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Isso ilustra três pontos:  

- O novo blog 'Meu novo Blog' está incluído na coleção Local, mesmo que ainda não foi salvo no banco de dados. Este blog tem uma chave primária igual a zero, porque o banco de dados ainda não tiver gerado uma chave real para a entidade.  
- O Blog do ADO.NET' ' não está incluído na coleção local, mesmo que ainda está sendo acompanhado pelo contexto. Isso ocorre porque o removemos do DbSet, assim, marcando-o como excluído.  
- Quando DbSet é usado para executar uma consulta o blog marcado para exclusão (Blog do ADO.NET) está incluído nos resultados e novo blog (Blog de meu novo) que ainda não foram salvas no banco de dados não está incluído nos resultados. Isso ocorre porque o DbSet está executando uma consulta no banco de dados e os resultados retornados sempre refletem o que está no banco de dados.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Usando o Local para adicionar e remover entidades do contexto  

A propriedade Local no DbSet retorna um [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) com eventos conectados, de modo que ela permaneça em sincronia com o conteúdo do contexto. Isso significa que as entidades podem ser adicionadas ou removidas da coleção Local ou o DbSet. Isso também significa que as consultas que trazer novas entidades para o contexto resultará na coleção Local que está sendo atualizada com essas entidades. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

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

Supondo que tivemos algumas postagens marcadas com 'entity framework' e 'asp.net', a saída pode ser semelhante a:  

```  
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

- A nova postagem 'O que há de novo no EF' que foi adicionado ao Local coleção se torna controlada pelo contexto no estado adicionado. Ele será inserido, portanto, no banco de dados quando SaveChanges for chamado.  
- A postagem que foi removida da coleção (guia de iniciantes do EF) Local agora está marcada como excluído no contexto. Ele será excluído, portanto, do banco de dados quando SaveChanges for chamado.  
- A postagem adicional (guia de iniciantes do ASP.NET) carregada no contexto com a segunda consulta é automaticamente adicionada à coleção Local.  

Algo a observar sobre o Local final é que porque é que um ObservableCollection desempenho não é ótimo para um grande número de entidades. Portanto se você estiver lidando com milhares de entidades no seu contexto não é aconselhável usar o Local.  

## <a name="using-local-for-wpf-data-binding"></a>Usando o Local para vinculação de dados do WPF  

A propriedade Local no DbSet pode ser usada diretamente para a associação de dados em um aplicativo WPF porque ele é uma instância de ObservableCollection. Conforme descrito nas seções anteriores, que isso significa que ele será automaticamente permaneça em sincronia com o conteúdo do contexto e o conteúdo do contexto automaticamente permanecerão em sincronizado com ele. Observe que é necessário preencher previamente a coleta Local com os dados para haver nada para vincular a vez que o Local nunca faz com que uma consulta de banco de dados.  

Isso não é um local adequado para um exemplo de associação de dados completo do WPF, mas são os principais elementos:  

- Configurar uma origem da associação  
- Associá-lo à propriedade Local de seu conjunto  
- Preencha Local usando uma consulta ao banco de dados.  

## <a name="wpf-binding-to-navigation-properties"></a>Associação do WPF para propriedades de navegação  

Se você estiver fazendo dados mestre/detalhes de associação você talvez queira associar a exibição de detalhes a uma propriedade de navegação de uma das suas entidades. Uma maneira fácil de fazer isso funcionar é usar uma ObservableCollection para a propriedade de navegação. Por exemplo:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Usando o Local para limpar a entidades em SaveChanges  

Na maioria dos casos removidas de uma propriedade de navegação de entidades serão não automaticamente marcadas como excluído no contexto. Por exemplo, se você remover um objeto de postagem da coleção Blog.Posts que postar serão não excluídas automaticamente quando SaveChanges for chamado. Se você precisar da ser excluído, em seguida, você talvez precise encontrar essas entidades pendentes e marcá-los como excluído antes de chamar SaveChanges ou como parte de um SaveChanges substituído. Por exemplo:  

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

O código acima usa a coleta Local para localizar todas as postagens e marcas de qualquer um que não tem uma referência de blog como excluído. A chamada ToList é necessária porque, caso contrário, será possível modificar a coleção pela remover chamar enquanto ele está sendo enumerado. Na maioria das situações, você pode consultar diretamente a propriedade Local sem usar ToList pela primeira vez.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Usando vinculação de dados Local e ToBindingList para Windows Forms  

Formulários do Windows não oferece suporte a vinculação de dados de fidelidade total usando ObservableCollection diretamente. No entanto, você ainda pode usar a propriedade DbSet Local para vinculação de dados para obter todos os benefícios descritos nas seções anteriores. Isso é feito por meio do método de extensão ToBindingList que cria um [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementação apoiada por ObservableCollection Local.  

Isso não é um local adequado para um exemplo de associação de dados completo do Windows Forms, mas são os principais elementos:  

- Configurar uma fonte de associação de objeto  
- Associá-lo à propriedade Local de seu conjunto usando Local.ToBindingList()  
- Preencher Local usando uma consulta ao banco de dados  

## <a name="getting-detailed-information-about-tracked-entities"></a>Obtendo informações detalhadas sobre entidades controladas  

Muitos dos exemplos nesta série de usam o método de entrada para retornar uma instância de DbEntityEntry para uma entidade. Esse objeto de entrada, em seguida, atua como o ponto de partida para coletar informações sobre a entidade, como seu estado atual, bem como para executar operações de entidade, como carregamento explicitamente de uma entidade relacionada.  

Os métodos de entradas retornam objetos DbEntityEntry para muitas ou todas as entidades que estão sendo controladas pelo contexto. Isso permite que você coletar informações ou executar operações em muitas entidades em vez de apenas uma única entrada. Por exemplo:  

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

Você perceberá que estamos introduzindo uma classe de autor e leitor para o exemplo – essas duas classes implementam a interface de IPerson.  

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

Vamos supor que temos os seguintes dados no banco de dados:

Blog com BlogId = 1 e nome = 'ADO.NET Blog'  
Blog com BlogId = 2 e o nome = 'O Blog do Visual Studio'  
Blog com BlogId = 3 e nome = 'Blog do .NET Framework'  
Autor com AuthorId = 1 e nome = 'Joe Bloggs'  
Leitor com ReaderId = 1 e nome = 'John Doe'  

A saída da execução do código seria:  

```  
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

- Os métodos de entradas retornam as entradas para entidades em todos os estados, incluindo excluído. Compare isso ao Local que exclui excluído entidades.  
- As entradas para todos os tipos de entidade são retornadas quando o método de entradas não genérico é usado. Quando o método entradas genérico é usado apenas entradas para entidades que são instâncias do tipo genérico. Isso foi usado acima para obter entradas para todos os blogs. Ele também foi usado para obter entradas para todas as entidades que implementam IPerson. Isso demonstra que o tipo genérico não precisa ser um tipo de entidade real.  
- LINQ para objetos pode ser usado para filtrar os resultados retornados. Isso foi usado acima para localizar entidades de qualquer tipo, desde que eles sejam modificados.  

Observe que as instâncias de DbEntityEntry sempre contêm uma entidade não nulo. Entradas de relacionamento e entradas de stub não são representadas como instâncias de DbEntityEntry, portanto, não é necessário para filtrar essas.
