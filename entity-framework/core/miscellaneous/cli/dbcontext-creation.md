---
title: "Criação de DbContext de tempo de design - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="ca46e-102">Criação de tempo de design DbContext</span><span class="sxs-lookup"><span data-stu-id="ca46e-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="ca46e-103">Algumas das ferramentas EF comandos requerem uma instância de DbContext a ser criado no design de tempo (por exemplo, ao executar comandos de migrações).</span><span class="sxs-lookup"><span data-stu-id="ca46e-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="ca46e-104">Há várias maneiras que de ferramentas tentam criá-la.</span><span class="sxs-lookup"><span data-stu-id="ca46e-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="ca46e-105">De serviços de aplicativo</span><span class="sxs-lookup"><span data-stu-id="ca46e-105">From application services</span></span>
-------------------------
<span data-ttu-id="ca46e-106">Se seu projeto de inicialização é um aplicativo do ASP.NET Core, as ferramentas de tentar obter o objeto DbContext do provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ca46e-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="ca46e-107">Eles obtiverem-lo chamando `Program.BuildWebHost()` e acessar o `IWebHost.Services` propriedade.</span><span class="sxs-lookup"><span data-stu-id="ca46e-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="ca46e-108">Qualquer DbContext registrado usando `IServiceCollection.AddDbContext<TContext>()` pode ser encontrado e criado dessa maneira.</span><span class="sxs-lookup"><span data-stu-id="ca46e-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="ca46e-109">Esse padrão foi [introduzido no ASP.NET 2.0 de núcleo][1]</span><span class="sxs-lookup"><span data-stu-id="ca46e-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="ca46e-110">Usando o construtor padrão</span><span class="sxs-lookup"><span data-stu-id="ca46e-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="ca46e-111">Se o DbContext não pode ser obtido do provedor de serviço de aplicativo, as ferramentas de aparência para o tipo DbContext dentro do projeto.</span><span class="sxs-lookup"><span data-stu-id="ca46e-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="ca46e-112">Tente criá-la usando o construtor padrão.</span><span class="sxs-lookup"><span data-stu-id="ca46e-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="ca46e-113">Uma fábrica de tempo de design</span><span class="sxs-lookup"><span data-stu-id="ca46e-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="ca46e-114">Você também pode informar as ferramentas como criar seu DbContext implementando `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="ca46e-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="ca46e-115">Se uma classe que implemente essa interface foi encontrada no seu projeto, as ferramentas de ignoram outras maneiras de criar o DbContext.</span><span class="sxs-lookup"><span data-stu-id="ca46e-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="ca46e-116">Eles sempre usam a fábrica em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="ca46e-116">They always use the factory at design time.</span></span> <span data-ttu-id="ca46e-117">Uma fábrica é especialmente útil se você precisa configurar o DbContext diferente para tempo de design que em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ca46e-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

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

> [!NOTE]
> <span data-ttu-id="ca46e-118">O `args` parâmetro é usado no momento.</span><span class="sxs-lookup"><span data-stu-id="ca46e-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="ca46e-119">Não há [um problema] [ 2] a capacidade de especificar argumentos de tempo de design de ferramentas de controle.</span><span class="sxs-lookup"><span data-stu-id="ca46e-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
