---
title: Visão geral rápida do Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
caps.latest.revision: 5
uid: ef6/index
ms.openlocfilehash: df661f19afdeef53257c86bdd32b1444737c9b0a
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913493"
---
# <a name="entity-framework-6-quick-overview"></a><span data-ttu-id="6cf67-102">Visão geral rápida do Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6cf67-102">Entity Framework 6 Quick Overview</span></span>

<span data-ttu-id="6cf67-103">O EF6 (Entity Framework 6) é um O/RM (mapeador relacional de objetos) testado para .NET com muitos anos de desenvolvimento e estabilização de recursos.</span><span class="sxs-lookup"><span data-stu-id="6cf67-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="6cf67-104">O EF6 implementa vários recursos populares de O/RM:</span><span class="sxs-lookup"><span data-stu-id="6cf67-104">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="6cf67-105">Mapeamento de classes de entidade "ignoradas com persistência" (também conhecidas como "POCO", para Objeto de CLR Antigo Plano) que não dependem de nenhum dos tipos do EF</span><span class="sxs-lookup"><span data-stu-id="6cf67-105">Mapping of "persistence ignorant" (also known as "POCO", for Plan-Old CLR Object) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="6cf67-106">Controle de alterações automático</span><span class="sxs-lookup"><span data-stu-id="6cf67-106">Automatic change tracking</span></span>
- <span data-ttu-id="6cf67-107">Resolução de identidade e a unidade de trabalho</span><span class="sxs-lookup"><span data-stu-id="6cf67-107">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="6cf67-108">Carregamento adiantado, lento e explícito</span><span class="sxs-lookup"><span data-stu-id="6cf67-108">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="6cf67-109">Conversão das consultas fortemente tipadas usando LINQ (consulta integrada à linguagem)</span><span class="sxs-lookup"><span data-stu-id="6cf67-109">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span> 
- <span data-ttu-id="6cf67-110">Recursos avançados de mapeamento, incluindo compatibilidade com:</span><span class="sxs-lookup"><span data-stu-id="6cf67-110">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="6cf67-111">Herança (tabela por hierarquia, tabela por tipo e a tabela por classe concreta)</span><span class="sxs-lookup"><span data-stu-id="6cf67-111">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="6cf67-112">Tipos complexos</span><span class="sxs-lookup"><span data-stu-id="6cf67-112">Complex types</span></span>
  - <span data-ttu-id="6cf67-113">Procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="6cf67-113">Stored procedures</span></span>
- <span data-ttu-id="6cf67-114">Um designer visual para criar modelos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6cf67-114">A visual designer to create entity models.</span></span>
- <span data-ttu-id="6cf67-115">Uma experiência de "Code First" compatível com a criação de modelos de entidade escrevendo código.</span><span class="sxs-lookup"><span data-stu-id="6cf67-115">A "Code First" experience that supports creating entity models by writing code.</span></span>
- <span data-ttu-id="6cf67-116">Os modelos podem ser gerados por meio de bancos de dados existentes e editados manualmente ou podem ser criados do zero e usados para gerar novos bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="6cf67-116">Models can either be generated form existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="6cf67-117">Integração com os modelos de aplicativos do .NET Framework, incluindo ASP.NET e por meio de associação de dados, com o WPF e o WinForms.</span><span class="sxs-lookup"><span data-stu-id="6cf67-117">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="6cf67-118">Conectividade de banco de dados com base no ADO.NET e vários provedores disponíveis para se conectar ao SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 etc.</span><span class="sxs-lookup"><span data-stu-id="6cf67-118">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

<span data-ttu-id="6cf67-119">Como um O/RM, o EF6 reduz a incompatibilidade de impedância entre os mundos relacionais e orientado a objeto, permitindo que desenvolvedores escrevam aplicativos que interagem com os dados armazenados em bancos de dados relacionais usando objetos .NET fortemente tipados que representam o domínio do aplicativo e eliminando a necessidade de uma grande parte do acesso aos dados “canalizando” o código que normalmente eles precisariam escrever.</span><span class="sxs-lookup"><span data-stu-id="6cf67-119">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="6cf67-120">Devo usar o EF6 ou o EF Core?</span><span class="sxs-lookup"><span data-stu-id="6cf67-120">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="6cf67-121">O EF Core é uma versão mais moderna, leve e extensível do Entity Framework que tem recursos e benefícios muito semelhantes ao EF6.</span><span class="sxs-lookup"><span data-stu-id="6cf67-121">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="6cf67-122">O EF Core é uma reformulação completa e contém muitos recursos novos que não estão disponíveis no EF6, embora ele ainda não tenha alguns dos recursos mais avançados de mapeamento do EF6.</span><span class="sxs-lookup"><span data-stu-id="6cf67-122">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="6cf67-123">É recomendável usar o EF Core em novos aplicativos, desde que o conjunto de recursos corresponda aos seus requisitos.</span><span class="sxs-lookup"><span data-stu-id="6cf67-123">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="6cf67-124">[Comparar o EF Core e o EF6](xref:efcore-and-ef6/index) examina esta escolha mais detalhadamente.</span><span class="sxs-lookup"><span data-stu-id="6cf67-124">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="6cf67-125">Introdução</span><span class="sxs-lookup"><span data-stu-id="6cf67-125">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="6cf67-126">Adicione o pacote NuGet EntityFramework ao seu projeto ou instale o Entity Framework Tools para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6cf67-126">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="6cf67-127">Em seguida, assista a vídeos, leia tutoriais e a documentação avançada para ajudá-lo a aproveitar ao máximo o Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="6cf67-127">Then watch videos, read tutorials, and advanced documentation to help you make the most of Entity Framework 6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="6cf67-128">Versões anteriores do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6cf67-128">Past Entity Framework Versions</span></span>

<span data-ttu-id="6cf67-129">Esta é a documentação para a versão mais recente do Entity Framework 6, embora a maior parte dela também se aplique às versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="6cf67-129">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="6cf67-130">Confira as [Novidades](~/ef6/what-is-new/index.md) e as [Versões anteriores](~/ef6/what-is-new/past-releases.md) para encontrar uma lista completa das versões do EF e os recursos introduzidos.</span><span class="sxs-lookup"><span data-stu-id="6cf67-130">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
