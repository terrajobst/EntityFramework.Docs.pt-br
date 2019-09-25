---
title: Novidades – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: bb7038764644682c2149a8a500f342804d01f3d2
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198043"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="af47e-102">Novidades no EF6</span><span class="sxs-lookup"><span data-stu-id="af47e-102">What's new in EF6</span></span>

<span data-ttu-id="af47e-103">É altamente recomendável que você use a versão mais recente lançada do Entity Framework para garantir os recursos mais recentes e maior estabilidade.</span><span class="sxs-lookup"><span data-stu-id="af47e-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="af47e-104">No entanto, sabemos que talvez você precise usar uma versão anterior, ou talvez queira fazer experiências com novos aprimoramentos no pré-lançamento mais recente.</span><span class="sxs-lookup"><span data-stu-id="af47e-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="af47e-105">Para instalar versões específicas do EF, consulte [Obter o Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="af47e-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="af47e-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="af47e-106">EF 6.3.0</span></span>

<span data-ttu-id="af47e-107">O tempo de execução do EF 6.3.0 foi lançado para o NuGet em setembro de 2019.</span><span class="sxs-lookup"><span data-stu-id="af47e-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="af47e-108">O principal objetivo desse lançamento é facilitar a migração de aplicativos existentes que usam o EF 6 para o .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="af47e-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="af47e-109">A comunidade também contribuiu com diversos aprimoramentos e correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="af47e-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="af47e-110">Consulte os problemas fechados em cada [marco](https://github.com/aspnet/EntityFramework6/milestones?state=closed) do 6.3.0 para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="af47e-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="af47e-111">Estes são alguns dos mais notáveis:</span><span class="sxs-lookup"><span data-stu-id="af47e-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="af47e-112">Suporte para .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="af47e-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="af47e-113">Agora, o pacote do EntityFramework é destinado ao .NET Standard 2.1 além do .NET Framework 4.x</span><span class="sxs-lookup"><span data-stu-id="af47e-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="af47e-114">Os comandos de migração foram re-escritos para execução fora do processo e para funcionarem com projetos de estilo SDK</span><span class="sxs-lookup"><span data-stu-id="af47e-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="af47e-115">Suporte para HierarchyId do SQL Server</span><span class="sxs-lookup"><span data-stu-id="af47e-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="af47e-116">Melhoria da compatibilidade com PackageReference do Roslyn e do NuGet</span><span class="sxs-lookup"><span data-stu-id="af47e-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="af47e-117">Adição do utilitário `ef6.exe` para habilitar, adicionar, executar scripts e aplicar migrações de assemblies.</span><span class="sxs-lookup"><span data-stu-id="af47e-117">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="af47e-118">Substitui o `migrate.exe`</span><span class="sxs-lookup"><span data-stu-id="af47e-118">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="af47e-119">Suporte ao designer do EF</span><span class="sxs-lookup"><span data-stu-id="af47e-119">EF designer support</span></span>

<span data-ttu-id="af47e-120">Atualmente, não há suporte para usar o designer do EF diretamente em projetos do .NET Core ou .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="af47e-120">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="af47e-121">É possível contornar essa limitação adicionando o arquivo EDMX e as classes geradas para as entidades e o DbContext como arquivos vinculados a um projeto do .NET Core 3.0 ou .NET Standard 2.1 na mesma solução.</span><span class="sxs-lookup"><span data-stu-id="af47e-121">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="af47e-122">Os arquivos vinculados seriam semelhantes a este arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="af47e-122">The linked files will look like this in the project file:</span></span>

``` csproj 
&lt;ItemGroup&gt;
  &lt;EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" /&gt;
&lt;/ItemGroup&gt;
```

<span data-ttu-id="af47e-123">Observe que o arquivo EDMX está vinculado à ação de build de EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="af47e-123">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="af47e-124">Essa é uma tarefa especial do MSBuild (agora incluída no pacote do EF 6.3), que adiciona o modelo do EF no assembly de destino como recursos incorporados ou o copia como arquivos na pasta de saída, dependendo da configuração de processamento de artefatos de metadados no EDMX.</span><span class="sxs-lookup"><span data-stu-id="af47e-124">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="af47e-125">Para obter mais detalhes sobre como obter essa configuração, confira nosso [Exemplo de EDMX no .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="af47e-125">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="af47e-126">Versões anteriores</span><span class="sxs-lookup"><span data-stu-id="af47e-126">Past Releases</span></span>

<span data-ttu-id="af47e-127">A página [Versões Anteriores](past-releases.md) contém um arquivo morto de todas as versões anteriores do EF e os principais recursos que foram introduzidos em cada versão.</span><span class="sxs-lookup"><span data-stu-id="af47e-127">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
