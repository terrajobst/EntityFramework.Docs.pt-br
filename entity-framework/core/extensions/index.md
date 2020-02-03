---
title: Ferramentas e Extensões – EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 99f59153a452a2f4aad5811110ebc5b5da7717ef
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888047"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="44cd2-102">Ferramentas e Extensões do EF Core</span><span class="sxs-lookup"><span data-stu-id="44cd2-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="44cd2-103">Essas ferramentas e extensões oferecem funcionalidades adicionais para o Entity Framework Core 2.1 e as versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="44cd2-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="44cd2-104">As extensões são compostas de diversas fontes e não são mantidas como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="44cd2-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="44cd2-105">Ao considerar uma extensão de terceiros, avalie a qualidade, o licenciamento, a compatibilidade, o suporte, entre outras coisas, para garantir que ela atenda às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="44cd2-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="44cd2-106">Em particular, uma extensão criada para uma versão mais antiga do EF Core pode precisar de atualização para que funcione com as versões mais recentes.</span><span class="sxs-lookup"><span data-stu-id="44cd2-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="44cd2-107">Ferramentas</span><span class="sxs-lookup"><span data-stu-id="44cd2-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="44cd2-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="44cd2-108">LLBLGen Pro</span></span>

<span data-ttu-id="44cd2-109">O LLBLGen Pro é uma solução de modelagem de entidade compatível com o Entity Framework e o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="44cd2-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="44cd2-110">Com ele, é possível definir facilmente o modelo de entidade e mapeá-lo em seu banco de dados usando as abordagens Model First ou Database First. Assim, você pode começar a escrever consultas imediatamente.</span><span class="sxs-lookup"><span data-stu-id="44cd2-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="44cd2-111">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-111">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-112">Site</span><span class="sxs-lookup"><span data-stu-id="44cd2-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="44cd2-113">Desenvolvedor de Entidade Devart</span><span class="sxs-lookup"><span data-stu-id="44cd2-113">Devart Entity Developer</span></span>

<span data-ttu-id="44cd2-114">O Desenvolvedor de Entidade é um potente designer de ORM para o ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="44cd2-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="44cd2-115">Ele é compatível com a criação de modelos do EF Core visualmente, usando as primeiras abordagens do modelo ou do banco de dados e a geração de código em C # ou Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="44cd2-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="44cd2-116">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-116">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-117">Site</span><span class="sxs-lookup"><span data-stu-id="44cd2-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="44cd2-118">nHydrate ORM para Entity Framework</span><span class="sxs-lookup"><span data-stu-id="44cd2-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="44cd2-119">Um ORM que cria classes extensíveis fortemente tipadas para Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="44cd2-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="44cd2-120">O código gerado é Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="44cd2-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="44cd2-121">Não há nenhuma diferença.</span><span class="sxs-lookup"><span data-stu-id="44cd2-121">There is no difference.</span></span> <span data-ttu-id="44cd2-122">Isso não é uma substituição do EF nem de um ORM personalizado.</span><span class="sxs-lookup"><span data-stu-id="44cd2-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="44cd2-123">É um visual, uma camada de modelagem que permite que uma equipe gerencie esquemas de banco de dados complexos.</span><span class="sxs-lookup"><span data-stu-id="44cd2-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="44cd2-124">Ele funciona bem com software SCM como o Git, permitindo o acesso de vários usuários ao seu modelo com o mínimo de conflito.</span><span class="sxs-lookup"><span data-stu-id="44cd2-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="44cd2-125">O instalador rastreia as alterações do modelo e cria scripts de atualização.</span><span class="sxs-lookup"><span data-stu-id="44cd2-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="44cd2-126">Para o EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-126">For EF Core: 3.</span></span>

