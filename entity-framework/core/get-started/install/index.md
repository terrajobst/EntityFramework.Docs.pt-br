---
title: Instalar o Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 62194d1db4efcdaed53ca0e14f160315f8e3cf03
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921751"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="c07ab-102">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c07ab-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c07ab-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c07ab-103">Prerequisites</span></span>

* <span data-ttu-id="c07ab-104">O EF Core é uma biblioteca do [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="c07ab-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="c07ab-105">Dessa maneira, para o EF Core ser executado, é necessária uma implementação do .NET compatível com o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="c07ab-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="c07ab-106">O EF Core também pode ser referenciado por outras bibliotecas do .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="c07ab-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span> 

* <span data-ttu-id="c07ab-107">Por exemplo, é possível usar o EF Core para desenvolver aplicativos para o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c07ab-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="c07ab-108">Para criar aplicativos do .NET Core, é necessária a [SDK do .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="c07ab-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="c07ab-109">Como opção, também é possível usar um ambiente de desenvolvimento como o Visual Studio, o Visual Studio para Mac ou o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c07ab-109">Optionally, you can also use a development environment like Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span> <span data-ttu-id="c07ab-110">Para saber mais, confira a [Introdução ao .NET Core](/dotnet/core/get-started).</span><span class="sxs-lookup"><span data-stu-id="c07ab-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="c07ab-111">É possível usar o EF Core para desenvolver aplicativos para o .NET Framework 4.6.1 ou posterior no Windows usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c07ab-111">You can use EF Core to develop applications that target .NET Framework 4.6.1 or later on Windows, using Visual Studio.</span></span> <span data-ttu-id="c07ab-112">É recomendável usar a versão mais recente do [Visual Studio](https://visualstudio.microsoft.com/vs).</span><span class="sxs-lookup"><span data-stu-id="c07ab-112">The latest version of [Visual Studio](https://visualstudio.microsoft.com/vs) is recommended.</span></span> <span data-ttu-id="c07ab-113">Se você quiser usar uma versão mais antiga, como o Visual Studio 2015, [atualize o cliente NuGet para a versão 3.6.0](https://www.nuget.org/downloads). Assim, a versão anterior funcionará com as bibliotecas do .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="c07ab-113">If you want to use an older version, like Visual Studio 2015, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="c07ab-114">O EF Core pode ser executado em outras implementações do .NET, como [Xamarin](https://dotnet.microsoft.com/apps/xamarin) e .NET Native.</span><span class="sxs-lookup"><span data-stu-id="c07ab-114">EF Core can run on other .NET implementations like [Xamarin](https://dotnet.microsoft.com/apps/xamarin) and .NET Native.</span></span> <span data-ttu-id="c07ab-115">No entanto, na prática, essas implementações têm limitações de tempo de execução que podem afetar o funcionamento do EF Core no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c07ab-115">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="c07ab-116">Para saber mais, confira [Implementações do .NET compatíveis com o EF Core](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="c07ab-116">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="c07ab-117">Por fim, provedores de banco de dados diferentes podem exigir versões específicas de mecanismo de banco de dados, implementações do .NET ou sistemas operacionais.</span><span class="sxs-lookup"><span data-stu-id="c07ab-117">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="c07ab-118">Verifique se há um [provedor de banco de dados do EF Core](xref:core/providers/index) disponível que seja compatível com o ambiente correto para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c07ab-118">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="c07ab-119">Obter o tempo de execução do Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c07ab-119">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="c07ab-120">Para adicionar o EF Core a um aplicativo, instale o pacote do NuGet do provedor de banco de dados que você quer usar.</span><span class="sxs-lookup"><span data-stu-id="c07ab-120">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="c07ab-121">Se você estiver criando um aplicativo do ASP.NET Core, não precisará instalar os provedores in-memory e do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c07ab-121">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="c07ab-122">Esses provedores estão incluídos nas versões atuais do ASP.NET Core, juntamente com o tempo de execução do EF Core.</span><span class="sxs-lookup"><span data-stu-id="c07ab-122">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="c07ab-123">Para instalar ou atualizar os pacotes NuGet, use a CLI (interface de linha de comando) do .NET Core, a Caixa de Diálogo do Gerenciador de Pacotes do Visual Studio ou o Console do Gerenciador de Pacotes do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c07ab-123">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="c07ab-124">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="c07ab-124">.NET Core CLI</span></span>

* <span data-ttu-id="c07ab-125">Use o seguinte comando da CLI do .NET Core na linha de comando do sistema operacional para instalar ou atualizar o provedor de SQL Server do EF Core:</span><span class="sxs-lookup"><span data-stu-id="c07ab-125">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="c07ab-126">É possível indicar uma versão específica no comando `dotnet add package`, usando o modificador `-v`.</span><span class="sxs-lookup"><span data-stu-id="c07ab-126">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="c07ab-127">Por exemplo, para instalar pacotes do EF Core 2.2.0, acrescente `-v 2.2.0` ao comando.</span><span class="sxs-lookup"><span data-stu-id="c07ab-127">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="c07ab-128">Para saber mais, confira [Ferramentas da CLI (interface de linha de comando) do .NET](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="c07ab-128">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="c07ab-129">Caixa de diálogo do gerenciador de pacotes do NuGet do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c07ab-129">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="c07ab-130">No menu do Visual Studio, selecione **Projeto > Gerenciar Pacotes do NuGet**</span><span class="sxs-lookup"><span data-stu-id="c07ab-130">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="c07ab-131">Clique na guia **Procurar** ou **Atualizações**</span><span class="sxs-lookup"><span data-stu-id="c07ab-131">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="c07ab-132">Para instalar ou atualizar o provedor do SQL Server, selecione o pacote `Microsoft.EntityFrameworkCore.SqlServer` e confirme.</span><span class="sxs-lookup"><span data-stu-id="c07ab-132">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="c07ab-133">Para saber mais, confira [Caixa de Diálogo do Gerenciador de Pacotes do NuGet](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="c07ab-133">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="c07ab-134">Console do gerenciador de pacotes do NuGet do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c07ab-134">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="c07ab-135">No menu do Visual Studio, selecione **Ferramentas > Gerenciador de Pacotes do NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="c07ab-135">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c07ab-136">Para instalar o provedor do SQL Server, execute o seguinte comando no Console do Gerenciador de Pacotes:</span><span class="sxs-lookup"><span data-stu-id="c07ab-136">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="c07ab-137">Para atualizar o provedor, use o comando `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="c07ab-137">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="c07ab-138">Para especificar uma versão, use o modificador `-Version`.</span><span class="sxs-lookup"><span data-stu-id="c07ab-138">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="c07ab-139">Por exemplo, para instalar pacotes do EF Core 2.2.0, acrescente `-Version 2.2.0` aos comandos</span><span class="sxs-lookup"><span data-stu-id="c07ab-139">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="c07ab-140">Para saber mais, confira [Console do gerenciador de pacotes](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="c07ab-140">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="c07ab-141">Obtenha as ferramentas do Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c07ab-141">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="c07ab-142">É possível instalar ferramentas para realizar tarefas relacionadas com o EF Core no projeto, como criar e aplicar migrações de banco de dados ou criar um modelo do EF Core baseado em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="c07ab-142">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="c07ab-143">Dois conjuntos de ferramentas estão disponíveis:</span><span class="sxs-lookup"><span data-stu-id="c07ab-143">Two sets of tools are available:</span></span>

* <span data-ttu-id="c07ab-144">As [ferramentas da CLI (interface de linha de comando) do .NET Core](xref:core/miscellaneous/cli/dotnet) podem ser usadas no Windows, no Linux ou no macOS.</span><span class="sxs-lookup"><span data-stu-id="c07ab-144">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="c07ab-145">Os comandos começam com `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="c07ab-145">These commands begin with `dotnet ef`.</span></span> 

* <span data-ttu-id="c07ab-146">As [ferramentas do PMC (Console do Gerenciador de Pacotes)](xref:core/miscellaneous/cli/powershell) são executadas no Visual Studio no Windows.</span><span class="sxs-lookup"><span data-stu-id="c07ab-146">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="c07ab-147">Esses comandos começam com um verbo, por exemplo `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="c07ab-147">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="c07ab-148">Embora você possa usar os comandos `dotnet ef` no Console do Gerenciador de Pacotes, é recomendável usar as ferramentas dele quando estiver usando o Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c07ab-148">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="c07ab-149">Elas funcionam automaticamente com o projeto atual selecionado no PMC no Visual Studio, sem necessidade de trocar manualmente os diretórios.</span><span class="sxs-lookup"><span data-stu-id="c07ab-149">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="c07ab-150">Eles abrem automaticamente os arquivos gerados pelos comandos no Visual Studio depois que o comando for concluído.</span><span class="sxs-lookup"><span data-stu-id="c07ab-150">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="c07ab-151">Obtenha as ferramentas da CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="c07ab-151">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="c07ab-152">As ferramentas da CLI do .NET Core precisam do SDK do .NET Core mencionado anteriormente nos [Pré-requisitos](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="c07ab-152">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="c07ab-153">Os comandos `dotnet ef` estão incluídos nas versões atuais do SDK do .NET Core, mas para habilitá-los em um projeto específico, é necessário instalar o pacote `Microsoft.EntityFrameworkCore.Design`:</span><span class="sxs-lookup"><span data-stu-id="c07ab-153">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

``` Console 
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="c07ab-154">Nos aplicativos do ASP.NET Core, esse pacote está incluído automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c07ab-154">For ASP.NET Core apps, this package is included automatically.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="c07ab-155">Sempre use a versão do pacote de ferramentas que corresponda à versão principal dos pacotes em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c07ab-155">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="c07ab-156">Obter as ferramentas do Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="c07ab-156">Get the Package Manager Console tools</span></span>

<span data-ttu-id="c07ab-157">Para obter as ferramentas do Console do Gerenciador de Pacotes para EF Core, instale o pacote `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="c07ab-157">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="c07ab-158">Por exemplo, no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c07ab-158">For example, from Visual Studio:</span></span>

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="c07ab-159">Nos aplicativos do ASP.NET Core, esse pacote está incluído automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c07ab-159">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="c07ab-160">Atualizar para o EF Core mais recente</span><span class="sxs-lookup"><span data-stu-id="c07ab-160">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="c07ab-161">Sempre que lançamos uma nova versão do EF Core, também lançamos uma nova versão dos provedores que fazem parte do projeto do EF Core, como Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.InMemory.</span><span class="sxs-lookup"><span data-stu-id="c07ab-161">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="c07ab-162">Basta atualizar para a nova versão do provedor para obter todas as melhorias.</span><span class="sxs-lookup"><span data-stu-id="c07ab-162">You can just upgrade to the new version of the provider to get all the improvements.</span></span> 

* <span data-ttu-id="c07ab-163">O EF Core, junto com o SQL Server e os provedores in-memory, estão incluídos nas versões atuais do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c07ab-163">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="c07ab-164">Para atualizar um aplicativo ASP.NET Core existente para uma nova versão do EF Core, sempre atualize a versão do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c07ab-164">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="c07ab-165">Se você precisar atualizar um aplicativo que esteja usando um provedor de banco de dados de terceiros, sempre verifique se há uma atualização do provedor compatível com a versão do EF Core que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="c07ab-165">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="c07ab-166">Por exemplo, provedores de banco de dados para versões anteriores não são compatíveis com a versão 2.0 do tempo de execução do EF Core.</span><span class="sxs-lookup"><span data-stu-id="c07ab-166">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="c07ab-167">Normalmente, os provedores de terceiros do EF Core não lançam versões de patch junto com o tempo de execução do EF Core.</span><span class="sxs-lookup"><span data-stu-id="c07ab-167">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="c07ab-168">Para atualizar um aplicativo que usa um provedor de terceiros para uma versão de patch do EF Core, talvez seja necessário adicionar uma referência direta aos componentes de tempo de execução do EF Core, como Microsoft.EntityFrameworkCore e Microsoft.EntityFrameworkCore.Relational.</span><span class="sxs-lookup"><span data-stu-id="c07ab-168">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="c07ab-169">Se você estiver atualizando um aplicativo existente para a versão mais recente do EF Core, talvez seja necessário remover manualmente algumas referências aos pacotes mais antigos do EF Core:</span><span class="sxs-lookup"><span data-stu-id="c07ab-169">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="c07ab-170">Os pacotes de tempo de design do provedor do banco de dados, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, não são mais necessários nem compatíveis com o EF Core 2.0, mas não são automaticamente removidos ao atualizar os outros pacotes.</span><span class="sxs-lookup"><span data-stu-id="c07ab-170">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="c07ab-171">As ferramentas da CLI do .NET estão incluídas no SDK do .NET desde a versão 2.1, portanto, a referência a esse pacote pode ser removida do arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="c07ab-171">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* <span data-ttu-id="c07ab-172">Os aplicativos para .NET Framework podem precisar de alterações para funcionar com as bibliotecas do .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="c07ab-172">Applications that target .NET Framework may need changes to work with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="c07ab-173">Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:</span><span class="sxs-lookup"><span data-stu-id="c07ab-173">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="c07ab-174">Para projetos de teste, verifique também se a seguinte entrada está presente:</span><span class="sxs-lookup"><span data-stu-id="c07ab-174">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
