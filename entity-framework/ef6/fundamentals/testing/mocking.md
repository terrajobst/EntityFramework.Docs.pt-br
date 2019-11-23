---
title: Testando com uma estrutura fictícia-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 790e077c5b30c4a68a96b3c1a99b40893b2bbe55
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181568"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="382f4-102">Testando com uma estrutura fictícia</span><span class="sxs-lookup"><span data-stu-id="382f4-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="382f4-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="382f4-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="382f4-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="382f4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="382f4-105">Ao escrever testes para seu aplicativo, geralmente é desejável evitar atingir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="382f4-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="382f4-106">Entity Framework permite que você faça isso criando um contexto – com o comportamento definido pelos seus testes – que usa dados na memória.</span><span class="sxs-lookup"><span data-stu-id="382f4-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="382f4-107">Opções para criar duplicatas de teste</span><span class="sxs-lookup"><span data-stu-id="382f4-107">Options for creating test doubles</span></span>  

<span data-ttu-id="382f4-108">Há duas abordagens diferentes que podem ser usadas para criar uma versão na memória do seu contexto.</span><span class="sxs-lookup"><span data-stu-id="382f4-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="382f4-109">**Crie suas próprias duplicatas de teste** – essa abordagem envolve escrever sua própria implementação na memória de seu contexto e DbSets.</span><span class="sxs-lookup"><span data-stu-id="382f4-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="382f4-110">Isso lhe dá muito controle sobre como as classes se comportam, mas pode envolver escrever e possuir uma quantidade razoável de código.</span><span class="sxs-lookup"><span data-stu-id="382f4-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="382f4-111">**Use uma estrutura fictícia para criar duplicatas de teste** – usando uma estrutura fictícia (como MOQ) você pode ter as implementações na memória do contexto e os conjuntos criados dinamicamente no tempo de execução para você.</span><span class="sxs-lookup"><span data-stu-id="382f4-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="382f4-112">Este artigo tratará do uso de uma estrutura fictícia.</span><span class="sxs-lookup"><span data-stu-id="382f4-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="382f4-113">Para criar suas próprias duplicatas de teste, consulte [testando com suas próprias duplicatas de teste](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="382f4-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="382f4-114">Para demonstrar o uso do EF com uma estrutura fictícia, vamos usar MOQ.</span><span class="sxs-lookup"><span data-stu-id="382f4-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="382f4-115">A maneira mais fácil de obter o MOQ é instalar o [pacote MOQ do NuGet](https://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="382f4-115">The easiest way to get Moq is to install the [Moq package from NuGet](https://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="382f4-116">Testando com versões EF6</span><span class="sxs-lookup"><span data-stu-id="382f4-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="382f4-117">O cenário mostrado neste artigo depende de algumas alterações feitas no DbSet no EF6.</span><span class="sxs-lookup"><span data-stu-id="382f4-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="382f4-118">Para testar com o EF5 e a versão anterior, consulte [testando com um contexto falso](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="382f4-118">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="382f4-119">Limitações de duplicatas de teste na memória do EF</span><span class="sxs-lookup"><span data-stu-id="382f4-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="382f4-120">As duplicatas de teste na memória podem ser uma boa maneira de fornecer uma cobertura de nível de teste de unidade de bits do seu aplicativo que usa o EF.</span><span class="sxs-lookup"><span data-stu-id="382f4-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="382f4-121">No entanto, ao fazer isso, você está usando LINQ to Objects para executar consultas em dados na memória.</span><span class="sxs-lookup"><span data-stu-id="382f4-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="382f4-122">Isso pode resultar em comportamento diferente do que usar o provedor LINQ do EF (LINQ to Entities) para converter consultas em SQL que são executadas em seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="382f4-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="382f4-123">Um exemplo de tal diferença é carregar dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="382f4-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="382f4-124">Se você criar uma série de Blogs que cada um tem postagens relacionadas, ao usar dados na memória, as postagens relacionadas sempre serão carregadas para cada blog.</span><span class="sxs-lookup"><span data-stu-id="382f4-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="382f4-125">No entanto, durante a execução em um banco de dados, eles serão carregados somente se você usar o método include.</span><span class="sxs-lookup"><span data-stu-id="382f4-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="382f4-126">Por esse motivo, é recomendável sempre incluir algum nível de teste de ponta a ponta (além dos testes de unidade) para garantir que seu aplicativo funcione corretamente em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="382f4-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="382f4-127">Acompanhando este artigo</span><span class="sxs-lookup"><span data-stu-id="382f4-127">Following along with this article</span></span>  

