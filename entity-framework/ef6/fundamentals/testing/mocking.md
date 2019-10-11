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
# <a name="testing-with-a-mocking-framework"></a>Testando com uma estrutura fictícia
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Ao escrever testes para seu aplicativo, geralmente é desejável evitar atingir o banco de dados.  Entity Framework permite que você faça isso criando um contexto – com o comportamento definido pelos seus testes – que usa dados na memória.  

## <a name="options-for-creating-test-doubles"></a>Opções para criar duplicatas de teste  

Há duas abordagens diferentes que podem ser usadas para criar uma versão na memória do seu contexto.  

- **Crie suas próprias duplicatas de teste** – essa abordagem envolve escrever sua própria implementação na memória de seu contexto e DbSets. Isso lhe dá muito controle sobre como as classes se comportam, mas pode envolver escrever e possuir uma quantidade razoável de código.  
- **Use uma estrutura fictícia para criar duplicatas de teste** – usando uma estrutura fictícia (como MOQ) você pode ter as implementações na memória do contexto e os conjuntos criados dinamicamente no tempo de execução para você.  

Este artigo tratará do uso de uma estrutura fictícia. Para criar suas próprias duplicatas de teste, consulte [testando com suas próprias duplicatas de teste](writing-test-doubles.md).  

Para demonstrar o uso do EF com uma estrutura fictícia, vamos usar MOQ. A maneira mais fácil de obter o MOQ é instalar o [pacote MOQ do NuGet](https://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Testando com versões EF6  

O cenário mostrado neste artigo depende de algumas alterações feitas no DbSet no EF6. Para testar com o EF5 e a versão anterior, consulte [testando com um contexto falso](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitações de duplicatas de teste na memória do EF  

As duplicatas de teste na memória podem ser uma boa maneira de fornecer uma cobertura de nível de teste de unidade de bits do seu aplicativo que usa o EF. No entanto, ao fazer isso, você está usando LINQ to Objects para executar consultas em dados na memória. Isso pode resultar em comportamento diferente do que usar o provedor LINQ do EF (LINQ to Entities) para converter consultas em SQL que são executadas em seu banco de dados.  

Um exemplo de tal diferença é carregar dados relacionados. Se você criar uma série de Blogs que cada um tem postagens relacionadas, ao usar dados na memória, as postagens relacionadas sempre serão carregadas para cada blog. No entanto, durante a execução em um banco de dados, eles serão carregados somente se você usar o método include.  

Por esse motivo, é recomendável sempre incluir algum nível de teste de ponta a ponta (além dos testes de unidade) para garantir que seu aplicativo funcione corretamente em um banco de dados.  

## <a name="following-along-with-this-article"></a>Acompanhando este artigo  

Este artigo fornece listagens de código completas que você pode copiar para o Visual Studio para acompanhar, se desejar. É mais fácil criar um projeto de **teste de unidade** e você precisará direcionar **.NET Framework 4,5** para concluir as seções que usam Async.  

## <a name="the-ef-model"></a>O modelo do EF  

O serviço que vamos testar faz uso de um modelo do EF composto pelas classes BloggingContext e blog e post. Esse código pode ter sido gerado pelo designer do EF ou ser um modelo de Code First.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Propriedades de DbSet virtual com o designer do EF  

Observe que as propriedades DbSet no contexto são marcadas como virtuais. Isso permitirá que a estrutura fictícia derive de nosso contexto e substituindo essas propriedades por uma implementação fictícia.  

Se você estiver usando Code First, poderá editar suas classes diretamente. Se você estiver usando o designer do EF, precisará editar o modelo T4 que gera seu contexto. Abra o \<model_name @ no__t-1. Arquivo Context.tt que está aninhado no arquivo EDMX, localize o fragmento de código a seguir e adicione a palavra-chave virtual, conforme mostrado.  

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

## <a name="service-to-be-tested"></a>Serviço a ser testado  

Para demonstrar o teste com duplicatas de teste na memória, vamos escrever alguns testes para um BlogService. O serviço é capaz de criar novos Blogs (addblog) e retornar todos os Blogs ordenados pelo nome (GetAllBlogs). Além de GetAllBlogs, também fornecemos um método que receberá de forma assíncrona todos os Blogs ordenados pelo nome (GetAllBlogsAsync).  

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

## <a name="testing-non-query-scenarios"></a>Testando cenários de não consulta  

Isso é tudo o que precisamos fazer para começar a testar métodos que não são de consulta. O teste a seguir usa MOQ para criar um contexto. Em seguida, ele cria um DbSet @ no__t-0Blog @ no__t-1 e conecta-o para ser retornado da propriedade Blogs do contexto. Em seguida, o contexto é usado para criar um novo BlogService, que é usado para criar um novo blog, usando o método addblog. Por fim, o teste verifica se o serviço adicionou um novo blog e se chamou SaveChanges no contexto.  

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

## <a name="testing-query-scenarios"></a>Testando cenários de consulta  

Para poder executar consultas em nosso duplo de teste DbSet, precisamos configurar uma implementação de IQueryable. A primeira etapa é criar alguns dados na memória – estamos usando uma lista @ no__t-0Blog @ no__t-1. Em seguida, criamos um contexto e DBSet @ no__t-0Blog @ no__t-1 e, em seguida, conectamos a implementação IQueryable para o DbSet – eles estão apenas delegando ao provedor de LINQ to Objects que funciona com a lista @ no__t-2T @ no__t-3.  

Em seguida, podemos criar um BlogService com base em nossas duplicatas de teste e garantir que os dados que obtemos de GetAllBlogs sejam ordenados por nome.  

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

### <a name="testing-with-async-queries"></a>Testando com consultas assíncronas

Entity Framework 6 introduziu um conjunto de métodos de extensão que podem ser usados para executar uma consulta de forma assíncrona. Exemplos desses métodos incluem ToListAsync, FirstAsync, ForEachAsync, etc.  

Como Entity Framework consultas fazem uso do LINQ, os métodos de extensão são definidos em IQueryable e IEnumerable. No entanto, como eles são projetados apenas para serem usados com Entity Framework você poderá receber o seguinte erro se tentar usá-los em uma consulta LINQ que não seja uma consulta Entity Framework:

> O IQueryable de origem não implementa IDbAsyncEnumerable @ no__t-0. Somente as fontes que implementam IDbAsyncEnumerable podem ser usadas para Entity Framework operações assíncronas. Para obter mais detalhes [, consulte http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).  

Embora os métodos assíncronos tenham suporte apenas durante a execução em uma consulta do EF, talvez você queira usá-los em seu teste de unidade ao executar um duplo de teste na memória de um DbSet.  

Para usar os métodos assíncronos, precisamos criar um DbAsyncQueryProvider na memória para processar a consulta assíncrona. Embora seja possível configurar um provedor de consultas usando o MOQ, é muito mais fácil criar uma implementação dupla de teste no código. O código para essa implementação é o seguinte:  

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

Agora que temos um provedor de consulta assíncrona, podemos escrever um teste de unidade para nosso novo método GetAllBlogsAsync.  

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
