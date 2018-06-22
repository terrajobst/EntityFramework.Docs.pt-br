---
title: Ferramentas e Extensões – EF Core
author: ErikEJ
ms.author: divega
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/extensions/index
ms.openlocfilehash: 6c8cb3e0d8552f274118e4020b7e2e8009af7e11
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769433"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="abedf-102">Ferramentas e Extensões do EF Core</span><span class="sxs-lookup"><span data-stu-id="abedf-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="abedf-103">Ferramentas e extensões oferecem funcionalidade adicional para o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="abedf-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="abedf-104">As extensões são compostas de diversas fontes e não são mantidas como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="abedf-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="abedf-105">Ao considerar uma extensão de terceiros, avalie a qualidade, o licenciamento, a compatibilidade, o suporte etc. para garantir ela cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="abedf-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="abedf-106">Ferramentas</span><span class="sxs-lookup"><span data-stu-id="abedf-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="abedf-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="abedf-107">LLBLGen Pro</span></span>

<span data-ttu-id="abedf-108">O LLBLGen Pro é uma solução de modelagem de entidade compatível com o Entity Framework e o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="abedf-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="abedf-109">Com ele, é possível definir facilmente o modelo de entidade e mapeá-lo em seu banco de dados usando as abordagens Model First ou Database First. Assim, você pode começar a escrever consultas imediatamente.</span><span class="sxs-lookup"><span data-stu-id="abedf-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="abedf-110">site</span><span class="sxs-lookup"><span data-stu-id="abedf-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="abedf-111">Desenvolvedor de Entidade Devart</span><span class="sxs-lookup"><span data-stu-id="abedf-111">Devart Entity Developer</span></span>

<span data-ttu-id="abedf-112">O Desenvolvedor de Entidade é um potente designer de ORM para o ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="abedf-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="abedf-113">Use as abordagens Model First e Database First para criar seu modelo de ORM e gerar um código C# ou Visual Basic .NET para ele.</span><span class="sxs-lookup"><span data-stu-id="abedf-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="abedf-114">O Desenvolvedor de Entidade apresenta novas abordagens para a criação de modelos de ORM, aumenta a produtividade e facilita o desenvolvimento de aplicativos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="abedf-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="abedf-115">site</span><span class="sxs-lookup"><span data-stu-id="abedf-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="abedf-116">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="abedf-116">EF Core Power Tools</span></span>

<span data-ttu-id="abedf-117">Visual Studio 2017+ extensão.</span><span class="sxs-lookup"><span data-stu-id="abedf-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="abedf-118">É possível fazer engenharia reversa do DbContext e de classes POCO por meio de um banco de dados existente ou projeto de banco de dados SQL Server, bem como visualizar e inspecionar seu DbContext de diversas maneiras.</span><span class="sxs-lookup"><span data-stu-id="abedf-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="abedf-119">Wiki do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a><span data-ttu-id="abedf-120">Extensões</span><span class="sxs-lookup"><span data-stu-id="abedf-120">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="abedf-121">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="abedf-121">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="abedf-122">Plug-in para o Microsoft.EntityFrameworkCore para oferecer suporte à gravação automática do histórico de alterações de dados.</span><span class="sxs-lookup"><span data-stu-id="abedf-122">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="abedf-123">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-123">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="abedf-124">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="abedf-124">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="abedf-125">Extensões do Dynamic Linq para Microsoft.EntityFrameworkCore que adicionam suporte assíncrono</span><span class="sxs-lookup"><span data-stu-id="abedf-125">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="abedf-126">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-126">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="abedf-127">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="abedf-127">EFCore.Practices</span></span>

<span data-ttu-id="abedf-128">Tente capturar algumas boas ou melhores práticas em uma API com suporte para testes – incluindo uma pequena estrutura para verificar se há consultas N+1.</span><span class="sxs-lookup"><span data-stu-id="abedf-128">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="abedf-129">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-129">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="abedf-130">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="abedf-130">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="abedf-131">Biblioteca de Cache de Segundo Nível.</span><span class="sxs-lookup"><span data-stu-id="abedf-131">Second Level Caching Library.</span></span> <span data-ttu-id="abedf-132">O cache segundo nível é um cache de consulta.</span><span class="sxs-lookup"><span data-stu-id="abedf-132">Second level caching is a query cache.</span></span> <span data-ttu-id="abedf-133">Os resultados dos comandos do EF serão armazenados no cache. Esses mesmos comandos recuperarão seus dados do cache em vez de executá-los no banco de dados novamente.</span><span class="sxs-lookup"><span data-stu-id="abedf-133">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="abedf-134">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-134">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="abedf-135">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="abedf-135">Detached.EntityFramework</span></span>

