---
title: Configurando um DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3ab90d46b7a4476044e5ea38eaf04f995708e7bf
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416665"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="c6bcd-102">Configurar um DbContext</span><span class="sxs-lookup"><span data-stu-id="c6bcd-102">Configuring a DbContext</span></span>

<span data-ttu-id="c6bcd-103">Este artigo mostra padrões básicos para configurar um `DbContext` por meio de um `DbContextOptions` para se conectar a um banco de dados usando um provedor de EF Core específico e comportamentos opcionais.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="c6bcd-104">Configuração de DbContext de tempo de design</span><span class="sxs-lookup"><span data-stu-id="c6bcd-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="c6bcd-105">EF Core ferramentas de tempo de design, como as [migrações](xref:core/managing-schemas/migrations/index) , precisam ser capazes de descobrir e criar uma instância de trabalho de um tipo de `DbContext` para coletar detalhes sobre os tipos de entidade do aplicativo e como eles são mapeados para um esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="c6bcd-106">Esse processo pode ser automático, contanto que a ferramenta possa criar facilmente o `DbContext` de forma que ele seja configurado da mesma forma que seria configurado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="c6bcd-107">Embora qualquer padrão que forneça as informações de configuração necessárias para o `DbContext` possa funcionar em tempo de execução, as ferramentas que exigem o uso de um `DbContext` em tempo de design só podem funcionar com um número limitado de padrões.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="c6bcd-108">Eles são abordados em mais detalhes na seção [criação de contexto em tempo de design](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="c6bcd-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="c6bcd-109">Configurando o DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="c6bcd-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="c6bcd-110">`DbContext` deve ter uma instância de `DbContextOptions` para executar qualquer trabalho.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="c6bcd-111">A instância de `DbContextOptions` transporta informações de configuração, como:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="c6bcd-112">O provedor de banco de dados a ser usado, normalmente selecionado invocando um método como `UseSqlServer` ou `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="c6bcd-113">Esses métodos de extensão exigem o pacote de provedor correspondente, como `Microsoft.EntityFrameworkCore.SqlServer` ou `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="c6bcd-114">Os métodos são definidos no namespace `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="c6bcd-115">Qualquer cadeia de conexão ou identificador de instância de banco de dados necessário, normalmente passado como um argumento para o método de seleção de provedor mencionado acima</span><span class="sxs-lookup"><span data-stu-id="c6bcd-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="c6bcd-116">Qualquer seletor de comportamento opcional em nível de provedor, normalmente também encadeado dentro da chamada para o método de seleção de provedor</span><span class="sxs-lookup"><span data-stu-id="c6bcd-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="c6bcd-117">Qualquer seletor geral de EF Core de comportamento, normalmente encadeado após ou antes do método de seletor de provedor</span><span class="sxs-lookup"><span data-stu-id="c6bcd-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="c6bcd-118">O exemplo a seguir configura o `DbContextOptions` para usar o provedor de SQL Server, uma conexão contida na variável `connectionString`, um tempo limite de comando no nível do provedor e um seletor de comportamento de EF Core que faz todas as consultas executadas no `DbContext` [sem rastreamento](xref:core/querying/tracking#no-tracking-queries) por padrão:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="c6bcd-119">Os métodos de seletor de provedor e outros métodos de seletor de comportamento mencionados acima são métodos de extensão em `DbContextOptions` ou classes de opção específicas do provedor.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="c6bcd-120">Para ter acesso a esses métodos de extensão, talvez seja necessário ter um namespace (geralmente `Microsoft.EntityFrameworkCore`) no escopo e incluir dependências de pacote adicionais no projeto.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="c6bcd-121">O `DbContextOptions` pode ser fornecido para o `DbContext` substituindo o método `OnConfiguring` ou externamente por meio de um argumento de construtor.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="c6bcd-122">Se ambos forem usados, `OnConfiguring` será aplicado por último e poderá substituir as opções fornecidas para o argumento do construtor.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="c6bcd-123">Argumento do Construtor</span><span class="sxs-lookup"><span data-stu-id="c6bcd-123">Constructor argument</span></span>

<span data-ttu-id="c6bcd-124">O construtor pode simplesmente aceitar uma `DbContextOptions` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-124">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="c6bcd-125">O construtor base de DbContext também aceita a versão não genérica do `DbContextOptions`, mas o uso da versão não genérica não é recomendado para aplicativos com vários tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="c6bcd-126">Seu aplicativo agora pode passar o `DbContextOptions` ao instanciar um contexto, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-126">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="c6bcd-127">Onconfiguring</span><span class="sxs-lookup"><span data-stu-id="c6bcd-127">OnConfiguring</span></span>

<span data-ttu-id="c6bcd-128">Você também pode inicializar o `DbContextOptions` dentro do próprio contexto.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-128">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="c6bcd-129">Embora você possa usar essa técnica para a configuração básica, normalmente ainda precisará obter determinados detalhes de configuração de fora, por exemplo, uma cadeia de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-129">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="c6bcd-130">Isso pode ser feito com uma API de configuração ou qualquer outro meio.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-130">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="c6bcd-131">Para inicializar `DbContextOptions` no contexto, substitua o método `OnConfiguring` e chame os métodos no `DbContextOptionsBuilder`fornecido:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-131">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="c6bcd-132">Um aplicativo pode simplesmente instanciar tal contexto sem passar nada para seu construtor:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-132">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="c6bcd-133">Essa abordagem não se presta ao teste, a menos que os testes tenham como destino o banco de dados completo.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-133">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="c6bcd-134">Usando DbContext com injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="c6bcd-134">Using DbContext with dependency injection</span></span>

<span data-ttu-id="c6bcd-135">EF Core dá suporte ao uso de `DbContext` com um contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-135">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="c6bcd-136">O tipo DbContext pode ser adicionado ao contêiner de serviço usando o método `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-136">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="c6bcd-137">`AddDbContext<TContext>` tornará o tipo DbContext, `TContext`e a `DbContextOptions<TContext>` correspondente disponível para injeção no contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-137">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="c6bcd-138">Veja [mais leitura](#more-reading) abaixo para obter informações adicionais sobre injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-138">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="c6bcd-139">Adicionando o `DbContext` à injeção de dependência:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-139">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="c6bcd-140">Isso requer a adição de um [argumento de Construtor](#constructor-argument) ao tipo DbContext que aceita `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-140">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="c6bcd-141">Código de contexto:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-141">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="c6bcd-142">Código do aplicativo (em ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="c6bcd-142">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="c6bcd-143">Código do aplicativo (usando ServiceProvider diretamente, menos comum):</span><span class="sxs-lookup"><span data-stu-id="c6bcd-143">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="c6bcd-144">Evitando problemas de threading de DbContext</span><span class="sxs-lookup"><span data-stu-id="c6bcd-144">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="c6bcd-145">Entity Framework Core não dá suporte a várias operações paralelas sendo executadas na mesma instância de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-145">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="c6bcd-146">Isso inclui a execução paralela de consultas assíncronas e qualquer uso simultâneo explícito de vários threads.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-146">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="c6bcd-147">Portanto, sempre `await` chamadas assíncronas imediatamente ou usar instâncias `DbContext` separadas para operações executadas em paralelo.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-147">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="c6bcd-148">Quando EF Core detectar uma tentativa de usar uma instância de `DbContext` simultaneamente, você verá uma `InvalidOperationException` com uma mensagem como esta:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-148">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="c6bcd-149">Uma segunda operação foi iniciada neste contexto antes da conclusão de uma operação anterior.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-149">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="c6bcd-150">Isso geralmente é causado por threads diferentes usando a mesma instância de DbContext, no entanto, não há garantia de que os membros de instância sejam thread-safe.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-150">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="c6bcd-151">Quando o acesso simultâneo deixa de ser detectado, ele pode resultar em comportamento indefinido, falhas do aplicativo e corrupção de dados.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-151">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="c6bcd-152">Há erros comuns que podem causar inadvertidamente o acesso simultâneo na mesma instância de `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="c6bcd-152">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="c6bcd-153">Esquecendo de aguardar a conclusão de uma operação assíncrona antes de iniciar qualquer outra operação no mesmo DbContext</span><span class="sxs-lookup"><span data-stu-id="c6bcd-153">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="c6bcd-154">Os métodos assíncronos permitem que EF Core iniciem operações que acessam o banco de dados de uma maneira sem bloqueio.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-154">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="c6bcd-155">Mas se um chamador não aguardar a conclusão de um desses métodos e continuar a executar outras operações na `DbContext`, o estado do `DbContext` poderá ser (e provavelmente será) corrompido.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-155">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="c6bcd-156">Sempre Await EF Core métodos assíncronos imediatamente.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-156">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="c6bcd-157">Compartilhando instâncias DbContext implicitamente em vários threads por meio de injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="c6bcd-157">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="c6bcd-158">O método de extensão [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra `DbContext` tipos com um [tempo de vida com escopo](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) definido por padrão.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-158">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="c6bcd-159">Isso é seguro contra problemas de acesso simultâneos em aplicativos ASP.NET Core porque há apenas um thread executando cada solicitação de cliente em um determinado momento, e como cada solicitação Obtém um escopo de injeção de dependência separado (e, portanto, uma instância de `DbContext` separada).</span><span class="sxs-lookup"><span data-stu-id="c6bcd-159">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="c6bcd-160">No entanto, qualquer código que execute explicitamente vários threads em paralelo deve garantir que `DbContext` instâncias nunca sejam acessadas simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-160">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="c6bcd-161">Usando a injeção de dependência, isso pode ser feito registrando o contexto como escopo e criando escopos (usando `IServiceScopeFactory`) para cada thread ou registrando o `DbContext` como transitório (usando a sobrecarga de `AddDbContext` que usa um parâmetro `ServiceLifetime`).</span><span class="sxs-lookup"><span data-stu-id="c6bcd-161">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="c6bcd-162">Mais leitura</span><span class="sxs-lookup"><span data-stu-id="c6bcd-162">More reading</span></span>

- <span data-ttu-id="c6bcd-163">Leia [injeção de dependência](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para saber mais sobre como usar di.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-163">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="c6bcd-164">Leia [teste](testing/index.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c6bcd-164">Read [Testing](testing/index.md) for more information.</span></span>
