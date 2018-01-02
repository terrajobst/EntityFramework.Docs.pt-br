---
title: "Referência de Linha de Comando – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="5db23-102">Ferramentas do Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5db23-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="5db23-103">As Ferramentas do Entity Framework Core ajudam durante o desenvolvimento de aplicativos EF Core.</span><span class="sxs-lookup"><span data-stu-id="5db23-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="5db23-104">Eles são usados principalmente para o scaffold de DbContext e tipos de entidade fazendo engenharia reversa do esquema de um banco de dados e para gerenciar as Migrações.</span><span class="sxs-lookup"><span data-stu-id="5db23-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="5db23-105">As [Ferramentas PMC (Console do Gerenciador de Pacotes) do EF Core][1] oferece uma experiência superior no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5db23-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="5db23-106">Execute-as usando o [PMC (Console do Gerenciador de Pacotes)][2] do NuGet.</span><span class="sxs-lookup"><span data-stu-id="5db23-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="5db23-107">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5db23-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="5db23-108">As [Ferramentas de Linha de Comando do EF Core .NET][3] são uma extensão das [Ferramentas da CLI (Interface de Linha de Comando) do .NET Core][4] que são de multiplaforma e podem ser executadas fora do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5db23-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="5db23-109">Essas ferramentas exigem um projeto de SDK do .NET Core (com `Sdk="Microsoft.NET.Sdk"` ou semelhantes no arquivo de projeto).</span><span class="sxs-lookup"><span data-stu-id="5db23-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="5db23-110">Ambas as ferramentas expõem a mesma funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="5db23-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="5db23-111">Se você estiver desenvolvendo no Visual Studio, é recomendável usar as Ferramentas de PMC, pois elas oferecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="5db23-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="5db23-112">Estruturas</span><span class="sxs-lookup"><span data-stu-id="5db23-112">Frameworks</span></span>
----------
<span data-ttu-id="5db23-113">As ferramentas são compatíveis com projetos voltados para o .NET Framework ou o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5db23-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="5db23-114">Se o seu projeto tiver como destino outra estrutura (por exemplo, Universal do Windows ou Xamarin), é recomendável criar um projeto .NET Standard separado e fazer o direcionamento cruzado das estruturas compatíveis.</span><span class="sxs-lookup"><span data-stu-id="5db23-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="5db23-115">Para fazer o direcionamento cruzado do .NET Core, por exemplo, clique com o botão direito do mouse no projeto e selecione **Editar \*.csproj**.</span><span class="sxs-lookup"><span data-stu-id="5db23-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="5db23-116">Atualize a propriedade `TargetFramework` da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="5db23-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="5db23-117">(Observe que o nome da propriedade passa a estar no plural.)</span><span class="sxs-lookup"><span data-stu-id="5db23-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="5db23-118">Se você estiver usando uma biblioteca de classes .NET Standard, não precisará fazer o direcionamento cruzado caso seu projeto de inicialização tenha como destino o .NET Framework ou o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5db23-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="5db23-119">Inicialização e projetos de destino</span><span class="sxs-lookup"><span data-stu-id="5db23-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="5db23-120">Sempre que você invoca um comando, existem dois projetos envolvidos: o projeto de destino e o projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="5db23-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="5db23-121">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="5db23-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="5db23-122">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5db23-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="5db23-123">O projeto de destino e o projeto de inicialização podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="5db23-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