<span data-ttu-id="382f4-128">Este artigo fornece listagens de código completas que você pode copiar para o Visual Studio para acompanhar, se desejar.</span><span class="sxs-lookup"><span data-stu-id="382f4-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="382f4-129">É mais fácil criar um projeto de **teste de unidade** e você precisará direcionar **.NET Framework 4,5** para concluir as seções que usam Async.</span><span class="sxs-lookup"><span data-stu-id="382f4-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="382f4-130">O modelo do EF</span><span class="sxs-lookup"><span data-stu-id="382f4-130">The EF model</span></span>  

<span data-ttu-id="382f4-131">O serviço que vamos testar faz uso de um modelo do EF composto pelas classes BloggingContext e blog e post.</span><span class="sxs-lookup"><span data-stu-id="382f4-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="382f4-132">Esse código pode ter sido gerado pelo designer do EF ou ser um modelo de Code First.</span><span class="sxs-lookup"><span data-stu-id="382f4-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="382f4-133">Propriedades de DbSet virtual com o designer do EF</span><span class="sxs-lookup"><span data-stu-id="382f4-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="382f4-134">Observe que as propriedades DbSet no contexto são marcadas como virtuais.</span><span class="sxs-lookup"><span data-stu-id="382f4-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="382f4-135">Isso permitirá que a estrutura fictícia derive de nosso contexto e substituindo essas propriedades por uma implementação fictícia.</span><span class="sxs-lookup"><span data-stu-id="382f4-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="382f4-136">Se você estiver usando Code First, poderá editar suas classes diretamente.</span><span class="sxs-lookup"><span data-stu-id="382f4-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="382f4-137">Se você estiver usando o designer do EF, precisará editar o modelo T4 que gera seu contexto.</span><span class="sxs-lookup"><span data-stu-id="382f4-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="382f4-138">Abra o\>de model_name de \<. Arquivo Context.tt que está aninhado no arquivo EDMX, localize o fragmento de código a seguir e adicione a palavra-chave virtual, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="382f4-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="382f4-139">Serviço a ser testado</span><span class="sxs-lookup"><span data-stu-id="382f4-139">Service to be tested</span></span>  

