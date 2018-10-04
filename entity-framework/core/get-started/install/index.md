---
title: Instalar o Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 455eccbb149f0980cefa250ef5db443c73e66603
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575633"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="e0086-102">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e0086-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0086-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e0086-103">Prerequisites</span></span>

* <span data-ttu-id="e0086-104">Para desenvolver aplicativos destinados ao .NET Core 2.1, instale o [SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="e0086-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="e0086-105">O SDK precisa ser instalado, mesmo se você tiver a versão mais recente do Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e0086-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="e0086-106">Para usar o Visual Studio para desenvolvimento de aplicativos destinados ao .NET Core 2.1, instale o Visual Studio 2017 versão 15.7 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="e0086-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="e0086-107">Para usar o Entity Framework 2.1 em aplicativos do ASP.NET Core, use o ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e0086-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="e0086-108">Aplicativos que usam versões anteriores do ASP.NET Core precisam ser atualizados para o 2.1.</span><span class="sxs-lookup"><span data-stu-id="e0086-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="e0086-109">É possível usar o Visual Studio 2015 para aplicativos destinados ao .NET Framework 4.6.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="e0086-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="e0086-110">No entanto, é necessário ter uma versão do NuGet que reconheça .NET Standard 2.0 e suas estruturas compatíveis.</span><span class="sxs-lookup"><span data-stu-id="e0086-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="e0086-111">Para fazer isso no Visual Studio 2015, [atualize o cliente do NuGet para a versão 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="e0086-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="e0086-112">Obter o tempo de execução do Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e0086-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="e0086-113">Para adicionar as bibliotecas de tempo de execução do EF Core a um aplicativo, instale o pacote do NuGet para o provedor de banco de dados que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="e0086-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="e0086-114">Para ver uma lista de provedores compatíveis e seus nomes de pacote do NuGet, confira [Provedores de banco de dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e0086-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="e0086-115">Para instalar ou atualizar pacotes do NuGet, use a CLI do .NET Core, a caixa de diálogo do gerenciador de pacotes do Visual Studio ou o console do gerenciador de pacotes do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0086-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="e0086-116">Para aplicativos do ASP.NET Core 2.1, os provedores SQL Server e na memória são incluídos automaticamente, portanto, não é necessário instalá-los separadamente.</span><span class="sxs-lookup"><span data-stu-id="e0086-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="e0086-117">Se você precisar atualizar um aplicativo que esteja usando um provedor de banco de dados de terceiros, sempre verifique se há uma atualização do provedor compatível com a versão do EF Core que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="e0086-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="e0086-118">Por exemplo, provedores de banco de dados para versões anteriores não são compatíveis com a versão 2.1 do tempo de execução do EF Core.</span><span class="sxs-lookup"><span data-stu-id="e0086-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="e0086-119">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e0086-119">.NET Core CLI</span></span>

<span data-ttu-id="e0086-120">O seguinte comando da CLI do .NET Core instala ou atualiza o provedor do SQL Server:</span><span class="sxs-lookup"><span data-stu-id="e0086-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="e0086-121">É possível indicar uma versão específica no comando `dotnet add package`, usando o modificador `-v`.</span><span class="sxs-lookup"><span data-stu-id="e0086-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="e0086-122">Por exemplo, para instalar pacotes do EF Core 2.1.0, acrescente `-v 2.1.0` ao comando.</span><span class="sxs-lookup"><span data-stu-id="e0086-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="e0086-123">Caixa de diálogo do gerenciador de pacotes do NuGet do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0086-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="e0086-124">No menu, selecione **Projeto > Gerenciar Pacotes do NuGet**</span><span class="sxs-lookup"><span data-stu-id="e0086-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="e0086-125">Clique na guia **Procurar** ou **Atualizações**</span><span class="sxs-lookup"><span data-stu-id="e0086-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="e0086-126">Para instalar ou atualizar o provedor do SQL Server, selecione o pacote `Microsoft.EntityFrameworkCore.SqlServer` e confirme.</span><span class="sxs-lookup"><span data-stu-id="e0086-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="e0086-127">Para saber mais, confira [Caixa de Diálogo do Gerenciador de Pacotes do NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="e0086-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="e0086-128">Console do gerenciador de pacotes do NuGet do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0086-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="e0086-129">No menu, selecione **Ferramentas > Gerenciador de Pacotes do NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="e0086-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="e0086-130">Para instalar o provedor do SQL Server, execute o seguinte comando no Console do Gerenciador de Pacotes:</span><span class="sxs-lookup"><span data-stu-id="e0086-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="e0086-131">Para atualizar o provedor, use o comando `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="e0086-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="e0086-132">Para especificar uma versão, use o modificador `-Version`.</span><span class="sxs-lookup"><span data-stu-id="e0086-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="e0086-133">Por exemplo, para instalar pacotes do EF Core 2.1.0, acrescente `-Version 2.1.0` aos comandos</span><span class="sxs-lookup"><span data-stu-id="e0086-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="e0086-134">Para saber mais, confira [Console do gerenciador de pacotes](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="e0086-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="e0086-135">Obter ferramentas do Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e0086-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="e0086-136">Além das bibliotecas de tempo de execução, é possível instalar as ferramentas que podem executar algumas tarefas relacionadas ao EF Core no projeto em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="e0086-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="e0086-137">Por exemplo, é possível criar migrações, aplicá-las e criar um modelo com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="e0086-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="e0086-138">Dois conjuntos de ferramentas estão disponíveis:</span><span class="sxs-lookup"><span data-stu-id="e0086-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="e0086-139">As [ferramentas da CLI (interface de linha de comando)](../../miscellaneous/cli/dotnet.md) do .NET Core podem ser usadas no Windows, Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="e0086-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="e0086-140">Os comandos começam com `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="e0086-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="e0086-141">As [ferramentas do Console do Gerenciador de Pacotes](../../miscellaneous/cli/powershell.md) são executadas no Visual Studio 2017 no Windows.</span><span class="sxs-lookup"><span data-stu-id="e0086-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="e0086-142">Esses comandos começam com um verbo, por exemplo `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="e0086-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="e0086-143">Embora você possa usar os comandos `dotnet ef` no Console do Gerenciador de Pacotes, é mais conveniente usar as ferramentas dele quando estiver usando o Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e0086-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="e0086-144">Elas funcionam automaticamente com o projeto atual selecionado no Console do Gerenciador de Pacotes sem necessidade de trocar manualmente os diretórios.</span><span class="sxs-lookup"><span data-stu-id="e0086-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="e0086-145">Eles abrem automaticamente os arquivos gerados pelos comandos no Visual Studio depois que o comando for concluído.</span><span class="sxs-lookup"><span data-stu-id="e0086-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="e0086-146">Obter as ferramentas da CLI</span><span class="sxs-lookup"><span data-stu-id="e0086-146">Get the CLI tools</span></span>

<span data-ttu-id="e0086-147">Os comandos `dotnet ef` estão incluídos no SDK do .NET Core, mas para habilitá-los é necessário instalar o pacote `Microsoft.EntityFrameworkCore.Design`:</span><span class="sxs-lookup"><span data-stu-id="e0086-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="e0086-148">Nos aplicativos do ASP.NET Core 2.1, esse pacote é incluído automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e0086-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="e0086-149">Conforme explicado anteriormente em [Pré-requisitos](#prerequisites), também será necessário instalar o SDK do .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e0086-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="e0086-150">Sempre use a versão do pacote de ferramentas que corresponda à versão principal dos pacotes em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="e0086-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="e0086-151">Obter as ferramentas do Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="e0086-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="e0086-152">Para obter as ferramentas do Console do Gerenciador de Pacotes para EF Core, instale o pacote `Microsoft.EntityFrameworkCore.Tools`:</span><span class="sxs-lookup"><span data-stu-id="e0086-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="e0086-153">Nos aplicativos do ASP.NET Core 2.1, esse pacote é incluído automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e0086-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="e0086-154">Atualizar para o EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="e0086-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="e0086-155">Se você estiver atualizando um aplicativo existente para o EF Core 2.1, talvez seja necessário remover manualmente algumas referências aos pacotes mais antigos do EF Core:</span><span class="sxs-lookup"><span data-stu-id="e0086-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="e0086-156">Pacotes de tempo de design do provedor do banco de dados, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, não são mais necessários nem compatíveis com o EF Core 2.1, mas não são automaticamente removidos ao atualizar os outros pacotes.</span><span class="sxs-lookup"><span data-stu-id="e0086-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="e0086-157">As ferramentas da CLI do .NET agora estão incluídas no SDK do .NET, portanto, a referência a esse pacote pode ser removida do arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e0086-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="e0086-158">Quanto aos aplicativos destinados ao .NET Framework e criados em versões mais antigas do Visual Studio, é necessário verificar se são compatíveis com as bibliotecas do .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="e0086-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="e0086-159">Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:</span><span class="sxs-lookup"><span data-stu-id="e0086-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="e0086-160">Para projetos de teste, verifique também se a seguinte entrada está presente:</span><span class="sxs-lookup"><span data-stu-id="e0086-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
