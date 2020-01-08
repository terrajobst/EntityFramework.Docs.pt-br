---
title: Ferramentas e Extensões – EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: bab725afffe1fbf9f8c0abeef58579ac9dc842d2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502076"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="ac2ee-102">Ferramentas e Extensões do EF Core</span><span class="sxs-lookup"><span data-stu-id="ac2ee-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="ac2ee-103">Essas ferramentas e extensões oferecem funcionalidades adicionais para o Entity Framework Core 2.1 e as versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ac2ee-104">As extensões são compostas de diversas fontes e não são mantidas como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ac2ee-105">Ao considerar uma extensão de terceiros, avalie a qualidade, o licenciamento, a compatibilidade, o suporte, entre outras coisas, para garantir que ela atenda às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="ac2ee-106">Em particular, uma extensão criada para uma versão mais antiga do EF Core pode precisar de atualização para que funcione com as versões mais recentes.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="ac2ee-107">Ferramentas</span><span class="sxs-lookup"><span data-stu-id="ac2ee-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="ac2ee-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="ac2ee-108">LLBLGen Pro</span></span>

<span data-ttu-id="ac2ee-109">O LLBLGen Pro é uma solução de modelagem de entidade compatível com o Entity Framework e o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="ac2ee-110">Com ele, é possível definir facilmente o modelo de entidade e mapeá-lo em seu banco de dados usando as abordagens Model First ou Database First. Assim, você pode começar a escrever consultas imediatamente.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="ac2ee-111">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-111">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-112">Site</span><span class="sxs-lookup"><span data-stu-id="ac2ee-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="ac2ee-113">Desenvolvedor de Entidade Devart</span><span class="sxs-lookup"><span data-stu-id="ac2ee-113">Devart Entity Developer</span></span>

<span data-ttu-id="ac2ee-114">O Desenvolvedor de Entidade é um potente designer de ORM para o ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="ac2ee-115">Ele é compatível com a criação de modelos do EF Core visualmente, usando as primeiras abordagens do modelo ou do banco de dados e a geração de código em C # ou Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="ac2ee-116">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-116">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-117">Site</span><span class="sxs-lookup"><span data-stu-id="ac2ee-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="ac2ee-118">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="ac2ee-118">EF Core Power Tools</span></span>

<span data-ttu-id="ac2ee-119">O EF Core Power Tools é uma extensão do Visual Studio que expõe várias tarefas de tempo de design do EF Core em uma interface do usuário simples.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-119">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="ac2ee-120">Ele inclui a engenharia reversa do DbContext e classes de entidade de bancos de dados existentes e do [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), gerenciamento de migrações de banco de dados e visualizações de modelo.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-120">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="ac2ee-121">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-121">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ac2ee-122">Wiki do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-122">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="ac2ee-123">Editor Visual do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ac2ee-123">Entity Framework Visual Editor</span></span>

<span data-ttu-id="ac2ee-124">O Entity Framework Visual Editor é uma extensão do Visual Studio que adiciona um designer de ORM ao design visual do EF 6 e às classes do EF Core.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-124">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="ac2ee-125">O código é gerado usando modelos T4. Portanto, ele pode ser personalizado para atender a quaisquer necessidades.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-125">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="ac2ee-126">Ele é compatível com herança, associações unidirecionais e bidirecionais, enumerações e com a capacidade de atribuir código de cor às suas classes e de adicionar blocos de texto para explicar partes potencialmente misteriosas do seu design.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-126">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="ac2ee-127">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-127">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-128">Marketplace</span><span class="sxs-lookup"><span data-stu-id="ac2ee-128">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="ac2ee-129">CatFactory</span><span class="sxs-lookup"><span data-stu-id="ac2ee-129">CatFactory</span></span>

