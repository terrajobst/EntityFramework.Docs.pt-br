---
title: O que há de novo no EF Core 5,0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052028"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="970b5-102">O que há de novo no EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="970b5-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="970b5-103">O EF Core 5,0 está em desenvolvimento no momento.</span><span class="sxs-lookup"><span data-stu-id="970b5-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="970b5-104">Esta página conterá uma visão geral das alterações interessantes introduzidas em cada visualização.</span><span class="sxs-lookup"><span data-stu-id="970b5-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="970b5-105">A primeira visualização do EF Core 5,0 é provisoriamente esperada no primeiro trimestre de 2020.</span><span class="sxs-lookup"><span data-stu-id="970b5-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="970b5-106">Esta página não duplica o [plano para EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="970b5-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="970b5-107">O plano descreve os temas gerais para EF Core 5,0, incluindo tudo o que estamos planejando incluir antes de enviar a versão final.</span><span class="sxs-lookup"><span data-stu-id="970b5-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="970b5-108">Vamos adicionar links daqui à documentação oficial conforme ele é publicado.</span><span class="sxs-lookup"><span data-stu-id="970b5-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="970b5-109">Versão prévia 1 (ainda não enviada)</span><span class="sxs-lookup"><span data-stu-id="970b5-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="970b5-110">Log simples</span><span class="sxs-lookup"><span data-stu-id="970b5-110">Simple logging</span></span>

