---
title: Instalar o EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 00924af2a7beaba8575e12d91678208b517f1a09
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614266"
---
# <a name="installing-ef-core"></a><span data-ttu-id="0bce7-102">Instalar o EF Core</span><span class="sxs-lookup"><span data-stu-id="0bce7-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0bce7-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="0bce7-103">Prerequisites</span></span>

<span data-ttu-id="0bce7-104">Para desenvolver aplicativos .NET Core 2.1 (incluindo aplicativos ASP.NET Core 2.1 que têm como destino o .NET Core), será necessário baixar e instalar uma versão do [SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core) apropriado para a sua plataforma.</span><span class="sxs-lookup"><span data-stu-id="0bce7-104">In order to develop .NET Core 2.1 applications (including ASP.NET Core 2.1 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="0bce7-105">**Isso vale mesmo que você tenha instalado o Visual Studio 2017 versão 15.7.**</span><span class="sxs-lookup"><span data-stu-id="0bce7-105">**This is true even if you have installed Visual Studio 2017 version 15.7.**</span></span>

<span data-ttu-id="0bce7-106">Para usar o EF Core 2.1 ou qualquer outra biblioteca .NET Standard 2.0 padrão com uma plataforma .NET além do .NET Core 2.1 (por exemplo, com o .NET Framework 4.6.1 ou superior), você precisará de uma versão do NuGet que reconheça o padrão .NET 2.0 e suas estruturas compatíveis.</span><span class="sxs-lookup"><span data-stu-id="0bce7-106">In order to use EF Core 2.1 or any other .NET Standard 2.0 library with a .NET platform besides .NET Core 2.1 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="0bce7-107">Aqui estão algumas maneiras de obter isso:</span><span class="sxs-lookup"><span data-stu-id="0bce7-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="0bce7-108">Instalar o Visual Studio 2017 versão 15.7</span><span class="sxs-lookup"><span data-stu-id="0bce7-108">Install Visual Studio 2017 version 15.7</span></span>
* <span data-ttu-id="0bce7-109">Se você estiver usando o Visual Studio 2015, [baixe e atualize o cliente do NuGet para a versão 3.6.0](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="0bce7-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="0bce7-110">Projetos criados com versões anteriores do Visual Studio e que tenham .NET Framework como destino podem precisar de modificações adicionais para serem compatíveis com bibliotecas .NET 2.0 Standard:</span><span class="sxs-lookup"><span data-stu-id="0bce7-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="0bce7-111">Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:</span><span class="sxs-lookup"><span data-stu-id="0bce7-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="0bce7-112">Para projetos de teste, verifique também se a seguinte entrada está presente:</span><span class="sxs-lookup"><span data-stu-id="0bce7-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="0bce7-113">Obter os bits</span><span class="sxs-lookup"><span data-stu-id="0bce7-113">Getting the bits</span></span>
<span data-ttu-id="0bce7-114">A maneira recomendada de adicionar bibliotecas de tempo de execução do EF Core a um aplicativo é instalar um provedor de banco de dados do EF Core do NuGet.</span><span class="sxs-lookup"><span data-stu-id="0bce7-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="0bce7-115">Além das bibliotecas de tempo de execução, você pode instalar as ferramentas que facilitam executar várias tarefas relacionadas ao EF Core em seu projeto no tempo de design, como criar e aplicar migrações e criar um modelo com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="0bce7-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="0bce7-116">Se você precisar atualizar um aplicativo que esteja usando um provedor de banco de dados de terceiros, sempre verifique se há uma atualização do provedor compatível com a versão do EF Core que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="0bce7-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="0bce7-117">Por exemplo, provedores de banco de dados para versões anteriores não são compatíveis com a versão 2.1 do tempo de execução do EF Core.</span><span class="sxs-lookup"><span data-stu-id="0bce7-117">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="0bce7-118">Aplicativos que têm como destino o ASP.NET Core 2.1 podem usar o EF Core 2.1 sem dependências adicionais além de provedores de banco de dados de terceiros.</span><span class="sxs-lookup"><span data-stu-id="0bce7-118">Applications targeting ASP.NET Core 2.1 can use EF Core 2.1 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="0bce7-119">Aplicativos que têm como destino versões anteriores do ASP.NET Core precisam fazer upgrade para o ASP.NET Core 2.1 para usar o EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0bce7-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.1 in order to use EF Core 2.1.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="0bce7-120">Desenvolvimento de multiplaforma usando a CLI (Interface de Linha de Comando) do .NET Core</span><span class="sxs-lookup"><span data-stu-id="0bce7-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="0bce7-121">Para desenvolver aplicativos que têm como destino o [.NET Core](https://www.microsoft.com/net/download/core), você pode optar por usar [`dotnet` comandos da CLI](https://docs.microsoft.com/dotnet/core/tools/) combinados ao seu editor de texto favorito ou um IDE (Ambiente de Desenvolvimento Integrado), como o Visual Studio, o Visual Studio para Mac ou o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0bce7-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="0bce7-122">Aplicativos que têm como destino o .NET Core requerem versões específicas do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0bce7-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="0bce7-123">Por exemplo, o desenvolvimento do .NET Core 1.x requer o Visual Studio 2017, já o desenvolvimento do .NET Core 2.1 requer a versão 15.7 do Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0bce7-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.1 development requires Visual Studio 2017 version 15.7.</span></span>

<span data-ttu-id="0bce7-124">Para instalar ou fazer o upgrade para o provedor do SQL Server em um aplicativo .NET Core de multiplaforma, alterne para o diretório do aplicativo e execute o seguinte em uma linha de comando:</span><span class="sxs-lookup"><span data-stu-id="0bce7-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="0bce7-125">Você pode indicar a instalação de uma versão específica no comando `dotnet add package` usando o modificador `-v`.</span><span class="sxs-lookup"><span data-stu-id="0bce7-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="0bce7-126">Por exemplo, para instalar pacotes do EF Core 2.1, acrescente `-v 2.1.0` ao comando.</span><span class="sxs-lookup"><span data-stu-id="0bce7-126">For example, to install EF Core 2.1 packages, append `-v 2.1.0` to the command.</span></span>

<span data-ttu-id="0bce7-127">O EF Core inclui um conjunto de [comandos adicionais à `dotnet` CLI](../../miscellaneous/cli/dotnet.md), começando com `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="0bce7-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="0bce7-128">As ferramentas de CLI do .NET Core para o EF Core exigem um pacote chamado `Microsoft.EntityFrameworkCore.Design`.</span><span class="sxs-lookup"><span data-stu-id="0bce7-128">The .NET Core CLI tools for EF Core require a package called `Microsoft.EntityFrameworkCore.Design`.</span></span> <span data-ttu-id="0bce7-129">Você pode adicioná-lo ao projeto usando:</span><span class="sxs-lookup"><span data-stu-id="0bce7-129">You can add it to the project using:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> <span data-ttu-id="0bce7-130">Sempre use a versão do pacote de ferramentas que corresponda à versão principal dos pacotes em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0bce7-130">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="0bce7-131">Desenvolvimento do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0bce7-131">Visual Studio development</span></span>

<span data-ttu-id="0bce7-132">Você pode desenvolver vários tipos diferentes de aplicativos que tenham como destino .NET Core, .NET Framework ou outras plataformas compatíveis no EF Core usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0bce7-132">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="0bce7-133">Há duas maneiras de instalar um provedor de banco de dados do EF Core em seu aplicativo usando o Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0bce7-133">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="0bce7-134">Usando a [Interface do Usuário do Gerenciador de Pacotes](https://docs.microsoft.com/nuget/tools/package-manager-ui) do NuGet</span><span class="sxs-lookup"><span data-stu-id="0bce7-134">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="0bce7-135">Selecione no menu **Projeto > Gerenciar Pacotes NuGet**</span><span class="sxs-lookup"><span data-stu-id="0bce7-135">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="0bce7-136">Clique na guia **Procurar** ou **Atualizações**</span><span class="sxs-lookup"><span data-stu-id="0bce7-136">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="0bce7-137">Selecione o pacote `Microsoft.EntityFrameworkCore.SqlServer` e a versão desejada e confirme</span><span class="sxs-lookup"><span data-stu-id="0bce7-137">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="0bce7-138">Usando o [PMC (Console do Gerenciador de Pacotes)](https://docs.microsoft.com/nuget/tools/package-manager-console) do NuGet</span><span class="sxs-lookup"><span data-stu-id="0bce7-138">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="0bce7-139">Selecione no menu **Ferramentas > Gerenciador de Pacotes do NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="0bce7-139">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="0bce7-140">Digite e execute o seguinte comando no PMC:</span><span class="sxs-lookup"><span data-stu-id="0bce7-140">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="0bce7-141">Você pode usar o comando `Update-Package` em vez de atualizar um pacote que já esteja instalado para uma versão mais recente</span><span class="sxs-lookup"><span data-stu-id="0bce7-141">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="0bce7-142">Para especificar uma determinada versão, use o modificador `-Version`.</span><span class="sxs-lookup"><span data-stu-id="0bce7-142">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="0bce7-143">Por exemplo, para instalar pacotes do EF Core 2.1, acrescente `-Version 2.1.0` aos comandos</span><span class="sxs-lookup"><span data-stu-id="0bce7-143">For example, to install EF Core 2.1 packages, append `-Version 2.1.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="0bce7-144">Ferramentas</span><span class="sxs-lookup"><span data-stu-id="0bce7-144">Tools</span></span>

<span data-ttu-id="0bce7-145">Também há uma versão do PowerShell dos comandos do [EF Core que é executada dentro do PMC](../../miscellaneous/cli/powershell.md) no Visual Studio, com recursos semelhantes aos dos comandos do `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="0bce7-145">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> 

> [!TIP]  
> <span data-ttu-id="0bce7-146">Embora seja possível usar os comandos do `dotnet ef` do PMC no Visual Studio, é muito mais conveniente usar a versão do PowerShell:</span><span class="sxs-lookup"><span data-stu-id="0bce7-146">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="0bce7-147">Eles funcionam automaticamente com o projeto atual selecionado no PMC sem necessidade de trocar manualmente os diretórios.</span><span class="sxs-lookup"><span data-stu-id="0bce7-147">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="0bce7-148">Eles abrem automaticamente os arquivos gerados pelos comandos no Visual Studio depois que o comando for concluído.</span><span class="sxs-lookup"><span data-stu-id="0bce7-148">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="0bce7-149">**Pacotes preterido no EF Core 2.1:** se você estiver fazendo o upgrade de um aplicativo existente para o EF Core 2.1, talvez seja necessário remover manualmente algumas referências aos pacotes mais antigos do EF Core:</span><span class="sxs-lookup"><span data-stu-id="0bce7-149">**Deprecated packages in EF Core 2.1:** If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>
> * <span data-ttu-id="0bce7-150">Pacotes de tempo de design do provedor do banco de dados, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, não são mais necessários nem compatíveis com o EF Core 2.1, mas não serão automaticamente removidos ao atualizar os outros pacotes.</span><span class="sxs-lookup"><span data-stu-id="0bce7-150">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but will not be automatically removed when upgrading the other packages.</span></span>
> * <span data-ttu-id="0bce7-151">As ferramentas da CLI do .NET agora estão incluídas no SDK do .NET, portanto, a referência a esse pacote pode ser removida do arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="0bce7-151">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
