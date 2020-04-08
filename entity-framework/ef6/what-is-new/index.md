---
title: Novidades – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136134"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="4434d-102">Novidades no EF6</span><span class="sxs-lookup"><span data-stu-id="4434d-102">What's new in EF6</span></span>

<span data-ttu-id="4434d-103">É altamente recomendável que você use a versão mais recente lançada do Entity Framework para garantir os recursos mais recentes e maior estabilidade.</span><span class="sxs-lookup"><span data-stu-id="4434d-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="4434d-104">No entanto, sabemos que talvez você precise usar uma versão anterior, ou talvez queira fazer experiências com novos aprimoramentos no pré-lançamento mais recente.</span><span class="sxs-lookup"><span data-stu-id="4434d-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="4434d-105">Para instalar versões específicas do EF, consulte [Obter o Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="4434d-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-640"></a><span data-ttu-id="4434d-106">EF 6.4.0</span><span class="sxs-lookup"><span data-stu-id="4434d-106">EF 6.4.0</span></span>

<span data-ttu-id="4434d-107">O runtime do EF 6.4.0 foi lançado para o NuGet em dezembro de 2019.</span><span class="sxs-lookup"><span data-stu-id="4434d-107">The EF 6.4.0 runtime was released to NuGet in December  2019.</span></span> <span data-ttu-id="4434d-108">O objetivo principal do EF 6.4 é aprimorar os recursos e cenários fornecidos no EF 6.3.</span><span class="sxs-lookup"><span data-stu-id="4434d-108">The primary goal of EF 6.4 is to polish the features and scenarios that was delivered in EF 6.3.</span></span> <span data-ttu-id="4434d-109">Confira a [lista de correções importantes](https://github.com/dotnet/ef6/milestone/14?closed=1) no Github.</span><span class="sxs-lookup"><span data-stu-id="4434d-109">See [list of important fixes](https://github.com/dotnet/ef6/milestone/14?closed=1) on Github.</span></span>

## <a name="ef-630"></a><span data-ttu-id="4434d-110">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="4434d-110">EF 6.3.0</span></span>

<span data-ttu-id="4434d-111">O runtime do EF 6.3.0 foi lançado para o NuGet em setembro de 2019.</span><span class="sxs-lookup"><span data-stu-id="4434d-111">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="4434d-112">O principal objetivo desse lançamento é facilitar a migração de aplicativos existentes que usam o EF 6 para o .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="4434d-112">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="4434d-113">A comunidade também contribuiu com diversos aprimoramentos e correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="4434d-113">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="4434d-114">Consulte os problemas fechados em cada [marco](https://github.com/aspnet/EntityFramework6/milestones?state=closed) do 6.3.0 para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="4434d-114">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="4434d-115">Estes são alguns dos mais notáveis:</span><span class="sxs-lookup"><span data-stu-id="4434d-115">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="4434d-116">Suporte para .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="4434d-116">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="4434d-117">Agora, o pacote do EntityFramework é destinado ao .NET Standard 2.1, além do .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="4434d-117">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="4434d-118">Isso significa que o EF 6.3 é multiplataforma e tem suporte em outros sistemas operacionais além do Windows, como Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="4434d-118">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="4434d-119">Os comandos de migrações foram reescritos para executar fora do processo e trabalhar com projetos de estilo SDK.</span><span class="sxs-lookup"><span data-stu-id="4434d-119">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="4434d-120">Suporte para HierarchyId do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4434d-120">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="4434d-121">Melhoria da compatibilidade com PackageReference do Roslyn e do NuGet.</span><span class="sxs-lookup"><span data-stu-id="4434d-121">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="4434d-122">Adição do utilitário `ef6.exe` para habilitar, adicionar, executar scripts e aplicar migrações de assemblies.</span><span class="sxs-lookup"><span data-stu-id="4434d-122">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="4434d-123">Substitui o `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="4434d-123">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="4434d-124">Suporte ao designer do EF</span><span class="sxs-lookup"><span data-stu-id="4434d-124">EF designer support</span></span>

<span data-ttu-id="4434d-125">Atualmente, não há suporte para usar o designer do EF diretamente em projetos do .NET Core ou .NET Standard ou em um projeto do .NET Framework no estilo SDK.</span><span class="sxs-lookup"><span data-stu-id="4434d-125">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects or on an SDK-style .NET Framework project.</span></span> 

<span data-ttu-id="4434d-126">É possível contornar essa limitação adicionando o arquivo EDMX e as classes geradas para as entidades e o DbContext como arquivos vinculados a um projeto do .NET Core 3.0 ou .NET Standard 2.1 na mesma solução.</span><span class="sxs-lookup"><span data-stu-id="4434d-126">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="4434d-127">Os arquivos vinculados seriam semelhantes a este arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="4434d-127">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="4434d-128">Observe que o arquivo EDMX está vinculado à ação de build de EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="4434d-128">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="4434d-129">Essa é uma tarefa especial do MSBuild (agora incluída no pacote do EF 6.3), que adiciona o modelo do EF no assembly de destino como recursos incorporados ou o copia como arquivos na pasta de saída, dependendo da configuração de processamento de artefatos de metadados no EDMX.</span><span class="sxs-lookup"><span data-stu-id="4434d-129">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="4434d-130">Para obter mais detalhes sobre como obter essa configuração, confira nosso [Exemplo de EDMX no .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="4434d-130">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

<span data-ttu-id="4434d-131">Aviso: verifique se o projeto do .NET Framework do estilo antigo (ou seja, não SDK) que define o arquivo .edmx "real" é _anterior_ ao projeto que define o link dentro do arquivo .sln.</span><span class="sxs-lookup"><span data-stu-id="4434d-131">Warning: make sure the old style (i.e. non-SDK-style) .NET Framework project defining the "real" .edmx file comes _before_ the project defining the link inside the .sln file.</span></span> <span data-ttu-id="4434d-132">Caso contrário, ao abrir o arquivo .edmx no designer, você verá a mensagem de erro "O Entity Framework não está disponível na estrutura de destino especificada no momento para o projeto.</span><span class="sxs-lookup"><span data-stu-id="4434d-132">Otherwise, when you open the .edmx file in the designer, you see the error message "The Entity Framework is not available in the target framework currently specified for the project.</span></span> <span data-ttu-id="4434d-133">Você pode alterar a estrutura de destino do projeto ou editar o modelo no XmlEditor".</span><span class="sxs-lookup"><span data-stu-id="4434d-133">You can change the target framework of the project or edit the model in the XmlEditor".</span></span>

## <a name="past-releases"></a><span data-ttu-id="4434d-134">Versões anteriores</span><span class="sxs-lookup"><span data-stu-id="4434d-134">Past Releases</span></span>

<span data-ttu-id="4434d-135">A página [Versões Anteriores](past-releases.md) contém um arquivo morto de todas as versões anteriores do EF e os principais recursos que foram introduzidos em cada versão.</span><span class="sxs-lookup"><span data-stu-id="4434d-135">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
