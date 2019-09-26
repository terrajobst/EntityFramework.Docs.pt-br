---
title: Novidades – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: c49f4cba0066d1e218f11c3959d96f9cafa913f4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266791"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="74b84-102">Novidades no EF6</span><span class="sxs-lookup"><span data-stu-id="74b84-102">What's new in EF6</span></span>

<span data-ttu-id="74b84-103">É altamente recomendável que você use a versão mais recente lançada do Entity Framework para garantir os recursos mais recentes e maior estabilidade.</span><span class="sxs-lookup"><span data-stu-id="74b84-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="74b84-104">No entanto, sabemos que talvez você precise usar uma versão anterior, ou talvez queira fazer experiências com novos aprimoramentos no pré-lançamento mais recente.</span><span class="sxs-lookup"><span data-stu-id="74b84-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="74b84-105">Para instalar versões específicas do EF, consulte [Obter o Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="74b84-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="74b84-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="74b84-106">EF 6.3.0</span></span>

<span data-ttu-id="74b84-107">O tempo de execução do EF 6.3.0 foi lançado para o NuGet em setembro de 2019.</span><span class="sxs-lookup"><span data-stu-id="74b84-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="74b84-108">O principal objetivo desse lançamento é facilitar a migração de aplicativos existentes que usam o EF 6 para o .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="74b84-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="74b84-109">A comunidade também contribuiu com diversos aprimoramentos e correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="74b84-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="74b84-110">Consulte os problemas fechados em cada [marco](https://github.com/aspnet/EntityFramework6/milestones?state=closed) do 6.3.0 para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="74b84-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="74b84-111">Estes são alguns dos mais notáveis:</span><span class="sxs-lookup"><span data-stu-id="74b84-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="74b84-112">Suporte para .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="74b84-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="74b84-113">Agora, o pacote do EntityFramework é destinado ao .NET Standard 2.1, além do .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="74b84-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="74b84-114">Isso significa que o EF 6.3 é multiplataforma e tem suporte em outros sistemas operacionais além do Windows, como Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="74b84-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="74b84-115">Os comandos de migrações foram reescritos para executar fora do processo e trabalhar com projetos de estilo SDK.</span><span class="sxs-lookup"><span data-stu-id="74b84-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="74b84-116">Suporte para HierarchyId do SQL Server</span><span class="sxs-lookup"><span data-stu-id="74b84-116">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="74b84-117">Melhoria da compatibilidade com PackageReference do Roslyn e do NuGet</span><span class="sxs-lookup"><span data-stu-id="74b84-117">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="74b84-118">Adição do utilitário `ef6.exe` para habilitar, adicionar, executar scripts e aplicar migrações de assemblies.</span><span class="sxs-lookup"><span data-stu-id="74b84-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="74b84-119">Substitui o `migrate.exe`</span><span class="sxs-lookup"><span data-stu-id="74b84-119">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="74b84-120">Suporte ao designer do EF</span><span class="sxs-lookup"><span data-stu-id="74b84-120">EF designer support</span></span>

<span data-ttu-id="74b84-121">Atualmente, não há suporte para usar o designer do EF diretamente em projetos do .NET Core ou .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="74b84-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="74b84-122">É possível contornar essa limitação adicionando o arquivo EDMX e as classes geradas para as entidades e o DbContext como arquivos vinculados a um projeto do .NET Core 3.0 ou .NET Standard 2.1 na mesma solução.</span><span class="sxs-lookup"><span data-stu-id="74b84-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="74b84-123">Os arquivos vinculados seriam semelhantes a este arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="74b84-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="74b84-124">Observe que o arquivo EDMX está vinculado à ação de build de EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="74b84-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="74b84-125">Essa é uma tarefa especial do MSBuild (agora incluída no pacote do EF 6.3), que adiciona o modelo do EF no assembly de destino como recursos incorporados ou o copia como arquivos na pasta de saída, dependendo da configuração de processamento de artefatos de metadados no EDMX.</span><span class="sxs-lookup"><span data-stu-id="74b84-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="74b84-126">Para obter mais detalhes sobre como obter essa configuração, confira nosso [Exemplo de EDMX no .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="74b84-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="74b84-127">Versões anteriores</span><span class="sxs-lookup"><span data-stu-id="74b84-127">Past Releases</span></span>

<span data-ttu-id="74b84-128">A página [Versões Anteriores](past-releases.md) contém um arquivo morto de todas as versões anteriores do EF e os principais recursos que foram introduzidos em cada versão.</span><span class="sxs-lookup"><span data-stu-id="74b84-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
