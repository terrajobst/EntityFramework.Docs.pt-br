---
title: Log de alterações que afetam o provedor – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998360"
---
# <a name="provider-impacting-changes"></a><span data-ttu-id="66a7e-102">Alterações que afetam o provedor</span><span class="sxs-lookup"><span data-stu-id="66a7e-102">Provider-impacting changes</span></span>

<span data-ttu-id="66a7e-103">Esta página contém links para efetuar pull de solicitações feitas no repositório do EF Core que pode exigir que os autores de outros provedores de banco de dados para reagir.</span><span class="sxs-lookup"><span data-stu-id="66a7e-103">This page contains links to pull requests made on the EF Core repo that may require authors of other database providers to react.</span></span> <span data-ttu-id="66a7e-104">A intenção é fornecer um ponto de partida para autores de provedores de banco de dados de terceiros existente ao atualizar o seu provedor para uma nova versão.</span><span class="sxs-lookup"><span data-stu-id="66a7e-104">The intention is to provide a starting point for authors of existing third-party database providers when updating their provider to a new version.</span></span>

<span data-ttu-id="66a7e-105">Esse log estamos começando com alterações da 2.1 para 2.2.</span><span class="sxs-lookup"><span data-stu-id="66a7e-105">We are starting this log with changes from 2.1 to 2.2.</span></span> <span data-ttu-id="66a7e-106">Antes de 2.1 usamos a [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) rótulos em nossos problemas e solicitações de pull.</span><span class="sxs-lookup"><span data-stu-id="66a7e-106">Prior to 2.1 we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our issues and pull requests.</span></span>

### <a name="21-----22"></a><span data-ttu-id="66a7e-107">2.1---> 2.2</span><span class="sxs-lookup"><span data-stu-id="66a7e-107">2.1 ---> 2.2</span></span>

#### <a name="test-only-changes"></a><span data-ttu-id="66a7e-108">Alterações de teste</span><span class="sxs-lookup"><span data-stu-id="66a7e-108">Test-only changes</span></span>

* <span data-ttu-id="66a7e-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Permitir delimitadores personalizáveis do SQL nos testes</span><span class="sxs-lookup"><span data-stu-id="66a7e-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 - Allow customizable SQL delimeters in tests</span></span>
  * <span data-ttu-id="66a7e-110">Testar as alterações que permitem que comparações de ponto flutuante não restrito em BuiltInDataTypesTestBase</span><span class="sxs-lookup"><span data-stu-id="66a7e-110">Test changes that allow non-strict floating point comparisons in BuiltInDataTypesTestBase</span></span>
  * <span data-ttu-id="66a7e-111">Alterações de teste que permitem testes de consulta ser reutilizado com diferentes dos delimitadores SQL</span><span class="sxs-lookup"><span data-stu-id="66a7e-111">Test changes that allow query tests to be re-used with different SQL delimeters</span></span>
* <span data-ttu-id="66a7e-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Adicionar testes de DbFunction para os testes de especificação relacional</span><span class="sxs-lookup"><span data-stu-id="66a7e-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 - Add DbFunction tests to the relational specification tests</span></span>
  * <span data-ttu-id="66a7e-113">De modo que esses testes podem ser executados em relação a todos os provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="66a7e-113">Such that these tests can be run against all database providers</span></span>
* <span data-ttu-id="66a7e-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Limpeza de teste Async</span><span class="sxs-lookup"><span data-stu-id="66a7e-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 - Async test cleanup</span></span>
  * <span data-ttu-id="66a7e-115">Remover `Wait` chamadas, desnecessários async e renomeado alguns métodos de teste</span><span class="sxs-lookup"><span data-stu-id="66a7e-115">Remove `Wait` calls, unneeded async, and renamed some test methods</span></span>
* <span data-ttu-id="66a7e-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Unificar a infraestrutura de teste do log</span><span class="sxs-lookup"><span data-stu-id="66a7e-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 - Unify logging test infrastructure</span></span>
  * <span data-ttu-id="66a7e-117">Adicionado `CreateListLoggerFactory` e removidos alguns infraestrutura de log anterior, o que exigirá provedores que usam esses testes para reagir</span><span class="sxs-lookup"><span data-stu-id="66a7e-117">Added `CreateListLoggerFactory` and removed some previous logging infrastructure, which will require providers using these tests to react</span></span>
* <span data-ttu-id="66a7e-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Executar mais testes de consulta de forma síncrona e assíncrona</span><span class="sxs-lookup"><span data-stu-id="66a7e-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 - Run more query tests both synchronously and asynchronously</span></span>
  * <span data-ttu-id="66a7e-119">Nomes de teste e a fatoração foi alterado, que exigirá provedores que usam esses testes para reagir</span><span class="sxs-lookup"><span data-stu-id="66a7e-119">Test names and factoring has changed, which will require providers using these tests to react</span></span>
* <span data-ttu-id="66a7e-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Renomeação navegações no modelo ComplexNavigations</span><span class="sxs-lookup"><span data-stu-id="66a7e-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 - Renaming navigations in the ComplexNavigations model</span></span>
  * <span data-ttu-id="66a7e-121">Talvez seja necessário reagir provedores que usam esses testes</span><span class="sxs-lookup"><span data-stu-id="66a7e-121">Providers using these tests may need to react</span></span>
* <span data-ttu-id="66a7e-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Retornar o contexto para o pool em vez de descartar em testes funcionais</span><span class="sxs-lookup"><span data-stu-id="66a7e-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 - Return the context to the pool instead of disposing in functional tests</span></span>
  * <span data-ttu-id="66a7e-123">Essa alteração inclui certa refatoração do teste que podem exigir provedores reagir</span><span class="sxs-lookup"><span data-stu-id="66a7e-123">This change includes some test refactoring which may require providers to react</span></span>


#### <a name="test-and-product-code-changes"></a><span data-ttu-id="66a7e-124">Alterações de código do produto e de teste</span><span class="sxs-lookup"><span data-stu-id="66a7e-124">Test and product code changes</span></span>

* <span data-ttu-id="66a7e-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Consolidar RelationalTypeMapping.Clone métodos</span><span class="sxs-lookup"><span data-stu-id="66a7e-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 - Consolidate RelationalTypeMapping.Clone methods</span></span>
  * <span data-ttu-id="66a7e-126">As alterações no 2.1 para a RelationalTypeMapping permitidas para uma simplificação em classes derivadas.</span><span class="sxs-lookup"><span data-stu-id="66a7e-126">Changes in 2.1 to the RelationalTypeMapping allowed for a simplification in derived classes.</span></span> <span data-ttu-id="66a7e-127">Não acreditamos que isso estava interrompendo aos provedores, mas provedores podem tirar proveito dessa alteração no seu tipo derivado classes de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="66a7e-127">We don't believe this was breaking to providers, but providers can take advantage of this change in their derived type mapping classes.</span></span>
* <span data-ttu-id="66a7e-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Consultas nomeadas ou marcadas</span><span class="sxs-lookup"><span data-stu-id="66a7e-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 - Tagged or named queries</span></span>
  * <span data-ttu-id="66a7e-129">Adiciona a infraestrutura para marcação de consultas LINQ e ter essas marcas que aparecem como comentários no SQL.</span><span class="sxs-lookup"><span data-stu-id="66a7e-129">Adds infrastructure for tagging LINQ queries and having those tags show up as comments in the SQL.</span></span> <span data-ttu-id="66a7e-130">Isso pode exigir provedores reagir na geração de SQL.</span><span class="sxs-lookup"><span data-stu-id="66a7e-130">This may require providers to react in SQL generation.</span></span>
