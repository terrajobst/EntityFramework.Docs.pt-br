---
title: Testar componentes usando o EF Core – EF Core
description: Abordagens diferentes para testar aplicativos que usam o EF Core
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634246"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="21079-103">Testar código que usa o EF Core</span><span class="sxs-lookup"><span data-stu-id="21079-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="21079-104">Para testar um código que acessa um banco de dados, é necessário:</span><span class="sxs-lookup"><span data-stu-id="21079-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="21079-105">Executar consultas e atualizações no mesmo sistema de banco de dados usado na produção.</span><span class="sxs-lookup"><span data-stu-id="21079-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="21079-106">Executar consultas e atualizações em outro sistema de banco de dados mais fácil de gerenciar.</span><span class="sxs-lookup"><span data-stu-id="21079-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="21079-107">Usar duplicatas de teste ou algum outro mecanismo para evitar por completo o uso de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="21079-108">Este documento descreve as compensações envolvidas em cada uma dessas opções e mostra como o EF Core pode ser usado com cada abordagem.</span><span class="sxs-lookup"><span data-stu-id="21079-108">This document outlines the trade offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="21079-109">Os provedores de banco de dados não são todos iguais</span><span class="sxs-lookup"><span data-stu-id="21079-109">All database providers are not equal</span></span>

<span data-ttu-id="21079-110">É muito importante entender que o EF Core não foi projetado para abstrair todos os aspectos do sistema de banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="21079-110">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="21079-111">Em vez disso, o EF Core é um conjunto comum de padrões e conceitos que podem ser usados com qualquer sistema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-111">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="21079-112">Os provedores de banco de dados do EF Core usam então uma funcionalidade e um comportamento específicos a um banco de dados como camadas sobre essa estrutura comum.</span><span class="sxs-lookup"><span data-stu-id="21079-112">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="21079-113">Isso permite que cada sistema de banco de dados seja usado para aquilo que ele faz de melhor, mantendo ainda a convergência, quando apropriado, com outros sistemas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-113">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="21079-114">Fundamentalmente, isso significa que a troca do provedor de banco de dados alterará o comportamento do EF Core. Além disso, não podemos esperar que o aplicativo funcione corretamente, a menos que ele leve explicitamente em conta todas as diferenças no comportamento.</span><span class="sxs-lookup"><span data-stu-id="21079-114">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for all differences in behavior.</span></span>
<span data-ttu-id="21079-115">Dito isso, em muitos casos, isso funcionará devido à existência de um alto grau de convergência entre bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="21079-115">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="21079-116">Isso tem um lado bom e outro ruim.</span><span class="sxs-lookup"><span data-stu-id="21079-116">This is good and bad.</span></span>
<span data-ttu-id="21079-117">Bom porque a movimentação entre bancos de dados pode ser relativamente fácil.</span><span class="sxs-lookup"><span data-stu-id="21079-117">Good because moving between databases can be relatively easy.</span></span>
<span data-ttu-id="21079-118">Ruim porque isso pode dar uma falsa sensação de segurança caso o aplicativo não seja totalmente testado no novo sistema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-118">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="21079-119">Abordagem 1: Sistema de banco de dados de produção</span><span class="sxs-lookup"><span data-stu-id="21079-119">Approach 1: Production database system</span></span>

<span data-ttu-id="21079-120">Conforme descrito na seção anterior, a única maneira de ter certeza de que você está testando o que é executado em produção é usar o mesmo sistema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-120">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="21079-121">Por exemplo, se o aplicativo implantado usar o SQL Azure, o teste também deverá ser feito no SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="21079-121">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="21079-122">No entanto, fazer com que cada desenvolvedor execute testes no SQL Azure enquanto trabalha ativamente no código seria uma alternativa lenta e cara.</span><span class="sxs-lookup"><span data-stu-id="21079-122">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="21079-123">Isso ilustra a principal compensação envolvida em todas essas abordagens: quando é apropriado deixar de usar o sistema de banco de dados de produção a fim de melhorar a eficiência do teste?</span><span class="sxs-lookup"><span data-stu-id="21079-123">This illustrates the main trade off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="21079-124">Nesse caso, a resposta é bem fácil: use um SQL Server local para testes por desenvolvedores.</span><span class="sxs-lookup"><span data-stu-id="21079-124">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="21079-125">O SQL Azure e o SQL Server são extremamente semelhantes, portanto, o teste em relação ao SQL Server geralmente é uma compensação razoável.</span><span class="sxs-lookup"><span data-stu-id="21079-125">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade off.</span></span>
<span data-ttu-id="21079-126">Dito isso, ainda é inteligente executar testes no SQL Azure antes de entrar em um cenário de produção.</span><span class="sxs-lookup"><span data-stu-id="21079-126">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="21079-127">LocalDb</span><span class="sxs-lookup"><span data-stu-id="21079-127">LocalDb</span></span> 

