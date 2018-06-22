---
title: Instalar o EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26049238"
---
# <a name="installing-ef-core"></a><span data-ttu-id="d14df-102">Instalar o EF Core</span><span class="sxs-lookup"><span data-stu-id="d14df-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d14df-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="d14df-103">Prerequisites</span></span>

<span data-ttu-id="d14df-104">Para desenvolver aplicativos .NET Core 2.0 (incluindo aplicativos ASP.NET Core 2.0 que têm como destino o .NET Core), será necessário baixar e instalar uma versão do [SDK do .NET Core 2.0](https://www.microsoft.com/net/download/core) apropriado para a sua plataforma.</span><span class="sxs-lookup"><span data-stu-id="d14df-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="d14df-105">**Isso é verdadeiro mesmo que você tenha instalado o Visual Studio 2017 versão 15.3.**</span><span class="sxs-lookup"><span data-stu-id="d14df-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="d14df-106">Para usar o EF Core 2.0 ou qualquer outra biblioteca .NET Standard 2.0 padrão com uma plataforma .NET além do .NET Core 2.0 (por exemplo, com o .NET Framework 4.6.1 ou superior), você precisará de uma versão do NuGet que reconheça o padrão .NET 2.0 e suas estruturas compatíveis.</span><span class="sxs-lookup"><span data-stu-id="d14df-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="d14df-107">Aqui estão algumas maneiras de obter isso:</span><span class="sxs-lookup"><span data-stu-id="d14df-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="d14df-108">Instalar o Visual Studio 2017 versão 15.3</span><span class="sxs-lookup"><span data-stu-id="d14df-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="d14df-109">Se você estiver usando o Visual Studio 2015, [baixe e atualize o cliente do NuGet para a versão 3.6.0](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="d14df-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="d14df-110">Projetos criados com versões anteriores do Visual Studio e que tenham .NET Framework como destino podem precisar de modificações adicionais para serem compatíveis com bibliotecas .NET 2.0 Standard:</span><span class="sxs-lookup"><span data-stu-id="d14df-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="d14df-111">Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:</span><span class="sxs-lookup"><span data-stu-id="d14df-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="d14df-112">Para projetos de teste, verifique também se a seguinte entrada está presente:</span><span class="sxs-lookup"><span data-stu-id="d14df-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="d14df-113">Obter os bits</span><span class="sxs-lookup"><span data-stu-id="d14df-113">Getting the bits</span></span>
<span data-ttu-id="d14df-114">A maneira recomendada de adicionar bibliotecas de tempo de execução do EF Core a um aplicativo é instalar um provedor de banco de dados do EF Core do NuGet.</span><span class="sxs-lookup"><span data-stu-id="d14df-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="d14df-115">Além das bibliotecas de tempo de execução, você pode instalar as ferramentas que facilitam executar várias tarefas relacionadas ao EF Core em seu projeto no tempo de design, como criar e aplicar migrações e criar um modelo com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="d14df-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="d14df-116">Se você precisar atualizar um aplicativo que esteja usando um provedor de banco de dados de terceiros, sempre verifique se há uma atualização do provedor compatível com a versão do EF Core que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="d14df-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="d14df-117">Por ex.:</span><span class="sxs-lookup"><span data-stu-id="d14df-117">E.g.</span></span> <span data-ttu-id="d14df-118">provedores de banco de dados para versões anteriores não são compatíveis com a versão 2.0 do tempo de execução do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d14df-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="d14df-119">Aplicativos que têm como destino o ASP.NET Core 2.0 podem usar o EF Core 2.0 sem dependências adicionais além de provedores de banco de dados de terceiros.</span><span class="sxs-lookup"><span data-stu-id="d14df-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="d14df-120">Aplicativos que têm como destino versões anteriores do ASP.NET Core precisam fazer upgrade para o ASP.NET Core 2.0 para usar o EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d14df-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="d14df-121">Desenvolvimento de multiplaforma usando a CLI (Interface de Linha de Comando) do .NET Core</span><span class="sxs-lookup"><span data-stu-id="d14df-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="d14df-122">Para desenvolver aplicativos que têm como destino o [.NET Core](https://www.microsoft.com/net/download/core), você pode optar por usar [`dotnet` comandos da CLI](https://docs.microsoft.com/dotnet/core/tools/) combinados ao seu editor de texto favorito ou um IDE (Ambiente de Desenvolvimento Integrado), como o Visual Studio, o Visual Studio para Mac ou o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d14df-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="d14df-123">Aplicativos voltados para .NET Core exigem versões específicas do Visual Studio, por exemplo, .NET Core 1.x requer Visual Studio 2017, enquanto o desenvolvimento em .NET Core 2.0 requer o Visual Studio 2017 versão 15.3.</span><span class="sxs-lookup"><span data-stu-id="d14df-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="d14df-124">Para instalar ou fazer o upgrade para o provedor do SQL Server em um aplicativo .NET Core de multiplaforma, alterne para o diretório do aplicativo e execute o seguinte em uma linha de comando:</span><span class="sxs-lookup"><span data-stu-id="d14df-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="d14df-125">Você pode indicar a instalação de uma versão específica no comando `dotnet add package` usando o modificador `-v`.</span><span class="sxs-lookup"><span data-stu-id="d14df-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="d14df-126">Por ex.:</span><span class="sxs-lookup"><span data-stu-id="d14df-126">E.g.</span></span> <span data-ttu-id="d14df-127">para instalar pacotes do EF Core 2.0, acrescente `-v 2.0.0` ao comando.</span><span class="sxs-lookup"><span data-stu-id="d14df-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="d14df-128">O EF Core inclui um conjunto de [comandos adicionais à `dotnet` CLI](../../miscellaneous/cli/dotnet.md), começando com `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="d14df-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="d14df-129">Para usar os comandos da CLI `dotnet ef`, o arquivo `.csproj` do seu aplicativo deve conter a seguinte entrada:</span><span class="sxs-lookup"><span data-stu-id="d14df-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="d14df-130">As ferramentas da CLI do .NET Core para EF Core também exigem um pacote separado chamado Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="d14df-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="d14df-131">Você pode simplesmente adicioná-lo ao projeto usando:</span><span class="sxs-lookup"><span data-stu-id="d14df-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="d14df-132">Sempre use versões dos pacotes de ferramentas que correspondam à versão principal dos pacotes em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="d14df-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="d14df-133">Desenvolvimento do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d14df-133">Visual Studio development</span></span>

<span data-ttu-id="d14df-134">Você pode desenvolver vários tipos diferentes de aplicativos que tenham como destino .NET Core, .NET Framework ou outras plataformas compatíveis no EF Core usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d14df-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="d14df-135">Há duas maneiras de instalar um provedor de banco de dados do EF Core em seu aplicativo usando o Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d14df-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="d14df-136">Usando a [Interface do Usuário do Gerenciador de Pacotes](https://docs.microsoft.com/nuget/tools/package-manager-ui) do NuGet</span><span class="sxs-lookup"><span data-stu-id="d14df-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="d14df-137">Selecione no menu **Projeto > Gerenciar Pacotes NuGet**</span><span class="sxs-lookup"><span data-stu-id="d14df-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="d14df-138">Clique na guia **Procurar** ou **Atualizações**</span><span class="sxs-lookup"><span data-stu-id="d14df-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="d14df-139">Selecione o pacote `Microsoft.EntityFrameworkCore.SqlServer` e a versão desejada e confirme</span><span class="sxs-lookup"><span data-stu-id="d14df-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="d14df-140">Usando o [PMC (Console do Gerenciador de Pacotes)](https://docs.microsoft.com/nuget/tools/package-manager-console) do NuGet</span><span class="sxs-lookup"><span data-stu-id="d14df-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="d14df-141">Selecione no menu **Ferramentas > Gerenciador de Pacotes do NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="d14df-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="d14df-142">Digite e execute o seguinte comando no PMC:</span><span class="sxs-lookup"><span data-stu-id="d14df-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="d14df-143">Você pode usar o comando `Update-Package` em vez de atualizar um pacote que já esteja instalado para uma versão mais recente</span><span class="sxs-lookup"><span data-stu-id="d14df-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="d14df-144">Para especificar uma versão específica, você pode usar o modificador `-Version`, por exemplo, para instalar pacotes EF Core 2.0, acrescente `-Version 2.0.0` aos comandos</span><span class="sxs-lookup"><span data-stu-id="d14df-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="d14df-145">Ferramentas</span><span class="sxs-lookup"><span data-stu-id="d14df-145">Tools</span></span>

<span data-ttu-id="d14df-146">Também há uma versão do PowerShell dos comandos do [EF Core que é executada dentro do PMC](../../miscellaneous/cli/powershell.md) no Visual Studio, com recursos semelhantes aos dos comandos do `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="d14df-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="d14df-147">Para usá-los, instale o pacote `Microsoft.EntityFrameworkCore.Tools` usando a interface do usuário do Gerenciador de Pacotes ou o PMC.</span><span class="sxs-lookup"><span data-stu-id="d14df-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="d14df-148">Sempre use versões dos pacotes de ferramentas que correspondam à versão principal dos pacotes em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="d14df-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="d14df-149">Embora seja possível usar os comandos do `dotnet ef` do PMC no Visual Studio, é muito mais conveniente usar a versão do PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d14df-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="d14df-150">Eles funcionam automaticamente com o projeto atual selecionado no PMC sem necessidade de trocar manualmente os diretórios.</span><span class="sxs-lookup"><span data-stu-id="d14df-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="d14df-151">Eles abrem automaticamente os arquivos gerados pelos comandos no Visual Studio depois que o comando for concluído.</span><span class="sxs-lookup"><span data-stu-id="d14df-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="d14df-152">**Pacotes preterido no EF Core 2.0:** se você estiver fazendo o upgrade de um aplicativo existente para o EF Core 2.0, talvez seja necessário remover manualmente algumas referências aos pacotes mais antigos do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d14df-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="d14df-153">Em particular, pacotes de tempo de design do provedor do banco de dados, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, não são mais necessários nem compatíveis com o EF Core 2.0, mas não serão automaticamente removidos ao atualizar os outros pacotes.</span><span class="sxs-lookup"><span data-stu-id="d14df-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
