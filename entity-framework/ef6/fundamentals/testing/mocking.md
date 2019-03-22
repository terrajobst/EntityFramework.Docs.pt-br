---
title: Teste com uma estrutura de simulação - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 3d39b41018beb70b72105dfb2fe4d61afc0b0525
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319199"
---
# <a name="testing-with-a-mocking-framework"></a>Teste com uma estrutura de simulação
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Ao escrever testes para seu aplicativo geralmente é desejável evitar atingir o banco de dados.  Entity Framework permite que você fazer isso criando um contexto – com o comportamento definido por seus testes – que faz uso de dados na memória.  

## <a name="options-for-creating-test-doubles"></a>Opções para a criação de duplicatas de teste  

Há duas abordagens diferentes que podem ser usadas para criar uma versão na memória de seu contexto.  

- **Criar seus próprio duplicatas de teste** – essa abordagem envolve escrever sua própria implementação na memória de seu contexto e o DbSets. Isso lhe dá um grande controle sobre como as classes se comportam, mas podem envolver a gravação e possui uma quantidade razoável de código.  
- **Use uma estrutura de simulação para criar duplicatas de teste** – usando uma estrutura de simulação (como o Moq) que as implementações de na memória de seu contexto e os conjuntos criados dinamicamente em tempo de execução para você.  

Este artigo irá lidar com o uso de uma estrutura de simulação. Para criar seus próprio duplicatas de teste, consulte [testes com seu duplicatas de teste próprio](writing-test-doubles.md).  

Para demonstrar o uso do EF com uma estrutura de simulação vamos usar o Moq. A maneira mais fácil de obter Moq é instalar o [Moq pacote do NuGet](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Teste com as versões de pré-EF6  

O cenário mostrado neste artigo é dependente de algumas alterações feitas no DbSet no EF6. Para testar com o EF5 e a versão anterior, consulte [testando com um contexto forjar](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitações de duplicatas de teste na memória do EF  

Duplicatas de teste na memória podem ser uma boa maneira de fornecer a cobertura do nível de bits que usam o EF do seu aplicativo de teste de unidade. No entanto, ao fazer isso, você está usando LINQ to Objects para executar consultas em dados na memória. Isso pode resultar em comportamento diferente de usando o provedor LINQ do EF (LINQ to Entities) para converter consultas em SQL que é executada em seu banco de dados.  

Um exemplo de uma diferença tão está carregando dados relacionados. Se você criar uma série de Blogs que cada que as postagens relacionadas, em seguida, ao usar os dados na memória as postagens relacionadas sempre serão carregadas para cada Blog. No entanto, ao executar em um banco de dados os dados só serão carregados se você usar o método Include.  

Por esse motivo, é recomendável sempre incluir algum nível de testes de ponta a ponta (além de seus testes de unidade) para garantir que seu aplicativo funciona corretamente em relação a um banco de dados.  

## <a name="following-along-with-this-article"></a>A seguir, juntamente com este artigo  

Este artigo fornece as listagens de código completo que você pode copiar para o Visual Studio para acompanhá-lo se desejar. É mais fácil criar uma **projeto de teste de unidade** e será necessário para o destino **.NET Framework 4.5** para concluir as seções que usam async.  

## <a name="the-ef-model"></a>O modelo do EF  

O serviço, vamos testar faz uso de um EF modelo composto pela BloggingContext e as classes de postagem de Blog. Esse código pode ter sido gerado pelo Designer de EF ou ser um modelo Code First.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Propriedades DbSet virtual com o EF Designer  

Observe que as propriedades DbSet no contexto são marcadas como virtuais. Isso permitirá que a estrutura de simulação derivar de nosso contexto e substituir essas propriedades com uma implementação fictícia.  

Se você estiver usando o Code First, em seguida, você pode editar suas classes diretamente. Se você estiver usando o EF Designer, em seguida, você precisará editar o modelo T4 que gera seu contexto. Abra o \<model_name\>. Arquivo context.TT que é aninhado sob você arquivo edmx, localize o fragmento de código a seguir e adicione a palavra-chave virtual, conforme mostrado.  

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

Para demonstrar testes com duplicatas de teste de na memória, vamos escrever alguns testes para um BlogService. O serviço é capaz de criar novos blogs (AddBlog) e retornar todos os Blogs ordenados por nome (GetAllBlogs). Além de GetAllBlogs, também fornecemos um método que assincronamente obterá todos os blogs ordenados por nome (GetAllBlogsAsync).  

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

## <a name="testing-non-query-scenarios"></a>Cenários sem consulta de teste  

Isso é tudo que precisamos fazer para começar a testar métodos sem consulta. O teste a seguir usa o Moq para criar um contexto. Depois, ele cria um DbSet\<Blog\> e conecta a ser retornado da propriedade de Blogs do contexto. Em seguida, o contexto é usado para criar um novo BlogService que é usado para criar um novo blog – usando o método AddBlog. Por fim, o teste verifica que o serviço adicionado um novo Blog e chamei SaveChanges no contexto.  

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

## <a name="testing-query-scenarios"></a>Cenários de teste de consulta  

Para poder executar consultas em nosso duplicata de teste do DbSet, precisamos configurar uma implementação de IQueryable. A primeira etapa é criar alguns dados na memória – estamos usando uma lista\<Blog\>. Em seguida, criamos um contexto e DBSet\<Blog\> , em seguida, conectar a implementação IQueryable DbSet – eles simplesmente delegar para o provedor LINQ to Objects que funciona com a lista\<T\>.  

Em seguida, podemos criar um BlogService com base em nossa duplicatas de teste e certifique-se de que os dados que obtemos de GetAllBlogs são ordenados por nome.  

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

Entity Framework 6 introduziu um conjunto de métodos de extensão que pode ser usado de forma assíncrona, executar uma consulta. Exemplos desses métodos incluem ToListAsync, FirstAsync, ForEachAsync, etc.  

Porque as consultas do Entity Framework fazem uso do LINQ, os métodos de extensão são definidos no IQueryable e IEnumerable. No entanto, porque eles são criados somente para ser usado com o Entity Framework pode receber o seguinte erro se você tentar usá-las em uma consulta LINQ que não é uma consulta do Entity Framework:

> A fonte de IQueryable não implementa IDbAsyncEnumerable{0}. Apenas as fontes que implementam IDbAsyncEnumerable podem ser usadas para operações assíncronas do Entity Framework. Para obter mais detalhes, consulte [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068).  

Embora os métodos assíncronos têm suporte apenas ao executar em uma consulta do EF, talvez queira usá-los em seu teste de unidade quando dupla de um DbSet de teste em execução em relação a na memória.  

Para usar os métodos assíncronos, precisamos criar um DbAsyncQueryProvider na memória para processar a consulta de async. Embora seria possível configurar um provedor de consulta usando o Moq, é muito mais fácil de criar uma implementação de duplo de teste no código. O código para esta implementação é da seguinte maneira:  

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

Agora que temos um provedor de consulta assíncrona podemos escrever um teste de unidade para nosso novo método GetAllBlogsAsync.  

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