<span data-ttu-id="970b5-111">Esse recurso adiciona uma funcionalidade semelhante à `Database.Log` em EF6.</span><span class="sxs-lookup"><span data-stu-id="970b5-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="970b5-112">Ou seja, ele fornece uma maneira simples de obter logs de EF Core sem a necessidade de configurar qualquer tipo de estrutura de log externa.</span><span class="sxs-lookup"><span data-stu-id="970b5-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="970b5-113">A documentação preliminar está incluída no [status semanal do EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="970b5-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="970b5-114">A documentação adicional é acompanhada pelo problema [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="970b5-114">Additional documentation is tracked by issue [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="970b5-115">Maneira simples de obter o SQL gerado</span><span class="sxs-lookup"><span data-stu-id="970b5-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="970b5-116">EF Core 5,0 apresenta o método de extensão `ToQueryString`, que retornará o SQL que EF Core gerará ao executar uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="970b5-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="970b5-117">A documentação preliminar está incluída no [status semanal do EF para 9 de janeiro de 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="970b5-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="970b5-118">A documentação adicional é acompanhada pelo problema [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="970b5-118">Additional documentation is tracked by issue [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="970b5-119">Exibições de depuração aprimoradas</span><span class="sxs-lookup"><span data-stu-id="970b5-119">Enhanced debug views</span></span>

<span data-ttu-id="970b5-120">As exibições de depuração são uma maneira fácil de examinar os elementos internos de EF Core ao depurar problemas.</span><span class="sxs-lookup"><span data-stu-id="970b5-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="970b5-121">Uma exibição de depuração para o modelo foi implementada há algum tempo.</span><span class="sxs-lookup"><span data-stu-id="970b5-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="970b5-122">Para EF Core 5,0, tornamos a exibição de modelo mais fácil de ler e adicionamos uma nova exibição de depuração para entidades rastreadas no Gerenciador de estado.</span><span class="sxs-lookup"><span data-stu-id="970b5-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="970b5-123">A documentação preliminar está incluída no [status semanal do EF para 12 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="970b5-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="970b5-124">A documentação adicional é acompanhada pelo problema [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="970b5-124">Additional documentation is tracked by issue [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="970b5-125">A conexão ou a cadeia de conexão pode ser alterada no DbContext inicializado</span><span class="sxs-lookup"><span data-stu-id="970b5-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="970b5-126">Agora é mais fácil criar uma instância DbContext sem nenhuma conexão ou cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="970b5-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="970b5-127">Além disso, a conexão ou a cadeia de conexão agora pode ser modificada na instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="970b5-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="970b5-128">Isso permite que a mesma instância de contexto se conecte dinamicamente a bancos de dados diferentes.</span><span class="sxs-lookup"><span data-stu-id="970b5-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="970b5-129">A documentação é controlada pelo problema [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="970b5-129">Documentation is tracked by issue [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="970b5-130">Proxies de controle de alterações</span><span class="sxs-lookup"><span data-stu-id="970b5-130">Change-tracking proxies</span></span>

<span data-ttu-id="970b5-131">EF Core agora pode gerar proxies de tempo de execução que implementam automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="970b5-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="970b5-132">Em seguida, eles relatam alterações de valor nas propriedades de entidade diretamente em EF Core, evitando a necessidade de verificar se há alterações.</span><span class="sxs-lookup"><span data-stu-id="970b5-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="970b5-133">No entanto, os proxies vêm com seu próprio conjunto de limitações, para que eles não sejam para todos.</span><span class="sxs-lookup"><span data-stu-id="970b5-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="970b5-134">A documentação é controlada pelo problema [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="970b5-134">Documentation is tracked by issue [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="970b5-135">Tratamento aprimorado da semântica nula do banco de dados</span><span class="sxs-lookup"><span data-stu-id="970b5-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="970b5-136">Os bancos de dados relacionais normalmente tratam nulo como um valor desconhecido e, portanto, não são iguais a nenhum outro nulo.</span><span class="sxs-lookup"><span data-stu-id="970b5-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="970b5-137">C#, por outro lado, trata NULL como um valor definido que compara igual a qualquer outro nulo.</span><span class="sxs-lookup"><span data-stu-id="970b5-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="970b5-138">EF Core, por padrão, traduz as consultas para que elas C# usem a semântica nula.</span><span class="sxs-lookup"><span data-stu-id="970b5-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="970b5-139">EF Core 5,0 melhora muito a eficiência dessas traduções.</span><span class="sxs-lookup"><span data-stu-id="970b5-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="970b5-140">A documentação é controlada pelo problema [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="970b5-140">Documentation is tracked by issue [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="970b5-141">Propriedades do indexador</span><span class="sxs-lookup"><span data-stu-id="970b5-141">Indexer properties</span></span>

<span data-ttu-id="970b5-142">EF Core 5,0 dá suporte ao C# mapeamento de propriedades do indexador.</span><span class="sxs-lookup"><span data-stu-id="970b5-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="970b5-143">Isso permite que as entidades atuem como pacotes de propriedades, em que as colunas são mapeadas para as propriedades nomeadas na bolsa.</span><span class="sxs-lookup"><span data-stu-id="970b5-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="970b5-144">A documentação é controlada pelo problema [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="970b5-144">Documentation is tracked by issue [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="970b5-145">Geração de restrições de verificação para mapeamentos de enumeração</span><span class="sxs-lookup"><span data-stu-id="970b5-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="970b5-146">EF Core migrações 5,0 agora podem gerar restrições de verificação para mapeamentos de propriedade de enumeração.</span><span class="sxs-lookup"><span data-stu-id="970b5-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="970b5-147">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="970b5-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="970b5-148">A documentação é controlada pelo problema [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="970b5-148">Documentation is tracked by issue [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="970b5-149">Conversões de consulta para mais construções DateTime</span><span class="sxs-lookup"><span data-stu-id="970b5-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="970b5-150">As consultas que contêm a nova construção DataTime agora estão traduzidas.</span><span class="sxs-lookup"><span data-stu-id="970b5-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="970b5-151">Além disso, a função SQL Server DateDiffWeek agora está mapeada.</span><span class="sxs-lookup"><span data-stu-id="970b5-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="970b5-152">A documentação é controlada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="970b5-152">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="970b5-153">Conversões de consulta para mais constructos de matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="970b5-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="970b5-154">As consultas que usam Contains, Length, SequenceEqual, etc. nas propriedades byte [] agora são convertidas em SQL.</span><span class="sxs-lookup"><span data-stu-id="970b5-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="970b5-155">A documentação preliminar está incluída no [status semanal do EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="970b5-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="970b5-156">A documentação adicional é acompanhada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="970b5-156">Additional documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="970b5-157">Conversão de consulta para reversa</span><span class="sxs-lookup"><span data-stu-id="970b5-157">Query translation for Reverse</span></span>

<span data-ttu-id="970b5-158">As consultas que usam `Reverse` agora são traduzidas.</span><span class="sxs-lookup"><span data-stu-id="970b5-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="970b5-159">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="970b5-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="970b5-160">A documentação é controlada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="970b5-160">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="970b5-161">Conversão de consulta para operadores bit a bit</span><span class="sxs-lookup"><span data-stu-id="970b5-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="970b5-162">Agora, as consultas que usam operadores de bit que são são traduzidas em mais casos, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="970b5-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="970b5-163">A documentação é controlada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="970b5-163">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="970b5-164">Conversão de consulta para cadeias de caracteres em Cosmos</span><span class="sxs-lookup"><span data-stu-id="970b5-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="970b5-165">As consultas que usam os métodos de cadeia de caracteres contém, StartsWith e EndsWith agora são traduzidas ao usar o provedor de Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="970b5-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="970b5-166">A documentação é controlada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="970b5-166">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>
