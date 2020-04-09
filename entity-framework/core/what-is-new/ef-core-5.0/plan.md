---
title: Plano para o Núcleo-Quadro de Entidades 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136233"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="c7cb0-102">Plano para o Núcleo-Quadro de Entidades 5.0</span><span class="sxs-lookup"><span data-stu-id="c7cb0-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="c7cb0-103">Como descrito no [processo de planejamento,](../release-planning.md)reunimos contribuições das partes interessadas em um plano provisório para a versão do EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="c7cb0-104">Este plano ainda é um trabalho em andamento.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="c7cb0-105">Nada aqui é um compromisso.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-105">Nothing here is a commitment.</span></span> <span data-ttu-id="c7cb0-106">Este plano é um ponto de partida que evoluirá à medida que aprendermos mais.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="c7cb0-107">Algumas coisas não planejadas para 5.0 podem ser puxadas para dentro.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="c7cb0-108">Algumas coisas atualmente planejadas para 5.0 podem ser pontuadas.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="c7cb0-109">Número da versão e data de lançamento.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-109">Version number and release date.</span></span>

<span data-ttu-id="c7cb0-110">O EF Core 5.0 está atualmente programado para ser lançado [ao mesmo tempo que .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="c7cb0-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="c7cb0-111">A versão "5.0" foi escolhida para alinhar com .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="c7cb0-112">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="c7cb0-112">Supported platforms</span></span>

<span data-ttu-id="c7cb0-113">O EF Core 5.0 está planejado para ser executado em qualquer plataforma .NET 5.0 com base na [convergência dessas plataformas para .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="c7cb0-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="c7cb0-114">O que isso significa em termos de .NET Standard e o TFM real usado ainda é TBD.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="c7cb0-115">O EF Core 5.0 não será executado no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="c7cb0-116">Alterações de quebra</span><span class="sxs-lookup"><span data-stu-id="c7cb0-116">Breaking changes</span></span>

<span data-ttu-id="c7cb0-117">O EF Core 5.0 conterá algumas alterações de ruptura, mas estas serão muito menos graves do que foi o caso do EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="c7cb0-118">Nosso objetivo é permitir que a grande maioria dos aplicativos se atualize sem quebrar.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="c7cb0-119">Espera-se que haja algumas mudanças de quebra para os provedores de banco de dados, especialmente em torno do suporte ao TPT.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="c7cb0-120">No entanto, esperamos que o trabalho para atualizar um provedor para 5.0 será menor do que o necessário para atualizar para 3.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="c7cb0-121">Temas</span><span class="sxs-lookup"><span data-stu-id="c7cb0-121">Themes</span></span>

<span data-ttu-id="c7cb0-122">Extraímos algumas áreas ou temas importantes que formarão a base para os grandes investimentos no Núcleo 5.0 da EF.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="c7cb0-123">Muitas a muitas propriedades de navegação (também conhecido como "skip navigations")</span><span class="sxs-lookup"><span data-stu-id="c7cb0-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="c7cb0-124">Desenvolvedores líderes: @smitpatel e@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="c7cb0-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="c7cb0-125">Rastreado por [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="c7cb0-126">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-126">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-127">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-127">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-128">Muitos para muitos é o [recurso mais solicitado](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votos) no backlog do GitHub.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="c7cb0-129">O apoio a muitos relacionamentos em sua totalidade é acompanhado como [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span><span class="sxs-lookup"><span data-stu-id="c7cb0-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="c7cb0-130">Isso pode ser dividido em três grandes áreas:</span><span class="sxs-lookup"><span data-stu-id="c7cb0-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="c7cb0-131">Pular propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-131">Skip navigation properties.</span></span> <span data-ttu-id="c7cb0-132">Estes permitem que o modelo seja usado para consultas, etc. sem referência à entidade de tabela de adesão subjacente.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="c7cb0-133">[(#19003)](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="c7cb0-134">Tipos de entidade sacolas de propriedade.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-134">Property-bag entity types.</span></span> <span data-ttu-id="c7cb0-135">Estes permitem que um tipo padrão `Dictionary`de CLR (por exemplo) seja usado para instâncias de entidade, de tal forma que um tipo de CLR explícito não seja necessário para cada tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="c7cb0-136">(Alongamento para 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="c7cb0-137">Açúcar para fácil configuração de muitos a muitos relacionamentos.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="c7cb0-138">(Estique para 5.0.)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="c7cb0-139">Acreditamos que o bloqueador mais significativo para aqueles que querem apoio de muitos a muitos é não poder usar as relações "naturais", sem se referir à mesa de adesão, na lógica empresarial, como consultas.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="c7cb0-140">O tipo de entidade de mesa de adesão ainda pode existir, mas não deve atrapalhar a lógica dos negócios.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="c7cb0-141">É por isso que optamos por enfrentar as propriedades de navegação skip para 5.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="c7cb0-142">Neste momento, as outras partes de muitos a muitos estão sendo perseguidas como uma meta de estiramento para o EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="c7cb0-143">Isso significa que eles não estão no plano para 5.0, mas se as coisas vão bem, esperamos puxá-los para dentro.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="c7cb0-144">Mapeamento de herança tabela por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="c7cb0-145">Desenvolvedor líder:@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="c7cb0-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="c7cb0-146">Rastreado por [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="c7cb0-147">Tamanho da camiseta: XL</span><span class="sxs-lookup"><span data-stu-id="c7cb0-147">T-shirt size: XL</span></span>

<span data-ttu-id="c7cb0-148">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-148">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-149">Estamos fazendo TPT porque é um recurso altamente solicitado (~254 votos; 3º no geral) e porque requer algumas mudanças de baixo nível que achamos apropriadas para a natureza fundamental do plano global .NET 5.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="c7cb0-150">Esperamos que isso resulte em mudanças para os provedores de banco de dados, embora estas devam ser muito menos severas do que as mudanças necessárias para o 3.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="c7cb0-151">Inclusão filtrada</span><span class="sxs-lookup"><span data-stu-id="c7cb0-151">Filtered Include</span></span>

<span data-ttu-id="c7cb0-152">Desenvolvedor líder:@maumar</span><span class="sxs-lookup"><span data-stu-id="c7cb0-152">Lead developer: @maumar</span></span>

<span data-ttu-id="c7cb0-153">Rastreado por [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="c7cb0-154">Tamanho da camiseta: M</span><span class="sxs-lookup"><span data-stu-id="c7cb0-154">T-shirt size: M</span></span>

<span data-ttu-id="c7cb0-155">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-155">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-156">Filtered Include é um recurso altamente solicitado (~317 votos; 2º no geral) que não é uma enorme quantidade de trabalho, e que acreditamos que irá desbloquear ou facilitar muitos cenários que atualmente exigem filtros de nível de modelo ou consultas mais complexas.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="c7cb0-157">Racionalize ToTable, ToQuery, ToView, FromSql, etc.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="c7cb0-158">Desenvolvedores líderes: @maumar e@smitpatel</span><span class="sxs-lookup"><span data-stu-id="c7cb0-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="c7cb0-159">Rastreado por [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="c7cb0-160">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-160">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-161">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-161">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-162">Fizemos progressos em versões anteriores para apoiar sql bruto, tipos sem chave e áreas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="c7cb0-163">No entanto, há lacunas e inconsistências na forma como tudo funciona em conjunto como um todo.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="c7cb0-164">O objetivo do 5.0 é corrigi-los e criar uma boa experiência para definir, migrar e usar diferentes tipos de entidades e suas consultas associadas e artefatos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="c7cb0-165">Isso também pode envolver atualizações na API de consulta compilada.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="c7cb0-166">Observe que este item pode resultar em algumas mudanças no nível do aplicativo, uma vez que algumas das funcionalidades que temos atualmente são muito permissivas de modo que pode rapidamente levar as pessoas a poços de falha.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="c7cb0-167">Provavelmente acabaremos bloqueando algumas dessas funcionalidades juntamente com orientações sobre o que fazer em vez disso.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="c7cb0-168">Aprimoramentos de consulta geral</span><span class="sxs-lookup"><span data-stu-id="c7cb0-168">General query enhancements</span></span>

<span data-ttu-id="c7cb0-169">Desenvolvedores líderes: @smitpatel e@maumar</span><span class="sxs-lookup"><span data-stu-id="c7cb0-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="c7cb0-170">Rastreado por [questões `area-query` rotuladas com o marco 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="c7cb0-171">Tamanho da camiseta: XL</span><span class="sxs-lookup"><span data-stu-id="c7cb0-171">T-shirt size: XL</span></span>

<span data-ttu-id="c7cb0-172">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-172">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-173">O código de tradução de consulta foi extensivamente reescrito para o EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="c7cb0-174">O código de consulta é geralmente em um estado muito mais robusto por causa disso.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="c7cb0-175">Para o 5.0 não estamos planejando fazer grandes alterações de consulta, fora aquelas necessárias para suportar o TPT e pular propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="c7cb0-176">No entanto, ainda há um trabalho significativo necessário para corrigir alguma dívida técnica que sobrou da revisão 3.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="c7cb0-177">Também planejamos corrigir muitos bugs e implementar pequenos aprimoramentos para melhorar ainda mais a experiência geral de consulta.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="c7cb0-178">Migrações e experiência de implantação</span><span class="sxs-lookup"><span data-stu-id="c7cb0-178">Migrations and deployment experience</span></span>

<span data-ttu-id="c7cb0-179">Desenvolvedores líderes:@bricelam</span><span class="sxs-lookup"><span data-stu-id="c7cb0-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="c7cb0-180">Rastreado por [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="c7cb0-181">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-181">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-182">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-182">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-183">Atualmente, muitos desenvolvedores migram seus bancos de dados na hora da inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="c7cb0-184">Isso é fácil, mas não é recomendado porque:</span><span class="sxs-lookup"><span data-stu-id="c7cb0-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="c7cb0-185">Vários threads/processos/servidores podem tentar migrar o banco de dados simultaneamente</span><span class="sxs-lookup"><span data-stu-id="c7cb0-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="c7cb0-186">Os aplicativos podem tentar acessar estado inconsistente enquanto isso está acontecendo</span><span class="sxs-lookup"><span data-stu-id="c7cb0-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="c7cb0-187">Normalmente, as permissões do banco de dados para modificar o esquema não devem ser concedidas para execução de aplicativos</span><span class="sxs-lookup"><span data-stu-id="c7cb0-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="c7cb0-188">É difícil voltar a um estado limpo se algo der errado.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="c7cb0-189">Queremos oferecer uma experiência melhor aqui que permita uma maneira fácil de migrar o banco de dados no momento da implantação.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="c7cb0-190">Isso deve:</span><span class="sxs-lookup"><span data-stu-id="c7cb0-190">This should:</span></span>

* <span data-ttu-id="c7cb0-191">Trabalhar em Linux, Mac e Windows</span><span class="sxs-lookup"><span data-stu-id="c7cb0-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="c7cb0-192">Seja uma boa experiência na linha de comando</span><span class="sxs-lookup"><span data-stu-id="c7cb0-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="c7cb0-193">Cenários de suporte com contêineres</span><span class="sxs-lookup"><span data-stu-id="c7cb0-193">Support scenarios with containers</span></span>
* <span data-ttu-id="c7cb0-194">Trabalhe com ferramentas/fluxos de implantação do mundo real comumente usados</span><span class="sxs-lookup"><span data-stu-id="c7cb0-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="c7cb0-195">Integre-se ao menos no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7cb0-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="c7cb0-196">O resultado provavelmente será muitas pequenas melhorias no EF Core (por exemplo, melhores migrações no SQLite), juntamente com orientação e colaborações de longo prazo com outras equipes para melhorar experiências de ponta a ponta que vão além apenas da EF.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="c7cb0-197">Experiência em plataformas EF Core</span><span class="sxs-lookup"><span data-stu-id="c7cb0-197">EF Core platforms experience</span></span> 

<span data-ttu-id="c7cb0-198">Desenvolvedores líderes: @roji e@bricelam</span><span class="sxs-lookup"><span data-stu-id="c7cb0-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="c7cb0-199">Rastreado por [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="c7cb0-200">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-200">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-201">Status: Não iniciado</span><span class="sxs-lookup"><span data-stu-id="c7cb0-201">Status: Not started</span></span>

<span data-ttu-id="c7cb0-202">Temos uma boa orientação para o uso do EF Core em aplicações web tradicionais semelhantes a MVC.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="c7cb0-203">A orientação para outras plataformas e modelos de aplicativos está ausente ou desatualizada.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="c7cb0-204">Para o EF Core 5.0, planejamos investigar, melhorar e documentar a experiência de usar o EF Core com:</span><span class="sxs-lookup"><span data-stu-id="c7cb0-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="c7cb0-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="c7cb0-205">Blazor</span></span>
* <span data-ttu-id="c7cb0-206">Xamarin, inclusive usando a história AOT/linker</span><span class="sxs-lookup"><span data-stu-id="c7cb0-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="c7cb0-207">WinForms/WPF/WinUI e possivelmente outros U.I.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="c7cb0-208">estruturas</span><span class="sxs-lookup"><span data-stu-id="c7cb0-208">frameworks</span></span>

<span data-ttu-id="c7cb0-209">Isso provavelmente será muitas pequenas melhorias no EF Core, juntamente com orientação e colaborações de longo prazo com outras equipes para melhorar experiências de ponta a ponta que vão além apenas da EF.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="c7cb0-210">Áreas específicas que planejamos analisar são:</span><span class="sxs-lookup"><span data-stu-id="c7cb0-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="c7cb0-211">Implantação, incluindo a experiência para usar ferramentas EF, como para migrações</span><span class="sxs-lookup"><span data-stu-id="c7cb0-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="c7cb0-212">Modelos de aplicação, incluindo Xamarin e Blazor, e provavelmente outros</span><span class="sxs-lookup"><span data-stu-id="c7cb0-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="c7cb0-213">Experiências SQLite, incluindo a experiência espacial e reconstruções de mesa</span><span class="sxs-lookup"><span data-stu-id="c7cb0-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="c7cb0-214">AOT e experiências de vinculação</span><span class="sxs-lookup"><span data-stu-id="c7cb0-214">AOT and linking experiences</span></span>
* <span data-ttu-id="c7cb0-215">Integração de diagnósticos, incluindo contadores perf</span><span class="sxs-lookup"><span data-stu-id="c7cb0-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="c7cb0-216">Desempenho</span><span class="sxs-lookup"><span data-stu-id="c7cb0-216">Performance</span></span>

<span data-ttu-id="c7cb0-217">Desenvolvedor líder:@roji</span><span class="sxs-lookup"><span data-stu-id="c7cb0-217">Lead developer: @roji</span></span>

<span data-ttu-id="c7cb0-218">Rastreado por [questões `area-perf` rotuladas com o marco 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="c7cb0-219">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-219">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-220">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-220">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-221">Para a EF Core, planejamos melhorar nosso conjunto de benchmarks de desempenho e fazer melhorias de desempenho direcionadas para o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="c7cb0-222">Além disso, planejamos concluir a nova ADO.NET API de loteamento que foi protótipo durante o ciclo de lançamento 3.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="c7cb0-223">Também na camada ADO.NET, planejamos melhorias adicionais de desempenho para o provedor Npgsql.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="c7cb0-224">Como parte deste trabalho, também planejamos adicionar ADO.NET/EF contadores de desempenho Core e outros diagnósticos conforme apropriado.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="c7cb0-225">Documentação arquitetônica/contribuinte</span><span class="sxs-lookup"><span data-stu-id="c7cb0-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="c7cb0-226">Documentador líder:@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="c7cb0-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="c7cb0-227">Rastreado por [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="c7cb0-228">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-228">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-229">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-229">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-230">A ideia aqui é facilitar a compreensão do que está acontecendo nos internos da EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="c7cb0-231">Isso pode ser útil para qualquer pessoa que use o EF Core, mas a principal motivação é tornar mais fácil para as pessoas externas:</span><span class="sxs-lookup"><span data-stu-id="c7cb0-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="c7cb0-232">Contribua para o código EF Core</span><span class="sxs-lookup"><span data-stu-id="c7cb0-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="c7cb0-233">Criar provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="c7cb0-233">Create database providers</span></span>
* <span data-ttu-id="c7cb0-234">Construa outras extensões</span><span class="sxs-lookup"><span data-stu-id="c7cb0-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="c7cb0-235">Documentação do Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="c7cb0-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="c7cb0-236">Documentador líder:@bricelam</span><span class="sxs-lookup"><span data-stu-id="c7cb0-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="c7cb0-237">Rastreado por [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="c7cb0-238">Tamanho da camiseta: M</span><span class="sxs-lookup"><span data-stu-id="c7cb0-238">T-shirt size: M</span></span>

<span data-ttu-id="c7cb0-239">Status: Concluído.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-239">Status: Completed.</span></span> <span data-ttu-id="c7cb0-240">A nova documentação está [ao vivo no site de documentos da Microsoft.](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="c7cb0-241">A Equipe EF também possui o provedor de ADO.NET Microsoft.Data.Sqlite.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="c7cb0-242">Planejamos documentar totalmente este provedor como parte da versão 5.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="c7cb0-243">Documentação geral</span><span class="sxs-lookup"><span data-stu-id="c7cb0-243">General documentation</span></span>

<span data-ttu-id="c7cb0-244">Documentador líder:@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="c7cb0-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="c7cb0-245">Rastreado por [problemas no repo docs na marca 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="c7cb0-246">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-246">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-247">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-247">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-248">Já estamos em processo de atualização da documentação para as versões 3.0 e 3.1.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="c7cb0-249">Também estamos trabalhando em:</span><span class="sxs-lookup"><span data-stu-id="c7cb0-249">We are also working on:</span></span>
  * <span data-ttu-id="c7cb0-250">Uma revisão dos docs iniciais para torná-los mais acessíveis/mais fáceis de seguir</span><span class="sxs-lookup"><span data-stu-id="c7cb0-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="c7cb0-251">Reorganização de docs para tornar as coisas mais fáceis de encontrar e adicionar referências cruzadas</span><span class="sxs-lookup"><span data-stu-id="c7cb0-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="c7cb0-252">Adicionando mais detalhes e esclarecimentos aos docs existentes</span><span class="sxs-lookup"><span data-stu-id="c7cb0-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="c7cb0-253">Atualizando as amostras e adicionando mais exemplos</span><span class="sxs-lookup"><span data-stu-id="c7cb0-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="c7cb0-254">Corrigindo bugs</span><span class="sxs-lookup"><span data-stu-id="c7cb0-254">Fixing bugs</span></span>

<span data-ttu-id="c7cb0-255">Rastreado por [questões `type-bug` rotuladas com o marco 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="c7cb0-256">Desenvolvedores: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="c7cb0-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="c7cb0-257">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-257">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-258">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-258">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-259">No momento da escrita, temos 135 bugs triados para serem corrigidos na versão 5.0 (com 62 já _corrigidos),_ mas há uma sobreposição significativa com a seção de aprimoramentos de consulta geral acima.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="c7cb0-260">A taxa de entrada (questões que acabam como trabalho em um marco) foi de cerca de 23 questões por mês ao longo da versão 3.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="c7cb0-261">Nem tudo isso precisará ser corrigido em 5.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="c7cb0-262">Como uma estimativa aproximada, planejamos corrigir 150 problemas adicionais no período de tempo 5.0.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="c7cb0-263">Pequenos aprimoramentos</span><span class="sxs-lookup"><span data-stu-id="c7cb0-263">Small enhancements</span></span>

<span data-ttu-id="c7cb0-264">Rastreado por [questões `type-enhancement` rotuladas com o marco 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="c7cb0-265">Desenvolvedores: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="c7cb0-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="c7cb0-266">Tamanho da camiseta: L</span><span class="sxs-lookup"><span data-stu-id="c7cb0-266">T-shirt size: L</span></span>

<span data-ttu-id="c7cb0-267">Status: Em andamento</span><span class="sxs-lookup"><span data-stu-id="c7cb0-267">Status: In-progress</span></span>

<span data-ttu-id="c7cb0-268">Além das características maiores descritas acima, também temos muitas melhorias menores programadas para o 5.0 para corrigir "cortes de papel".</span><span class="sxs-lookup"><span data-stu-id="c7cb0-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="c7cb0-269">Note que muitos desses aprimoramentos também são cobertos pelos temas mais gerais descritos acima.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="c7cb0-270">Abaixo da linha</span><span class="sxs-lookup"><span data-stu-id="c7cb0-270">Below-the-line</span></span>

<span data-ttu-id="c7cb0-271">Rastreado por [questões `consider-for-next-release` rotuladas com](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="c7cb0-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="c7cb0-272">Estas são correções de bugs e melhorias que **não** estão programadas para a versão 5.0 no momento, mas vamos olhar como metas de alongamento dependendo do progresso feito no trabalho acima.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="c7cb0-273">Além disso, sempre consideramos as [questões mais votadas](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) no planejamento.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="c7cb0-274">Cortar qualquer uma dessas questões de uma liberação é sempre doloroso, mas precisamos de um plano realista para os recursos que temos.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="c7cb0-275">Comentários</span><span class="sxs-lookup"><span data-stu-id="c7cb0-275">Feedback</span></span>

<span data-ttu-id="c7cb0-276">Seus comentários sobre o planejamento são importantes.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-276">Your feedback on planning is important.</span></span> <span data-ttu-id="c7cb0-277">A melhor maneira de indicar a importância de um problema é votar (polegar para cima) nesse problema no GitHub.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="c7cb0-278">Esses dados serão então alimentados no [processo de planejamento](../release-planning.md) para a próxima versão.</span><span class="sxs-lookup"><span data-stu-id="c7cb0-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
