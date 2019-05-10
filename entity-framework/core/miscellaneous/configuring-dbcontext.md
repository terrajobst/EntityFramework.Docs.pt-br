---
title: Configurar um DbContext – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 316d363d4a1b8a909efc1c32b492280c0d16cb4e
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405216"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="4ddb5-102">Configurar um DbContext</span><span class="sxs-lookup"><span data-stu-id="4ddb5-102">Configuring a DbContext</span></span>

<span data-ttu-id="4ddb5-103">Este artigo mostra os padrões básicos para a configuração de um `DbContext` por meio de um `DbContextOptions` para se conectar a um banco de dados usando um provedor específico do EF Core e comportamentos opcionais.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="4ddb5-104">Configuração do tempo de design DbContext</span><span class="sxs-lookup"><span data-stu-id="4ddb5-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="4ddb5-105">Ferramentas de tempo de design do EF Core, como [migrações](xref:core/managing-schemas/migrations/index) precisa ser capaz de detectar e criar uma instância de trabalho de um `DbContext` tipo para obter detalhes sobre os tipos de entidade e como eles são mapeados para um esquema de banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="4ddb5-106">Esse processo pode ser automático, desde que a ferramenta pode criar facilmente o `DbContext` de tal forma que ele será configurado da mesma forma como deve ser configurado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="4ddb5-107">Embora qualquer padrão que fornece informações de configuração necessárias para o `DbContext` pode funcionar em tempo de execução, as ferramentas que exigem o uso de um `DbContext` em tempo de design só pode trabalhar com um número limitado de padrões.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="4ddb5-108">Isso é abordado em mais detalhes os [criação de contexto de tempo de Design](xref:core/miscellaneous/cli/dbcontext-creation) seção.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="4ddb5-109">Configurando DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="4ddb5-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="4ddb5-110">`DbContext` deve ter uma instância de `DbContextOptions` para realizar qualquer trabalho.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="4ddb5-111">O `DbContextOptions` instância carrega as informações de configuração, como:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="4ddb5-112">O provedor de banco de dados a ser usado, normalmente selecionado ao invocar um método como `UseSqlServer` ou `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="4ddb5-113">Esses métodos de extensão exigem o pacote correspondente do provedor, como `Microsoft.EntityFrameworkCore.SqlServer` ou `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="4ddb5-114">Os métodos são definidos na `Microsoft.EntityFrameworkCore` namespace.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="4ddb5-115">Qualquer cadeia de caracteres de conexão necessárias ou o identificador da instância do banco de dados, normalmente passado como um argumento para o método de seleção de provedor mencionado acima</span><span class="sxs-lookup"><span data-stu-id="4ddb5-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="4ddb5-116">Seletores qualquer comportamento opcional do nível de provedor, normalmente também encadeadas dentro da chamada para o método de seleção de provedor</span><span class="sxs-lookup"><span data-stu-id="4ddb5-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="4ddb5-117">Qualquer gerais seletores de comportamento do EF Core, encadeados normalmente depois ou antes que o método de seletor de provedor</span><span class="sxs-lookup"><span data-stu-id="4ddb5-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="4ddb5-118">O exemplo a seguir configura a `DbContextOptions` para usar o provedor do SQL Server, uma conexão contidos em de `connectionString` variável, um tempo limite de comando de nível de provedor e um seletor de comportamento do EF Core que faz com que todas as consultas executadas no `DbContext` [sem controle](xref:core/querying/tracking#no-tracking-queries) por padrão:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="4ddb5-119">Métodos de seletor de provedor e outros métodos de seletor de comportamento mencionados acima são métodos de extensão em `DbContextOptions` ou classes de opção específico do provedor.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="4ddb5-120">Para ter acesso a esses métodos de extensão que você talvez precise ter um namespace (normalmente `Microsoft.EntityFrameworkCore`) no escopo e incluir as dependências de pacote adicionais no projeto.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="4ddb5-121">O `DbContextOptions` pode ser fornecido para o `DbContext` , substituindo o `OnConfiguring` método ou externamente por meio de um argumento do construtor.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="4ddb5-122">Se ambos forem usadas, `OnConfiguring` é aplicada por último e podem substituir as opções fornecidas para o argumento do construtor.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="4ddb5-123">Argumento de construtor</span><span class="sxs-lookup"><span data-stu-id="4ddb5-123">Constructor argument</span></span>

<span data-ttu-id="4ddb5-124">Código do contexto com o construtor:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="4ddb5-125">O construtor de base do DbContext também aceita a versão não genérica de `DbContextOptions`, mas não é recomendável usar a versão não genérica para aplicativos com vários tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="4ddb5-126">Código do aplicativo para inicializar a partir do argumento do construtor:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="4ddb5-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="4ddb5-127">OnConfiguring</span></span>

