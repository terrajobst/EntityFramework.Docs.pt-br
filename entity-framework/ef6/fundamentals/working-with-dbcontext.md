---
title: Trabalhando com DbContext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416318"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="db8c9-102">Como trabalhar com DbContext</span><span class="sxs-lookup"><span data-stu-id="db8c9-102">Working with DbContext</span></span>

<span data-ttu-id="db8c9-103">Para usar Entity Framework para consultar, inserir, atualizar e excluir dados usando objetos .NET, primeiro você precisa [criar um modelo](~/ef6/modeling/index.md) que mapeie as entidades e relações definidas em seu modelo para tabelas em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="db8c9-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="db8c9-104">Quando você tiver um modelo, a classe primária com a qual seu aplicativo interage é `System.Data.Entity.DbContext` (geralmente conhecida como a classe de contexto).</span><span class="sxs-lookup"><span data-stu-id="db8c9-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="db8c9-105">Você pode usar um DbContext associado a um modelo para:</span><span class="sxs-lookup"><span data-stu-id="db8c9-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="db8c9-106">Criar e executar consultas</span><span class="sxs-lookup"><span data-stu-id="db8c9-106">Write and execute queries</span></span>   
- <span data-ttu-id="db8c9-107">Materializar resultados da consulta como objetos de entidade</span><span class="sxs-lookup"><span data-stu-id="db8c9-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="db8c9-108">Controlar alterações feitas nesses objetos</span><span class="sxs-lookup"><span data-stu-id="db8c9-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="db8c9-109">Persistir alterações no objeto de volta no banco de dados</span><span class="sxs-lookup"><span data-stu-id="db8c9-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="db8c9-110">Associar objetos na memória a controles da interface do usuário</span><span class="sxs-lookup"><span data-stu-id="db8c9-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="db8c9-111">Esta página fornece algumas diretrizes sobre como gerenciar a classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="db8c9-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="db8c9-112">Definindo uma classe derivada DbContext</span><span class="sxs-lookup"><span data-stu-id="db8c9-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="db8c9-113">A maneira recomendada para trabalhar com o contexto é definir uma classe derivada de DbContext e expõe as propriedades DbSet que representam as coleções das entidades especificadas no contexto.</span><span class="sxs-lookup"><span data-stu-id="db8c9-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="db8c9-114">Se você estiver trabalhando com o designer do EF, o contexto será gerado para você.</span><span class="sxs-lookup"><span data-stu-id="db8c9-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="db8c9-115">Se você estiver trabalhando com Code First, normalmente escreverá o contexto por conta própria.</span><span class="sxs-lookup"><span data-stu-id="db8c9-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="db8c9-116">Depois de ter um contexto, você consultaria, adicionaria (usando os métodos `Add` ou `Attach`) ou removerá as entidades (usando `Remove`) no contexto por meio dessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="db8c9-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="db8c9-117">O acesso a uma propriedade `DbSet` em um objeto de contexto representa uma consulta inicial que retorna todas as entidades do tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="db8c9-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="db8c9-118">Observe que apenas o acesso a uma propriedade não executará a consulta.</span><span class="sxs-lookup"><span data-stu-id="db8c9-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="db8c9-119">Uma consulta é executada quando:</span><span class="sxs-lookup"><span data-stu-id="db8c9-119">A query is executed when:</span></span>  

- <span data-ttu-id="db8c9-120">Ele é enumerado por uma instrução `foreach`C#() ou `For Each` (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="db8c9-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="db8c9-121">Ele é enumerado por uma operação de coleta, como `ToArray`, `ToDictionary`ou `ToList`.</span><span class="sxs-lookup"><span data-stu-id="db8c9-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="db8c9-122">Operadores LINQ, como `First` ou `Any`, são especificados na parte mais externa da consulta.</span><span class="sxs-lookup"><span data-stu-id="db8c9-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="db8c9-123">Um dos seguintes métodos é chamado de: o método de extensão `Load`, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`e `DbSet<T>.Find`, se uma entidade com a chave especificada não for encontrada já carregada no contexto.</span><span class="sxs-lookup"><span data-stu-id="db8c9-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="db8c9-124">Tempo de vida</span><span class="sxs-lookup"><span data-stu-id="db8c9-124">Lifetime</span></span>  