[<span data-ttu-id="44cd2-127">Site do Github</span><span class="sxs-lookup"><span data-stu-id="44cd2-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="44cd2-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="44cd2-128">EF Core Power Tools</span></span>

<span data-ttu-id="44cd2-129">O EF Core Power Tools é uma extensão do Visual Studio que expõe várias tarefas de tempo de design do EF Core em uma interface do usuário simples.</span><span class="sxs-lookup"><span data-stu-id="44cd2-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="44cd2-130">Ele inclui a engenharia reversa do DbContext e classes de entidade de bancos de dados existentes e do [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), gerenciamento de migrações de banco de dados e visualizações de modelo.</span><span class="sxs-lookup"><span data-stu-id="44cd2-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="44cd2-131">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="44cd2-132">Wiki do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="44cd2-133">Editor Visual do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="44cd2-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="44cd2-134">O Entity Framework Visual Editor é uma extensão do Visual Studio que adiciona um designer de ORM ao design visual do EF 6 e às classes do EF Core.</span><span class="sxs-lookup"><span data-stu-id="44cd2-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="44cd2-135">O código é gerado usando modelos T4. Portanto, ele pode ser personalizado para atender a quaisquer necessidades.</span><span class="sxs-lookup"><span data-stu-id="44cd2-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="44cd2-136">Ele é compatível com herança, associações unidirecionais e bidirecionais, enumerações e com a capacidade de atribuir código de cor às suas classes e de adicionar blocos de texto para explicar partes potencialmente misteriosas do seu design.</span><span class="sxs-lookup"><span data-stu-id="44cd2-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="44cd2-137">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-137">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="44cd2-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="44cd2-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="44cd2-139">CatFactory</span></span>

<span data-ttu-id="44cd2-140">O CatFactory é um mecanismo de scaffolding para o .NET Core que pode automatizar a geração de classes DbContext, entidades, configurações de mapeamento e classes de repositório de um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="44cd2-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="44cd2-141">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-141">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-142">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="44cd2-143">Gerador do Entity Framework Core da LoreSoft</span><span class="sxs-lookup"><span data-stu-id="44cd2-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="44cd2-144">O Entity Framework Core Generator (efg) é uma ferramenta de CLI do .NET Core que pode gerar modelos do EF Core de um banco de dados existente, muito parecido com `dotnet ef dbcontext scaffold`, mas que também é compatível com a [regeneração](https://efg.loresoft.com/en/latest/regeneration/) do código seguro por meio da substituição de região ou da análise dos arquivos de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="44cd2-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="44cd2-145">Essa ferramenta é compatível com a geração de modelos de exibição, com a validação e o código do mapeador de objetos.</span><span class="sxs-lookup"><span data-stu-id="44cd2-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="44cd2-146">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-146">For EF Core: 2.</span></span>

<span data-ttu-id="44cd2-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentação](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="44cd2-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="44cd2-148">Extensões</span><span class="sxs-lookup"><span data-stu-id="44cd2-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="44cd2-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="44cd2-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="44cd2-150">Uma biblioteca de plug-in que permite registrar automaticamente as alterações de dados feitas pelo EF Core em uma tabela de histórico.</span><span class="sxs-lookup"><span data-stu-id="44cd2-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="44cd2-151">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-151">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-152">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="44cd2-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="44cd2-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="44cd2-154">Uma extensão que permite armazenar os resultados das consultas do EF Core em um cache de segundo nível, para que as execuções subsequentes das mesmas consultas possam evitar o acesso ao banco de dados e recuperar os dados diretamente do cache.</span><span class="sxs-lookup"><span data-stu-id="44cd2-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="44cd2-155">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-155">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-156">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="44cd2-157">Geco</span><span class="sxs-lookup"><span data-stu-id="44cd2-157">Geco</span></span>

<span data-ttu-id="44cd2-158">O Geco (Generator Console) é um gerador de código simples baseado em um projeto de console, que é executado no .NET Core e usa cadeias de caracteres interpoladas em C# para geração de código.</span><span class="sxs-lookup"><span data-stu-id="44cd2-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="44cd2-159">O Geco inclui um gerador de modelo reverso para o EF Core, com suporte para pluralização, singularização e modelos editáveis.</span><span class="sxs-lookup"><span data-stu-id="44cd2-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="44cd2-160">Ele também fornece um gerador de script de dados de semente, um executor de script e um limpador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="44cd2-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="44cd2-161">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-161">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-162">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="44cd2-163">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="44cd2-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="44cd2-164">Permite a personalização de classes com engenharia reversa de um banco de dados existente usando a cadeia de ferramentas do Entity Framework Core com modelos de Handlebars.</span><span class="sxs-lookup"><span data-stu-id="44cd2-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="44cd2-165">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="44cd2-166">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="44cd2-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="44cd2-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="44cd2-168">O NeinLinq estende provedores LINQ, como o Entity Framework, para habilitar a reutilização de funções, reescrever consultas e criar consultas dinâmicas usando predicados e seletores traduzíveis.</span><span class="sxs-lookup"><span data-stu-id="44cd2-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="44cd2-169">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="44cd2-170">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="44cd2-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="44cd2-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="44cd2-172">Um plug-in para Microsoft.EntityFrameworkCore para dar suporte ao repositório, aos padrões de unidade de trabalho e a vários bancos de dados compatíveis com a transação distribuída.</span><span class="sxs-lookup"><span data-stu-id="44cd2-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="44cd2-173">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-173">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-174">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="44cd2-175">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="44cd2-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="44cd2-176">Extensões do EF Core para operações em massa (Inserir, Atualizar e Excluir).</span><span class="sxs-lookup"><span data-stu-id="44cd2-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="44cd2-177">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="44cd2-178">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="44cd2-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="44cd2-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="44cd2-180">Adiciona a pluralização de tempo de design.</span><span class="sxs-lookup"><span data-stu-id="44cd2-180">Adds design-time pluralization.</span></span> <span data-ttu-id="44cd2-181">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-181">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-182">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="44cd2-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="44cd2-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="44cd2-184">Reativação do atributo [Index] para o EF Core (com a extensão para criação de modelo).</span><span class="sxs-lookup"><span data-stu-id="44cd2-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="44cd2-185">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="44cd2-186">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="44cd2-187">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="44cd2-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="44cd2-188">Fornece um wrapper em torno do provedor de banco de dados na memória EF Core.</span><span class="sxs-lookup"><span data-stu-id="44cd2-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="44cd2-189">Faz com que ele funcione mais como um provedor relacional.</span><span class="sxs-lookup"><span data-stu-id="44cd2-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="44cd2-190">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-190">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-191">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="44cd2-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="44cd2-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="44cd2-193">Uma implementação de suporte temporal.</span><span class="sxs-lookup"><span data-stu-id="44cd2-193">An implementation of temporal support.</span></span> <span data-ttu-id="44cd2-194">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-194">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-195">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="44cd2-196">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="44cd2-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="44cd2-197">Realize consultas temporais com facilidade em seu banco de dados favorito usando métodos de extensão introduzidos: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="44cd2-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="44cd2-198">Para o EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-198">For EF Core: 3.</span></span>

[<span data-ttu-id="44cd2-199">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="44cd2-200">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="44cd2-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="44cd2-201">Permita consultas completas do Entity Framework Core ao [Histórico Temporal do SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) usando o código, as entidades e os mapeamentos do EF Core que você já definiu.</span><span class="sxs-lookup"><span data-stu-id="44cd2-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="44cd2-202">Viaje ao longo do tempo encapsulando seu código em `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="44cd2-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="44cd2-203">Para o EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-203">For EF Core: 3.</span></span>

[<span data-ttu-id="44cd2-204">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="44cd2-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="44cd2-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="44cd2-206">Biblioteca de extensões para Entity Framework Core que permite aos desenvolvedores que usam SQL Server usar tabelas temporais com facilidade.</span><span class="sxs-lookup"><span data-stu-id="44cd2-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="44cd2-207">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-207">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-208">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="44cd2-209">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="44cd2-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="44cd2-210">Um cache de consulta de segundo nível de alto desempenho.</span><span class="sxs-lookup"><span data-stu-id="44cd2-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="44cd2-211">Para o EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="44cd2-211">For EF Core: 2.</span></span>

[<span data-ttu-id="44cd2-212">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="44cd2-213">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="44cd2-213">Entity Framework Plus</span></span>

<span data-ttu-id="44cd2-214">Amplia o DbContext com recursos como: Filtro de Include, Auditoria, Cache, Consultar futuro, Exclusão em lote, Atualização em lote e muito mais.</span><span class="sxs-lookup"><span data-stu-id="44cd2-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="44cd2-215">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="44cd2-216">[Site](https://entityframework-plus.net/)
[Repositório do GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="44cd2-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="44cd2-217">Extensões do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="44cd2-217">Entity Framework Extensions</span></span>

<span data-ttu-id="44cd2-218">Amplia o DbContext com operações em massa de alto desempenho: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge e muito mais.</span><span class="sxs-lookup"><span data-stu-id="44cd2-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="44cd2-219">Para o EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="44cd2-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="44cd2-220">Site</span><span class="sxs-lookup"><span data-stu-id="44cd2-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="44cd2-221">Expressionify</span><span class="sxs-lookup"><span data-stu-id="44cd2-221">Expressionify</span></span>

<span data-ttu-id="44cd2-222">Adicione suporte para chamar métodos de extensão em lambdas LINQ.</span><span class="sxs-lookup"><span data-stu-id="44cd2-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="44cd2-223">Para o EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="44cd2-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="44cd2-224">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="44cd2-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)