<span data-ttu-id="4ddb5-128">Código de contexto com `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="4ddb5-129">Código do aplicativo para inicializar uma `DbContext` que usa `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="4ddb5-130">Essa abordagem não se prestam para teste, a menos que os testes de banco de dados completo de destino.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="4ddb5-131">Usando o DbContext com injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="4ddb5-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="4ddb5-132">O EF Core dá suporte ao uso `DbContext` com um contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="4ddb5-133">O tipo DbContext pode ser adicionado ao contêiner de serviço usando o `AddDbContext<TContext>` método.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="4ddb5-134">`AddDbContext<TContext>` fará os dois é o tipo DbContext, `TContext`e o correspondente `DbContextOptions<TContext>` disponível para injeção de contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="4ddb5-135">Ver [ler mais](#more-reading) abaixo para obter mais informações sobre injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="4ddb5-136">Adicionando o `Dbcontext` a injeção de dependência:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="4ddb5-137">Isso requer a adição de um [argumento do construtor](#constructor-argument) para o tipo de DbContext que aceita `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="4ddb5-138">Código do contexto:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="4ddb5-139">Código do aplicativo (no ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="4ddb5-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="4ddb5-140">Código do aplicativo (usando o provedor de serviços diretamente, menos comum):</span><span class="sxs-lookup"><span data-stu-id="4ddb5-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="4ddb5-141">Evitando problemas de threading de DbContext</span><span class="sxs-lookup"><span data-stu-id="4ddb5-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="4ddb5-142">O Entity Framework Core não oferece suporte a várias operações simultâneas que estão sendo executadas no mesmo `DbContext` instância.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="4ddb5-143">Isso inclui a execução paralela de consultas de async e qualquer uso explícito de simultâneo de vários threads.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-143">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="4ddb5-144">Portanto, sempre `await` assíncrono chama imediatamente ou usar separado `DbContext` instâncias para operações executadas em paralelo.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-144">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="4ddb5-145">Quando o EF Core detecta uma tentativa de usar um `DbContext` da instância ao mesmo tempo, você verá um `InvalidOperationException` com uma mensagem como esta:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-145">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="4ddb5-146">Uma segunda operação iniciada nesse contexto antes de uma operação anterior foi concluída.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-146">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="4ddb5-147">Isso geralmente é causado por threads diferentes usando a mesma instância do DbContext, no entanto o membros de instância não são garantidos ser thread-safe.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-147">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="4ddb5-148">Quando o acesso simultâneo sendo detectado, ele pode resultar em comportamento indefinido, panes de aplicativos e dados corrompidos.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-148">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="4ddb5-149">Há erros comuns que podem fazer o inadvernetly access simultâneas na mesma `DbContext` instância:</span><span class="sxs-lookup"><span data-stu-id="4ddb5-149">There are common mistakes that can inadvernetly cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="4ddb5-150">Esquecer de aguardar a conclusão de uma operação assíncrona antes de iniciar qualquer outra operação no mesmo DbContext</span><span class="sxs-lookup"><span data-stu-id="4ddb5-150">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="4ddb5-151">Métodos assíncronos permitem que o EF Core iniciar as operações que acessam o banco de dados de uma maneira sem bloqueio.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-151">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="4ddb5-152">Mas, se um chamador não aguardar a conclusão de um desses métodos e continua a executar outras operações de `DbContext`, o estado do `DbContext` pode ser (e será muito provável) corrompido.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-152">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="4ddb5-153">Sempre aguardar os métodos assíncronos do EF Core imediatamente.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-153">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="4ddb5-154">Instâncias de DbContext implicitamente de compartilhamento entre vários threads por meio da injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="4ddb5-154">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="4ddb5-155">O [ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra o método de extensão `DbContext` tipos com uma [tempo de vida com escopo](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) por padrão.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-155">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="4ddb5-156">Isso é seguro contra problemas de acesso simultâneo em aplicativos ASP.NET Core porque há apenas um thread de execução de cada solicitação de cliente em um determinado momento, e como cada solicitação recebe um escopo de injeção de dependência separados (e, portanto, um separado `DbContext` instância).</span><span class="sxs-lookup"><span data-stu-id="4ddb5-156">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="4ddb5-157">No entanto a qualquer código que executa explicitamente vários threads em paralelo deve garantir que `DbContext` instâncias não são nunca accesed simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-157">However any code that explicitly executes multiple threads in paralell should ensure that `DbContext` instances aren't ever accesed concurrently.</span></span>

<span data-ttu-id="4ddb5-158">Usando a injeção de dependência, isso pode ser obtido ao registrar o contexto como escopos de criação e escopo (usando `IServiceScopeFactory`) para cada thread ou registrando-se a `DbContext` como transitório (usando a sobrecarga de `AddDbContext` que utiliza um `ServiceLifetime` parâmetro).</span><span class="sxs-lookup"><span data-stu-id="4ddb5-158">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="4ddb5-159">Ler mais</span><span class="sxs-lookup"><span data-stu-id="4ddb5-159">More reading</span></span>

* <span data-ttu-id="4ddb5-160">Leia [Introdução ao ASP.NET Core](../get-started/aspnetcore/index.md) para obter mais informações sobre como usar o EF com ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-160">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="4ddb5-161">Leia [injeção de dependência](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para saber mais sobre como usar a DI.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-161">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="4ddb5-162">Leia [testes](testing/index.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="4ddb5-162">Read [Testing](testing/index.md) for more information.</span></span>
