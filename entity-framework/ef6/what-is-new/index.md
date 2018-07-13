---
title: Novidades – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: dba6403fa341e1abfe8da488a19cf8520e3ea574
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914170"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="573bb-102">Novidades no EF6</span><span class="sxs-lookup"><span data-stu-id="573bb-102">What's New in EF6</span></span>

<span data-ttu-id="573bb-103">É altamente recomendável que você use a versão mais recente lançada do Entity Framework para garantir os recursos mais recentes e maior estabilidade.</span><span class="sxs-lookup"><span data-stu-id="573bb-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="573bb-104">No entanto, sabemos que talvez você precise usar uma versão anterior, ou talvez queira fazer experiências com novos aprimoramentos no pré-lançamento mais recente.</span><span class="sxs-lookup"><span data-stu-id="573bb-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="573bb-105">Para instalar versões específicas do EF, consulte [Obter o Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="573bb-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="573bb-106">Esta página documenta os recursos que são incluídos em cada nova versão.</span><span class="sxs-lookup"><span data-stu-id="573bb-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="573bb-107">Versões recentes</span><span class="sxs-lookup"><span data-stu-id="573bb-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="573bb-108">Atualização de ferramentas do EF no Visual Studio 2017 15.7</span><span class="sxs-lookup"><span data-stu-id="573bb-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="573bb-109">Em maio de 2018, lançamos uma versão atualizada das ferramentas do EF como parte do Visual Studio 2017 15.7.</span><span class="sxs-lookup"><span data-stu-id="573bb-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="573bb-110">Ela inclui melhorias para alguns pontos de problemas comuns:</span><span class="sxs-lookup"><span data-stu-id="573bb-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="573bb-111">Correções para vários bugs de acessibilidade da interface do usuário</span><span class="sxs-lookup"><span data-stu-id="573bb-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="573bb-112">Solução alternativa para a regressão de desempenho do SQL Server ao gerar modelos de bancos de dados existentes [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="573bb-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="573bb-113">Suporte para atualizar modelos maiores no SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="573bb-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="573bb-114">Outra melhoria nessa nova versão das ferramentas do EF é que ela instala o tempo de execução do EF 6.2 ao criar um modelo em um novo projeto.</span><span class="sxs-lookup"><span data-stu-id="573bb-114">Another improvement in this this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="573bb-115">Com versões mais antigas do Visual Studio, é possível usar o tempo de execução do EF 6.2 (bem como qualquer versão anterior do EF) instalando a versão correspondente do pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="573bb-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="573bb-116">Tempo de execução do EF 6.2</span><span class="sxs-lookup"><span data-stu-id="573bb-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="573bb-117">O tempo de execução do EF 6.2 foi lançado para o NuGet em outubro de 2017.</span><span class="sxs-lookup"><span data-stu-id="573bb-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="573bb-118">Graças, em grande parte, aos esforços de nossa comunidade de colaboradores de software livre, o EF 6.2 inclui várias [correções de bugs](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) e [aprimoramentos de produtos](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="573bb-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="573bb-119">Aqui está uma lista resumida das alterações mais importantes que afetam o tempo de execução do EF 6.2:</span><span class="sxs-lookup"><span data-stu-id="573bb-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="573bb-120">Redução do tempo de inicialização carregando modelos do Code First concluídos de um cache persistente [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="573bb-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="573bb-121">API fluente para definir índices [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="573bb-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="573bb-122">DbFunctions.Like() para habilitar a gravação de consultas LINQ que são traduzidas como LIKE no SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="573bb-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="573bb-123">Agora Migrate.exe é compatível com a opção -script [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="573bb-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="573bb-124">Agora o EF6 pode trabalhar com valores de chave gerados por uma sequência no SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="573bb-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="573bb-125">Atualização da lista de erros transitórios para a estratégia de execução do SQL Azure [n#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="573bb-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="573bb-126">Bug: a repetição de consultas ou comandos SQL falha com "O SqlParameter já está contido em outro SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="573bb-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="573bb-127">Bug: a avaliação de DbQuery.ToString() frequentemente atinge o tempo limite no depurador [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="573bb-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="573bb-128">Versões futuras</span><span class="sxs-lookup"><span data-stu-id="573bb-128">Future Releases</span></span>

<span data-ttu-id="573bb-129">Para obter informações sobre a versão futura do EF6, examine nosso [roteiro](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="573bb-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="573bb-130">Versões anteriores</span><span class="sxs-lookup"><span data-stu-id="573bb-130">Past Releases</span></span>

<span data-ttu-id="573bb-131">A página [Versões Anteriores](past-releases.md) contém um arquivo morto de todas as versões anteriores do EF e os principais recursos que foram introduzidos em cada versão.</span><span class="sxs-lookup"><span data-stu-id="573bb-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
