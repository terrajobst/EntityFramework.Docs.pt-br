---
title: Dados locais - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
caps.latest.revision: 3
ms.openlocfilehash: 79f0d2175199780d41b43088832bab808ab2fff0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119828"
---
# <a name="local-data"></a><span data-ttu-id="142f5-102">Dados locais</span><span class="sxs-lookup"><span data-stu-id="142f5-102">Local Data</span></span>
<span data-ttu-id="142f5-103">Executando uma consulta LINQ diretamente em um DbSet sempre enviar uma consulta ao banco de dados, mas você pode acessar os dados que estão atualmente na memória usando a propriedade DbSet.</span><span class="sxs-lookup"><span data-stu-id="142f5-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="142f5-104">Você também pode acessar as informações extras para acompanhar o EF sobre suas entidades usando os métodos Entry e DbContext.ChangeTracker.Entries.</span><span class="sxs-lookup"><span data-stu-id="142f5-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="142f5-105">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="142f5-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="142f5-106">Usando o Local para examinar dados locais</span><span class="sxs-lookup"><span data-stu-id="142f5-106">Using Local to look at local data</span></span>  

<span data-ttu-id="142f5-107">A propriedade Local do DbSet fornece acesso simples às entidades do conjunto que atualmente estão sendo controladas pelo contexto e não foram marcadas como excluídas.</span><span class="sxs-lookup"><span data-stu-id="142f5-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="142f5-108">Acessando a propriedade Local nunca faz com que uma consulta a ser enviado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="142f5-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="142f5-109">Isso significa que ele normalmente é usado depois que uma consulta já foi executada.</span><span class="sxs-lookup"><span data-stu-id="142f5-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="142f5-110">O método de extensão de carga pode ser usado para executar uma consulta para que o contexto controla os resultados.</span><span class="sxs-lookup"><span data-stu-id="142f5-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="142f5-111">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="142f5-111">For example:</span></span>  

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

<span data-ttu-id="142f5-112">Se tivéssemos dois blogs no banco de dados - 'ADO.NET Blog' com um BlogId 1 - e 'Blog do Visual Studio' com um BlogId de 2 nós poderia esperar a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="142f5-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="142f5-113">Isso ilustra três pontos:</span><span class="sxs-lookup"><span data-stu-id="142f5-113">This illustrates three points:</span></span>  

- <span data-ttu-id="142f5-114">O novo blog 'Meu novo Blog' está incluído na coleção Local, mesmo que ainda não foi salvo no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="142f5-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="142f5-115">Este blog tem uma chave primária igual a zero, porque o banco de dados ainda não tiver gerado uma chave real para a entidade.</span><span class="sxs-lookup"><span data-stu-id="142f5-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="142f5-116">O Blog do ADO.NET' ' não está incluído na coleção local, mesmo que ainda está sendo acompanhado pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="142f5-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="142f5-117">Isso ocorre porque o removemos do DbSet, assim, marcando-o como excluído.</span><span class="sxs-lookup"><span data-stu-id="142f5-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="142f5-118">Quando DbSet é usado para executar uma consulta o blog marcado para exclusão (Blog do ADO.NET) está incluído nos resultados e novo blog (Blog de meu novo) que ainda não foram salvas no banco de dados não está incluído nos resultados.</span><span class="sxs-lookup"><span data-stu-id="142f5-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="142f5-119">Isso ocorre porque o DbSet está executando uma consulta no banco de dados e os resultados retornados sempre refletem o que está no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="142f5-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="142f5-120">Usando o Local para adicionar e remover entidades do contexto</span><span class="sxs-lookup"><span data-stu-id="142f5-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="142f5-121">A propriedade Local no DbSet retorna um [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) com eventos conectados, de modo que ela permaneça em sincronia com o conteúdo do contexto.</span><span class="sxs-lookup"><span data-stu-id="142f5-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="142f5-122">Isso significa que as entidades podem ser adicionadas ou removidas da coleção Local ou o DbSet.</span><span class="sxs-lookup"><span data-stu-id="142f5-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="142f5-123">Isso também significa que as consultas que trazer novas entidades para o contexto resultará na coleção Local que está sendo atualizada com essas entidades.</span><span class="sxs-lookup"><span data-stu-id="142f5-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="142f5-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="142f5-124">For example:</span></span>  

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

<span data-ttu-id="142f5-125">Supondo que tivemos algumas postagens marcadas com 'entity framework' e 'asp.net', a saída pode ser semelhante a:</span><span class="sxs-lookup"><span data-stu-id="142f5-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

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

<span data-ttu-id="142f5-126">Isso ilustra três pontos:</span><span class="sxs-lookup"><span data-stu-id="142f5-126">This illustrates three points:</span></span>  

