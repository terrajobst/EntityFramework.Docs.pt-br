---
title: Visão Geral – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 5218f3f8149b15cb5ad3a14779dfa9c23eb71a0e
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152488"
---
# <a name="entity-framework-6"></a><span data-ttu-id="f55ec-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f55ec-102">Entity Framework 6</span></span>
<span data-ttu-id="f55ec-103">O EF6 (Entity Framework 6) é um O/RM (mapeador relacional de objetos) testado para .NET com muitos anos de desenvolvimento e estabilização de recursos.</span><span class="sxs-lookup"><span data-stu-id="f55ec-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="f55ec-104">Como um O/RM, o EF6 reduz a incompatibilidade de impedância entre os mundos relacionais e orientado a objeto, permitindo que desenvolvedores escrevam aplicativos que interagem com os dados armazenados em bancos de dados relacionais usando objetos .NET fortemente tipados que representam o domínio do aplicativo e eliminando a necessidade de uma grande parte do acesso aos dados “canalizando” o código que normalmente eles precisariam escrever.</span><span class="sxs-lookup"><span data-stu-id="f55ec-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="f55ec-105">O EF6 implementa vários recursos populares de O/RM:</span><span class="sxs-lookup"><span data-stu-id="f55ec-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="f55ec-106">Mapeamento de classes de entidade [POCO](~/ef6/resources/glossary.md#poco) que não dependem de nenhum tipo de EF</span><span class="sxs-lookup"><span data-stu-id="f55ec-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="f55ec-107">Controle de alterações automático</span><span class="sxs-lookup"><span data-stu-id="f55ec-107">Automatic change tracking</span></span>
- <span data-ttu-id="f55ec-108">Resolução de identidade e a unidade de trabalho</span><span class="sxs-lookup"><span data-stu-id="f55ec-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="f55ec-109">Carregamento adiantado, lento e explícito</span><span class="sxs-lookup"><span data-stu-id="f55ec-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="f55ec-110">Conversão das consultas fortemente tipadas usando LINQ (consulta integrada à linguagem)</span><span class="sxs-lookup"><span data-stu-id="f55ec-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="f55ec-111">Recursos avançados de mapeamento, incluindo compatibilidade com:</span><span class="sxs-lookup"><span data-stu-id="f55ec-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="f55ec-112">Relações um para um, um para muitos e muitos para muitos</span><span class="sxs-lookup"><span data-stu-id="f55ec-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="f55ec-113">Herança (tabela por hierarquia, tabela por tipo e a tabela por classe concreta)</span><span class="sxs-lookup"><span data-stu-id="f55ec-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="f55ec-114">Tipos complexos</span><span class="sxs-lookup"><span data-stu-id="f55ec-114">Complex types</span></span>
  - <span data-ttu-id="f55ec-115">Procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="f55ec-115">Stored procedures</span></span>
- <span data-ttu-id="f55ec-116">Um designer visual para criar modelos de entidade.</span><span class="sxs-lookup"><span data-stu-id="f55ec-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="f55ec-117">Uma experiência de "Code First" para criar modelos de entidade escrevendo código.</span><span class="sxs-lookup"><span data-stu-id="f55ec-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="f55ec-118">Os modelos podem ser gerados de bancos de dados existentes e editados manualmente ou podem ser criados do zero e usados para gerar novos bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="f55ec-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="f55ec-119">Integração com os modelos de aplicativos do .NET Framework, incluindo ASP.NET e por meio de associação de dados, com o WPF e o WinForms.</span><span class="sxs-lookup"><span data-stu-id="f55ec-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="f55ec-120">Conectividade de banco de dados com base no ADO.NET e vários provedores disponíveis para se conectar ao SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 etc.</span><span class="sxs-lookup"><span data-stu-id="f55ec-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="f55ec-121">Devo usar o EF6 ou o EF Core?</span><span class="sxs-lookup"><span data-stu-id="f55ec-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="f55ec-122">O EF Core é uma versão mais moderna, leve e extensível do Entity Framework que tem recursos e benefícios muito semelhantes ao EF6.</span><span class="sxs-lookup"><span data-stu-id="f55ec-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="f55ec-123">O EF Core é uma reformulação completa e contém muitos recursos novos que não estão disponíveis no EF6, embora ele ainda não tenha alguns dos recursos mais avançados de mapeamento do EF6.</span><span class="sxs-lookup"><span data-stu-id="f55ec-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="f55ec-124">É recomendável usar o EF Core em novos aplicativos, desde que o conjunto de recursos corresponda aos seus requisitos.</span><span class="sxs-lookup"><span data-stu-id="f55ec-124">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="f55ec-125">[Comparar o EF Core e o EF6](xref:efcore-and-ef6/index) examina esta escolha mais detalhadamente.</span><span class="sxs-lookup"><span data-stu-id="f55ec-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="f55ec-126">Introdução</span><span class="sxs-lookup"><span data-stu-id="f55ec-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="f55ec-127">Adicione o pacote NuGet EntityFramework ao seu projeto ou instale o Entity Framework Tools para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f55ec-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="f55ec-128">Em seguida, assista a vídeos e leia tutoriais e a documentação avançada para saber como aproveitar o EF6 ao máximo.</span><span class="sxs-lookup"><span data-stu-id="f55ec-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="f55ec-129">Versões anteriores do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f55ec-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="f55ec-130">Esta é a documentação para a versão mais recente do Entity Framework 6, embora a maior parte dela também se aplique às versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="f55ec-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="f55ec-131">Confira as [Novidades](~/ef6/what-is-new/index.md) e as [Versões anteriores](~/ef6/what-is-new/past-releases.md) para encontrar uma lista completa das versões do EF e os recursos introduzidos.</span><span class="sxs-lookup"><span data-stu-id="f55ec-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
