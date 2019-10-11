---
title: Obter Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181732"
---
# <a name="get-entity-framework"></a><span data-ttu-id="4b4da-102">Como obter o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4b4da-102">Get Entity Framework</span></span>
<span data-ttu-id="4b4da-103">Entity Framework é constituído das ferramentas do EF para Visual Studio e do tempo de execução do EF.</span><span class="sxs-lookup"><span data-stu-id="4b4da-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="4b4da-104">Ferramentas do EF para Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4b4da-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="4b4da-105">O Entity Framework Tools para o Visual Studio inclui o designer do EF e o assistente de modelo do EF e são necessários primeiro ao banco de dados e ao modelo dos primeiros fluxos de trabalho.</span><span class="sxs-lookup"><span data-stu-id="4b4da-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="4b4da-106">As ferramentas do EF estão incluídas em todas as versões recentes do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b4da-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="4b4da-107">Se você executar uma instalação personalizada do Visual Studio, precisará garantir que o item "Entity Framework 6 ferramentas" seja selecionado escolhendo uma carga de trabalho que o inclua ou selecionando-o como um componente individual.</span><span class="sxs-lookup"><span data-stu-id="4b4da-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="4b4da-108">Para algumas versões anteriores do Visual Studio, as ferramentas do EF atualizadas estão disponíveis como um download.</span><span class="sxs-lookup"><span data-stu-id="4b4da-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="4b4da-109">Consulte [versões do Visual Studio](~/ef6/what-is-new/visual-studio.md) para obter orientação sobre como obter a versão mais recente das ferramentas do EF disponíveis para sua versão do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b4da-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="4b4da-110">Tempo de execução do EF</span><span class="sxs-lookup"><span data-stu-id="4b4da-110">EF Runtime</span></span>

<span data-ttu-id="4b4da-111">A versão mais recente do Entity Framework está disponível como o [pacote NuGet do EntityFramework](https://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="4b4da-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="4b4da-112">Se você não estiver familiarizado com o Gerenciador de pacotes NuGet, recomendamos que leia a [visão geral do NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="4b4da-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="4b4da-113">Instalando o pacote NuGet do EF</span><span class="sxs-lookup"><span data-stu-id="4b4da-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="4b4da-114">Você pode instalar o pacote do EntityFramework clicando com o botão direito do mouse na pasta **referências** do seu projeto e selecionando **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="4b4da-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Gerenciar pacotes NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="4b4da-116">Instalando do console do Gerenciador de pacotes</span><span class="sxs-lookup"><span data-stu-id="4b4da-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="4b4da-117">Como alternativa, você pode instalar o EntityFramework executando o comando a seguir no [console do Gerenciador de pacotes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="4b4da-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="4b4da-118">Instalando uma versão específica do EF</span><span class="sxs-lookup"><span data-stu-id="4b4da-118">Installing a specific version of EF</span></span>

<span data-ttu-id="4b4da-119">Do EF 4,1 em diante, novas versões do tempo de execução do EF foram lançadas como o [pacote NuGet do EntityFramework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="4b4da-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="4b4da-120">Qualquer uma dessas versões pode ser adicionada a um projeto baseado em .NET Framework executando o seguinte comando no console do Gerenciador de [pacotes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)do Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4b4da-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="4b4da-121">Observe que `<number>` representa a versão específica do EF a ser instalada.</span><span class="sxs-lookup"><span data-stu-id="4b4da-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="4b4da-122">Por exemplo, 6.2.0 é a versão do número para o EF 6,2.</span><span class="sxs-lookup"><span data-stu-id="4b4da-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="4b4da-123">Os tempos de execução do EF anteriores a 4,1 faziam parte do .NET Framework e não podem ser instalados separadamente.</span><span class="sxs-lookup"><span data-stu-id="4b4da-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="4b4da-124">Instalando a versão prévia mais recente</span><span class="sxs-lookup"><span data-stu-id="4b4da-124">Installing the Latest Preview</span></span>

<span data-ttu-id="4b4da-125">Os métodos acima fornecerão a versão mais recente com suporte total do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4b4da-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="4b4da-126">Geralmente, há versões de pré-lançamento do Entity Framework disponíveis que adoraríamos que você experimente e nos envie comentários.</span><span class="sxs-lookup"><span data-stu-id="4b4da-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="4b4da-127">Para instalar a versão prévia mais recente do EntityFramework, você pode selecionar **incluir pré-lançamento** na janela gerenciar pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="4b4da-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="4b4da-128">Se não houver versões de pré-lançamento disponíveis, você obterá automaticamente a versão mais recente com suporte total do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4b4da-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Incluir pré-lançamento](~/ef6/media/includeprerelease.png)

<span data-ttu-id="4b4da-130">Como alternativa, você pode executar o comando a seguir no [console do Gerenciador de pacotes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="4b4da-130">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
