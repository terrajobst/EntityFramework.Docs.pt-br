---
title: Planejar para o Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125378"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="942ce-102">Planejar para o Entity Framework Core 5,0</span><span class="sxs-lookup"><span data-stu-id="942ce-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="942ce-103">Conforme descrito no [processo de planejamento](../release-planning.md), reunimos a entrada dos participantes em um plano provisório para a versão EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="942ce-104">Esse plano ainda é um trabalho em andamento.</span><span class="sxs-lookup"><span data-stu-id="942ce-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="942ce-105">Nada aqui é um compromisso.</span><span class="sxs-lookup"><span data-stu-id="942ce-105">Nothing here is a commitment.</span></span> <span data-ttu-id="942ce-106">Este plano é um ponto de partida que evoluirá à medida que aprendemos mais.</span><span class="sxs-lookup"><span data-stu-id="942ce-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="942ce-107">Algumas coisas que não estão atualmente planejadas para 5,0 podem ser retiradas.</span><span class="sxs-lookup"><span data-stu-id="942ce-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="942ce-108">Algumas coisas atualmente planejadas para 5,0 podem ser desativadas.</span><span class="sxs-lookup"><span data-stu-id="942ce-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="942ce-109">Número de versão e data de lançamento.</span><span class="sxs-lookup"><span data-stu-id="942ce-109">Version number and release date.</span></span>

