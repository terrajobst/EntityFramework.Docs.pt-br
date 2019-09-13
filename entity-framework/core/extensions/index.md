---
title: Ferramentas e Extensões – EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 43b98c1f09a89f7e5451e28cbf2f78a2cb1040e5
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921772"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="ed706-102">Ferramentas e Extensões do EF Core</span><span class="sxs-lookup"><span data-stu-id="ed706-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="ed706-103">Essas ferramentas e extensões oferecem funcionalidades adicionais para o Entity Framework Core 2.0 e as versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="ed706-103">These tools and extensions provide additional functionality for Entity Framework Core 2.0 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ed706-104">As extensões são compostas de diversas fontes e não são mantidas como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ed706-105">Ao considerar uma extensão de terceiros, avalie a qualidade, o licenciamento, a compatibilidade, o suporte, entre outras coisas, para garantir que ela atenda às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="ed706-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="ed706-106">Ferramentas</span><span class="sxs-lookup"><span data-stu-id="ed706-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="ed706-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="ed706-107">LLBLGen Pro</span></span>

<span data-ttu-id="ed706-108">O LLBLGen Pro é uma solução de modelagem de entidade compatível com o Entity Framework e o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="ed706-109">Com ele, é possível definir facilmente o modelo de entidade e mapeá-lo em seu banco de dados usando as abordagens Model First ou Database First. Assim, você pode começar a escrever consultas imediatamente.</span><span class="sxs-lookup"><span data-stu-id="ed706-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="ed706-110">Site</span><span class="sxs-lookup"><span data-stu-id="ed706-110">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="ed706-111">Desenvolvedor de Entidade Devart</span><span class="sxs-lookup"><span data-stu-id="ed706-111">Devart Entity Developer</span></span>

<span data-ttu-id="ed706-112">O Desenvolvedor de Entidade é um potente designer de ORM para o ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="ed706-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="ed706-113">Ele é compatível com a criação de modelos do EF Core visualmente, usando as primeiras abordagens do modelo ou do banco de dados e a geração de código em C # ou Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ed706-113">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> 