<span data-ttu-id="abedf-136">Carrega e salva grafos de entidade desanexados inteiros (a entidade com suas entidades filho e listas).</span><span class="sxs-lookup"><span data-stu-id="abedf-136">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="abedf-137">Inspirado por [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="abedf-137">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="abedf-138">A ideia é também adicionar alguns plug-ins para simplificar algumas tarefas repetitivas, como auditoria e paginação.</span><span class="sxs-lookup"><span data-stu-id="abedf-138">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="abedf-139">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-139">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="abedf-140">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="abedf-140">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="abedf-141">Recupere a chave primária (incluindo chaves compostas) de qualquer entidade como um dicionário.</span><span class="sxs-lookup"><span data-stu-id="abedf-141">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="abedf-142">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-142">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="abedf-143">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="abedf-143">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="abedf-144">Wrappers de extensão reativa para observáveis ativos das entidades do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="abedf-144">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="abedf-145">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-145">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="abedf-146">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="abedf-146">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="abedf-147">Adicione gatilhos às suas entidades com eventos de inserção, atualização e exclusão.</span><span class="sxs-lookup"><span data-stu-id="abedf-147">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="abedf-148">Há três eventos para cada um: antes, após e em caso de falha.</span><span class="sxs-lookup"><span data-stu-id="abedf-148">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="abedf-149">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-149">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="abedf-150">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="abedf-150">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="abedf-151">Obtenha acesso tipado ao OriginalValue de suas propriedades de entidade.</span><span class="sxs-lookup"><span data-stu-id="abedf-151">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="abedf-152">Há suporte para propriedades simples e complexas. Não há suporte para navegação/coleções.</span><span class="sxs-lookup"><span data-stu-id="abedf-152">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="abedf-153">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-153">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="abedf-154">Geco</span><span class="sxs-lookup"><span data-stu-id="abedf-154">Geco</span></span>

<span data-ttu-id="abedf-155">O Geco oferece um gerador de Modelo Reverso com suporte para Pluralização/Singularização e modelos editáveis baseados em cadeias de caracteres interpoladas do C# 6.0 executado no .Net Core.</span><span class="sxs-lookup"><span data-stu-id="abedf-155">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="abedf-156">Também conta com um gerador de script Seed com scripts SQL Merge e um executor de script.</span><span class="sxs-lookup"><span data-stu-id="abedf-156">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="abedf-157">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-157">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="abedf-158">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="abedf-158">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="abedf-159">O LinqKit.Microsoft.EntityFrameworkCore é um conjunto gratuito de extensões para usuários avançados do LINQ to SQL e do EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="abedf-159">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="abedf-160">Com suporte para Include(...) e IDbAsync.</span><span class="sxs-lookup"><span data-stu-id="abedf-160">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="abedf-161">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-161">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="abedf-162">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="abedf-162">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="abedf-163">O NeinLinq.EntityFrameworkCore oferece extensões úteis para provedores LINQ, como o Entity Framework compatível com apenas um subconjunto menor de funções do .NET, reutilização de funções, reescrita de consultas e transformação delas em null-safe e criação de consultas dinâmicas com predicados e seletores traduzíveis.</span><span class="sxs-lookup"><span data-stu-id="abedf-163">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="abedf-164">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-164">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="abedf-165">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="abedf-165">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="abedf-166">Um plug-in para Microsoft.EntityFrameworkCore para dar suporte a repositório, padrões de unidade de trabalho e vários bancos de dados compatíveis com transação distribuída.</span><span class="sxs-lookup"><span data-stu-id="abedf-166">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="abedf-167">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-167">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="abedf-168">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="abedf-168">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="abedf-169">Carregamento lento para EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="abedf-169">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="abedf-170">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-170">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="abedf-171">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="abedf-171">EFCore.BulkExtensions</span></span>

<span data-ttu-id="abedf-172">Extensões do EntityFrameworkCore para operações em massa (Inserir, Atualizar e Excluir).</span><span class="sxs-lookup"><span data-stu-id="abedf-172">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="abedf-173">Repositório do GitHub</span><span class="sxs-lookup"><span data-stu-id="abedf-173">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)
