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
# <a name="local-data"></a><span data-ttu-id="ab072-102">Dados locais</span><span class="sxs-lookup"><span data-stu-id="ab072-102">Local Data</span></span>
<span data-ttu-id="ab072-103">A execução de uma consulta LINQ diretamente em um DbSet sempre enviará uma consulta para o banco de dados, mas você pode acessá-los atualmente na memória usando a propriedade DbSet. local.</span><span class="sxs-lookup"><span data-stu-id="ab072-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="ab072-104">Você também pode acessar as informações extras que o EF está acompanhando sobre suas entidades usando os métodos DbContext. entry e DbContext. ChangeTracker. Entries.</span><span class="sxs-lookup"><span data-stu-id="ab072-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="ab072-105">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="ab072-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="ab072-106">Usando local para examinar dados locais</span><span class="sxs-lookup"><span data-stu-id="ab072-106">Using Local to look at local data</span></span>  

<span data-ttu-id="ab072-107">A propriedade local de DbSet fornece acesso simples às entidades do conjunto que estão sendo rastreadas no momento pelo contexto e não foram marcadas como excluídas.</span><span class="sxs-lookup"><span data-stu-id="ab072-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="ab072-108">O acesso à propriedade local nunca faz com que uma consulta seja enviada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ab072-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="ab072-109">Isso significa que geralmente é usado depois que uma consulta já foi executada.</span><span class="sxs-lookup"><span data-stu-id="ab072-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="ab072-110">O método de extensão de carregamento pode ser usado para executar uma consulta para que o contexto acompanhe os resultados.</span><span class="sxs-lookup"><span data-stu-id="ab072-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="ab072-111">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab072-111">For example:</span></span>  

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

<span data-ttu-id="ab072-112">Se tivéssemos dois Blogs no banco de dados-' blog ADO.NET ' com um blogid de 1 e ' o blog do Visual Studio ' com um blogid de 2-poderíamos esperar a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="ab072-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="ab072-113">Isso ilustra três pontos:</span><span class="sxs-lookup"><span data-stu-id="ab072-113">This illustrates three points:</span></span>  

- <span data-ttu-id="ab072-114">O novo blog "meu novo blog" está incluído na coleção local, embora ainda não tenha sido salvo no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ab072-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="ab072-115">Este blog tem uma chave primária de zero porque o banco de dados ainda não gerou uma chave real para a entidade.</span><span class="sxs-lookup"><span data-stu-id="ab072-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="ab072-116">O ' blog ADO.NET ' não está incluído na coleção local, embora ainda esteja sendo acompanhado pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="ab072-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="ab072-117">Isso ocorre porque nós o removemos da DbSet, marcando-o como excluído.</span><span class="sxs-lookup"><span data-stu-id="ab072-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="ab072-118">Quando DbSet é usado para executar uma consulta, o blog marcado para exclusão (blog ADO.NET) é incluído nos resultados e o novo blog (meu novo blog) que ainda não foi salvo no banco de dados não está incluído nos resultados.</span><span class="sxs-lookup"><span data-stu-id="ab072-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="ab072-119">Isso ocorre porque DbSet está executando uma consulta no banco de dados e os resultados retornados sempre refletem o que está no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ab072-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="ab072-120">Usando local para adicionar e remover entidades do contexto</span><span class="sxs-lookup"><span data-stu-id="ab072-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="ab072-121">A propriedade local em DbSet retorna uma [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) com eventos conectados, de modo que permaneça em sincronia com o conteúdo do contexto.</span><span class="sxs-lookup"><span data-stu-id="ab072-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="ab072-122">Isso significa que as entidades podem ser adicionadas ou removidas da coleção local ou do DbSet.</span><span class="sxs-lookup"><span data-stu-id="ab072-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="ab072-123">Isso também significa que as consultas que trazem novas entidades no contexto resultarão na atualização da coleção local com essas entidades.</span><span class="sxs-lookup"><span data-stu-id="ab072-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="ab072-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab072-124">For example:</span></span>  

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

<span data-ttu-id="ab072-125">Supondo que tivemos algumas postagens marcadas com ' Entity-Framework ' e ' asp.net ', a saída pode ser semelhante a esta:</span><span class="sxs-lookup"><span data-stu-id="ab072-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

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

<span data-ttu-id="ab072-126">Isso ilustra três pontos:</span><span class="sxs-lookup"><span data-stu-id="ab072-126">This illustrates three points:</span></span>  

