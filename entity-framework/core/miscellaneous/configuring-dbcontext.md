---
title: Configurando um DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="8bdb8-102">Configurando um DbContext</span><span class="sxs-lookup"><span data-stu-id="8bdb8-102">Configuring a DbContext</span></span>

<span data-ttu-id="8bdb8-103">Este artigo mostra os padrões para configurar um `DbContext` com `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="8bdb8-104">Opções são usadas principalmente para selecionar e configurar o repositório de dados.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="8bdb8-105">Configurando DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="8bdb8-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="8bdb8-106">`DbContext`deve ter uma instância de `DbContextOptions` para executar.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="8bdb8-107">Isso pode ser configurado por meio da substituição `OnConfiguring`, ou externamente fornecido por meio de um argumento de construtor.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="8bdb8-108">Se ambos forem usadas, `OnConfiguring` é executado nas opções fornecidas, que significa que é aditivo e pode substituir opções fornecidas para o argumento de construtor.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="8bdb8-109">Argumento de construtor</span><span class="sxs-lookup"><span data-stu-id="8bdb8-109">Constructor argument</span></span>

<span data-ttu-id="8bdb8-110">Código de contexto com o construtor</span><span class="sxs-lookup"><span data-stu-id="8bdb8-110">Context code with constructor</span></span>

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
> <span data-ttu-id="8bdb8-111">O construtor base do DbContext também aceita a versão não genérica de `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="8bdb8-112">Não é recomendável usar a versão não genérica para aplicativos com vários tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="8bdb8-113">Código do aplicativo para inicializar a partir do argumento de construtor</span><span class="sxs-lookup"><span data-stu-id="8bdb8-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="8bdb8-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="8bdb8-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="8bdb8-115">`OnConfiguring`ocorre por último e pode substituir opções de DI ou o construtor.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="8bdb8-116">Essa abordagem não se prestam ao teste (a menos que o banco de dados completo de destino).</span><span class="sxs-lookup"><span data-stu-id="8bdb8-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="8bdb8-117">Código de contexto com `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="8bdb8-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="8bdb8-118">Código do aplicativo ao inicializar com `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="8bdb8-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="8bdb8-119">Usando DbContext com injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="8bdb8-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="8bdb8-120">EF oferece suporte ao uso `DbContext` com um contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="8bdb8-121">O tipo DbContext pode ser adicionado ao contêiner de serviço usando `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="8bdb8-122">`AddDbContext`fará com que os dois o tipo DbContext, `TContext`, e `DbContextOptions<TContext>` disponíveis para a injeção do contêiner de serviços.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="8bdb8-123">Consulte [leitura mais](#more-reading) abaixo para obter informações sobre injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="8bdb8-124">Adicionando dbcontext a injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="8bdb8-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="8bdb8-125">Isso requer a adição de um [argumento do construtor](#constructor-argument) para o tipo DbContext que aceita `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="8bdb8-126">Código do contexto:</span><span class="sxs-lookup"><span data-stu-id="8bdb8-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="8bdb8-127">Código do aplicativo (no ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="8bdb8-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="8bdb8-128">Código do aplicativo (usando o provedor de serviços diretamente, menos comuns):</span><span class="sxs-lookup"><span data-stu-id="8bdb8-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="8bdb8-129">Usando o`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="8bdb8-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="8bdb8-130">Como alternativa para as opções acima, você também pode fornecer uma implementação de `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="8bdb8-131">Ferramentas EF podem usar esta fábrica para criar uma instância do seu DbContext.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="8bdb8-132">Isso pode ser necessário para habilitar experiências de tempo de design específicas, como as migrações.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="8bdb8-133">Implemente essa interface para habilitar os serviços de tempo de design para tipos de contexto que não têm um construtor padrão público.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="8bdb8-134">Serviços de tempo de design descobrirá automaticamente implementações dessa interface que estão no mesmo assembly como o contexto de derivada.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="8bdb8-135">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="8bdb8-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="8bdb8-136">Ler mais</span><span class="sxs-lookup"><span data-stu-id="8bdb8-136">More reading</span></span>

* <span data-ttu-id="8bdb8-137">Leitura [para iniciar o ASP.NET Core](../get-started/aspnetcore/index.md) para obter mais informações sobre como usar o EF com ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="8bdb8-138">Leitura [injeção de dependência](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) para saber mais sobre como usar a DI.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="8bdb8-139">Leitura [teste](testing/index.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="8bdb8-139">Read [Testing](testing/index.md) for more information.</span></span>
