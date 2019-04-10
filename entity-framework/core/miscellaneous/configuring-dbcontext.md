---
title: Configurar um DbContext – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 0350b25d0d0efe05df7cb9e93a3f4ae2d864fd63
ms.sourcegitcommit: 47e0a66a136e743a815d099d2bee5f0da1a068c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59363931"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="03cd6-102">Configurar um DbContext</span><span class="sxs-lookup"><span data-stu-id="03cd6-102">Configuring a DbContext</span></span>

<span data-ttu-id="03cd6-103">Este artigo mostra os padrões básicos para a configuração de um `DbContext` por meio de um `DbContextOptions` para se conectar a um banco de dados usando um provedor específico do EF Core e comportamentos opcionais.</span><span class="sxs-lookup"><span data-stu-id="03cd6-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="03cd6-104">Configuração do tempo de design DbContext</span><span class="sxs-lookup"><span data-stu-id="03cd6-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="03cd6-105">Ferramentas de tempo de design do EF Core, como [migrações](xref:core/managing-schemas/migrations/index) precisa ser capaz de detectar e criar uma instância de trabalho de um `DbContext` tipo para obter detalhes sobre os tipos de entidade e como eles são mapeados para um esquema de banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="03cd6-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="03cd6-106">Esse processo pode ser automático, desde que a ferramenta pode criar facilmente o `DbContext` de tal forma que ele será configurado da mesma forma como deve ser configurado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="03cd6-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="03cd6-107">Embora qualquer padrão que fornece informações de configuração necessárias para o `DbContext` pode funcionar em tempo de execução, as ferramentas que exigem o uso de um `DbContext` em tempo de design só pode trabalhar com um número limitado de padrões.</span><span class="sxs-lookup"><span data-stu-id="03cd6-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="03cd6-108">Isso é abordado em mais detalhes os [criação de contexto de tempo de Design](xref:core/miscellaneous/cli/dbcontext-creation) seção.</span><span class="sxs-lookup"><span data-stu-id="03cd6-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="03cd6-109">Configurando DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="03cd6-109">Configuring DbContextOptions</span></span>

