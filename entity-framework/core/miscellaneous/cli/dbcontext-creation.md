---
title: Criação de DbContext de tempo de design - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202478"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="e8883-102">Criação de tempo de design DbContext</span><span class="sxs-lookup"><span data-stu-id="e8883-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="e8883-103">Alguns dos comandos EF principais ferramentas (por exemplo, o [migrações] [ 1] comandos) exigem um derivado `DbContext` instância a ser criada em tempo de design para obter detalhes sobre o aplicativo tipos de entidade e como eles serão mapeados para um esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e8883-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="e8883-104">Na maioria dos casos, é desejável que o `DbContext` criado, portanto, está configurado de maneira semelhante a como seria [configurado em tempo de execução][2].</span><span class="sxs-lookup"><span data-stu-id="e8883-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="e8883-105">Há várias maneiras que as ferramentas de tentarem criar o `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="e8883-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="e8883-106">De serviços de aplicativo</span><span class="sxs-lookup"><span data-stu-id="e8883-106">From application services</span></span>
-------------------------
<span data-ttu-id="e8883-107">Se seu projeto de inicialização é um aplicativo do ASP.NET Core, as ferramentas de tentar obter o objeto DbContext do provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e8883-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="e8883-108">A ferramenta primeiro tenta obter o provedor de serviços invocando `Program.BuildWebHost()` e acessar o `IWebHost.Services` propriedade.</span><span class="sxs-lookup"><span data-stu-id="e8883-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="e8883-109">Quando você cria um novo aplicativo ASP.NET Core 2.0, esse gancho está incluído por padrão.</span><span class="sxs-lookup"><span data-stu-id="e8883-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="e8883-110">Em versões anteriores do EF Core e ASP.NET Core, as ferramentas tentam invocar `Startup.ConfigureServices` diretamente para obter o provedor de serviços do aplicativo, mas esse padrão não funciona corretamente em aplicativos ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="e8883-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="e8883-111">Se você estiver atualizando um aplicativo ASP.NET Core 1. x 2.0, você pode [modificar sua `Program` classe seguir o padrão de novos][3].</span><span class="sxs-lookup"><span data-stu-id="e8883-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="e8883-112">O `DbContext` em si e as dependências em seu construtor precisam ser registrados como serviços no provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e8883-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="e8883-113">Isso pode ser facilmente conseguido tendo [um construtor no `DbContext` que usa uma instância do `DbContextOptions<TContext>` como um argumento] [ 4] e usando o [ `AddDbContext<TContext>` método] [5].</span><span class="sxs-lookup"><span data-stu-id="e8883-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="e8883-114">Usar um construtor sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="e8883-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="e8883-115">Se o DbContext não pode ser obtido do provedor de serviço de aplicativo, as ferramentas procure derivada `DbContext` tipo dentro do projeto.</span><span class="sxs-lookup"><span data-stu-id="e8883-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="e8883-116">Em seguida, tente criar uma instância usando um construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e8883-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="e8883-117">Isso pode ser o construtor padrão se o `DbContext` é configurado usando o [ `OnConfiguring` ] [ 6] método.</span><span class="sxs-lookup"><span data-stu-id="e8883-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="e8883-118">Uma fábrica de tempo de design</span><span class="sxs-lookup"><span data-stu-id="e8883-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="e8883-119">Você também pode informar as ferramentas como criar seu DbContext Implementando o `IDesignTimeDbContextFactory<TContext>` interface: se uma classe que implemente essa interface é encontrada no mesmo projeto como derivada `DbContext` ou no projeto de inicialização do aplicativo, as ferramentas de ignoram outras maneiras de criar o DbContext e usar a fábrica de tempo de design em vez disso.</span><span class="sxs-lookup"><span data-stu-id="e8883-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="e8883-120">O `args` parâmetro é usado no momento.</span><span class="sxs-lookup"><span data-stu-id="e8883-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="e8883-121">Não há [um problema] [ 7] a capacidade de especificar argumentos de tempo de design de ferramentas de controle.</span><span class="sxs-lookup"><span data-stu-id="e8883-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="e8883-122">Uma fábrica de tempo de design pode ser especialmente útil se você precisa configurar o DbContext diferente para tempo de design que em tempo de execução, se o `DbContext` leva construtor parâmetros adicionais não estão registrados no DI, se você não estiver usando DI em todos os, ou se alguns motivo você preferir não tem um `BuildWebHost` método no seu aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8883-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="e8883-123">`Main` classe.</span><span class="sxs-lookup"><span data-stu-id="e8883-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
