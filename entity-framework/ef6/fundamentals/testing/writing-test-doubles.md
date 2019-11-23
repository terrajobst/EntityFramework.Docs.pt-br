---
title: Testando com suas próprias duplicatas de teste-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446033"
---
# <a name="testing-with-your-own-test-doubles"></a>Testando com suas próprias duplicatas de teste
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Ao escrever testes para seu aplicativo, geralmente é desejável evitar atingir o banco de dados.  Entity Framework permite que você faça isso criando um contexto – com o comportamento definido pelos seus testes – que usa dados na memória.  

## <a name="options-for-creating-test-doubles"></a>Opções para criar duplicatas de teste  

Há duas abordagens diferentes que podem ser usadas para criar uma versão na memória do seu contexto.  

- **Crie suas próprias duplicatas de teste** – essa abordagem envolve escrever sua própria implementação na memória de seu contexto e DbSets. Isso lhe dá muito controle sobre como as classes se comportam, mas pode envolver escrever e possuir uma quantidade razoável de código.  
- **Use uma estrutura fictícia para criar duplicatas de teste** – usando uma estrutura fictícia (como MOQ) você pode ter as implementações na memória do contexto e conjuntos criados dinamicamente no tempo de execução para você.  

Este artigo tratará da criação do seu próprio teste duplo. Para obter informações sobre como usar uma estrutura fictícia, consulte [testando com uma estrutura fictícia](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Testando com versões EF6  

O código mostrado neste artigo é compatível com EF6. Para testar com o EF5 e a versão anterior, consulte [testando com um contexto falso](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitações de duplicatas de teste na memória do EF  

As duplicatas de teste na memória podem ser uma boa maneira de fornecer uma cobertura de nível de teste de unidade de bits do seu aplicativo que usa o EF. No entanto, ao fazer isso, você está usando LINQ to Objects para executar consultas em dados na memória. Isso pode resultar em comportamento diferente do que usar o provedor LINQ do EF (LINQ to Entities) para converter consultas em SQL que são executadas em seu banco de dados.  

Um exemplo de tal diferença é carregar dados relacionados. Se você criar uma série de Blogs que cada um tem postagens relacionadas, ao usar dados na memória, as postagens relacionadas sempre serão carregadas para cada blog. No entanto, durante a execução em um banco de dados, eles serão carregados somente se você usar o método include.  

Por esse motivo, é recomendável sempre incluir algum nível de teste de ponta a ponta (além dos testes de unidade) para garantir que seu aplicativo funcione corretamente em um banco de dados.  

## <a name="following-along-with-this-article"></a>Acompanhando este artigo  

Este artigo fornece listagens de código completas que você pode copiar para o Visual Studio para acompanhar, se desejar. É mais fácil criar um projeto de **teste de unidade** e você precisará direcionar **.NET Framework 4,5** para concluir as seções que usam Async.  

## <a name="creating-a-context-interface"></a>Criando uma interface de contexto  

Vamos examinar o teste de um serviço que usa um modelo do EF. Para poder substituir nosso contexto do EF por uma versão na memória para teste, definiremos uma interface que o nosso contexto do EF (e que seja o dobro da memória) irá implementar.

O serviço que vamos testar consultará e modificará os dados usando as propriedades DbSet de nosso contexto e também chamará SaveChanges para enviar por push as alterações para o banco de dados. Então, estamos incluindo esses membros na interface.  

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

## <a name="the-ef-model"></a>O modelo do EF  

O serviço que vamos testar faz uso de um modelo do EF composto pelas classes BloggingContext e blog e post. Esse código pode ter sido gerado pelo designer do EF ou ser um modelo de Code First.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementando a interface de contexto com o designer do EF  

Observe que nosso contexto implementa a interface IBloggingContext.  

Se você estiver usando Code First, poderá editar o contexto diretamente para implementar a interface. Se você estiver usando o designer do EF, precisará editar o modelo T4 que gera seu contexto. Abra o\>de model_name de \<. Arquivo Context.tt que está aninhado no arquivo EDMX, localize o fragmento de código a seguir e adicione na interface, conforme mostrado.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
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

## <a name="creating-the-in-memory-test-doubles"></a>Criando duplicatas de teste na memória  

Agora que temos o modelo real de EF e o serviço que pode usá-lo, é hora de criar o duplo de teste na memória que podemos usar para teste. Criamos um duplo de teste TestContext para nosso contexto. Em duplicatas de teste, podemos escolher o comportamento que queremos para dar suporte aos testes que vamos executar. Neste exemplo, estamos apenas capturando o número de vezes que SaveChanges é chamado, mas você pode incluir qualquer lógica necessária para verificar o cenário que você está testando.  

Também criamos um TestDbSet que fornece uma implementação na memória de DbSet. Fornecemos uma implementação completa para todos os métodos no DbSet (exceto para localizar), mas você só precisa implementar os membros que o seu cenário de teste usará.  

O TestDbSet usa algumas outras classes de infraestrutura que incluímos para garantir que as consultas assíncronas possam ser processadas.  

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

### <a name="implementing-find"></a>Implementando a localização  

O método Find é difícil de implementar de maneira genérica. Se você precisar testar o código que usa o método Find, é mais fácil criar um DbSet de teste para cada um dos tipos de entidade que precisam oferecer suporte a Find. Em seguida, você pode escrever lógica para encontrar esse tipo específico de entidade, como mostrado abaixo.  

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

## <a name="writing-some-tests"></a>Escrevendo alguns testes  

Isso é tudo o que precisamos fazer para começar a testar. O teste a seguir cria um TestContext e, em seguida, um serviço baseado nesse contexto. Em seguida, o serviço é usado para criar um novo blog – usando o método addblog. Por fim, o teste verifica se o serviço adicionou um novo blog à propriedade Blogs do contexto e se chamou SaveChanges no contexto.  

Esse é apenas um exemplo dos tipos de coisas que você pode testar com um duplo de teste na memória e pode ajustar a lógica das duplicatas de teste e a verificação para atender às suas necessidades.  

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

Aqui está outro exemplo de um teste-desta vez, um que executa uma consulta. O teste começa criando um contexto de teste com alguns dados em sua propriedade de blog – Observe que os dados não estão em ordem alfabética. Em seguida, podemos criar um BlogService com base em nosso contexto de teste e garantir que os dados que obtemos de GetAllBlogs sejam ordenados por nome.  

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

Por fim, vamos escrever mais um teste que usa nosso método Async para garantir que a infraestrutura assíncrona que incluímos no [TestDbSet](#creating-the-in-memory-test-doubles) esteja funcionando.  

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