`DbContext` <span data-ttu-id="03cd6-110">deve ter uma instância de `DbContextOptions` para realizar qualquer trabalho.</span><span class="sxs-lookup"><span data-stu-id="03cd6-110">must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="03cd6-111">O `DbContextOptions` instância carrega as informações de configuração, como:</span><span class="sxs-lookup"><span data-stu-id="03cd6-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="03cd6-112">O provedor de banco de dados a ser usado, normalmente selecionado ao invocar um método como `UseSqlServer` ou `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="03cd6-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="03cd6-113">Esses métodos de extensão exigem o pacote correspondente do provedor, como `Microsoft.EntityFrameworkCore.SqlServer` ou `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="03cd6-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="03cd6-114">Os métodos são definidos na `Microsoft.EntityFrameworkCore` namespace.</span><span class="sxs-lookup"><span data-stu-id="03cd6-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="03cd6-115">Qualquer cadeia de caracteres de conexão necessárias ou o identificador da instância do banco de dados, normalmente passado como um argumento para o método de seleção de provedor mencionado acima</span><span class="sxs-lookup"><span data-stu-id="03cd6-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="03cd6-116">Seletores qualquer comportamento opcional do nível de provedor, normalmente também encadeadas dentro da chamada para o método de seleção de provedor</span><span class="sxs-lookup"><span data-stu-id="03cd6-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="03cd6-117">Qualquer gerais seletores de comportamento do EF Core, encadeados normalmente depois ou antes que o método de seletor de provedor</span><span class="sxs-lookup"><span data-stu-id="03cd6-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="03cd6-118">O exemplo a seguir configura a `DbContextOptions` para usar o provedor do SQL Server, uma conexão contidos em de `connectionString` variável, um tempo limite de comando de nível de provedor e um seletor de comportamento do EF Core que faz com que todas as consultas executadas no `DbContext` [sem controle](xref:core/querying/tracking#no-tracking-queries) por padrão:</span><span class="sxs-lookup"><span data-stu-id="03cd6-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="03cd6-119">Métodos de seletor de provedor e outros métodos de seletor de comportamento mencionados acima são métodos de extensão em `DbContextOptions` ou classes de opção específico do provedor.</span><span class="sxs-lookup"><span data-stu-id="03cd6-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="03cd6-120">Para ter acesso a esses métodos de extensão que você talvez precise ter um namespace (normalmente `Microsoft.EntityFrameworkCore`) no escopo e incluir as dependências de pacote adicionais no projeto.</span><span class="sxs-lookup"><span data-stu-id="03cd6-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="03cd6-121">O `DbContextOptions` pode ser fornecido para o `DbContext` , substituindo o `OnConfiguring` método ou externamente por meio de um argumento do construtor.</span><span class="sxs-lookup"><span data-stu-id="03cd6-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="03cd6-122">Se ambos forem usadas, `OnConfiguring` é aplicada por último e podem substituir as opções fornecidas para o argumento do construtor.</span><span class="sxs-lookup"><span data-stu-id="03cd6-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="03cd6-123">Argumento de construtor</span><span class="sxs-lookup"><span data-stu-id="03cd6-123">Constructor argument</span></span>

<span data-ttu-id="03cd6-124">Código do contexto com o construtor:</span><span class="sxs-lookup"><span data-stu-id="03cd6-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="03cd6-125">O construtor de base do DbContext também aceita a versão não genérica de `DbContextOptions`, mas não é recomendável usar a versão não genérica para aplicativos com vários tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="03cd6-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="03cd6-126">Código do aplicativo para inicializar a partir do argumento do construtor:</span><span class="sxs-lookup"><span data-stu-id="03cd6-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="03cd6-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="03cd6-127">OnConfiguring</span></span>

<span data-ttu-id="03cd6-128">Código de contexto com `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="03cd6-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="03cd6-129">Código do aplicativo para inicializar uma `DbContext` que usa `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="03cd6-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="03cd6-130">Essa abordagem não se prestam para teste, a menos que os testes de banco de dados completo de destino.</span><span class="sxs-lookup"><span data-stu-id="03cd6-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="03cd6-131">Usando o DbContext com injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="03cd6-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="03cd6-132">O EF Core dá suporte ao uso `DbContext` com um contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="03cd6-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="03cd6-133">O tipo DbContext pode ser adicionado ao contêiner de serviço usando o `AddDbContext<TContext>` método.</span><span class="sxs-lookup"><span data-stu-id="03cd6-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