- <span data-ttu-id="142f5-127">A nova postagem 'O que há de novo no EF' que foi adicionado ao Local coleção se torna controlada pelo contexto no estado adicionado.</span><span class="sxs-lookup"><span data-stu-id="142f5-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="142f5-128">Ele será inserido, portanto, no banco de dados quando SaveChanges for chamado.</span><span class="sxs-lookup"><span data-stu-id="142f5-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="142f5-129">A postagem que foi removida da coleção (guia de iniciantes do EF) Local agora está marcada como excluído no contexto.</span><span class="sxs-lookup"><span data-stu-id="142f5-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="142f5-130">Ele será excluído, portanto, do banco de dados quando SaveChanges for chamado.</span><span class="sxs-lookup"><span data-stu-id="142f5-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="142f5-131">A postagem adicional (guia de iniciantes do ASP.NET) carregada no contexto com a segunda consulta é automaticamente adicionada à coleção Local.</span><span class="sxs-lookup"><span data-stu-id="142f5-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="142f5-132">Algo a observar sobre o Local final é que porque é que um ObservableCollection desempenho não é ótimo para um grande número de entidades.</span><span class="sxs-lookup"><span data-stu-id="142f5-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="142f5-133">Portanto se você estiver lidando com milhares de entidades no seu contexto não é aconselhável usar o Local.</span><span class="sxs-lookup"><span data-stu-id="142f5-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="142f5-134">Usando o Local para vinculação de dados do WPF</span><span class="sxs-lookup"><span data-stu-id="142f5-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="142f5-135">A propriedade Local no DbSet pode ser usada diretamente para a associação de dados em um aplicativo WPF porque ele é uma instância de ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="142f5-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="142f5-136">Conforme descrito nas seções anteriores, que isso significa que ele será automaticamente permaneça em sincronia com o conteúdo do contexto e o conteúdo do contexto automaticamente permanecerão em sincronizado com ele.</span><span class="sxs-lookup"><span data-stu-id="142f5-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="142f5-137">Observe que é necessário preencher previamente a coleta Local com os dados para haver nada para vincular a vez que o Local nunca faz com que uma consulta de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="142f5-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="142f5-138">Isso não é um local adequado para um exemplo de associação de dados completo do WPF, mas são os principais elementos:</span><span class="sxs-lookup"><span data-stu-id="142f5-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="142f5-139">Configurar uma origem da associação</span><span class="sxs-lookup"><span data-stu-id="142f5-139">Setup a binding source</span></span>  
- <span data-ttu-id="142f5-140">Associá-lo à propriedade Local de seu conjunto</span><span class="sxs-lookup"><span data-stu-id="142f5-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="142f5-141">Preencha Local usando uma consulta ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="142f5-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="142f5-142">Associação do WPF para propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="142f5-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="142f5-143">Se você estiver fazendo dados mestre/detalhes de associação você talvez queira associar a exibição de detalhes a uma propriedade de navegação de uma das suas entidades.</span><span class="sxs-lookup"><span data-stu-id="142f5-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="142f5-144">Uma maneira fácil de fazer isso funcionar é usar uma ObservableCollection para a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="142f5-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="142f5-145">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="142f5-145">For example:</span></span>  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="142f5-146">Usando o Local para limpar a entidades em SaveChanges</span><span class="sxs-lookup"><span data-stu-id="142f5-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="142f5-147">Na maioria dos casos removidas de uma propriedade de navegação de entidades serão não automaticamente marcadas como excluído no contexto.</span><span class="sxs-lookup"><span data-stu-id="142f5-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="142f5-148">Por exemplo, se você remover um objeto de postagem da coleção Blog.Posts que postar serão não excluídas automaticamente quando SaveChanges for chamado.</span><span class="sxs-lookup"><span data-stu-id="142f5-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="142f5-149">Se você precisar da ser excluído, em seguida, você talvez precise encontrar essas entidades pendentes e marcá-los como excluído antes de chamar SaveChanges ou como parte de um SaveChanges substituído.</span><span class="sxs-lookup"><span data-stu-id="142f5-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="142f5-150">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="142f5-150">For example:</span></span>  

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