[<span data-ttu-id="ed706-114">Site</span><span class="sxs-lookup"><span data-stu-id="ed706-114">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="ed706-115">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="ed706-115">EF Core Power Tools</span></span>

<span data-ttu-id="ed706-116">O EF Core Power Tools é uma extensão do Visual Studio 2017 que expõe várias tarefas de tempo de design do EF Core em uma interface do usuário simples.</span><span class="sxs-lookup"><span data-stu-id="ed706-116">EF Core Power Tools is a Visual Studio 2017 extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="ed706-117">Ele inclui a engenharia reversa do DbContext e classes de entidade de bancos de dados existentes e do [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), gerenciamento de migrações de banco de dados e visualizações de modelo.</span><span class="sxs-lookup"><span data-stu-id="ed706-117">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span>

[<span data-ttu-id="ed706-118">Wiki do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-118">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="ed706-119">Editor Visual do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ed706-119">Entity Framework Visual Editor</span></span>

<span data-ttu-id="ed706-120">O Entity Framework Visual Editor é uma extensão do Visual Studio que adiciona um designer de ORM ao design visual do EF 6 e às classes do EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-120">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="ed706-121">O código é gerado usando modelos T4. Portanto, ele pode ser personalizado para atender a quaisquer necessidades.</span><span class="sxs-lookup"><span data-stu-id="ed706-121">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="ed706-122">Ele é compatível com herança, associações unidirecionais e bidirecionais, enumerações e com a capacidade de atribuir código de cor às suas classes e de adicionar blocos de texto para explicar partes potencialmente misteriosas do seu design.</span><span class="sxs-lookup"><span data-stu-id="ed706-122">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="ed706-123">Marketplace</span><span class="sxs-lookup"><span data-stu-id="ed706-123">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="ed706-124">CatFactory</span><span class="sxs-lookup"><span data-stu-id="ed706-124">CatFactory</span></span>

<span data-ttu-id="ed706-125">O CatFactory é um mecanismo de scaffolding para o .NET Core que pode automatizar a geração de classes DbContext, entidades, configurações de mapeamento e classes de repositório de um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ed706-125">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span>

[<span data-ttu-id="ed706-126">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-126">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="ed706-127">Gerador do Entity Framework Core da LoreSoft</span><span class="sxs-lookup"><span data-stu-id="ed706-127">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="ed706-128">O Entity Framework Core Generator (efg) é uma ferramenta de CLI do .NET Core que pode gerar modelos do EF Core de um banco de dados existente, muito parecido com `dotnet ef dbcontext scaffold`, mas que também é compatível com a [regeneração](https://efg.loresoft.com/en/latest/regeneration/) do código seguro por meio da substituição de região ou da análise dos arquivos de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="ed706-128">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="ed706-129">Essa ferramenta é compatível com a geração de modelos de exibição, com a validação e o código do mapeador de objetos.</span><span class="sxs-lookup"><span data-stu-id="ed706-129">This tool supports generating view models, validation, and object mapper code.</span></span> 

<span data-ttu-id="ed706-130">[Tutorial](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentação](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="ed706-130">[Tutorial](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="ed706-131">Extensões</span><span class="sxs-lookup"><span data-stu-id="ed706-131">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="ed706-132">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="ed706-132">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="ed706-133">Uma biblioteca de plug-in que permite registrar automaticamente as alterações de dados feitas pelo EF Core em uma tabela de histórico.</span><span class="sxs-lookup"><span data-stu-id="ed706-133">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span>

[<span data-ttu-id="ed706-134">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-134">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="ed706-135">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="ed706-135">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="ed706-136">Um núcleo da porta .NET Core / .NET Standard do System.Linq.Dynamic, que inclui o suporte assíncrono com o EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-136">A .NET Core / .NET Standard port of System.Linq.Dynamic that includes async support with EF Core.</span></span>
<span data-ttu-id="ed706-137">System.Linq.Dynamic originado como uma amostra da Microsoft, que demonstra como criar consultas LINQ dinamicamente utilizando expressões de cadeia de caracteres em vez de código.</span><span class="sxs-lookup"><span data-stu-id="ed706-137">System.Linq.Dynamic originated as a Microsoft sample that shows how to construct LINQ queries dynamically from string expressions rather than code.</span></span>

[<span data-ttu-id="ed706-138">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-138">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="ed706-139">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="ed706-139">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="ed706-140">Uma extensão que permite armazenar os resultados das consultas do EF Core em um cache de segundo nível, para que as execuções subsequentes das mesmas consultas possam evitar o acesso ao banco de dados e recuperar os dados diretamente do cache.</span><span class="sxs-lookup"><span data-stu-id="ed706-140">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span>

[<span data-ttu-id="ed706-141">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-141">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="ed706-142">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="ed706-142">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="ed706-143">Essa biblioteca permite recuperar os valores da chave primária (incluindo chaves compostas) de qualquer entidade como um dicionário.</span><span class="sxs-lookup"><span data-stu-id="ed706-143">This library allows retrieving the values of primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="ed706-144">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-144">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="ed706-145">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="ed706-145">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="ed706-146">Essa biblioteca habilita o acesso fortemente tipado aos valores originais das propriedades da entidade.</span><span class="sxs-lookup"><span data-stu-id="ed706-146">This library enables strongly typed access to the original values of entity properties.</span></span> 

[<span data-ttu-id="ed706-147">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="ed706-148">Geco</span><span class="sxs-lookup"><span data-stu-id="ed706-148">Geco</span></span>

<span data-ttu-id="ed706-149">O Geco (Generator Console) é um gerador de código simples baseado em um projeto de console, que é executado no .NET Core e usa cadeias de caracteres interpoladas em C# para geração de código.</span><span class="sxs-lookup"><span data-stu-id="ed706-149">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="ed706-150">O Geco inclui um gerador de modelo reverso para o EF Core, com suporte para pluralização, singularização e modelos editáveis.</span><span class="sxs-lookup"><span data-stu-id="ed706-150">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="ed706-151">Ele também fornece um gerador de script de dados de semente, um executor de script e um limpador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ed706-151">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span>

[<span data-ttu-id="ed706-152">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="ed706-153">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ed706-153">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="ed706-154">O LinqKit.Microsoft.EntityFrameworkCore é uma versão da biblioteca LINQKit compatível com o EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-154">LinqKit.Microsoft.EntityFrameworkCore is an EF Core-compatible version of the LINQKit library.</span></span> <span data-ttu-id="ed706-155">O LINQKit é um conjunto gratuito de extensões para LINQ to SQL e para usuários avançados do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ed706-155">LINQKit is a free set of extensions for LINQ to SQL and Entity Framework power users.</span></span> <span data-ttu-id="ed706-156">Ele habilita funcionalidades avançadas, como a criação dinâmica de expressões de predicado, e o uso de variáveis de expressão em subconsultas.</span><span class="sxs-lookup"><span data-stu-id="ed706-156">It enables advanced functionality like dynamic building of predicate expressions, and using expression variables in subqueries.</span></span>  

[<span data-ttu-id="ed706-157">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-157">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="ed706-158">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ed706-158">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="ed706-159">O NeinLinq estende provedores LINQ, como o Entity Framework, para habilitar a reutilização de funções, reescrever consultas e criar consultas dinâmicas usando predicados e seletores traduzíveis.</span><span class="sxs-lookup"><span data-stu-id="ed706-159">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="ed706-160">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="ed706-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="ed706-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="ed706-162">Um plug-in para Microsoft.EntityFrameworkCore para dar suporte ao repositório, aos padrões de unidade de trabalho e a vários bancos de dados compatíveis com a transação distribuída.</span><span class="sxs-lookup"><span data-stu-id="ed706-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span>

[<span data-ttu-id="ed706-163">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-163">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="ed706-164">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="ed706-164">EFCore.BulkExtensions</span></span>

<span data-ttu-id="ed706-165">Extensões do EF Core para operações em massa (Inserir, Atualizar e Excluir).</span><span class="sxs-lookup"><span data-stu-id="ed706-165">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="ed706-166">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-166">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="ed706-167">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="ed706-167">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="ed706-168">Adiciona a pluralização de tempo de design ao EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-168">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="ed706-169">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-169">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a><span data-ttu-id="ed706-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span><span class="sxs-lookup"><span data-stu-id="ed706-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span></span>

<span data-ttu-id="ed706-171">Um método de extensão simples que obtém a instrução SQL que o EF Core geraria para uma determinada consulta LINQ em cenários simples.</span><span class="sxs-lookup"><span data-stu-id="ed706-171">A simple extension method that obtains the SQL statement EF Core would generate for a given LINQ query in simple scenarios.</span></span> <span data-ttu-id="ed706-172">O método ToSql é limitado a cenários simples porque o EF Core pode gerar mais de uma instrução SQL para uma única consulta LINQ e diferentes instruções SQL, dependendo dos valores dos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ed706-172">The ToSql method is limited to simple scenarios because EF Core can generate more than one SQL statement for a single LINQ query, and different SQL statements depending on parameter values.</span></span>

[<span data-ttu-id="ed706-173">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-173">GitHub repository</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="ed706-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="ed706-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="ed706-175">Reativação do atributo [Index] para o EF Core (com a extensão para criação de modelo).</span><span class="sxs-lookup"><span data-stu-id="ed706-175">Revival of [Index] attribute for EF Core (with extension for model building).</span></span>

[<span data-ttu-id="ed706-176">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="ed706-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="ed706-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="ed706-178">Fornece um wrapper em torno do provedor de banco de dados na memória EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="ed706-179">Faz com que ele funcione mais como um provedor relacional.</span><span class="sxs-lookup"><span data-stu-id="ed706-179">Makes it act more like a relational provider.</span></span>

[<span data-ttu-id="ed706-180">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-180">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="ed706-181">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="ed706-181">EFCore.TemporalSupport</span></span>

<span data-ttu-id="ed706-182">Uma implementação de suporte temporal para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-182">An implementation of temporal support for EF Core.</span></span>

[<span data-ttu-id="ed706-183">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-183">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="ed706-184">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="ed706-184">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="ed706-185">Um cache de consulta de segundo nível de alto desempenho para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed706-185">A high-performance second-level query cache for EF Core.</span></span>

[<span data-ttu-id="ed706-186">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-186">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="ed706-187">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="ed706-187">Entity Framework Plus</span></span>

<span data-ttu-id="ed706-188">Amplia o DbContext com recursos como: Filtro de Include, Auditoria, Cache, Consultar futuro, Exclusão em lote, Atualização em lote e muito mais.</span><span class="sxs-lookup"><span data-stu-id="ed706-188">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span>

<span data-ttu-id="ed706-189">[Site](https://entityframework-plus.net/)
[Repositório do GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="ed706-189">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="ed706-190">Extensões do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ed706-190">Entity Framework Extensions</span></span>

<span data-ttu-id="ed706-191">Amplia o DbContext com operações em massa de alto desempenho: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge e muito mais.</span><span class="sxs-lookup"><span data-stu-id="ed706-191">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span>

[<span data-ttu-id="ed706-192">Site</span><span class="sxs-lookup"><span data-stu-id="ed706-192">Website</span></span>](https://entityframework-extensions.net/)

### <a name="reconciler"></a><span data-ttu-id="ed706-193">Reconciler</span><span class="sxs-lookup"><span data-stu-id="ed706-193">Reconciler</span></span>

<span data-ttu-id="ed706-194">Atualize um grafo de entidade no repositório para um determinado grafo, inserindo, atualizando e removendo as respectivas entidades.</span><span class="sxs-lookup"><span data-stu-id="ed706-194">Update an entity graph in store to a given one by inserting, updating and removing the respective entities.</span></span>

[<span data-ttu-id="ed706-195">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="ed706-195">GitHub repository</span></span>](https://github.com/jtheisen/reconciler)