`AddDbContext<TContext>` <span data-ttu-id="03cd6-134">fará os dois é o tipo DbContext, `TContext`e o correspondente `DbContextOptions<TContext>` disponível para injeção de contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="03cd6-134">will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="03cd6-135">Ver [ler mais](#more-reading) abaixo para obter mais informações sobre injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="03cd6-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="03cd6-136">Adicionando o `Dbcontext` a injeção de dependência:</span><span class="sxs-lookup"><span data-stu-id="03cd6-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="03cd6-137">Isso requer a adição de um [argumento do construtor](#constructor-argument) para o tipo de DbContext que aceita `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="03cd6-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="03cd6-138">Código do contexto:</span><span class="sxs-lookup"><span data-stu-id="03cd6-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="03cd6-139">Código do aplicativo (no ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="03cd6-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="03cd6-140">Código do aplicativo (usando o provedor de serviços diretamente, menos comum):</span><span class="sxs-lookup"><span data-stu-id="03cd6-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="03cd6-141">Evitando problemas de threading de DbContext</span><span class="sxs-lookup"><span data-stu-id="03cd6-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="03cd6-142">O Entity Framework Core não oferece suporte a várias operações simultâneas que estão sendo executadas no mesmo `DbContext` instância.</span><span class="sxs-lookup"><span data-stu-id="03cd6-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="03cd6-143">Acesso simultâneo pode resultar em comportamento indefinido, panes de aplicativos e dados corrompidos.</span><span class="sxs-lookup"><span data-stu-id="03cd6-143">Concurrent access can result in undefined behavior, application crashes and data corruption.</span></span> <span data-ttu-id="03cd6-144">Por isso é importante usar sempre separar `DbContext` instâncias para operações executadas em paralelo.</span><span class="sxs-lookup"><span data-stu-id="03cd6-144">Because of this it's important to always use separate `DbContext` instances for operations that execute in parallel.</span></span> 

<span data-ttu-id="03cd6-145">Há erros comuns que podem fazer o inadvernetly access simultâneas na mesma `DbContext` instância:</span><span class="sxs-lookup"><span data-stu-id="03cd6-145">There are common mistakes that can inadvernetly cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="03cd6-146">Esquecer de aguardar a conclusão de uma operação assíncrona antes de iniciar qualquer outra operação no mesmo DbContext</span><span class="sxs-lookup"><span data-stu-id="03cd6-146">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="03cd6-147">Métodos assíncronos permitem que o EF Core iniciar as operações que acessam o banco de dados de uma maneira sem bloqueio.</span><span class="sxs-lookup"><span data-stu-id="03cd6-147">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="03cd6-148">Mas, se um chamador não aguardar a conclusão de um desses métodos e continua a executar outras operações de `DbContext`, o estado do `DbContext` pode ser (e será muito provável) corrompido.</span><span class="sxs-lookup"><span data-stu-id="03cd6-148">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="03cd6-149">Sempre aguardar os métodos assíncronos do EF Core imediatamente.</span><span class="sxs-lookup"><span data-stu-id="03cd6-149">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="03cd6-150">Instâncias de DbContext implicitamente de compartilhamento entre vários threads por meio da injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="03cd6-150">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="03cd6-151">O [ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra o método de extensão `DbContext` tipos com uma [tempo de vida com escopo](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) por padrão.</span><span class="sxs-lookup"><span data-stu-id="03cd6-151">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="03cd6-152">Isso é seguro contra problemas de acesso simultâneo em aplicativos ASP.NET Core porque há apenas um thread de execução de cada solicitação de cliente em um determinado momento, e como cada solicitação recebe um escopo de injeção de dependência separados (e, portanto, um separado `DbContext` instância).</span><span class="sxs-lookup"><span data-stu-id="03cd6-152">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="03cd6-153">No entanto a qualquer código que executa explicitamente vários threads em paralelo deve garantir que `DbContext` instâncias não são nunca accesed simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="03cd6-153">However any code that explicitly executes multiple threads in paralell should ensure that `DbContext` instances aren't ever accesed concurrently.</span></span>

<span data-ttu-id="03cd6-154">Usando a injeção de dependência, isso pode ser obtido ao registrar o contexto como escopos de criação e escopo (usando `IServiceScopeFactory`) para cada thread ou registrando-se a `DbContext` como transitório (usando a sobrecarga de `AddDbContext` que utiliza um `ServiceLifetime` parâmetro).</span><span class="sxs-lookup"><span data-stu-id="03cd6-154">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="03cd6-155">Ler mais</span><span class="sxs-lookup"><span data-stu-id="03cd6-155">More reading</span></span>

* <span data-ttu-id="03cd6-156">Leia [Introdução ao ASP.NET Core](../get-started/aspnetcore/index.md) para obter mais informações sobre como usar o EF com ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="03cd6-156">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="03cd6-157">Leia [injeção de dependência](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para saber mais sobre como usar a DI.</span><span class="sxs-lookup"><span data-stu-id="03cd6-157">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="03cd6-158">Leia [testes](testing/index.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="03cd6-158">Read [Testing](testing/index.md) for more information.</span></span>
