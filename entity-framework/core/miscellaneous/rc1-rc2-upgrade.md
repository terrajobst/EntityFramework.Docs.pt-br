---
title: "A atualização do EF Core 1.0 RC1 para RC2 - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="542f6-102">A atualização do EF Core 1.0 RC1 para 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="542f6-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="542f6-103">Este artigo fornece diretrizes para mover um aplicativo compilado com os pacotes RC1 para RC2.</span><span class="sxs-lookup"><span data-stu-id="542f6-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="542f6-104">Versões e nomes de pacote</span><span class="sxs-lookup"><span data-stu-id="542f6-104">Package Names and Versions</span></span>

<span data-ttu-id="542f6-105">Entre RC1 e RC2, é alterado de "Entity Framework 7" a "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="542f6-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="542f6-106">Você pode ler mais sobre os motivos para que a alteração no [esta postagem por Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="542f6-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="542f6-107">Devido a essa alteração, os nomes de pacote é alterado de `EntityFramework.*` para `Microsoft.EntityFrameworkCore.*` e nossas versões de `7.0.0-rc1-final` para `1.0.0-rc2-final` (ou `1.0.0-preview1-final` para ferramentas).</span><span class="sxs-lookup"><span data-stu-id="542f6-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="542f6-108">**Você precisará remover completamente os pacotes RC1 e, em seguida, instale o RC2 aqueles.**</span><span class="sxs-lookup"><span data-stu-id="542f6-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="542f6-109">Aqui está o mapeamento para alguns pacotes comuns.</span><span class="sxs-lookup"><span data-stu-id="542f6-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="542f6-110">Pacote do RC1</span><span class="sxs-lookup"><span data-stu-id="542f6-110">RC1 Package</span></span>                                               | <span data-ttu-id="542f6-111">RC2 equivalente</span><span class="sxs-lookup"><span data-stu-id="542f6-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="542f6-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="542f6-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="542f6-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="542f6-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="542f6-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="542f6-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="542f6-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="542f6-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="542f6-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="542f6-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="542f6-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="542f6-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="542f6-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="542f6-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="542f6-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="542f6-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="542f6-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="542f6-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="542f6-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="542f6-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="542f6-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="542f6-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="542f6-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="542f6-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="542f6-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="542f6-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="542f6-125">Ainda não está disponível para RC2</span><span class="sxs-lookup"><span data-stu-id="542f6-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="542f6-126">EntityFramework.Commands                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="542f6-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="542f6-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="542f6-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="542f6-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="542f6-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="542f6-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="542f6-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="542f6-130">Namespaces</span><span class="sxs-lookup"><span data-stu-id="542f6-130">Namespaces</span></span>

<span data-ttu-id="542f6-131">Juntamente com os nomes de pacote, namespaces alterado de `Microsoft.Data.Entity.*` para `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="542f6-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="542f6-132">Você pode lidar com essa alteração com um localizar/substituir de `using Microsoft.Data.Entity` com `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="542f6-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="542f6-133">Tabela de alterações de convenção de nomenclatura</span><span class="sxs-lookup"><span data-stu-id="542f6-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="542f6-134">Uma alteração significativa de funcional que tomamos no RC2 era usar o nome do `DbSet<TEntity>` propriedade para uma determinada entidade como o nome da tabela que ele seja mapeado para, em vez de apenas o nome da classe.</span><span class="sxs-lookup"><span data-stu-id="542f6-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="542f6-135">Você pode ler mais sobre essa alteração no [o problema de lançamento relacionados](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="542f6-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="542f6-136">Para aplicativos existentes do RC1, é recomendável adicionar o código a seguir para o início da sua `OnModelCreating` método para manter a estratégia de nomenclatura RC1:</span><span class="sxs-lookup"><span data-stu-id="542f6-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="542f6-137">Se você desejar adotar a nova estratégia de nomenclatura, recomendamos com êxito renomeia concluir o restante das etapas de atualização e, em seguida, remover o código e a criação de uma migração para aplicar a tabela.</span><span class="sxs-lookup"><span data-stu-id="542f6-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="542f6-138">AddDbContext Startup.cs alterações (somente para projetos ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="542f6-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="542f6-139">No RC1, você precisava adicionar serviços do Entity Framework para o provedor de serviço de aplicativo - `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="542f6-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="542f6-140">No RC2, você pode remover as chamadas para `AddEntityFramework()`, `AddSqlServer()`, etc.:</span><span class="sxs-lookup"><span data-stu-id="542f6-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="542f6-141">Você também precisará adicionar um construtor para o contexto derivado, que usa opções de contexto e os passa para o construtor base.</span><span class="sxs-lookup"><span data-stu-id="542f6-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="542f6-142">Isso é necessário porque removemos alguns mágica assustador que entrou furtivamente-los em segundo plano:</span><span class="sxs-lookup"><span data-stu-id="542f6-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="542f6-143">Passar um IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="542f6-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="542f6-144">Se você tiver código RC1 transmite um `IServiceProvider` para o contexto, isso agora foi movido para `DbContextOptions`, em vez de ser um parâmetro de construtor separado.</span><span class="sxs-lookup"><span data-stu-id="542f6-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="542f6-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` para definir o provedor de serviço.</span><span class="sxs-lookup"><span data-stu-id="542f6-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="542f6-146">Testes</span><span class="sxs-lookup"><span data-stu-id="542f6-146">Testing</span></span>

<span data-ttu-id="542f6-147">O cenário mais comum para isso era controlar o escopo de um banco de dados InMemory durante o teste.</span><span class="sxs-lookup"><span data-stu-id="542f6-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="542f6-148">Consulte a atualização [teste](testing/index.md) artigo para obter um exemplo de como fazer isso com RC2.</span><span class="sxs-lookup"><span data-stu-id="542f6-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="542f6-149">Resolvendo serviços internos do provedor de serviços de aplicativo (somente para projetos ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="542f6-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="542f6-150">Se você tiver um aplicativo ASP.NET Core e você desejar EF para resolver serviços internos do provedor de serviço de aplicativo, há uma sobrecarga de `AddDbContext` que permite que você configure isso:</span><span class="sxs-lookup"><span data-stu-id="542f6-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="542f6-151">É recomendável permitir que o EF internamente gerenciar seus próprios serviços, a menos que você tenha um motivo para combinar os serviços EF internos em seu provedor de serviços de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="542f6-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="542f6-152">O principal motivo que talvez você queira fazer isso é usar o provedor de serviços de aplicativo para substituir os serviços que o EF usa internamente</span><span class="sxs-lookup"><span data-stu-id="542f6-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="542f6-153">Comandos do DNX = > .NET CLI (apenas para projetos ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="542f6-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="542f6-154">Se você tiver usado o `dnx ef` comandos para projetos ASP.NET 5, eles agora foram movidas para `dotnet ef` comandos.</span><span class="sxs-lookup"><span data-stu-id="542f6-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="542f6-155">A mesma sintaxe de comando ainda se aplica.</span><span class="sxs-lookup"><span data-stu-id="542f6-155">The same command syntax still applies.</span></span> <span data-ttu-id="542f6-156">Você pode usar `dotnet ef --help` para obter informações de sintaxe.</span><span class="sxs-lookup"><span data-stu-id="542f6-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="542f6-157">A maneira como os comandos são registrados foi alterado no RC2, devido a DNX está sendo substituída pela CLI do .NET.</span><span class="sxs-lookup"><span data-stu-id="542f6-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="542f6-158">Comandos são registrados em um `tools` seção `project.json`:</span><span class="sxs-lookup"><span data-stu-id="542f6-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="542f6-159">Se você usar o Visual Studio, agora você pode usar o Console do Gerenciador de pacotes para executar comandos EF para projetos ASP.NET Core (isso não era suportado no RC1).</span><span class="sxs-lookup"><span data-stu-id="542f6-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="542f6-160">Você ainda precisará registrar os comandos de `tools` seção `project.json` para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="542f6-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="542f6-161">Comandos do Gerenciador de pacote exigem o PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="542f6-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="542f6-162">Se você usar os comandos do Entity Framework no Console do Gerenciador de pacotes no Visual Studio, você precisará ter 5 PowerShell instalado.</span><span class="sxs-lookup"><span data-stu-id="542f6-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="542f6-163">Esse é um requisito temporário que será removido na próxima versão (consulte [emitir #5327](https://github.com/aspnet/EntityFramework/issues/5327) para obter mais detalhes).</span><span class="sxs-lookup"><span data-stu-id="542f6-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="542f6-164">Usando "imports" no Project</span><span class="sxs-lookup"><span data-stu-id="542f6-164">Using "imports" in project.json</span></span>

<span data-ttu-id="542f6-165">Algumas das dependências do EF Core ainda não suporta .NET padrão.</span><span class="sxs-lookup"><span data-stu-id="542f6-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="542f6-166">EF Core em projetos do .NET padrão e .NET Core talvez seja necessário adicionar "importar" para Project como uma solução temporária.</span><span class="sxs-lookup"><span data-stu-id="542f6-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="542f6-167">Ao adicionar EF, a restauração do NuGet exibirá essa mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="542f6-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="542f6-168">A solução alternativa é importar manualmente o perfil portátil "portátil net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="542f6-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="542f6-169">Isso força o NuGet para tratar esse binários que correspondem a esse fornece como uma estrutura compatível com .NET padrão, mesmo que eles não são.</span><span class="sxs-lookup"><span data-stu-id="542f6-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="542f6-170">Embora "portátil net451 + win8" não é 100% compatível com .NET padrão, é compatível para a transição do PCL para .NET padrão.</span><span class="sxs-lookup"><span data-stu-id="542f6-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="542f6-171">Importações podem ser removidas quando dependências do EF eventualmente atualizar para o .NET padrão.</span><span class="sxs-lookup"><span data-stu-id="542f6-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="542f6-172">Várias estruturas podem ser adicionadas para "imports" na sintaxe da matriz.</span><span class="sxs-lookup"><span data-stu-id="542f6-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="542f6-173">Outras importações podem ser necessárias se você adicionar bibliotecas adicionais ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="542f6-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="542f6-174">Consulte [emitir &#5176;](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="542f6-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
