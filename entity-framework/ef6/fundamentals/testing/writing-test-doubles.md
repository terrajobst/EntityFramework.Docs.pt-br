---
title: Teste com seus próprio duplicatas de teste - o EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
caps.latest.revision: 4
ms.openlocfilehash: 4e2511f92f9bb034ab468dd030ef238e325ce7c0
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39119895"
---
# <a name="testing-with-your-own-test-doubles"></a>Teste com seus próprio duplicatas de teste
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Ao escrever testes para seu aplicativo geralmente é desejável evitar atingir o banco de dados.  Entity Framework permite que você fazer isso criando um contexto – com o comportamento definido por seus testes – que faz uso de dados na memória.  

## <a name="options-for-creating-test-doubles"></a>Opções para a criação de duplicatas de teste  

Há duas abordagens diferentes que podem ser usadas para criar uma versão na memória de seu contexto.  

- **Criar seus próprio duplicatas de teste** – essa abordagem envolve escrever sua própria implementação na memória de seu contexto e o DbSets. Isso lhe dá um grande controle sobre como as classes se comportam, mas podem envolver a gravação e possui uma quantidade razoável de código.  
- **Use uma estrutura de simulação para criar duplicatas de teste** – usando uma estrutura de simulação (como o Moq) que as implementações de na memória que contexto e os conjuntos criados dinamicamente em tempo de execução para você.  

Este artigo lidará com a criação de seu próprio teste duplo. Para obter informações sobre como usar uma estrutura de simulação, consulte [testes com uma estrutura de simulação](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Teste com as versões de pré-EF6  

O código mostrado neste artigo é compatível com o EF6. Para testar com o EF5 e a versão anterior, consulte [testando com um contexto forjar](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitações de duplicatas de teste na memória do EF  

Duplicatas de teste na memória podem ser uma boa maneira de fornecer a cobertura do nível de bits que usam o EF do seu aplicativo de teste de unidade. No entanto, ao fazer isso, você está usando LINQ to Objects para executar consultas em dados na memória. Isso pode resultar em comportamento diferente de usando o provedor LINQ do EF (LINQ to Entities) para converter consultas em SQL que é executada em seu banco de dados.  

Um exemplo de uma diferença tão está carregando dados relacionados. Se você criar uma série de Blogs que cada que as postagens relacionadas, em seguida, ao usar os dados na memória as postagens relacionadas sempre serão carregadas para cada Blog. No entanto, ao executar em um banco de dados os dados só serão carregados se você usar o método Include.  

Por esse motivo, é recomendável sempre incluir algum nível de testes de ponta a ponta (além de seus testes de unidade) para garantir que seu aplicativo funciona corretamente em relação a um banco de dados.  

## <a name="following-along-with-this-article"></a>A seguir, juntamente com este artigo  

Este artigo fornece as listagens de código completo que você pode copiar para o Visual Studio para acompanhá-lo se desejar. É mais fácil criar uma **projeto de teste de unidade** e será necessário para o destino **.NET Framework 4.5** para concluir as seções que usam async.  

## <a name="creating-a-context-interface"></a>Criando uma interface de contexto  

Vamos olhar no teste de um serviço que usa o EF um modelo. Para poder substituir nosso contexto EF com uma versão na memória para teste, vamos definir uma interface que o nosso contexto EF (e dupla na memória) serão imeplement.  

O serviço, vamos testar consultar e modificar dados usando as propriedades DbSet de nosso contexto e também chamar SaveChanges para enviar por push as alterações no banco de dados. Portanto, estamos incluindo esses membros na interface.  

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

O serviço, vamos testar faz uso de um EF modelo composto pela BloggingContext e as classes de postagem de Blog. Esse código pode ter sido gerado pelo Designer de EF ou ser um modelo Code First.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementando a interface de contexto com o EF Designer  

Observe que o nosso contexto implementa a interface IBloggingContext.  

Se você estiver usando o Code First, em seguida, você pode editar seu contexto diretamente para implementar a interface. Se você estiver usando o EF Designer, em seguida, você precisará editar o modelo T4 que gera seu contexto. Abra o \<model_name\>. Arquivo context.TT que é aninhado sob você arquivo edmx, localizar o fragmento de código a seguir e adicione na interface, conforme mostrado.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
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

<a name="creating-the-in-memory-test-doubles"/> # # Criar o teste na memória dobra  

Agora que temos o modelo do EF real e o serviço que possa usá-lo, é hora de criar o teste na memória duplo que podemos usar para teste. Criamos um teste de TestContext duplo para o nosso contexto. Duplicatas de teste, vamos escolher o comportamento que queremos para dar suporte os testes, vamos executar. Neste exemplo, estamos apenas está capturando o número de vezes que SaveChanges é chamado, mas você pode incluir qualquer lógica que é necessário para verificar o cenário que você está testando.  

Também criamos um TestDbSet que fornece uma implementação na memória do DbSet. Nós fornecemos uma implementação completa para todos os métodos no DbSet (exceto para localizar), mas você só precisa implementar os membros que seu cenário de teste usará.  

TestDbSet faz uso de algumas outras classes de infraestrutura que incluímos para garantir que as consultas assíncronas podem ser processadas.  

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

### <a name="implementing-find"></a>Implementação de Find  

O método Find é difícil de implementar de uma forma genérica. Se você precisa testar código que faz use o método Find é mais fácil de criar um teste de DbSet para cada um dos tipos de entidade que precisam para dar suporte a localizar. Em seguida, você pode escrever lógica para localizar esse tipo específico de entidade, conforme mostrado abaixo.  

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

## <a name="writing-some-tests"></a>Gravando alguns testes  

Isso é tudo que precisamos fazer para começar a testar. O teste a seguir cria um TestContext e, em seguida, um serviço com base nesse contexto. O serviço, em seguida, é usado para criar um novo blog – usando o método AddBlog. Por fim, o teste verifica que o serviço adicionado um novo Blog à propriedade de Blogs do contexto e chamei SaveChanges no contexto.  

Isso é apenas um exemplo dos tipos de coisas que você pode testar com uma duplicata de teste na memória, e você pode ajustar a lógica de duplicatas de teste e a verificação para atender às suas necessidades.  

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

Aqui está outro exemplo de um teste – desta vez aquele que executa uma consulta. O teste começa com a criação de um contexto de teste com alguns dados em sua propriedade Blog - Observe que os dados não estão em ordem alfabética. Em seguida, podemos criar um BlogService com base em nosso contexto de teste e certifique-se de que os dados que obtemos de GetAllBlogs são ordenados por nome.  

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

Por fim, vamos escrever mais um teste que usa nosso método assíncrono para garantir que a infra-estrutura de async que incluímos [TestDbSet](#creating-the-in-memory-test-doubles) está funcionando.  

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