<span data-ttu-id="942ce-110">O EF Core 5,0 está atualmente agendado para lançamento ao [mesmo tempo que o .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="942ce-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="942ce-111">A versão "5,0" foi escolhida para se alinhar com o .NET 5,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="942ce-112">Plataformas com suporte</span><span class="sxs-lookup"><span data-stu-id="942ce-112">Supported platforms</span></span>

<span data-ttu-id="942ce-113">EF Core 5,0 está planejado para ser executado em qualquer plataforma .NET 5,0 com base na [convergência dessas plataformas para o .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="942ce-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="942ce-114">O que isso significa em termos de .NET Standard e o TFM real usado ainda é TBD.</span><span class="sxs-lookup"><span data-stu-id="942ce-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="942ce-115">EF Core 5,0 não será executado em .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="942ce-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="942ce-116">Alterações da falha</span><span class="sxs-lookup"><span data-stu-id="942ce-116">Breaking changes</span></span>

<span data-ttu-id="942ce-117">EF Core 5,0 conterá algumas alterações significativas, mas elas serão muito menos graves do que o caso para EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="942ce-118">Nossa meta é permitir que a grande maioria dos aplicativos seja atualizada sem interrupções.</span><span class="sxs-lookup"><span data-stu-id="942ce-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="942ce-119">Espera-se que haja algumas alterações significativas para provedores de banco de dados, especialmente em relação ao suporte a TPT.</span><span class="sxs-lookup"><span data-stu-id="942ce-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="942ce-120">No entanto, esperamos que o trabalho para atualizar um provedor para 5,0 seja menor que o necessário para atualizar para 3,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="942ce-121">Temas</span><span class="sxs-lookup"><span data-stu-id="942ce-121">Themes</span></span>

<span data-ttu-id="942ce-122">Extraímos algumas áreas ou temas principais que formam a base dos grandes investimentos em EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="942ce-123">Propriedades de navegação muitos para muitos (a. k. a "ignorar navegações")</span><span class="sxs-lookup"><span data-stu-id="942ce-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="942ce-124">Desenvolvedores potenciais: @smitpatel e @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="942ce-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="942ce-125">Acompanhado por [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="942ce-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="942ce-126">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-126">T-shirt size: L</span></span>

<span data-ttu-id="942ce-127">Status: em andamento</span><span class="sxs-lookup"><span data-stu-id="942ce-127">Status: In-progress</span></span>

<span data-ttu-id="942ce-128">Muitos-para-muitos é o recurso mais solicitado (aproximadamente 407 votos) na pendência do GitHub.</span><span class="sxs-lookup"><span data-stu-id="942ce-128">Many-to-many is the most requested feature (~407 votes) on the GitHub backlog.</span></span> <span data-ttu-id="942ce-129">O suporte para relações muitos para muitos pode ser dividido em três áreas principais:</span><span class="sxs-lookup"><span data-stu-id="942ce-129">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="942ce-130">Ignorar Propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="942ce-130">Skip navigation properties.</span></span> <span data-ttu-id="942ce-131">Eles permitem que o modelo seja usado para consultas, etc. sem referência à entidade de tabela de junção subjacente.</span><span class="sxs-lookup"><span data-stu-id="942ce-131">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span>
* <span data-ttu-id="942ce-132">Tipos de entidade de conjunto de propriedades.</span><span class="sxs-lookup"><span data-stu-id="942ce-132">Property-bag entity types.</span></span> <span data-ttu-id="942ce-133">Eles permitem que um tipo CLR padrão (por exemplo, `Dictionary`) seja usado para instâncias de entidade, de modo que um tipo CLR explícito não seja necessário para cada tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="942ce-133">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span>
* <span data-ttu-id="942ce-134">Para facilitar a configuração de relações muitos para muitos.</span><span class="sxs-lookup"><span data-stu-id="942ce-134">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="942ce-135">Acreditamos que o bloqueador mais significativo para aqueles que desejam suporte muitos para muitos não é capaz de usar as relações "naturais", sem referir-se à tabela de junção, em lógica de negócios, como consultas.</span><span class="sxs-lookup"><span data-stu-id="942ce-135">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="942ce-136">O tipo de entidade da tabela de junção ainda pode existir, mas não deve ficar na forma de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="942ce-136">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="942ce-137">É por isso que optamos por lidar com as propriedades de navegação Skip para 5,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-137">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="942ce-138">Neste momento, as outras partes de muitos para muitos estão sendo buscadas como uma meta de ampliação para o EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-138">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="942ce-139">Isso significa que eles não estão atualmente no plano de 5,0, mas se as coisas estiverem bem, esperamos poder extraí-los.</span><span class="sxs-lookup"><span data-stu-id="942ce-139">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="942ce-140">Mapeamento de herança de tabela por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="942ce-140">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="942ce-141">Desenvolvedor líder: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="942ce-141">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="942ce-142">Acompanhado por [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="942ce-142">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="942ce-143">Tamanho de camiseta: XL</span><span class="sxs-lookup"><span data-stu-id="942ce-143">T-shirt size: XL</span></span>

<span data-ttu-id="942ce-144">Status: não iniciado</span><span class="sxs-lookup"><span data-stu-id="942ce-144">Status: Not started</span></span>

<span data-ttu-id="942ce-145">Estamos fazendo TPT porque ele é um recurso altamente solicitado (aproximadamente 254 votos; terceiro geral) e porque requer algumas alterações de baixo nível que sentem que são apropriadas para a natureza fundamental do plano geral do .NET 5.</span><span class="sxs-lookup"><span data-stu-id="942ce-145">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="942ce-146">Esperamos que isso resulte em alterações significativas para provedores de banco de dados, embora eles devam ser muito menos graves do que as alterações necessárias para 3,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-146">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="942ce-147">Inclusão filtrada</span><span class="sxs-lookup"><span data-stu-id="942ce-147">Filtered Include</span></span>

<span data-ttu-id="942ce-148">Desenvolvedor líder: @maumar</span><span class="sxs-lookup"><span data-stu-id="942ce-148">Lead developer: @maumar</span></span>

<span data-ttu-id="942ce-149">Acompanhado por [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="942ce-149">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="942ce-150">Tamanho de camiseta: M</span><span class="sxs-lookup"><span data-stu-id="942ce-150">T-shirt size: M</span></span>

<span data-ttu-id="942ce-151">Status: não iniciado</span><span class="sxs-lookup"><span data-stu-id="942ce-151">Status: Not started</span></span>

<span data-ttu-id="942ce-152">A inclusão filtrada é um recurso altamente solicitado (aproximadamente 317 votos; 2º geral) que não é uma grande quantidade de trabalho, e que acreditamos que desbloqueiaremos ou facilitaremos muitos cenários que atualmente exigem filtros no nível de modelo ou consultas mais complexas.</span><span class="sxs-lookup"><span data-stu-id="942ce-152">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="942ce-153">Racionalizar totabela, toconsulta, toView, das, etc.</span><span class="sxs-lookup"><span data-stu-id="942ce-153">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="942ce-154">Desenvolvedores potenciais: @maumar e @smitpatel</span><span class="sxs-lookup"><span data-stu-id="942ce-154">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="942ce-155">Acompanhado por [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="942ce-155">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="942ce-156">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-156">T-shirt size: L</span></span>

<span data-ttu-id="942ce-157">Status: não iniciado</span><span class="sxs-lookup"><span data-stu-id="942ce-157">Status: Not started</span></span>

<span data-ttu-id="942ce-158">Fizemos o progresso nas versões anteriores para oferecer suporte a SQL bruto, tipos sem formatação e áreas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="942ce-158">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="942ce-159">No entanto, há lacunas e inconsistências na maneira como tudo funciona em conjunto como um todo.</span><span class="sxs-lookup"><span data-stu-id="942ce-159">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="942ce-160">O objetivo do 5,0 é corrigi-los e criar uma boa experiência para definir, migrar e usar diferentes tipos de entidades e suas consultas associadas e artefatos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="942ce-160">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="942ce-161">Isso também pode envolver atualizações na API de consulta compilada.</span><span class="sxs-lookup"><span data-stu-id="942ce-161">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="942ce-162">Observe que esse item pode resultar em algumas alterações significativas no nível do aplicativo, pois algumas das funcionalidades que temos atualmente são muito permissivas, de modo que isso pode levar rapidamente as pessoas a Pits de falha.</span><span class="sxs-lookup"><span data-stu-id="942ce-162">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="942ce-163">Provavelmente, acabaremos bloqueando algumas dessas funcionalidades com diretrizes sobre o que fazer.</span><span class="sxs-lookup"><span data-stu-id="942ce-163">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="942ce-164">Aprimoramentos de consulta geral</span><span class="sxs-lookup"><span data-stu-id="942ce-164">General query enhancements</span></span>

<span data-ttu-id="942ce-165">Desenvolvedores potenciais: @smitpatel e @maumar</span><span class="sxs-lookup"><span data-stu-id="942ce-165">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="942ce-166">Acompanhado por [problemas rotulados com `area-query` no marco 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="942ce-166">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="942ce-167">Tamanho de camiseta: XL</span><span class="sxs-lookup"><span data-stu-id="942ce-167">T-shirt size: XL</span></span>

<span data-ttu-id="942ce-168">Status: em andamento</span><span class="sxs-lookup"><span data-stu-id="942ce-168">Status: In-progress</span></span>

<span data-ttu-id="942ce-169">O código de conversão de consulta foi reescrito extensivamente para EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-169">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="942ce-170">O código de consulta geralmente está em um estado muito mais robusto por causa disso.</span><span class="sxs-lookup"><span data-stu-id="942ce-170">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="942ce-171">Para 5,0, não estamos planejando fazer alterações de consulta principais, fora das necessárias para dar suporte a TPT e ignorar Propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="942ce-171">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="942ce-172">No entanto, ainda há um trabalho significativo necessário para corrigir alguma dívida técnica à esquerda da revisão 3,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-172">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="942ce-173">Também planejamos corrigir muitos bugs e implementar pequenos aprimoramentos para melhorar ainda mais a experiência de consulta geral.</span><span class="sxs-lookup"><span data-stu-id="942ce-173">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="942ce-174">Migração e experiência de implantação</span><span class="sxs-lookup"><span data-stu-id="942ce-174">Migrations and deployment experience</span></span>

<span data-ttu-id="942ce-175">Desenvolvedores potenciais: @bricelam</span><span class="sxs-lookup"><span data-stu-id="942ce-175">Lead developers: @bricelam</span></span>

<span data-ttu-id="942ce-176">Acompanhado por [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="942ce-176">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="942ce-177">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-177">T-shirt size: L</span></span>

<span data-ttu-id="942ce-178">Status: em andamento</span><span class="sxs-lookup"><span data-stu-id="942ce-178">Status: In-progress</span></span>

<span data-ttu-id="942ce-179">Atualmente, muitos desenvolvedores migram seus bancos de dados no momento da inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="942ce-179">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="942ce-180">Isso é fácil, mas não é recomendado porque:</span><span class="sxs-lookup"><span data-stu-id="942ce-180">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="942ce-181">Vários threads/processos/servidores podem tentar migrar o banco de dados simultaneamente</span><span class="sxs-lookup"><span data-stu-id="942ce-181">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="942ce-182">Os aplicativos podem tentar acessar o estado inconsistente enquanto isso acontece</span><span class="sxs-lookup"><span data-stu-id="942ce-182">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="942ce-183">Geralmente, as permissões de banco de dados para modificar o esquema não devem ser concedidas para a execução do aplicativo</span><span class="sxs-lookup"><span data-stu-id="942ce-183">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="942ce-184">Seu difícil de reverter para um estado limpo se algo der errado</span><span class="sxs-lookup"><span data-stu-id="942ce-184">Its hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="942ce-185">Queremos fornecer uma melhor experiência aqui que permite uma maneira fácil de migrar o banco de dados no momento da implantação.</span><span class="sxs-lookup"><span data-stu-id="942ce-185">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="942ce-186">Isso deve:</span><span class="sxs-lookup"><span data-stu-id="942ce-186">This should:</span></span>

* <span data-ttu-id="942ce-187">Trabalhar em Linux, Mac e Windows</span><span class="sxs-lookup"><span data-stu-id="942ce-187">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="942ce-188">Seja uma boa experiência na linha de comando</span><span class="sxs-lookup"><span data-stu-id="942ce-188">Be a good experience on the command line</span></span>
* <span data-ttu-id="942ce-189">Cenários de suporte com contêineres</span><span class="sxs-lookup"><span data-stu-id="942ce-189">Support scenarios with containers</span></span>
* <span data-ttu-id="942ce-190">Trabalhe com fluxos/ferramentas de implantação do mundo real usados com frequência</span><span class="sxs-lookup"><span data-stu-id="942ce-190">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="942ce-191">Integre em pelo menos o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="942ce-191">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="942ce-192">É provável que o resultado seja muitas pequenas melhorias no EF Core (por exemplo, melhores migrações no SQLite), juntamente com orientações e colaborações de longo prazo com outras equipes para melhorar experiências de ponta a ponta que vão além do EF.</span><span class="sxs-lookup"><span data-stu-id="942ce-192">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="942ce-193">Experiência em plataformas EF Core</span><span class="sxs-lookup"><span data-stu-id="942ce-193">EF Core platforms experience</span></span> 

<span data-ttu-id="942ce-194">Desenvolvedores potenciais: @roji e @bricelam</span><span class="sxs-lookup"><span data-stu-id="942ce-194">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="942ce-195">Acompanhado por [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="942ce-195">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="942ce-196">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-196">T-shirt size: L</span></span>

<span data-ttu-id="942ce-197">Status: não iniciado</span><span class="sxs-lookup"><span data-stu-id="942ce-197">Status: Not started</span></span>

<span data-ttu-id="942ce-198">Temos uma boa orientação para usar EF Core em aplicativos Web tradicionais como o MVC.</span><span class="sxs-lookup"><span data-stu-id="942ce-198">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="942ce-199">A orientação para outras plataformas e modelos de aplicativos está ausente ou desatualizado.</span><span class="sxs-lookup"><span data-stu-id="942ce-199">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="942ce-200">Para EF Core 5,0, planejamos investigar, melhorar e documentar a experiência de usar o EF Core com:</span><span class="sxs-lookup"><span data-stu-id="942ce-200">For EF Core 5.0 we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="942ce-201">Blazor</span><span class="sxs-lookup"><span data-stu-id="942ce-201">Blazor</span></span>
* <span data-ttu-id="942ce-202">Xamarin, incluindo o uso da história de AOT/vinculador</span><span class="sxs-lookup"><span data-stu-id="942ce-202">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="942ce-203">WinForms/WPF/WinUI e possivelmente outros U.I.</span><span class="sxs-lookup"><span data-stu-id="942ce-203">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="942ce-204">estruturas</span><span class="sxs-lookup"><span data-stu-id="942ce-204">frameworks</span></span>

<span data-ttu-id="942ce-205">É provável que isso seja muitas pequenas melhorias no EF Core, juntamente com orientações e colaborações de longo prazo com outras equipes para melhorar experiências de ponta a ponta que vão além do EF.</span><span class="sxs-lookup"><span data-stu-id="942ce-205">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="942ce-206">As áreas específicas que planejamos examinar são:</span><span class="sxs-lookup"><span data-stu-id="942ce-206">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="942ce-207">Implantação, incluindo a experiência de usar ferramentas do EF, como para migrações</span><span class="sxs-lookup"><span data-stu-id="942ce-207">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="942ce-208">Modelos de aplicativos, incluindo Xamarin e mais ou mais, e provavelmente outros</span><span class="sxs-lookup"><span data-stu-id="942ce-208">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="942ce-209">Experiências do SQLite, incluindo a experiência espacial e recompilações de tabela</span><span class="sxs-lookup"><span data-stu-id="942ce-209">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="942ce-210">Experiências de AOT e de vinculação</span><span class="sxs-lookup"><span data-stu-id="942ce-210">AOT and linking experiences</span></span>
* <span data-ttu-id="942ce-211">Integração de diagnóstico, incluindo contadores de desempenho</span><span class="sxs-lookup"><span data-stu-id="942ce-211">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="942ce-212">Desempenho</span><span class="sxs-lookup"><span data-stu-id="942ce-212">Performance</span></span>

<span data-ttu-id="942ce-213">Desenvolvedor líder: @roji</span><span class="sxs-lookup"><span data-stu-id="942ce-213">Lead developer: @roji</span></span>

<span data-ttu-id="942ce-214">Acompanhado por [problemas rotulados com `area-perf` no marco 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="942ce-214">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="942ce-215">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-215">T-shirt size: L</span></span>

<span data-ttu-id="942ce-216">Status: em andamento</span><span class="sxs-lookup"><span data-stu-id="942ce-216">Status: In-progress</span></span>

<span data-ttu-id="942ce-217">Por EF Core planejamos aprimorar nosso conjunto de benchmarks de desempenho e fazer melhorias de desempenho direcionadas para o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="942ce-217">For EF Core we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="942ce-218">Além disso, planejamos concluir a nova API de envio em lote ADO.NET que foi protótipo durante o ciclo de lançamento 3,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-218">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="942ce-219">Além disso, na camada ADO.NET, planejamos aprimoramentos de desempenho adicionais para o provedor Npgsql.</span><span class="sxs-lookup"><span data-stu-id="942ce-219">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="942ce-220">Como parte desse trabalho, também planejamos adicionar contadores de desempenho do ADO.NET/EF Core e outros diagnósticos, conforme apropriado.</span><span class="sxs-lookup"><span data-stu-id="942ce-220">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="942ce-221">Documentação de arquitetura/colaborador</span><span class="sxs-lookup"><span data-stu-id="942ce-221">Architectural/contributor documentation</span></span>

<span data-ttu-id="942ce-222">Documentador de leads: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="942ce-222">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="942ce-223">Acompanhado por [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="942ce-223">Tracked by [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="942ce-224">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-224">T-shirt size: L</span></span>

<span data-ttu-id="942ce-225">Status: não iniciado</span><span class="sxs-lookup"><span data-stu-id="942ce-225">Status: Not started</span></span>

<span data-ttu-id="942ce-226">A ideia aqui é tornar mais fácil entender o que está acontecendo nos elementos internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="942ce-226">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="942ce-227">Isso pode ser útil para qualquer pessoa que esteja usando EF Core, mas a principal motivação é tornar mais fácil para as pessoas externas:</span><span class="sxs-lookup"><span data-stu-id="942ce-227">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="942ce-228">Contribuir com o código de EF Core</span><span class="sxs-lookup"><span data-stu-id="942ce-228">Contribute to the EF Core code</span></span>
* <span data-ttu-id="942ce-229">Criar provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="942ce-229">Create database providers</span></span>
* <span data-ttu-id="942ce-230">Criar outras extensões</span><span class="sxs-lookup"><span data-stu-id="942ce-230">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="942ce-231">Documentação do Microsoft. Data. sqlite</span><span class="sxs-lookup"><span data-stu-id="942ce-231">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="942ce-232">Documentador de leads: @bricelam</span><span class="sxs-lookup"><span data-stu-id="942ce-232">Lead documenter: @bricelam</span></span>

<span data-ttu-id="942ce-233">Acompanhado por [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="942ce-233">Tracked by [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="942ce-234">Tamanho de camiseta: M</span><span class="sxs-lookup"><span data-stu-id="942ce-234">T-shirt size: M</span></span>

<span data-ttu-id="942ce-235">Status: concluído.</span><span class="sxs-lookup"><span data-stu-id="942ce-235">Status: Completed.</span></span> <span data-ttu-id="942ce-236">A nova documentação está ativa [no site do Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="942ce-236">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="942ce-237">A equipe do EF também possui o provedor de ADO.NET Microsoft. Data. sqlite.</span><span class="sxs-lookup"><span data-stu-id="942ce-237">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="942ce-238">Planejamos documentar completamente esse provedor como parte da versão 5,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-238">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="942ce-239">Documentação geral</span><span class="sxs-lookup"><span data-stu-id="942ce-239">General documentation</span></span>

<span data-ttu-id="942ce-240">Documentador de leads: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="942ce-240">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="942ce-241">Acompanhado por [problemas no repositório de documentos no marco 5,0](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="942ce-241">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="942ce-242">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-242">T-shirt size: L</span></span>

<span data-ttu-id="942ce-243">Status: em andamento</span><span class="sxs-lookup"><span data-stu-id="942ce-243">Status: In-progress</span></span>

<span data-ttu-id="942ce-244">Já estamos no processo de atualização de documentação para as versões 3,0 e 3,1.</span><span class="sxs-lookup"><span data-stu-id="942ce-244">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="942ce-245">Também estamos trabalhando em:</span><span class="sxs-lookup"><span data-stu-id="942ce-245">We are also working on:</span></span>
  * <span data-ttu-id="942ce-246">Uma revisão dos documentos de introdução para torná-los mais abordáveis/fáceis de acompanhar</span><span class="sxs-lookup"><span data-stu-id="942ce-246">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="942ce-247">Reorganização de docs para tornar as coisas mais fáceis de localizar e adicionar referências cruzadas</span><span class="sxs-lookup"><span data-stu-id="942ce-247">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="942ce-248">Adicionando mais detalhes e esclarecimentos aos documentos existentes</span><span class="sxs-lookup"><span data-stu-id="942ce-248">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="942ce-249">Atualizando os exemplos e adicionando mais exemplos</span><span class="sxs-lookup"><span data-stu-id="942ce-249">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="942ce-250">Corrigindo bugs</span><span class="sxs-lookup"><span data-stu-id="942ce-250">Fixing bugs</span></span>

<span data-ttu-id="942ce-251">Acompanhado por [problemas rotulados com `type-bug` no marco 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="942ce-251">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="942ce-252">Desenvolvedores: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="942ce-252">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="942ce-253">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-253">T-shirt size: L</span></span>

<span data-ttu-id="942ce-254">Status: em andamento</span><span class="sxs-lookup"><span data-stu-id="942ce-254">Status: In-progress</span></span>

<span data-ttu-id="942ce-255">No momento da elaboração do artigo, temos 135 bugs desclassificados para serem corrigidos na versão 5,0 (com 62 já corrigido), mas há uma sobreposição significativa com a seção de _aprimoramentos gerais de consulta_ acima.</span><span class="sxs-lookup"><span data-stu-id="942ce-255">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="942ce-256">A taxa de entrada (problemas que terminam como trabalho em uma etapa) foi de cerca de 23 problemas por mês ao longo da versão 3,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-256">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="942ce-257">Nem todos eles precisarão ser corrigidos em 5,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-257">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="942ce-258">Como uma estimativa aproximada, planejamos corrigir um adicional 150 problemas no período de 5,0.</span><span class="sxs-lookup"><span data-stu-id="942ce-258">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="942ce-259">Pequenos aprimoramentos</span><span class="sxs-lookup"><span data-stu-id="942ce-259">Small enhancements</span></span>

<span data-ttu-id="942ce-260">Acompanhado por [problemas rotulados com `type-enhancement` no marco 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="942ce-260">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="942ce-261">Desenvolvedores: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="942ce-261">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="942ce-262">Tamanho de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="942ce-262">T-shirt size: L</span></span>

<span data-ttu-id="942ce-263">Status: em andamento</span><span class="sxs-lookup"><span data-stu-id="942ce-263">Status: In-progress</span></span>

<span data-ttu-id="942ce-264">Além dos maiores recursos descritos acima, também temos muitas melhorias menores agendadas para 5,0 para corrigir "cortes de papel".</span><span class="sxs-lookup"><span data-stu-id="942ce-264">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="942ce-265">Observe que muitos desses aprimoramentos também são cobertos pelos temas mais gerais descritos acima.</span><span class="sxs-lookup"><span data-stu-id="942ce-265">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="942ce-266">Abaixo-a-linha</span><span class="sxs-lookup"><span data-stu-id="942ce-266">Below-the-line</span></span>

<span data-ttu-id="942ce-267">Acompanhado por [problemas rotulados com `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="942ce-267">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="942ce-268">Essas são correções de bugs e aprimoramentos que **não** estão atualmente agendados para a versão 5,0, mas veremos como metas de ampliação, dependendo do progresso feito no trabalho acima.</span><span class="sxs-lookup"><span data-stu-id="942ce-268">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="942ce-269">Além disso, sempre consideramos os [problemas mais votados](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) durante o planejamento.</span><span class="sxs-lookup"><span data-stu-id="942ce-269">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="942ce-270">A redução de qualquer um desses problemas de uma versão é sempre difícil, mas precisamos de um plano realista para os recursos que temos.</span><span class="sxs-lookup"><span data-stu-id="942ce-270">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="942ce-271">Comentários</span><span class="sxs-lookup"><span data-stu-id="942ce-271">Feedback</span></span>

<span data-ttu-id="942ce-272">Seus comentários sobre o planejamento são importantes.</span><span class="sxs-lookup"><span data-stu-id="942ce-272">Your feedback on planning is important.</span></span> <span data-ttu-id="942ce-273">A melhor maneira de indicar a importância de um problema é votar (polegares) para esse problema no GitHub.</span><span class="sxs-lookup"><span data-stu-id="942ce-273">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="942ce-274">Esses dados serão então alimentados no [processo de planejamento](../release-planning.md) para a próxima versão.</span><span class="sxs-lookup"><span data-stu-id="942ce-274">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
