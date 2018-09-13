---
title: Obter o Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 7f840a4f9e437ec12f699184339e386976e1528b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490620"
---
# <a name="get-entity-framework"></a><span data-ttu-id="fe756-102">Obter o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fe756-102">Get Entity Framework</span></span>
<span data-ttu-id="fe756-103">Entity Framework é composto das ferramentas do EF para Visual Studio e o tempo de execução do EF.</span><span class="sxs-lookup"><span data-stu-id="fe756-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="fe756-104">Ferramentas do EF para Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fe756-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="fe756-105">O Entity Framework Tools para Visual Studio incluem o EF Designer e o Assistente de modelo do EF e são necessário para o banco de dados pela primeira vez e modelar fluxos de trabalho primeiro.</span><span class="sxs-lookup"><span data-stu-id="fe756-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="fe756-106">Ferramentas do EF estão incluídas em todas as versões recentes do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe756-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="fe756-107">Se você executar uma instalação personalizada do Visual Studio, você precisará garantir que o item "Ferramentas do Entity Framework 6" é selecionada escolhendo uma carga de trabalho que inclui-lo ou selecionando-o como um componente individual.</span><span class="sxs-lookup"><span data-stu-id="fe756-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="fe756-108">Para algumas versões anteriores do Visual Studio, ferramentas atualizadas do EF estão disponíveis para download.</span><span class="sxs-lookup"><span data-stu-id="fe756-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="fe756-109">Ver [versões do Visual Studio](~/ef6/what-is-new/visual-studio.md) para obter orientação sobre como obter a versão mais recente das ferramentas do EF disponíveis para sua versão do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe756-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="fe756-110">Tempo de execução do EF</span><span class="sxs-lookup"><span data-stu-id="fe756-110">EF Runtime</span></span>

<span data-ttu-id="fe756-111">A versão mais recente do Entity Framework está disponível como a [pacote EntityFramework NuGet](http://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="fe756-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="fe756-112">Se você não estiver familiarizado com o Gerenciador de pacotes do NuGet, recomendamos que você leia as [visão geral do NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="fe756-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="fe756-113">Instalando o pacote do NuGet do EF</span><span class="sxs-lookup"><span data-stu-id="fe756-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="fe756-114">Você pode instalar o pacote EntityFramework clicando com o **referências** pasta do seu projeto e selecionando **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="fe756-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Gerenciar pacotes NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="fe756-116">Instalação do Console do Gerenciador de pacotes</span><span class="sxs-lookup"><span data-stu-id="fe756-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="fe756-117">Como alternativa, você pode instalar o EntityFramework, executando o seguinte comando na [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="fe756-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="fe756-118">Instalar uma versão específica do EF</span><span class="sxs-lookup"><span data-stu-id="fe756-118">Installing a specific version of EF</span></span>

<span data-ttu-id="fe756-119">Do EF 4.1 em diante, as novas versões do tempo de execução do EF foi lançadas como o [pacote do NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="fe756-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="fe756-120">Qualquer uma dessas versões podem ser adicionados a um projeto com base no .NET Framework, executando o seguinte comando no Visual Studio [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="fe756-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="fe756-121">Observe que `<number>` representa a versão específica do EF para instalar.</span><span class="sxs-lookup"><span data-stu-id="fe756-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="fe756-122">Por exemplo, 6.2.0 é a versão de número para o EF 6.2.</span><span class="sxs-lookup"><span data-stu-id="fe756-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="fe756-123">Tempos de execução do EF antes 4.1 faziam parte do .NET Framework e não podem ser instalados separadamente.</span><span class="sxs-lookup"><span data-stu-id="fe756-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="fe756-124">Instalar a visualização mais recente</span><span class="sxs-lookup"><span data-stu-id="fe756-124">Installing the Latest Preview</span></span>

<span data-ttu-id="fe756-125">Os métodos acima lhe dará a versão mais recente com suporte total a versão do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fe756-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="fe756-126">Geralmente há versões de pré-lançamento do Entity Framework disponível que Adoraríamos que você experimente e nos fornecer comentários sobre.</span><span class="sxs-lookup"><span data-stu-id="fe756-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="fe756-127">Para instalar a visualização mais recente do EntityFramework, você pode selecionar **Include Prerelease** na janela Gerenciar pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="fe756-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="fe756-128">Se não há versões de pré-lançamento estão disponível automaticamente obterá a versão mais recente versão com suporte total do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fe756-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Incluir pré-lançamento](~/ef6/media/includeprerelease.png)

<span data-ttu-id="fe756-130">Como alternativa, você pode executar o comando a seguir [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="fe756-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