<span data-ttu-id="382f4-140">Para demonstrar o teste com duplicatas de teste na memória, vamos escrever alguns testes para um BlogService.</span><span class="sxs-lookup"><span data-stu-id="382f4-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="382f4-141">O serviço é capaz de criar novos Blogs (addblog) e retornar todos os Blogs ordenados pelo nome (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="382f4-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="382f4-142">Além de GetAllBlogs, também fornecemos um método que receberá de forma assíncrona todos os Blogs ordenados pelo nome (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="382f4-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="382f4-143">Testando cenários de não consulta</span><span class="sxs-lookup"><span data-stu-id="382f4-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="382f4-144">Isso é tudo o que precisamos fazer para começar a testar métodos que não são de consulta.</span><span class="sxs-lookup"><span data-stu-id="382f4-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="382f4-145">O teste a seguir usa MOQ para criar um contexto.</span><span class="sxs-lookup"><span data-stu-id="382f4-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="382f4-146">Em seguida, ele cria um\> de blog DbSet\<e conecta-o de forma que ele seja retornado da propriedade Blogs do contexto.</span><span class="sxs-lookup"><span data-stu-id="382f4-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="382f4-147">Em seguida, o contexto é usado para criar um novo BlogService, que é usado para criar um novo blog, usando o método addblog.</span><span class="sxs-lookup"><span data-stu-id="382f4-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="382f4-148">Por fim, o teste verifica se o serviço adicionou um novo blog e se chamou SaveChanges no contexto.</span><span class="sxs-lookup"><span data-stu-id="382f4-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a><span data-ttu-id="382f4-149">Testando cenários de consulta</span><span class="sxs-lookup"><span data-stu-id="382f4-149">Testing query scenarios</span></span>  

<span data-ttu-id="382f4-150">Para poder executar consultas em nosso duplo de teste DbSet, precisamos configurar uma implementação de IQueryable.</span><span class="sxs-lookup"><span data-stu-id="382f4-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="382f4-151">A primeira etapa é criar alguns dados na memória – estamos usando uma lista\<\>do blog.</span><span class="sxs-lookup"><span data-stu-id="382f4-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="382f4-152">Em seguida, criamos um contexto e DBSet\<blog\>, em seguida, conectamos a implementação IQueryable para o DbSet – eles estão apenas delegando ao provedor de LINQ to Objects que funciona com list\<T\>.</span><span class="sxs-lookup"><span data-stu-id="382f4-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="382f4-153">Em seguida, podemos criar um BlogService com base em nossas duplicatas de teste e garantir que os dados que obtemos de GetAllBlogs sejam ordenados por nome.</span><span class="sxs-lookup"><span data-stu-id="382f4-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a><span data-ttu-id="382f4-154">Testando com consultas assíncronas</span><span class="sxs-lookup"><span data-stu-id="382f4-154">Testing with async queries</span></span>

<span data-ttu-id="382f4-155">Entity Framework 6 introduziu um conjunto de métodos de extensão que podem ser usados para executar uma consulta de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="382f4-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="382f4-156">Exemplos desses métodos incluem ToListAsync, FirstAsync, ForEachAsync, etc.</span><span class="sxs-lookup"><span data-stu-id="382f4-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="382f4-157">Como Entity Framework consultas fazem uso do LINQ, os métodos de extensão são definidos em IQueryable e IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="382f4-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="382f4-158">No entanto, como eles são projetados apenas para serem usados com Entity Framework você poderá receber o seguinte erro se tentar usá-los em uma consulta LINQ que não seja uma consulta Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="382f4-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="382f4-159">O IQueryable de origem não implementa{0}IDbAsyncEnumerable.</span><span class="sxs-lookup"><span data-stu-id="382f4-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="382f4-160">Somente as fontes que implementam IDbAsyncEnumerable podem ser usadas para Entity Framework operações assíncronas.</span><span class="sxs-lookup"><span data-stu-id="382f4-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="382f4-161">Para obter mais detalhes, consulte [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="382f4-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="382f4-162">Embora os métodos assíncronos tenham suporte apenas durante a execução em uma consulta do EF, talvez você queira usá-los em seu teste de unidade ao executar um duplo de teste na memória de um DbSet.</span><span class="sxs-lookup"><span data-stu-id="382f4-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="382f4-163">Para usar os métodos assíncronos, precisamos criar um DbAsyncQueryProvider na memória para processar a consulta assíncrona.</span><span class="sxs-lookup"><span data-stu-id="382f4-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="382f4-164">Embora seja possível configurar um provedor de consultas usando o MOQ, é muito mais fácil criar uma implementação dupla de teste no código.</span><span class="sxs-lookup"><span data-stu-id="382f4-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="382f4-165">O código para essa implementação é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="382f4-165">The code for this implementation is as follows:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
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

<span data-ttu-id="382f4-166">Agora que temos um provedor de consulta assíncrona, podemos escrever um teste de unidade para nosso novo método GetAllBlogsAsync.</span><span class="sxs-lookup"><span data-stu-id="382f4-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
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

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
