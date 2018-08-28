---
title: Referência de Linha de Comando – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.openlocfilehash: 757d6562f5d3bcd4f026def02f208f5827786873
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997064"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="c12d9-102">Ferramentas do Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c12d9-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="c12d9-103">As Ferramentas do Entity Framework Core ajudam durante o desenvolvimento de aplicativos EF Core.</span><span class="sxs-lookup"><span data-stu-id="c12d9-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="c12d9-104">Eles são usados principalmente para o scaffold de DbContext e tipos de entidade fazendo engenharia reversa do esquema de um banco de dados e para gerenciar as Migrações.</span><span class="sxs-lookup"><span data-stu-id="c12d9-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="c12d9-105">As [Ferramentas PMC (Console do Gerenciador de Pacotes) do EF Core][1] oferece uma experiência superior no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c12d9-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="c12d9-106">Execute-as usando o [PMC (Console do Gerenciador de Pacotes)][2] do NuGet.</span><span class="sxs-lookup"><span data-stu-id="c12d9-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="c12d9-107">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c12d9-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="c12d9-108">As [Ferramentas de Linha de Comando do EF Core .NET][3] são uma extensão das [Ferramentas da CLI (Interface de Linha de Comando) do .NET Core][4] que são de multiplaforma e podem ser executadas fora do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c12d9-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="c12d9-109">Essas ferramentas exigem um projeto de SDK do .NET Core (com `Sdk="Microsoft.NET.Sdk"` ou semelhantes no arquivo de projeto).</span><span class="sxs-lookup"><span data-stu-id="c12d9-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="c12d9-110">Ambas as ferramentas expõem a mesma funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="c12d9-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="c12d9-111">Se você estiver desenvolvendo no Visual Studio, é recomendável usar as Ferramentas de PMC, pois elas oferecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="c12d9-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="c12d9-112">Estruturas</span><span class="sxs-lookup"><span data-stu-id="c12d9-112">Frameworks</span></span>
----------
<span data-ttu-id="c12d9-113">As ferramentas são compatíveis com projetos voltados para o .NET Framework ou o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c12d9-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="c12d9-114">Se você quiser usar uma biblioteca de classes, considere usar uma biblioteca de classes .NET Framework ou .NET Core, se possível.</span><span class="sxs-lookup"><span data-stu-id="c12d9-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="c12d9-115">Assim, ocorrerão menos problemas com as ferramentas do .NET.</span><span class="sxs-lookup"><span data-stu-id="c12d9-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="c12d9-116">Se, em vez disso, você quiser utilizar uma biblioteca de classes do .NET Standard, será necessário usar um projeto de inicialização que direcione para o .NET Framework ou .NET Core. Desse modo, as ferramentas terão uma plataforma de destino concreta, na qual poderão carregar a biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="c12d9-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="c12d9-117">Esse projeto de inicialização pode ser fictício, sem nenhum código real – ele só é necessário para fornecer um destino para as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="c12d9-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="c12d9-118">Se o projeto direcionar para outra estrutura (por exemplo, Plataforma Universal do Windows ou Xamarin), será necessário criar uma biblioteca de classes .NET Standard separada.</span><span class="sxs-lookup"><span data-stu-id="c12d9-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="c12d9-119">Nesse caso, siga as diretrizes acima para criar um projeto de inicialização que possa ser usado pelas ferramentas.</span><span class="sxs-lookup"><span data-stu-id="c12d9-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="c12d9-120">Inicialização e projetos de destino</span><span class="sxs-lookup"><span data-stu-id="c12d9-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="c12d9-121">Sempre que você invoca um comando, existem dois projetos envolvidos: o projeto de destino e o projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c12d9-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="c12d9-122">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="c12d9-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="c12d9-123">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="c12d9-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="c12d9-124">O projeto de destino e o projeto de inicialização podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="c12d9-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
