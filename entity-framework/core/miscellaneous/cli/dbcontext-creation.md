---
title: Criação de DbContext de tempo de design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416738"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="c7c62-102">Criação de DbContext no tempo de design</span><span class="sxs-lookup"><span data-stu-id="c7c62-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="c7c62-103">Alguns dos comandos de ferramentas de EF Core (por exemplo, os comandos de [migração][1] ) exigem que uma instância de `DbContext` derivada seja criada em tempo de design para coletar detalhes sobre os tipos de entidade do aplicativo e como eles são mapeados para um esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c7c62-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="c7c62-104">Na maioria dos casos, é desejável que a `DbContext` criada assim seja configurada de forma semelhante a como ela seria [configurada em tempo de execução][2].</span><span class="sxs-lookup"><span data-stu-id="c7c62-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="c7c62-105">Há várias maneiras pelas quais as ferramentas tentam criar o `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="c7c62-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="c7c62-106">Dos serviços de aplicativos</span><span class="sxs-lookup"><span data-stu-id="c7c62-106">From application services</span></span>

<span data-ttu-id="c7c62-107">Se o seu projeto de inicialização usar o host do [ASP.NET Core Web][3] ou o [host genérico do .NET Core][4], as ferramentas tentarão obter o objeto DbContext do provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c7c62-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="c7c62-108">As ferramentas primeiro tentam obter o provedor de serviços invocando `Program.CreateHostBuilder()`, chamando `Build()`e acessando a propriedade `Services`.</span><span class="sxs-lookup"><span data-stu-id="c7c62-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> <span data-ttu-id="c7c62-109">Quando você cria um novo aplicativo ASP.NET Core, esse gancho é incluído por padrão.</span><span class="sxs-lookup"><span data-stu-id="c7c62-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="c7c62-110">O `DbContext` em si e quaisquer dependências em seu Construtor precisam ser registrados como serviços no provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c7c62-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="c7c62-111">Isso pode ser facilmente obtido com [um construtor na `DbContext` que usa uma instância de `DbContextOptions<TContext>` como um argumento][5] e usando o [método`AddDbContext<TContext>`][6].</span><span class="sxs-lookup"><span data-stu-id="c7c62-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="c7c62-112">Usando um construtor sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="c7c62-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="c7c62-113">Se DbContext não puder ser obtido do provedor de serviços de aplicativo, as ferramentas procurarão o tipo de `DbContext` derivado dentro do projeto.</span><span class="sxs-lookup"><span data-stu-id="c7c62-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="c7c62-114">Em seguida, eles tentam criar uma instância usando um construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c7c62-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="c7c62-115">Esse pode ser o construtor padrão se a `DbContext` for configurada usando o método [`OnConfiguring`][7] .</span><span class="sxs-lookup"><span data-stu-id="c7c62-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="c7c62-116">De uma fábrica de tempo de design</span><span class="sxs-lookup"><span data-stu-id="c7c62-116">From a design-time factory</span></span>

<span data-ttu-id="c7c62-117">Você também pode informar as ferramentas sobre como criar seu DbContext implementando a interface `IDesignTimeDbContextFactory<TContext>`: se uma classe que implementa essa interface for encontrada no mesmo projeto que o `DbContext` derivado ou no projeto de inicialização do aplicativo, as ferramentas ignorarão as outras maneiras de criar o DbContext e usar a fábrica de tempo de design em vez disso.</span><span class="sxs-lookup"><span data-stu-id="c7c62-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> <span data-ttu-id="c7c62-118">O parâmetro `args` não é usado no momento.</span><span class="sxs-lookup"><span data-stu-id="c7c62-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="c7c62-119">Há [um problema][8] que controla a capacidade de especificar argumentos de tempo de design a partir das ferramentas.</span><span class="sxs-lookup"><span data-stu-id="c7c62-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="c7c62-120">Uma fábrica de tempo de design pode ser especialmente útil se você precisar configurar o DbContext de forma diferente para o tempo de design do que no tempo de execução, se o construtor de `DbContext` usar parâmetros adicionais não estiver registrado em DI, se você não estiver usando DI, ou se por algum motivo você preferir não ter um método `BuildWebHost` na classe `Main` do seu aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7c62-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