- <span data-ttu-id="ab072-127">A nova postagem ' novidades no EF ' que foi adicionada à coleção local é controlada pelo contexto no estado adicionado.</span><span class="sxs-lookup"><span data-stu-id="ab072-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="ab072-128">Portanto, ele será inserido no banco de dados quando SaveChanges for chamado.</span><span class="sxs-lookup"><span data-stu-id="ab072-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="ab072-129">A postagem que foi removida da coleção local (guia de iniciantes do EF) agora está marcada como excluída no contexto.</span><span class="sxs-lookup"><span data-stu-id="ab072-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="ab072-130">Portanto, ele será excluído do banco de dados quando SaveChanges for chamado.</span><span class="sxs-lookup"><span data-stu-id="ab072-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="ab072-131">A postagem adicional (guia de iniciantes da ASP.NET) carregada no contexto com a segunda consulta é adicionada automaticamente à coleção local.</span><span class="sxs-lookup"><span data-stu-id="ab072-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="ab072-132">Uma coisa final a ser observada sobre local é que, como é um desempenho de ObservableCollection não é ótimo para grandes números de entidades.</span><span class="sxs-lookup"><span data-stu-id="ab072-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="ab072-133">Portanto, se você estiver lidando com milhares de entidades em seu contexto, talvez não seja aconselhável usar local.</span><span class="sxs-lookup"><span data-stu-id="ab072-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="ab072-134">Usando local para associação de dados do WPF</span><span class="sxs-lookup"><span data-stu-id="ab072-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="ab072-135">A propriedade local em DbSet pode ser usada diretamente para associação de dados em um aplicativo do WPF porque é uma instância de ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="ab072-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="ab072-136">Conforme descrito nas seções anteriores, isso significa que ela permanecerá automaticamente em sincronia com o conteúdo do contexto e o conteúdo do contexto permanecerá automaticamente em sincronia com ele.</span><span class="sxs-lookup"><span data-stu-id="ab072-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="ab072-137">Observe que você precisa preencher previamente a coleção local com os dados para que haja qualquer coisa a ser associada, pois o local nunca causa uma consulta de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="ab072-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="ab072-138">Este não é um local apropriado para um exemplo completo de ligação de dados do WPF, mas os elementos principais são:</span><span class="sxs-lookup"><span data-stu-id="ab072-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="ab072-139">Configurar uma origem de associação</span><span class="sxs-lookup"><span data-stu-id="ab072-139">Setup a binding source</span></span>  
- <span data-ttu-id="ab072-140">Associá-lo à propriedade local do seu conjunto</span><span class="sxs-lookup"><span data-stu-id="ab072-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="ab072-141">Preencha o local usando uma consulta ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ab072-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="ab072-142">Associação do WPF a propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="ab072-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="ab072-143">Se você estiver fazendo uma ligação de dados mestre/detalhes, talvez queira associar a exibição de detalhes a uma propriedade de navegação de uma de suas entidades.</span><span class="sxs-lookup"><span data-stu-id="ab072-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="ab072-144">Uma maneira fácil de fazer esse trabalho é usar uma ObservableCollection para a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="ab072-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="ab072-145">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab072-145">For example:</span></span>  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="ab072-146">Usando local para limpar entidades no SaveChanges</span><span class="sxs-lookup"><span data-stu-id="ab072-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="ab072-147">Na maioria dos casos, as entidades removidas de uma propriedade de navegação não serão marcadas automaticamente como excluídas no contexto.</span><span class="sxs-lookup"><span data-stu-id="ab072-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="ab072-148">Por exemplo, se você remover um objeto post da coleção blog. Posts, essa postagem não será excluída automaticamente quando SaveChanges for chamado.</span><span class="sxs-lookup"><span data-stu-id="ab072-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="ab072-149">Se você precisar que ele seja excluído, talvez seja necessário encontrar essas entidades pendente e marcá-las como excluídas antes de chamar SaveChanges ou como parte de um SaveChanges substituído.</span><span class="sxs-lookup"><span data-stu-id="ab072-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="ab072-150">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab072-150">For example:</span></span>  

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

