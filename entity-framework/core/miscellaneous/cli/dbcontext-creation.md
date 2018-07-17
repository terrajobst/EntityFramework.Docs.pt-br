---
title: Criação de DbContext no tempo de design - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 7c16017d3b97d115841050fe6ac0fdbeb5e71d94
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067525"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="c7384-102">Criação de DbContext no tempo de design</span><span class="sxs-lookup"><span data-stu-id="c7384-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="c7384-103">Alguns dos comandos de ferramentas do EF Core (por exemplo, o [migrações] [ 1] comandos) exigem um derivada `DbContext` instância a ser criada em tempo de design para obter detalhes sobre o aplicativo tipos de entidade e como eles são mapeados para um esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c7384-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="c7384-104">Na maioria dos casos, é desejável que o `DbContext` criado, portanto, está configurado de forma semelhante a como ele seria [configurado no tempo de execução][2].</span><span class="sxs-lookup"><span data-stu-id="c7384-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="c7384-105">Há várias maneiras, as ferramentas de tentam criar o `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="c7384-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="c7384-106">Dos serviços de aplicativo</span><span class="sxs-lookup"><span data-stu-id="c7384-106">From application services</span></span>
-------------------------
<span data-ttu-id="c7384-107">Se seu projeto de inicialização é um aplicativo ASP.NET Core, as ferramentas de tentar obter o objeto DbContext do provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c7384-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="c7384-108">A ferramenta primeiro tenta obter o provedor de serviço invocando `Program.BuildWebHost()` e acessar o `IWebHost.Services` propriedade.</span><span class="sxs-lookup"><span data-stu-id="c7384-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="c7384-109">Quando você cria um novo aplicativo ASP.NET Core 2.0, esse gancho é incluído por padrão.</span><span class="sxs-lookup"><span data-stu-id="c7384-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="c7384-110">Nas versões anteriores do EF Core e ASP.NET Core, as ferramentas de tentarem invocar `Startup.ConfigureServices` diretamente para obter o provedor de serviços do aplicativo, mas esse padrão não funciona corretamente em aplicativos ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c7384-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="c7384-111">Se você estiver atualizando um aplicativo ASP.NET Core 1.x 2.0, você poderá [modificar sua `Program` classe seguir o padrão de novos][3].</span><span class="sxs-lookup"><span data-stu-id="c7384-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="c7384-112">O `DbContext` em si e todas as dependências em seu construtor precisam ser registrados como serviços no provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c7384-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="c7384-113">Isso pode ser feito facilmente tendo [um construtor na `DbContext` que usa uma instância do `DbContextOptions<TContext>` como um argumento] [ 4] e usando o [ `AddDbContext<TContext>` método] [5].</span><span class="sxs-lookup"><span data-stu-id="c7384-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="c7384-114">Usando um construtor sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="c7384-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="c7384-115">Se o DbContext não pode ser obtido do provedor de serviço de aplicativo, as ferramentas procure derivado `DbContext` tipo dentro do projeto.</span><span class="sxs-lookup"><span data-stu-id="c7384-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="c7384-116">Em seguida, tente criar uma instância usando um construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c7384-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="c7384-117">Isso pode ser o construtor padrão se o `DbContext` é configurado usando o [ `OnConfiguring` ] [ 6] método.</span><span class="sxs-lookup"><span data-stu-id="c7384-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="c7384-118">De uma fábrica de tempo de design</span><span class="sxs-lookup"><span data-stu-id="c7384-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="c7384-119">Você também pode informar as ferramentas de como criar o DbContext, Implementando a `IDesignTimeDbContextFactory<TContext>` interface: se uma classe que implementa essa interface for encontrada no mesmo projeto derivado `DbContext` ou no projeto de inicialização do aplicativo, as ferramentas de ignoram outras maneiras de criar o DbContext e o uso a fábrica de tempo de design em vez disso.</span><span class="sxs-lookup"><span data-stu-id="c7384-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> <span data-ttu-id="c7384-120">O `args` parâmetro está sendo utilizado.</span><span class="sxs-lookup"><span data-stu-id="c7384-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="c7384-121">Não há [um problema] [ 7] a capacidade de especificar argumentos de tempo de design das ferramentas de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="c7384-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="c7384-122">Uma fábrica de tempo de design pode ser especialmente útil se você precisar configurar o DbContext de forma diferente para o tempo de design que em tempo de execução, se o `DbContext` leva construtor parâmetros adicionais não são registrados na DI, se você não estiver usando injeção de dependência em todos os ou se para alguns motivo você prefere não ter uma `BuildWebHost` método do seu aplicativo ASP.NET Core `Main` classe.</span><span class="sxs-lookup"><span data-stu-id="c7384-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