<span data-ttu-id="ac2ee-130">O CatFactory é um mecanismo de scaffolding para o .NET Core que pode automatizar a geração de classes DbContext, entidades, configurações de mapeamento e classes de repositório de um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-130">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="ac2ee-131">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-131">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-132">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-132">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="ac2ee-133">Gerador do Entity Framework Core da LoreSoft</span><span class="sxs-lookup"><span data-stu-id="ac2ee-133">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="ac2ee-134">O Entity Framework Core Generator (efg) é uma ferramenta de CLI do .NET Core que pode gerar modelos do EF Core de um banco de dados existente, muito parecido com `dotnet ef dbcontext scaffold`, mas que também é compatível com a [regeneração](https://efg.loresoft.com/en/latest/regeneration/) do código seguro por meio da substituição de região ou da análise dos arquivos de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-134">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="ac2ee-135">Essa ferramenta é compatível com a geração de modelos de exibição, com a validação e o código do mapeador de objetos.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-135">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="ac2ee-136">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-136">For EF Core: 2.</span></span>

<span data-ttu-id="ac2ee-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentação](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="ac2ee-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="ac2ee-138">Extensões</span><span class="sxs-lookup"><span data-stu-id="ac2ee-138">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="ac2ee-139">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="ac2ee-139">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="ac2ee-140">Uma biblioteca de plug-in que permite registrar automaticamente as alterações de dados feitas pelo EF Core em uma tabela de histórico.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-140">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="ac2ee-141">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-141">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-142">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-142">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="ac2ee-143">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="ac2ee-143">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="ac2ee-144">Uma extensão que permite armazenar os resultados das consultas do EF Core em um cache de segundo nível, para que as execuções subsequentes das mesmas consultas possam evitar o acesso ao banco de dados e recuperar os dados diretamente do cache.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-144">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="ac2ee-145">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-145">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-146">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-146">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="ac2ee-147">Geco</span><span class="sxs-lookup"><span data-stu-id="ac2ee-147">Geco</span></span>

<span data-ttu-id="ac2ee-148">O Geco (Generator Console) é um gerador de código simples baseado em um projeto de console, que é executado no .NET Core e usa cadeias de caracteres interpoladas em C# para geração de código.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-148">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="ac2ee-149">O Geco inclui um gerador de modelo reverso para o EF Core, com suporte para pluralização, singularização e modelos editáveis.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-149">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="ac2ee-150">Ele também fornece um gerador de script de dados de semente, um executor de script e um limpador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-150">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="ac2ee-151">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-151">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-152">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="ac2ee-153">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="ac2ee-153">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="ac2ee-154">Permite a personalização de classes com engenharia reversa de um banco de dados existente usando a cadeia de ferramentas do Entity Framework Core com modelos de Handlebars.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-154">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="ac2ee-155">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-155">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ac2ee-156">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-156">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="ac2ee-157">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ac2ee-157">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="ac2ee-158">O NeinLinq estende provedores LINQ, como o Entity Framework, para habilitar a reutilização de funções, reescrever consultas e criar consultas dinâmicas usando predicados e seletores traduzíveis.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-158">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="ac2ee-159">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-159">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ac2ee-160">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="ac2ee-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="ac2ee-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="ac2ee-162">Um plug-in para Microsoft.EntityFrameworkCore para dar suporte ao repositório, aos padrões de unidade de trabalho e a vários bancos de dados compatíveis com a transação distribuída.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="ac2ee-163">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-163">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-164">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-164">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="ac2ee-165">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="ac2ee-165">EFCore.BulkExtensions</span></span>

<span data-ttu-id="ac2ee-166">Extensões do EF Core para operações em massa (Inserir, Atualizar e Excluir).</span><span class="sxs-lookup"><span data-stu-id="ac2ee-166">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="ac2ee-167">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-167">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ac2ee-168">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-168">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="ac2ee-169">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="ac2ee-169">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="ac2ee-170">Adiciona a pluralização de tempo de design.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-170">Adds design-time pluralization.</span></span> <span data-ttu-id="ac2ee-171">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-171">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-172">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-172">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="ac2ee-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="ac2ee-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="ac2ee-174">Reativação do atributo [Index] para o EF Core (com a extensão para criação de modelo).</span><span class="sxs-lookup"><span data-stu-id="ac2ee-174">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="ac2ee-175">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-175">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ac2ee-176">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="ac2ee-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="ac2ee-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="ac2ee-178">Fornece um wrapper em torno do provedor de banco de dados na memória EF Core.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="ac2ee-179">Faz com que ele funcione mais como um provedor relacional.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-179">Makes it act more like a relational provider.</span></span> <span data-ttu-id="ac2ee-180">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-180">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-181">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-181">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="ac2ee-182">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="ac2ee-182">EFCore.TemporalSupport</span></span>

<span data-ttu-id="ac2ee-183">Uma implementação de suporte temporal.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-183">An implementation of temporal support.</span></span> <span data-ttu-id="ac2ee-184">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-184">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-185">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-185">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="ac2ee-186">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="ac2ee-186">EfCoreTemporalTable</span></span>

<span data-ttu-id="ac2ee-187">Realize consultas temporais com facilidade em seu banco de dados favorito usando métodos de extensão introduzidos: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-187">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="ac2ee-188">Para o EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-188">For EF Core: 3.</span></span>

[<span data-ttu-id="ac2ee-189">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-189">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="ac2ee-190">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="ac2ee-190">EFCore.TimeTraveler</span></span>

<span data-ttu-id="ac2ee-191">Permita consultas completas do Entity Framework Core ao [Histórico Temporal do SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) usando o código, as entidades e os mapeamentos do EF Core que você já definiu.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-191">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="ac2ee-192">Viaje ao longo do tempo encapsulando seu código em `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-192">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="ac2ee-193">Para o EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-193">For EF Core: 3.</span></span>

[<span data-ttu-id="ac2ee-194">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-194">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="ac2ee-195">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="ac2ee-195">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="ac2ee-196">Biblioteca de extensões para Entity Framework Core que permite aos desenvolvedores que usam SQL Server usar tabelas temporais com facilidade.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-196">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="ac2ee-197">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-197">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-198">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-198">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="ac2ee-199">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="ac2ee-199">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="ac2ee-200">Um cache de consulta de segundo nível de alto desempenho.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-200">A high-performance second-level query cache.</span></span> <span data-ttu-id="ac2ee-201">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-201">For EF Core: 2.</span></span>

[<span data-ttu-id="ac2ee-202">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ac2ee-202">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="ac2ee-203">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="ac2ee-203">Entity Framework Plus</span></span>

<span data-ttu-id="ac2ee-204">Amplia o DbContext com recursos como: Filtro de Include, Auditoria, Cache, Consultar futuro, Exclusão em lote, Atualização em lote e muito mais.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-204">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="ac2ee-205">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-205">For EF Core: 2, 3.</span></span>

<span data-ttu-id="ac2ee-206">[Site](https://entityframework-plus.net/)
[Repositório do GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="ac2ee-206">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="ac2ee-207">Extensões do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ac2ee-207">Entity Framework Extensions</span></span>

<span data-ttu-id="ac2ee-208">Amplia o DbContext com operações em massa de alto desempenho: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge e muito mais.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-208">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="ac2ee-209">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ac2ee-209">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ac2ee-210">Site</span><span class="sxs-lookup"><span data-stu-id="ac2ee-210">Website</span></span>](https://entityframework-extensions.net/)