<span data-ttu-id="ab072-151">O código acima usa a coleção local para localizar todas as postagens e marca qualquer que não tenha uma referência de blog como excluída.</span><span class="sxs-lookup"><span data-stu-id="ab072-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="ab072-152">A chamada ToList é necessária porque, caso contrário, a coleção será modificada pela chamada de remoção enquanto ela estiver sendo enumerada.</span><span class="sxs-lookup"><span data-stu-id="ab072-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="ab072-153">Na maioria das outras situações, você pode consultar diretamente a propriedade local sem usar o ToList primeiro.</span><span class="sxs-lookup"><span data-stu-id="ab072-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="ab072-154">Usando o local e tobindlist para Windows Forms Associação de dados</span><span class="sxs-lookup"><span data-stu-id="ab072-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="ab072-155">Windows Forms não dá suporte à vinculação de dados de fidelidade completa usando ObservableCollection diretamente.</span><span class="sxs-lookup"><span data-stu-id="ab072-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="ab072-156">No entanto, você ainda pode usar a propriedade local DbSet para a vinculação de dados para obter todos os benefícios descritos nas seções anteriores.</span><span class="sxs-lookup"><span data-stu-id="ab072-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="ab072-157">Isso é obtido por meio do método de extensão tobindlist que cria uma implementação [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) apoiada pela ObservableCollection local.</span><span class="sxs-lookup"><span data-stu-id="ab072-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="ab072-158">Esse não é um lugar apropriado para um exemplo completo de associação de dados Windows Forms, mas os elementos principais são:</span><span class="sxs-lookup"><span data-stu-id="ab072-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="ab072-159">Configurar uma origem de associação de objeto</span><span class="sxs-lookup"><span data-stu-id="ab072-159">Setup an object binding source</span></span>  
- <span data-ttu-id="ab072-160">Associe-o à propriedade local do conjunto usando local. toassocialist ()</span><span class="sxs-lookup"><span data-stu-id="ab072-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="ab072-161">Popular local usando uma consulta ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="ab072-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="ab072-162">Obtendo informações detalhadas sobre entidades rastreadas</span><span class="sxs-lookup"><span data-stu-id="ab072-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="ab072-163">Muitos dos exemplos nesta série usam o método de entrada para retornar uma instância de DbEntityEntry para uma entidade.</span><span class="sxs-lookup"><span data-stu-id="ab072-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="ab072-164">Esse objeto de entrada atua como o ponto de partida para coletar informações sobre a entidade, como seu estado atual, bem como para executar operações na entidade, como carregar explicitamente uma entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="ab072-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="ab072-165">Os métodos de entradas retornam objetos DbEntityEntry para muitas ou todas as entidades que estão sendo rastreadas pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="ab072-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="ab072-166">Isso permite que você reúna informações ou execute operações em várias entidades, em vez de apenas uma única entrada.</span><span class="sxs-lookup"><span data-stu-id="ab072-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="ab072-167">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ab072-167">For example:</span></span>  

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

<span data-ttu-id="ab072-168">Você notará que estamos introduzindo uma classe Author e Reader no exemplo – ambas as classes implementam a interface IPerson.</span><span class="sxs-lookup"><span data-stu-id="ab072-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

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

<span data-ttu-id="ab072-169">Vamos supor que tenhamos os seguintes dados no banco de dado:</span><span class="sxs-lookup"><span data-stu-id="ab072-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="ab072-170">Blog com blogid = 1 e Name = ' ADO.NET blog '</span><span class="sxs-lookup"><span data-stu-id="ab072-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="ab072-171">Blog com blogid = 2 e Name = ' o blog do Visual Studio '</span><span class="sxs-lookup"><span data-stu-id="ab072-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="ab072-172">Blog com blogid = 3 e Name = ' .NET Framework blog '</span><span class="sxs-lookup"><span data-stu-id="ab072-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="ab072-173">Autor com AuthorId = 1 e Name = ' Joe Bloggs '</span><span class="sxs-lookup"><span data-stu-id="ab072-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="ab072-174">Leitor com Readerid = 1 e Name = ' John Doe '</span><span class="sxs-lookup"><span data-stu-id="ab072-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="ab072-175">A saída da execução do código seria:</span><span class="sxs-lookup"><span data-stu-id="ab072-175">The output from running the code would be:</span></span>  

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

<span data-ttu-id="ab072-176">Estes exemplos ilustram vários pontos:</span><span class="sxs-lookup"><span data-stu-id="ab072-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="ab072-177">Os métodos de entradas retornam entradas para entidades em todos os Estados, incluindo excluído.</span><span class="sxs-lookup"><span data-stu-id="ab072-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="ab072-178">Compare isso com o local que exclui as entidades excluídas.</span><span class="sxs-lookup"><span data-stu-id="ab072-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="ab072-179">As entradas para todos os tipos de entidade são retornadas quando o método de entradas não genéricas é usado.</span><span class="sxs-lookup"><span data-stu-id="ab072-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="ab072-180">Quando o método de entradas genéricas é usado, as entradas são retornadas somente para entidades que são instâncias do tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="ab072-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="ab072-181">Isso foi usado acima para obter entradas para todos os Blogs.</span><span class="sxs-lookup"><span data-stu-id="ab072-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="ab072-182">Ele também foi usado para obter entradas para todas as entidades que implementam IPerson.</span><span class="sxs-lookup"><span data-stu-id="ab072-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="ab072-183">Isso demonstra que o tipo genérico não precisa ser um tipo de entidade real.</span><span class="sxs-lookup"><span data-stu-id="ab072-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="ab072-184">LINQ to Objects pode ser usado para filtrar os resultados retornados.</span><span class="sxs-lookup"><span data-stu-id="ab072-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="ab072-185">Isso foi usado acima para localizar entidades de qualquer tipo, desde que elas sejam modificadas.</span><span class="sxs-lookup"><span data-stu-id="ab072-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="ab072-186">Observe que as instâncias de DbEntityEntry sempre contêm uma entidade não nula.</span><span class="sxs-lookup"><span data-stu-id="ab072-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="ab072-187">Entradas de relação e entradas de stub não são representadas como instâncias de DbEntityEntry para que não haja necessidade de filtrar por elas.</span><span class="sxs-lookup"><span data-stu-id="ab072-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