<span data-ttu-id="db8c9-125">O tempo de vida do contexto começa quando a instância é criada e termina quando a instância é descartada ou coletada como lixo.</span><span class="sxs-lookup"><span data-stu-id="db8c9-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="db8c9-126">Use **usando** se você quiser que todos os recursos que os controles de contexto sejam descartados no final do bloco.</span><span class="sxs-lookup"><span data-stu-id="db8c9-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="db8c9-127">Quando **você usa o, o**compilador cria automaticamente um bloco try/finally e chama Dispose no bloco **finally** .</span><span class="sxs-lookup"><span data-stu-id="db8c9-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="db8c9-128">Aqui estão algumas diretrizes gerais ao decidir sobre o tempo de vida do contexto:</span><span class="sxs-lookup"><span data-stu-id="db8c9-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="db8c9-129">Ao trabalhar com aplicativos Web, use uma instância de contexto por solicitação.</span><span class="sxs-lookup"><span data-stu-id="db8c9-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="db8c9-130">Ao trabalhar com Windows Presentation Foundation (WPF) ou Windows Forms, use uma instância de contexto por formulário.</span><span class="sxs-lookup"><span data-stu-id="db8c9-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="db8c9-131">Isso permite que você use a funcionalidade de controle de alterações que o contexto fornece.</span><span class="sxs-lookup"><span data-stu-id="db8c9-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="db8c9-132">Se a instância de contexto for criada por um contêiner de injeção de dependência, geralmente é responsabilidade do contêiner descartar o contexto.</span><span class="sxs-lookup"><span data-stu-id="db8c9-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="db8c9-133">Se o contexto for criado no código do aplicativo, lembre-se de descartar o contexto quando ele não for mais necessário.</span><span class="sxs-lookup"><span data-stu-id="db8c9-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="db8c9-134">Ao trabalhar com contexto de execução longa, considere o seguinte:</span><span class="sxs-lookup"><span data-stu-id="db8c9-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="db8c9-135">Conforme você carrega mais objetos e suas referências na memória, o consumo de memória do contexto pode aumentar rapidamente.</span><span class="sxs-lookup"><span data-stu-id="db8c9-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="db8c9-136">Isso pode causar problemas de desempenho.</span><span class="sxs-lookup"><span data-stu-id="db8c9-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="db8c9-137">O contexto não é thread-safe, portanto, ele não deve ser compartilhado entre vários threads, fazendo o trabalho nele simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="db8c9-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="db8c9-138">Se uma exceção fizer com que o contexto esteja em um estado irrecuperável, todo o aplicativo poderá ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="db8c9-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="db8c9-139">As chances de ocorrerem problemas relacionados à simultaneidade aumentam de acordo com o intervalo entre o tempo em que os dados são consultados e atualizados.</span><span class="sxs-lookup"><span data-stu-id="db8c9-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="db8c9-140">conexões</span><span class="sxs-lookup"><span data-stu-id="db8c9-140">Connections</span></span>  

<span data-ttu-id="db8c9-141">Por padrão, o contexto gerencia as conexões com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="db8c9-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="db8c9-142">O contexto é aberto e fecha as conexões conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="db8c9-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="db8c9-143">Por exemplo, o contexto abre uma conexão para executar uma consulta e fecha a conexão quando todos os conjuntos de resultados são processados.</span><span class="sxs-lookup"><span data-stu-id="db8c9-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="db8c9-144">Há casos em que você deseja ter mais controle sobre quando a conexão abre e fecha.</span><span class="sxs-lookup"><span data-stu-id="db8c9-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="db8c9-145">Por exemplo, ao trabalhar com SQL Server Compact, geralmente é recomendável manter uma conexão aberta separada com o banco de dados durante o tempo de vida do aplicativo para melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="db8c9-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="db8c9-146">Você pode gerenciar esse processo manualmente usando a propriedade `Connection`.</span><span class="sxs-lookup"><span data-stu-id="db8c9-146">You can manage this process manually by using the `Connection` property.</span></span>  
