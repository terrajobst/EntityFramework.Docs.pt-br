---
title: Atualizando do EF Core 1,0 RC1 para RC2-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 887b7cd539b9c0f5a680398f5039757420228710
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416606"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="1bc4f-102">Atualizando do EF Core 1,0 RC1 para 1,0 RC2</span><span class="sxs-lookup"><span data-stu-id="1bc4f-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="1bc4f-103">Este artigo fornece diretrizes para mover um aplicativo criado com os pacotes RC1 para RC2.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="1bc4f-104">Nomes e versões de pacote</span><span class="sxs-lookup"><span data-stu-id="1bc4f-104">Package Names and Versions</span></span>

<span data-ttu-id="1bc4f-105">Entre o RC1 e o RC2, alteramos de "Entity Framework 7" para "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="1bc4f-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="1bc4f-106">Você pode ler mais sobre os motivos da alteração nesta [postagem de Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="1bc4f-106">You can read more about the reasons for the change in [this post by Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="1bc4f-107">Por causa dessa alteração, nossos nomes de pacote mudaram de `EntityFramework.*` para `Microsoft.EntityFrameworkCore.*` e nossas versões de `7.0.0-rc1-final` para `1.0.0-rc2-final` (ou `1.0.0-preview1-final` para ferramentas).</span><span class="sxs-lookup"><span data-stu-id="1bc4f-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="1bc4f-108">**Será necessário remover completamente os pacotes RC1 e, em seguida, instalar os RC2.**</span><span class="sxs-lookup"><span data-stu-id="1bc4f-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="1bc4f-109">Aqui está o mapeamento para alguns pacotes comuns.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="1bc4f-110">Pacote RC1</span><span class="sxs-lookup"><span data-stu-id="1bc4f-110">RC1 Package</span></span>                                               | <span data-ttu-id="1bc4f-111">RC2 equivalente</span><span class="sxs-lookup"><span data-stu-id="1bc4f-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="1bc4f-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="1bc4f-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="1bc4f-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="1bc4f-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="1bc4f-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="1bc4f-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="1bc4f-117">NpgSql. EntityFrameworkCore. postgres <to be advised></span><span class="sxs-lookup"><span data-stu-id="1bc4f-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="1bc4f-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="1bc4f-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="1bc4f-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="1bc4f-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="1bc4f-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="1bc4f-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="1bc4f-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1bc4f-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="1bc4f-125">Ainda não disponível para RC2</span><span class="sxs-lookup"><span data-stu-id="1bc4f-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="1bc4f-126">EntityFramework. Commands 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="1bc4f-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="1bc4f-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="1bc4f-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="1bc4f-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="1bc4f-130">Namespaces</span><span class="sxs-lookup"><span data-stu-id="1bc4f-130">Namespaces</span></span>

<span data-ttu-id="1bc4f-131">Juntamente com nomes de pacote, os namespaces foram alterados de `Microsoft.Data.Entity.*` para `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="1bc4f-132">Você pode lidar com essa alteração com uma localização/substituição de `using Microsoft.Data.Entity` com `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="1bc4f-133">Alterações na Convenção de nomenclatura de tabela</span><span class="sxs-lookup"><span data-stu-id="1bc4f-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="1bc4f-134">Uma alteração funcional significativa que fizemos no RC2 era usar o nome da propriedade `DbSet<TEntity>` para uma determinada entidade como o nome da tabela que ele mapeia, em vez de apenas o nome da classe.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="1bc4f-135">Você pode ler mais sobre essa alteração no [problema relacionado do comunicado](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="1bc4f-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="1bc4f-136">Para aplicativos RC1 existentes, recomendamos adicionar o seguinte código ao início do seu método de `OnModelCreating` para manter a estratégia de nomenclatura do RC1:</span><span class="sxs-lookup"><span data-stu-id="1bc4f-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="1bc4f-137">Se você quiser adotar a nova estratégia de nomenclatura, recomendamos que você conclua com êxito o restante das etapas de atualização e, em seguida, removesse o código e criando uma migração para aplicar os renomeações de tabela.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="1bc4f-138">Alterações de AddDbContext/Startup.cs (somente ASP.NET Core projetos)</span><span class="sxs-lookup"><span data-stu-id="1bc4f-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="1bc4f-139">No RC1, você precisava adicionar serviços de Entity Framework ao provedor de serviços de aplicativo-em `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="1bc4f-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="1bc4f-140">No RC2, você pode remover as chamadas para `AddEntityFramework()`, `AddSqlServer()`, etc.:</span><span class="sxs-lookup"><span data-stu-id="1bc4f-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="1bc4f-141">Você também precisa adicionar um construtor ao seu contexto derivado, que usa opções de contexto e os passa para o construtor base.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="1bc4f-142">Isso é necessário porque removemos algumas das mágicas assustadoras que as recorri em segundo plano:</span><span class="sxs-lookup"><span data-stu-id="1bc4f-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="1bc4f-143">Passando um IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="1bc4f-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="1bc4f-144">Se você tiver um código RC1 que passa um `IServiceProvider` para o contexto, isso agora foi movido para `DbContextOptions`, em vez de ser um parâmetro de Construtor separado.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="1bc4f-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` para definir o provedor de serviços.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="1bc4f-146">Testando</span><span class="sxs-lookup"><span data-stu-id="1bc4f-146">Testing</span></span>

<span data-ttu-id="1bc4f-147">O cenário mais comum para fazer isso era controlar o escopo de um banco de dados de inmemory durante o teste.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="1bc4f-148">Consulte o artigo [teste](testing/index.md) atualizado para obter um exemplo de como fazer isso com o RC2.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="1bc4f-149">Resolvendo serviços internos do provedor de serviços de aplicativo (somente ASP.NET Core projetos)</span><span class="sxs-lookup"><span data-stu-id="1bc4f-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="1bc4f-150">Se você tiver um aplicativo ASP.NET Core e quiser que o EF resolva os serviços internos do provedor de serviços de aplicativo, haverá uma sobrecarga de `AddDbContext` que permite que você configure isso:</span><span class="sxs-lookup"><span data-stu-id="1bc4f-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="1bc4f-151">É recomendável permitir que o EF gerencie internamente seus próprios serviços, a menos que você tenha um motivo para combinar os serviços internos do EF em seu provedor de serviços de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="1bc4f-152">O principal motivo para que você queira fazer isso é usar o provedor de serviços de aplicativo para substituir os serviços que o EF usa internamente</span><span class="sxs-lookup"><span data-stu-id="1bc4f-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="1bc4f-153">Comandos DNX = > CLI .NET (somente projetos ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="1bc4f-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="1bc4f-154">Se você usou anteriormente os comandos `dnx ef` para projetos ASP.NET 5, eles agora foram movidos para `dotnet ef` comandos.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="1bc4f-155">A mesma sintaxe de comando ainda se aplica.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-155">The same command syntax still applies.</span></span> <span data-ttu-id="1bc4f-156">Você pode usar `dotnet ef --help` para obter informações de sintaxe.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="1bc4f-157">A maneira como os comandos são registrados foi alterada no RC2, devido ao DNX sendo substituído pela CLI do .NET.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="1bc4f-158">Os comandos agora estão registrados em uma seção `tools` no `project.json`:</span><span class="sxs-lookup"><span data-stu-id="1bc4f-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="1bc4f-159">Se você usar o Visual Studio, agora poderá usar o console do Gerenciador de pacotes para executar comandos do EF para projetos de ASP.NET Core (não há suporte para isso no RC1).</span><span class="sxs-lookup"><span data-stu-id="1bc4f-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="1bc4f-160">Você ainda precisa registrar os comandos na seção `tools` de `project.json` para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="1bc4f-161">Os comandos do Gerenciador de pacotes exigem o PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="1bc4f-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="1bc4f-162">Se você usar os comandos de Entity Framework no console do Gerenciador de pacotes no Visual Studio, será necessário garantir que o PowerShell 5 esteja instalado.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="1bc4f-163">Esse é um requisito temporário que será removido na próxima versão (consulte o [problema #5327](https://github.com/aspnet/EntityFramework/issues/5327) para obter mais detalhes).</span><span class="sxs-lookup"><span data-stu-id="1bc4f-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="1bc4f-164">Usando "Imports" em Project. JSON</span><span class="sxs-lookup"><span data-stu-id="1bc4f-164">Using "imports" in project.json</span></span>

<span data-ttu-id="1bc4f-165">Algumas das dependências do EF Core ainda não dão suporte ao .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="1bc4f-166">EF Core em projetos do .NET Standard e do .NET Core podem exigir a adição de "Imports" a Project. JSON como uma solução alternativa temporária.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="1bc4f-167">Ao adicionar o EF, a restauração do NuGet exibirá esta mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="1bc4f-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="1bc4f-168">A solução alternativa é importar manualmente o perfil portátil "Portable-net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="1bc4f-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="1bc4f-169">Isso forçará o NuGet a tratar esses binários que correspondam a esse fornecimento como uma estrutura compatível com .NET Standard, embora não sejam.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="1bc4f-170">Embora "Portable-net451 + win8" não seja 100% compatível com .NET Standard, ele é compatível o suficiente para a transição de PCL para .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="1bc4f-171">As importações podem ser removidas quando as dependências do EF eventualmente são atualizadas para .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="1bc4f-172">Várias estruturas podem ser adicionadas a "Imports" na sintaxe de matriz.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="1bc4f-173">Outras importações poderão ser necessárias se você adicionar mais bibliotecas ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="1bc4f-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="1bc4f-174">Consulte o [problema #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="1bc4f-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