<span data-ttu-id="21079-128">Todos os principais sistemas de banco de dados têm alguma forma de "Developer Edition" para testes locais.</span><span class="sxs-lookup"><span data-stu-id="21079-128">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="21079-129">O SQL Server também tem um recurso chamado [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="21079-129">SQL Server also also has a feature called [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="21079-130">A principal vantagem do LocalDb é que ele ativa a instância do banco de dados sob demanda.</span><span class="sxs-lookup"><span data-stu-id="21079-130">The primary advantage of LocalDb is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="21079-131">Isso evita a execução de um serviço de banco de dados em seu computador mesmo quando você não está executando testes.</span><span class="sxs-lookup"><span data-stu-id="21079-131">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="21079-132">O LocalDb também tem suas desvantagens:</span><span class="sxs-lookup"><span data-stu-id="21079-132">LocalDb is not without it's issues:</span></span>
* <span data-ttu-id="21079-133">O escopo de suporte dele é menor do que o do [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="21079-133">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="21079-134">Ele não está disponível no Linux.</span><span class="sxs-lookup"><span data-stu-id="21079-134">It isn't available on Linux.</span></span>
* <span data-ttu-id="21079-135">Isso pode causar atraso na primeira execução de teste conforme o serviço é ativado.</span><span class="sxs-lookup"><span data-stu-id="21079-135">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="21079-136">Pessoalmente, nunca considerei um problema ter um serviço de banco de dados em execução no meu computador de desenvolvimento e geralmente recomendo, em vez disso, o uso da Developer Edition.</span><span class="sxs-lookup"><span data-stu-id="21079-136">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="21079-137">No entanto, isso pode ser apropriado para algumas pessoas, especialmente em computadores de desenvolvimento menos poderosos.</span><span class="sxs-lookup"><span data-stu-id="21079-137">However, it may be appropriate for some people, especially on less powerful dev machines.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="21079-138">Abordagem 2: SQLite</span><span class="sxs-lookup"><span data-stu-id="21079-138">Approach 2: SQLite</span></span>

<span data-ttu-id="21079-139">O EF Core testa o provedor do SQL Server principalmente executando-o em uma instância do SQL Server local.</span><span class="sxs-lookup"><span data-stu-id="21079-139">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="21079-140">Esses testes executam dezenas de milhares de consultas em alguns minutos em um computador rápido.</span><span class="sxs-lookup"><span data-stu-id="21079-140">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="21079-141">Isso ilustra que o uso do sistema de banco de dados real pode ser uma solução de alto desempenho.</span><span class="sxs-lookup"><span data-stu-id="21079-141">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="21079-142">A noção de que usar um banco de dados menos poderoso é a única maneira de executar testes rapidamente é um mito.</span><span class="sxs-lookup"><span data-stu-id="21079-142">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="21079-143">Dito isso, e se por qualquer motivo você não puder executar testes em algo próximo ao seu sistema de banco de dados de produção?</span><span class="sxs-lookup"><span data-stu-id="21079-143">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="21079-144">Nesse caso, a próxima melhor opção é usar algo com funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="21079-144">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="21079-145">Isso geralmente significa outro banco de dados relacional, para o qual o [SQLite](https://sqlite.org/index.html) é a escolha óbvia.</span><span class="sxs-lookup"><span data-stu-id="21079-145">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="21079-146">O SQLite é uma boa escolha porque:</span><span class="sxs-lookup"><span data-stu-id="21079-146">SQLite is a good choice because:</span></span>
* <span data-ttu-id="21079-147">Ele é executado em processo com seu aplicativo e, portanto, tem baixa sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="21079-147">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="21079-148">Ele usa arquivos simples, criados automaticamente para bancos de dados e, portanto, não requer gerenciamento de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-148">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="21079-149">Ele tem um modo na memória que evita até mesmo a criação do arquivo.</span><span class="sxs-lookup"><span data-stu-id="21079-149">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="21079-150">No entanto, lembre-se de que:</span><span class="sxs-lookup"><span data-stu-id="21079-150">However, remember that:</span></span>
* <span data-ttu-id="21079-151">O escopo de suporte do SQLite é inevitavelmente menor que o do seu sistema banco de dados de produção.</span><span class="sxs-lookup"><span data-stu-id="21079-151">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="21079-152">O SQLite se comportará de maneira diferente do sistema de banco de dados de produção para algumas consultas.</span><span class="sxs-lookup"><span data-stu-id="21079-152">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="21079-153">Portanto, se você usar o SQLite para alguns testes, certifique-se também de testar seu sistema de banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="21079-153">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="21079-154">Confira [Testes com o SQLite](xref:core/miscellaneous/testing/sqlite) para obter diretrizes específicas do EF Core.</span><span class="sxs-lookup"><span data-stu-id="21079-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="21079-155">Abordagem 3: Banco de dados em memória do EF Core</span><span class="sxs-lookup"><span data-stu-id="21079-155">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="21079-156">O EF Core vem com um banco de dados em memória que usamos para testes internos do EF Core propriamente dito.</span><span class="sxs-lookup"><span data-stu-id="21079-156">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="21079-157">Esse banco de dados **não é adequado, em termos gerais, como substituto para aplicativos de teste que usam o EF Core**.</span><span class="sxs-lookup"><span data-stu-id="21079-157">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="21079-158">Especificamente:</span><span class="sxs-lookup"><span data-stu-id="21079-158">Specifically:</span></span>
* <span data-ttu-id="21079-159">Ele não é um banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="21079-159">It is not a relational database</span></span>
* <span data-ttu-id="21079-160">Ele não dá suporte a transações</span><span class="sxs-lookup"><span data-stu-id="21079-160">It doesn't support transactions</span></span>
* <span data-ttu-id="21079-161">Ele não é otimizado para desempenho</span><span class="sxs-lookup"><span data-stu-id="21079-161">It is not optimized for performance</span></span>

<span data-ttu-id="21079-162">Nada disso é muito importante ao testar elementos internos do EF Core, pois nós o usamos especificamente em situações nas quais o banco de dados é irrelevante para o teste.</span><span class="sxs-lookup"><span data-stu-id="21079-162">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="21079-163">Por outro lado, essas coisas tendem a ser muito importantes ao testar um aplicativo que usa o EF Core.</span><span class="sxs-lookup"><span data-stu-id="21079-163">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="21079-164">Teste de unidade</span><span class="sxs-lookup"><span data-stu-id="21079-164">Unit testing</span></span>

<span data-ttu-id="21079-165">Considere testar uma parte da lógica de negócios que talvez precise usar alguns dados de um banco de dados, mas não esteja testando de maneira inerente as interações de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-165">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="21079-166">Uma opção é usar uma [duplicata de teste](https://en.wikipedia.org/wiki/Test_double), como uma imitação ou cópia.</span><span class="sxs-lookup"><span data-stu-id="21079-166">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="21079-167">Usamos duplicatas de teste para testes internos do EF Core.</span><span class="sxs-lookup"><span data-stu-id="21079-167">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="21079-168">No entanto, nunca tentamos fazer duplicatas de DbContext ou IQueryable.</span><span class="sxs-lookup"><span data-stu-id="21079-168">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="21079-169">Fazer isso é difícil, complicado e perigoso.</span><span class="sxs-lookup"><span data-stu-id="21079-169">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="21079-170">**Não tente fazer isso.**</span><span class="sxs-lookup"><span data-stu-id="21079-170">**Don't do it.**</span></span>

<span data-ttu-id="21079-171">Em vez disso, usamos o banco de dados em memória quando o teste de unidade é algo que usa DbContext.</span><span class="sxs-lookup"><span data-stu-id="21079-171">Instead we use the in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="21079-172">Nesse caso, o uso do banco de dados em memória é apropriado porque o teste não depende do comportamento do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-172">In this case using the in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="21079-173">Apenas não faça isso para testar as consultas ou atualizações reais do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21079-173">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="21079-174">Confira [Testes com o provedor em memória](xref:core/miscellaneous/testing/in-memory) para obter diretrizes específicas do EF Core sobre como usar o banco de dados em memória para testes de unidade.</span><span class="sxs-lookup"><span data-stu-id="21079-174">See [Testing with the in-memory provider](xref:core/miscellaneous/testing/in-memory) for EF Core specific guidance on using the in-memory database for unit testing.</span></span>