<span data-ttu-id="142f5-151">O código acima usa a coleta Local para localizar todas as postagens e marcas de qualquer um que não tem uma referência de blog como excluído.</span><span class="sxs-lookup"><span data-stu-id="142f5-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="142f5-152">A chamada ToList é necessária porque, caso contrário, será possível modificar a coleção pela remover chamar enquanto ele está sendo enumerado.</span><span class="sxs-lookup"><span data-stu-id="142f5-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="142f5-153">Na maioria das situações, você pode consultar diretamente a propriedade Local sem usar ToList pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="142f5-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="142f5-154">Usando vinculação de dados Local e ToBindingList para Windows Forms</span><span class="sxs-lookup"><span data-stu-id="142f5-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="142f5-155">Formulários do Windows não oferece suporte a vinculação de dados de fidelidade total usando ObservableCollection diretamente.</span><span class="sxs-lookup"><span data-stu-id="142f5-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="142f5-156">No entanto, você ainda pode usar a propriedade DbSet Local para vinculação de dados para obter todos os benefícios descritos nas seções anteriores.</span><span class="sxs-lookup"><span data-stu-id="142f5-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="142f5-157">Isso é feito por meio do método de extensão ToBindingList que cria um [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementação apoiada por ObservableCollection Local.</span><span class="sxs-lookup"><span data-stu-id="142f5-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="142f5-158">Isso não é um local adequado para um exemplo de associação de dados completo do Windows Forms, mas são os principais elementos:</span><span class="sxs-lookup"><span data-stu-id="142f5-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="142f5-159">Configurar uma fonte de associação de objeto</span><span class="sxs-lookup"><span data-stu-id="142f5-159">Setup an object binding source</span></span>  
- <span data-ttu-id="142f5-160">Associá-lo à propriedade Local de seu conjunto usando Local.ToBindingList()</span><span class="sxs-lookup"><span data-stu-id="142f5-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="142f5-161">Preencher Local usando uma consulta ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="142f5-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="142f5-162">Obtendo informações detalhadas sobre entidades controladas</span><span class="sxs-lookup"><span data-stu-id="142f5-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="142f5-163">Muitos dos exemplos nesta série de usam o método de entrada para retornar uma instância de DbEntityEntry para uma entidade.</span><span class="sxs-lookup"><span data-stu-id="142f5-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="142f5-164">Esse objeto de entrada, em seguida, atua como o ponto de partida para coletar informações sobre a entidade, como seu estado atual, bem como para executar operações de entidade, como carregamento explicitamente de uma entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="142f5-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="142f5-165">Os métodos de entradas retornam objetos DbEntityEntry para muitas ou todas as entidades que estão sendo controladas pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="142f5-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="142f5-166">Isso permite que você coletar informações ou executar operações em muitas entidades em vez de apenas uma única entrada.</span><span class="sxs-lookup"><span data-stu-id="142f5-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="142f5-167">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="142f5-167">For example:</span></span>  

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

<span data-ttu-id="142f5-168">Você perceberá que estamos introduzindo uma classe de autor e leitor para o exemplo – essas duas classes implementam a interface de IPerson.</span><span class="sxs-lookup"><span data-stu-id="142f5-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

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

<span data-ttu-id="142f5-169">Vamos supor que temos os seguintes dados no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="142f5-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="142f5-170">Blog com BlogId = 1 e nome = 'ADO.NET Blog'</span><span class="sxs-lookup"><span data-stu-id="142f5-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="142f5-171">Blog com BlogId = 2 e o nome = 'O Blog do Visual Studio'</span><span class="sxs-lookup"><span data-stu-id="142f5-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="142f5-172">Blog com BlogId = 3 e nome = 'Blog do .NET Framework'</span><span class="sxs-lookup"><span data-stu-id="142f5-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="142f5-173">Autor com AuthorId = 1 e nome = 'Joe Bloggs'</span><span class="sxs-lookup"><span data-stu-id="142f5-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="142f5-174">Leitor com ReaderId = 1 e nome = 'John Doe'</span><span class="sxs-lookup"><span data-stu-id="142f5-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="142f5-175">A saída da execução do código seria:</span><span class="sxs-lookup"><span data-stu-id="142f5-175">The output from running the code would be:</span></span>  

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

<span data-ttu-id="142f5-176">Estes exemplos ilustram vários pontos:</span><span class="sxs-lookup"><span data-stu-id="142f5-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="142f5-177">Os métodos de entradas retornam as entradas para entidades em todos os estados, incluindo excluído.</span><span class="sxs-lookup"><span data-stu-id="142f5-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="142f5-178">Compare isso ao Local que exclui excluído entidades.</span><span class="sxs-lookup"><span data-stu-id="142f5-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="142f5-179">As entradas para todos os tipos de entidade são retornadas quando o método de entradas não genérico é usado.</span><span class="sxs-lookup"><span data-stu-id="142f5-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="142f5-180">Quando o método entradas genérico é usado apenas entradas para entidades que são instâncias do tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="142f5-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="142f5-181">Isso foi usado acima para obter entradas para todos os blogs.</span><span class="sxs-lookup"><span data-stu-id="142f5-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="142f5-182">Ele também foi usado para obter entradas para todas as entidades que implementam IPerson.</span><span class="sxs-lookup"><span data-stu-id="142f5-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="142f5-183">Isso demonstra que o tipo genérico não precisa ser um tipo de entidade real.</span><span class="sxs-lookup"><span data-stu-id="142f5-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="142f5-184">LINQ para objetos pode ser usado para filtrar os resultados retornados.</span><span class="sxs-lookup"><span data-stu-id="142f5-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="142f5-185">Isso foi usado acima para localizar entidades de qualquer tipo, desde que eles sejam modificados.</span><span class="sxs-lookup"><span data-stu-id="142f5-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="142f5-186">Observe que as instâncias de DbEntityEntry sempre contêm uma entidade não nulo.</span><span class="sxs-lookup"><span data-stu-id="142f5-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="142f5-187">Entradas de relacionamento e entradas de stub não são representadas como instâncias de DbEntityEntry, portanto, não é necessário para filtrar essas.</span><span class="sxs-lookup"><span data-stu-id="142f5-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
