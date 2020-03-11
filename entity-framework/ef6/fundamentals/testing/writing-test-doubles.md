---
title: Testando com suas próprias duplicatas de teste-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416325"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="f0515-102">Testando com suas próprias duplicatas de teste</span><span class="sxs-lookup"><span data-stu-id="f0515-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="f0515-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f0515-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f0515-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="f0515-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="f0515-105">Ao escrever testes para seu aplicativo, geralmente é desejável evitar atingir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f0515-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="f0515-106">Entity Framework permite que você faça isso criando um contexto – com o comportamento definido pelos seus testes – que usa dados na memória.</span><span class="sxs-lookup"><span data-stu-id="f0515-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="f0515-107">Opções para criar duplicatas de teste</span><span class="sxs-lookup"><span data-stu-id="f0515-107">Options for creating test doubles</span></span>  

<span data-ttu-id="f0515-108">Há duas abordagens diferentes que podem ser usadas para criar uma versão na memória do seu contexto.</span><span class="sxs-lookup"><span data-stu-id="f0515-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="f0515-109">**Crie suas próprias duplicatas de teste** – essa abordagem envolve escrever sua própria implementação na memória de seu contexto e DbSets.</span><span class="sxs-lookup"><span data-stu-id="f0515-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="f0515-110">Isso lhe dá muito controle sobre como as classes se comportam, mas pode envolver escrever e possuir uma quantidade razoável de código.</span><span class="sxs-lookup"><span data-stu-id="f0515-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="f0515-111">**Use uma estrutura fictícia para criar duplicatas de teste** – usando uma estrutura fictícia (como MOQ) você pode ter as implementações na memória do contexto e conjuntos criados dinamicamente no tempo de execução para você.</span><span class="sxs-lookup"><span data-stu-id="f0515-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="f0515-112">Este artigo tratará da criação do seu próprio teste duplo.</span><span class="sxs-lookup"><span data-stu-id="f0515-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="f0515-113">Para obter informações sobre como usar uma estrutura fictícia, consulte [testando com uma estrutura fictícia](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="f0515-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="f0515-114">Testando com versões EF6</span><span class="sxs-lookup"><span data-stu-id="f0515-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="f0515-115">O código mostrado neste artigo é compatível com EF6.</span><span class="sxs-lookup"><span data-stu-id="f0515-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="f0515-116">Para testar com o EF5 e a versão anterior, consulte [testando com um contexto falso](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="f0515-116">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="f0515-117">Limitações de duplicatas de teste na memória do EF</span><span class="sxs-lookup"><span data-stu-id="f0515-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="f0515-118">As duplicatas de teste na memória podem ser uma boa maneira de fornecer uma cobertura de nível de teste de unidade de bits do seu aplicativo que usa o EF.</span><span class="sxs-lookup"><span data-stu-id="f0515-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="f0515-119">No entanto, ao fazer isso, você está usando LINQ to Objects para executar consultas em dados na memória.</span><span class="sxs-lookup"><span data-stu-id="f0515-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="f0515-120">Isso pode resultar em comportamento diferente do que usar o provedor LINQ do EF (LINQ to Entities) para converter consultas em SQL que são executadas em seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f0515-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="f0515-121">Um exemplo de tal diferença é carregar dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="f0515-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="f0515-122">Se você criar uma série de Blogs que cada um tem postagens relacionadas, ao usar dados na memória, as postagens relacionadas sempre serão carregadas para cada blog.</span><span class="sxs-lookup"><span data-stu-id="f0515-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="f0515-123">No entanto, durante a execução em um banco de dados, eles serão carregados somente se você usar o método include.</span><span class="sxs-lookup"><span data-stu-id="f0515-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="f0515-124">Por esse motivo, é recomendável sempre incluir algum nível de teste de ponta a ponta (além dos testes de unidade) para garantir que seu aplicativo funcione corretamente em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f0515-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="f0515-125">Acompanhando este artigo</span><span class="sxs-lookup"><span data-stu-id="f0515-125">Following along with this article</span></span>  

<span data-ttu-id="f0515-126">Este artigo fornece listagens de código completas que você pode copiar para o Visual Studio para acompanhar, se desejar.</span><span class="sxs-lookup"><span data-stu-id="f0515-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="f0515-127">É mais fácil criar um projeto de **teste de unidade** e você precisará direcionar **.NET Framework 4,5** para concluir as seções que usam Async.</span><span class="sxs-lookup"><span data-stu-id="f0515-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="f0515-128">Criando uma interface de contexto</span><span class="sxs-lookup"><span data-stu-id="f0515-128">Creating a context interface</span></span>  

<span data-ttu-id="f0515-129">Vamos examinar o teste de um serviço que usa um modelo do EF.</span><span class="sxs-lookup"><span data-stu-id="f0515-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="f0515-130">Para poder substituir nosso contexto do EF por uma versão na memória para teste, definiremos uma interface que o nosso contexto do EF (e que seja o dobro da memória) irá implementar.</span><span class="sxs-lookup"><span data-stu-id="f0515-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will implement.</span></span>

<span data-ttu-id="f0515-131">O serviço que vamos testar consultará e modificará os dados usando as propriedades DbSet de nosso contexto e também chamará SaveChanges para enviar por push as alterações para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f0515-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="f0515-132">Então, estamos incluindo esses membros na interface.</span><span class="sxs-lookup"><span data-stu-id="f0515-132">So we're including these members on the interface.</span></span>  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a><span data-ttu-id="f0515-133">O modelo do EF</span><span class="sxs-lookup"><span data-stu-id="f0515-133">The EF model</span></span>  

<span data-ttu-id="f0515-134">O serviço que vamos testar faz uso de um modelo do EF composto pelas classes BloggingContext e blog e post.</span><span class="sxs-lookup"><span data-stu-id="f0515-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="f0515-135">Esse código pode ter sido gerado pelo designer do EF ou ser um modelo de Code First.</span><span class="sxs-lookup"><span data-stu-id="f0515-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="f0515-136">Implementando a interface de contexto com o designer do EF</span><span class="sxs-lookup"><span data-stu-id="f0515-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="f0515-137">Observe que nosso contexto implementa a interface IBloggingContext.</span><span class="sxs-lookup"><span data-stu-id="f0515-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="f0515-138">Se você estiver usando Code First, poderá editar o contexto diretamente para implementar a interface.</span><span class="sxs-lookup"><span data-stu-id="f0515-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="f0515-139">Se você estiver usando o designer do EF, precisará editar o modelo T4 que gera seu contexto.</span><span class="sxs-lookup"><span data-stu-id="f0515-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="f0515-140">Abra o\>de model_name de \<. Arquivo Context.tt que está aninhado no arquivo EDMX, localize o fragmento de código a seguir e adicione na interface, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="f0515-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="f0515-141">Serviço a ser testado</span><span class="sxs-lookup"><span data-stu-id="f0515-141">Service to be tested</span></span>  

<span data-ttu-id="f0515-142">Para demonstrar o teste com duplicatas de teste na memória, vamos escrever alguns testes para um BlogService.</span><span class="sxs-lookup"><span data-stu-id="f0515-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="f0515-143">O serviço é capaz de criar novos Blogs (addblog) e retornar todos os Blogs ordenados pelo nome (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="f0515-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="f0515-144">Além de GetAllBlogs, também fornecemos um método que receberá de forma assíncrona todos os Blogs ordenados pelo nome (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="f0515-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```

## <a name="creating-the-in-memory-test-doubles"></a><span data-ttu-id="f0515-145">Criando duplicatas de teste na memória</span><span class="sxs-lookup"><span data-stu-id="f0515-145">Creating the in-memory test doubles</span></span>  

<span data-ttu-id="f0515-146">Agora que temos o modelo real de EF e o serviço que pode usá-lo, é hora de criar o duplo de teste na memória que podemos usar para teste.</span><span class="sxs-lookup"><span data-stu-id="f0515-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="f0515-147">Criamos um duplo de teste TestContext para nosso contexto.</span><span class="sxs-lookup"><span data-stu-id="f0515-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="f0515-148">Em duplicatas de teste, podemos escolher o comportamento que queremos para dar suporte aos testes que vamos executar.</span><span class="sxs-lookup"><span data-stu-id="f0515-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="f0515-149">Neste exemplo, estamos apenas capturando o número de vezes que SaveChanges é chamado, mas você pode incluir qualquer lógica necessária para verificar o cenário que você está testando.</span><span class="sxs-lookup"><span data-stu-id="f0515-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="f0515-150">Também criamos um TestDbSet que fornece uma implementação na memória de DbSet.</span><span class="sxs-lookup"><span data-stu-id="f0515-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="f0515-151">Fornecemos uma implementação completa para todos os métodos no DbSet (exceto para localizar), mas você só precisa implementar os membros que o seu cenário de teste usará.</span><span class="sxs-lookup"><span data-stu-id="f0515-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="f0515-152">O TestDbSet usa algumas outras classes de infraestrutura que incluímos para garantir que as consultas assíncronas possam ser processadas.</span><span class="sxs-lookup"><span data-stu-id="f0515-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

### <a name="implementing-find"></a><span data-ttu-id="f0515-153">Implementando a localização</span><span class="sxs-lookup"><span data-stu-id="f0515-153">Implementing Find</span></span>  

<span data-ttu-id="f0515-154">O método Find é difícil de implementar de maneira genérica.</span><span class="sxs-lookup"><span data-stu-id="f0515-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="f0515-155">Se você precisar testar o código que usa o método Find, é mais fácil criar um DbSet de teste para cada um dos tipos de entidade que precisam oferecer suporte a Find.</span><span class="sxs-lookup"><span data-stu-id="f0515-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="f0515-156">Em seguida, você pode escrever lógica para encontrar esse tipo específico de entidade, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f0515-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a><span data-ttu-id="f0515-157">Escrevendo alguns testes</span><span class="sxs-lookup"><span data-stu-id="f0515-157">Writing some tests</span></span>  

<span data-ttu-id="f0515-158">Isso é tudo o que precisamos fazer para começar a testar.</span><span class="sxs-lookup"><span data-stu-id="f0515-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="f0515-159">O teste a seguir cria um TestContext e, em seguida, um serviço baseado nesse contexto.</span><span class="sxs-lookup"><span data-stu-id="f0515-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="f0515-160">Em seguida, o serviço é usado para criar um novo blog – usando o método addblog.</span><span class="sxs-lookup"><span data-stu-id="f0515-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="f0515-161">Por fim, o teste verifica se o serviço adicionou um novo blog à propriedade Blogs do contexto e se chamou SaveChanges no contexto.</span><span class="sxs-lookup"><span data-stu-id="f0515-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="f0515-162">Esse é apenas um exemplo dos tipos de coisas que você pode testar com um duplo de teste na memória e pode ajustar a lógica das duplicatas de teste e a verificação para atender às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="f0515-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

<span data-ttu-id="f0515-163">Aqui está outro exemplo de um teste-desta vez, um que executa uma consulta.</span><span class="sxs-lookup"><span data-stu-id="f0515-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="f0515-164">O teste começa criando um contexto de teste com alguns dados em sua propriedade de blog – Observe que os dados não estão em ordem alfabética.</span><span class="sxs-lookup"><span data-stu-id="f0515-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="f0515-165">Em seguida, podemos criar um BlogService com base em nosso contexto de teste e garantir que os dados que obtemos de GetAllBlogs sejam ordenados por nome.</span><span class="sxs-lookup"><span data-stu-id="f0515-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

<span data-ttu-id="f0515-166">Por fim, vamos escrever mais um teste que usa nosso método Async para garantir que a infraestrutura assíncrona que incluímos no [TestDbSet](#creating-the-in-memory-test-doubles) esteja funcionando.</span><span class="sxs-lookup"><span data-stu-id="f0515-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
