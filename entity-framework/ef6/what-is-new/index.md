---
title: Novidades – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149126"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="a2e5c-102">Novidades no EF6</span><span class="sxs-lookup"><span data-stu-id="a2e5c-102">What's New in EF6</span></span>

<span data-ttu-id="a2e5c-103">É altamente recomendável que você use a versão mais recente lançada do Entity Framework para garantir os recursos mais recentes e maior estabilidade.</span><span class="sxs-lookup"><span data-stu-id="a2e5c-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="a2e5c-104">No entanto, sabemos que talvez você precise usar uma versão anterior, ou talvez queira fazer experiências com novos aprimoramentos no pré-lançamento mais recente.</span><span class="sxs-lookup"><span data-stu-id="a2e5c-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="a2e5c-105">Para instalar versões específicas do EF, consulte [Obter o Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="a2e5c-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="a2e5c-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="a2e5c-106">EF 6.3.0</span></span>

<span data-ttu-id="a2e5c-107">O tempo de execução do EF 6.3.0 foi lançado para o NuGet em setembro de 2019.</span><span class="sxs-lookup"><span data-stu-id="a2e5c-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="a2e5c-108">O principal objetivo desse lançamento é facilitar a migração de aplicativos existentes que usam o EF 6 para o .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="a2e5c-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="a2e5c-109">A comunidade também contribuiu com diversos aprimoramentos e correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="a2e5c-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="a2e5c-110">Consulte os problemas fechados em cada [marco](https://github.com/aspnet/EntityFramework6/milestones?state=closed) do 6.3.0 para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="a2e5c-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="a2e5c-111">Estes são alguns dos mais notáveis:</span><span class="sxs-lookup"><span data-stu-id="a2e5c-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="a2e5c-112">Suporte para .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="a2e5c-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="a2e5c-113">Agora, o pacote do EntityFramework é destinado ao .NET Standard 2.1 além do .NET Framework 4.x</span><span class="sxs-lookup"><span data-stu-id="a2e5c-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="a2e5c-114">Os comandos de migração foram re-escritos para execução fora do processo e para funcionarem com projetos de estilo SDK</span><span class="sxs-lookup"><span data-stu-id="a2e5c-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="a2e5c-115">Suporte para HierarchyId do SQL Server</span><span class="sxs-lookup"><span data-stu-id="a2e5c-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="a2e5c-116">Melhoria da compatibilidade com PackageReference do Roslyn e do NuGet</span><span class="sxs-lookup"><span data-stu-id="a2e5c-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="a2e5c-117">Adição de ef6.exe para habilitar, adicionar, executar scripts e aplicar migrações de assemblies.</span><span class="sxs-lookup"><span data-stu-id="a2e5c-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="a2e5c-118">Ele substitui migrate.exe</span><span class="sxs-lookup"><span data-stu-id="a2e5c-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="a2e5c-119">Versões anteriores</span><span class="sxs-lookup"><span data-stu-id="a2e5c-119">Past Releases</span></span>

<span data-ttu-id="a2e5c-120">A página [Versões Anteriores](past-releases.md) contém um arquivo morto de todas as versões anteriores do EF e os principais recursos que foram introduzidos em cada versão.</span><span class="sxs-lookup"><span data-stu-id="a2e5c-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
