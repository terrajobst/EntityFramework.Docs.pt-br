---
title: Introdução ao Entity Framework 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: 729dea2c474c35f638ccaf6673550f76e88e2667
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136263"
---
# <a name="get-started-with-entity-framework-6"></a><span data-ttu-id="d0a70-102">Introdução ao Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d0a70-102">Get started with Entity Framework 6</span></span>

<span data-ttu-id="d0a70-103">Este guia contém uma coleção de links para artigos de documentação selecionada, instruções passo a passo e vídeos que podem ajudar você a começar a usar rapidamente.</span><span class="sxs-lookup"><span data-stu-id="d0a70-103">This guide contains a collection of links to selected documentation articles, walkthroughs and videos that can help you get started quickly.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="d0a70-104">Conceitos básicos</span><span class="sxs-lookup"><span data-stu-id="d0a70-104">Fundamentals</span></span>

* [<span data-ttu-id="d0a70-105">Como obter o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d0a70-105">Get Entity Framework</span></span>](~/ef6/fundamentals/install.md)

  <span data-ttu-id="d0a70-106">Aqui você aprenderá a adicionar o Entity Framework aos seus aplicativos. Se desejar usar o EF Designer, o instale no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0a70-106">Here you will learn how to add Entity Framework to your applications and, if you want to use the EF Designer, make sure you get it installed in Visual Studio.</span></span>

* [<span data-ttu-id="d0a70-107">Criação de um modelo: o Code First, o EF Designer e os Fluxos de trabalho do EF</span><span class="sxs-lookup"><span data-stu-id="d0a70-107">Creating a Model: Code First, the EF Designer, and the EF Workflows</span></span>](~/ef6/modeling/index.md)

  <span data-ttu-id="d0a70-108">Você prefere especificar seu modelo do EF escrevendo código ou desenhando caixas e linhas?</span><span class="sxs-lookup"><span data-stu-id="d0a70-108">Do you prefer to specify your EF model writing code or drawing boxes and lines?</span></span>
<span data-ttu-id="d0a70-109">Você usará o EF para mapear seus objetos para um banco de dados existente ou gostaria que o EF criasse um banco de dados personalizado para seus objetos?</span><span class="sxs-lookup"><span data-stu-id="d0a70-109">Are you going to use EF to map your objects to an existing database or would you like EF to create a database tailored for your objects?</span></span>
<span data-ttu-id="d0a70-110">Aqui você aprende sobre duas abordagens diferentes para usar EF6: EF designer e Code First.</span><span class="sxs-lookup"><span data-stu-id="d0a70-110">Here you learn about two different approaches to use EF6: EF Designer and Code First.</span></span>
<span data-ttu-id="d0a70-111">Siga a discussão e assista ao vídeo sobre a diferença.</span><span class="sxs-lookup"><span data-stu-id="d0a70-111">Make sure you follow the discussion and watch the video about the difference.</span></span>

* [<span data-ttu-id="d0a70-112">Como trabalhar com DbContext</span><span class="sxs-lookup"><span data-stu-id="d0a70-112">Working with DbContext</span></span>](~/ef6/fundamentals/working-with-dbcontext.md)

  <span data-ttu-id="d0a70-113">O DbContext é o primeiro e mais importante tipo de EF que você precisa aprender como usar.</span><span class="sxs-lookup"><span data-stu-id="d0a70-113">DbContext is the first and most important EF type that you need to learn how to use.</span></span> <span data-ttu-id="d0a70-114">Ele funciona como a barra inicial para consultas de banco de dados e controla alterações feitas nos objetos para que elas possam ser persistidas novamente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d0a70-114">It serves as the launchpad for database queries and keeps track of changes you make to objects so that they can be persisted back to the database.</span></span>

* [<span data-ttu-id="d0a70-115">Fazer uma pergunta</span><span class="sxs-lookup"><span data-stu-id="d0a70-115">Ask a Question</span></span>](~/ef6/resources/get-help.md)

  <span data-ttu-id="d0a70-116">Descubra como obter ajuda de especialistas e contribuir com suas próprias respostas à comunidade.</span><span class="sxs-lookup"><span data-stu-id="d0a70-116">Find out how to get help from the experts and contribute your own answers to the community.</span></span>

* [<span data-ttu-id="d0a70-117">Contribuir</span><span class="sxs-lookup"><span data-stu-id="d0a70-117">Contribute</span></span>](https://github.com/aspnet/EntityFramework6/)

  <span data-ttu-id="d0a70-118">O Entity Framework 6 usa um modelo de desenvolvimento aberto.</span><span class="sxs-lookup"><span data-stu-id="d0a70-118">Entity Framework 6 uses an open development model.</span></span> <span data-ttu-id="d0a70-119">Descubra como é possível ajudar a aprimorar ainda mais o EF acessando nosso repositório do GitHub.</span><span class="sxs-lookup"><span data-stu-id="d0a70-119">Find out how you can help make EF even better by visiting our GitHub repository.</span></span>

## <a name="code-first-resources"></a><span data-ttu-id="d0a70-120">Recursos do Code First</span><span class="sxs-lookup"><span data-stu-id="d0a70-120">Code First resources</span></span>

  - [<span data-ttu-id="d0a70-121">Code First para um fluxo de trabalho existente do banco de dados</span><span class="sxs-lookup"><span data-stu-id="d0a70-121">Code First to an Existing Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [<span data-ttu-id="d0a70-122">Code First para um novo fluxo de trabalho do banco de dados</span><span class="sxs-lookup"><span data-stu-id="d0a70-122">Code First to a New Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/new-database.md)
  - [<span data-ttu-id="d0a70-123">Mapeamento de enumerações usando o Code First</span><span class="sxs-lookup"><span data-stu-id="d0a70-123">Mapping Enums Using Code First</span></span>](~/ef6/modeling/code-first/data-types/enums.md)
  - [<span data-ttu-id="d0a70-124">Mapeamento de tipos espaciais usando o Code First</span><span class="sxs-lookup"><span data-stu-id="d0a70-124">Mapping Spatial Types Using Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)
  - [<span data-ttu-id="d0a70-125">Como escrever convenções personalizadas do Code First</span><span class="sxs-lookup"><span data-stu-id="d0a70-125">Writing Custom Code First Conventions</span></span>](~/ef6/modeling/code-first/conventions/custom.md)
  - [<span data-ttu-id="d0a70-126">Uso da configuração fluente do Code First com o Visual Basic</span><span class="sxs-lookup"><span data-stu-id="d0a70-126">Using Code First Fluent Configuration with Visual Basic</span></span>](~/ef6/modeling/code-first/fluent/vb.md)
  - [<span data-ttu-id="d0a70-127">Migrações do Code First</span><span class="sxs-lookup"><span data-stu-id="d0a70-127">Code First Migrations</span></span>](~/ef6/modeling/code-first/migrations/index.md)
  - [<span data-ttu-id="d0a70-128">Migrações do Code First em ambientes de equipe</span><span class="sxs-lookup"><span data-stu-id="d0a70-128">Code First Migrations in Team Environments</span></span>](~/ef6/modeling/code-first/migrations/teams.md)
  - <span data-ttu-id="d0a70-129">[Migrações automáticas do Code First](~/ef6/modeling/code-first/migrations/automatic.md) (não é mais recomendado)</span><span class="sxs-lookup"><span data-stu-id="d0a70-129">[Automatic Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md) (This is no longer recommended)</span></span>

## <a name="ef-designer-resources"></a><span data-ttu-id="d0a70-130">Recursos do EF Designer</span><span class="sxs-lookup"><span data-stu-id="d0a70-130">EF Designer resources</span></span>
  - [<span data-ttu-id="d0a70-131">Fluxo de trabalho do Database First</span><span class="sxs-lookup"><span data-stu-id="d0a70-131">Database First Workflow</span></span>](~/ef6/modeling/designer/workflows/database-first.md)
  - [<span data-ttu-id="d0a70-132">Fluxo de trabalho do Model First</span><span class="sxs-lookup"><span data-stu-id="d0a70-132">Model First Workflow</span></span>](~/ef6/modeling/designer/workflows/model-first.md)
  - [<span data-ttu-id="d0a70-133">Mapeamento de enumerações</span><span class="sxs-lookup"><span data-stu-id="d0a70-133">Mapping Enums</span></span>](~/ef6/modeling/designer/data-types/enums.md)
  - [<span data-ttu-id="d0a70-134">Mapeamento de tipos espaciais</span><span class="sxs-lookup"><span data-stu-id="d0a70-134">Mapping Spatial Types</span></span>](~/ef6/modeling/designer/data-types/spatial.md)
  - [<span data-ttu-id="d0a70-135">Mapeamento de herança de tabela por hierarquia</span><span class="sxs-lookup"><span data-stu-id="d0a70-135">Table-Per Hierarchy Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tph.md)
  - [<span data-ttu-id="d0a70-136">Mapeamento de herança de tabela por tipo</span><span class="sxs-lookup"><span data-stu-id="d0a70-136">Table-Per Type Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tpt.md)
  - [<span data-ttu-id="d0a70-137">Mapeamento de procedimento armazenado para atualizações</span><span class="sxs-lookup"><span data-stu-id="d0a70-137">Stored Procedure Mapping for Updates</span></span>](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [<span data-ttu-id="d0a70-138">Mapeamento de procedimento armazenado para consulta</span><span class="sxs-lookup"><span data-stu-id="d0a70-138">Stored Procedure Mapping for Query</span></span>](~/ef6/modeling/designer/stored-procedures/query.md)
  - [<span data-ttu-id="d0a70-139">Divisão de entidade</span><span class="sxs-lookup"><span data-stu-id="d0a70-139">Entity Splitting</span></span>](~/ef6/modeling/designer/entity-splitting.md)
  - [<span data-ttu-id="d0a70-140">Divisão de tabela</span><span class="sxs-lookup"><span data-stu-id="d0a70-140">Table Splitting</span></span>](~/ef6/modeling/designer/table-splitting.md)
  - <span data-ttu-id="d0a70-141">[Definição de uma consulta](~/ef6/modeling/designer/advanced/defining-query.md) (Avançado)</span><span class="sxs-lookup"><span data-stu-id="d0a70-141">[Defining Query](~/ef6/modeling/designer/advanced/defining-query.md) (Advanced)</span></span>
  - <span data-ttu-id="d0a70-142">[Funções com valor de tabela](~/ef6/modeling/designer/advanced/tvfs.md) (Avançado)</span><span class="sxs-lookup"><span data-stu-id="d0a70-142">[Table-Valued Functions](~/ef6/modeling/designer/advanced/tvfs.md) (Advanced)</span></span>

## <a name="other-resources"></a><span data-ttu-id="d0a70-143">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="d0a70-143">Other resources</span></span>
  - [<span data-ttu-id="d0a70-144">Consulta assíncrona e salvamento</span><span class="sxs-lookup"><span data-stu-id="d0a70-144">Async Query and Save</span></span>](~/ef6/fundamentals/async.md)
  - [<span data-ttu-id="d0a70-145">Associação de dados com WinForms</span><span class="sxs-lookup"><span data-stu-id="d0a70-145">Databinding with WinForms</span></span>](~/ef6/fundamentals/databinding/winforms.md)
  - [<span data-ttu-id="d0a70-146">Associação de dados com WPF</span><span class="sxs-lookup"><span data-stu-id="d0a70-146">Databinding with WPF</span></span>](~/ef6/fundamentals/databinding/wpf.md)
  - <span data-ttu-id="d0a70-147">[Cenários desconectados com entidades de rastreamento automático](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (não mais recomendado)</span><span class="sxs-lookup"><span data-stu-id="d0a70-147">[Disconnected scenarios with Self-Tracking Entities](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (This is no longer recommended)</span></span>
