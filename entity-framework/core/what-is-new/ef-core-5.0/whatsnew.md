---
title: O que há de novo no EF Core 5,0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136253"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="d5261-102">O que há de novo no EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="d5261-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="d5261-103">O EF Core 5,0 está em desenvolvimento no momento.</span><span class="sxs-lookup"><span data-stu-id="d5261-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="d5261-104">Esta página conterá uma visão geral das alterações interessantes introduzidas em cada visualização.</span><span class="sxs-lookup"><span data-stu-id="d5261-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="d5261-105">Esta página não duplica o [plano para EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="d5261-105">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="d5261-106">O plano descreve os temas gerais para EF Core 5,0, incluindo tudo o que estamos planejando incluir antes de enviar a versão final.</span><span class="sxs-lookup"><span data-stu-id="d5261-106">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="d5261-107">Vamos adicionar links daqui à documentação oficial conforme ele é publicado.</span><span class="sxs-lookup"><span data-stu-id="d5261-107">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1"></a><span data-ttu-id="d5261-108">Preview 1</span><span class="sxs-lookup"><span data-stu-id="d5261-108">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="d5261-109">Log simples</span><span class="sxs-lookup"><span data-stu-id="d5261-109">Simple logging</span></span>

<span data-ttu-id="d5261-110">Esse recurso adiciona uma funcionalidade semelhante à `Database.Log` em EF6.</span><span class="sxs-lookup"><span data-stu-id="d5261-110">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="d5261-111">Ou seja, ele fornece uma maneira simples de obter logs de EF Core sem a necessidade de configurar qualquer tipo de estrutura de log externa.</span><span class="sxs-lookup"><span data-stu-id="d5261-111">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="d5261-112">A documentação preliminar está incluída no [status semanal do EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="d5261-112">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="d5261-113">A documentação adicional é acompanhada pelo problema [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="d5261-113">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="d5261-114">Maneira simples de obter o SQL gerado</span><span class="sxs-lookup"><span data-stu-id="d5261-114">Simple way to get generated SQL</span></span>

<span data-ttu-id="d5261-115">EF Core 5,0 apresenta o método de extensão `ToQueryString`, que retornará o SQL que EF Core gerará ao executar uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="d5261-115">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="d5261-116">A documentação preliminar está incluída no [status semanal do EF para 9 de janeiro de 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="d5261-116">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="d5261-117">A documentação adicional é acompanhada pelo problema [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="d5261-117">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="d5261-118">Use um C# atributo para indicar que uma entidade não tem nenhuma chave</span><span class="sxs-lookup"><span data-stu-id="d5261-118">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="d5261-119">Um tipo de entidade agora pode ser configurado como sem chave usando o novo `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="d5261-119">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="d5261-120">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d5261-120">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="d5261-121">A documentação é controlada pelo problema [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="d5261-121">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="d5261-122">A conexão ou a cadeia de conexão pode ser alterada no DbContext inicializado</span><span class="sxs-lookup"><span data-stu-id="d5261-122">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="d5261-123">Agora é mais fácil criar uma instância DbContext sem nenhuma conexão ou cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="d5261-123">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="d5261-124">Além disso, a conexão ou a cadeia de conexão agora pode ser modificada na instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="d5261-124">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="d5261-125">Isso permite que a mesma instância de contexto se conecte dinamicamente a bancos de dados diferentes.</span><span class="sxs-lookup"><span data-stu-id="d5261-125">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="d5261-126">A documentação é controlada pelo problema [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="d5261-126">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="d5261-127">Proxies de controle de alterações</span><span class="sxs-lookup"><span data-stu-id="d5261-127">Change-tracking proxies</span></span>

<span data-ttu-id="d5261-128">EF Core agora pode gerar proxies de tempo de execução que implementam automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="d5261-128">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="d5261-129">Em seguida, eles relatam alterações de valor nas propriedades de entidade diretamente em EF Core, evitando a necessidade de verificar se há alterações.</span><span class="sxs-lookup"><span data-stu-id="d5261-129">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="d5261-130">No entanto, os proxies vêm com seu próprio conjunto de limitações, para que eles não sejam para todos.</span><span class="sxs-lookup"><span data-stu-id="d5261-130">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="d5261-131">A documentação é controlada pelo problema [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="d5261-131">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="d5261-132">Exibições de depuração aprimoradas</span><span class="sxs-lookup"><span data-stu-id="d5261-132">Enhanced debug views</span></span>

<span data-ttu-id="d5261-133">As exibições de depuração são uma maneira fácil de examinar os elementos internos de EF Core ao depurar problemas.</span><span class="sxs-lookup"><span data-stu-id="d5261-133">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="d5261-134">Uma exibição de depuração para o modelo foi implementada há algum tempo.</span><span class="sxs-lookup"><span data-stu-id="d5261-134">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="d5261-135">Para EF Core 5,0, tornamos a exibição de modelo mais fácil de ler e adicionamos uma nova exibição de depuração para entidades rastreadas no Gerenciador de estado.</span><span class="sxs-lookup"><span data-stu-id="d5261-135">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="d5261-136">A documentação preliminar está incluída no [status semanal do EF para 12 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="d5261-136">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="d5261-137">A documentação adicional é acompanhada pelo problema [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="d5261-137">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="d5261-138">Tratamento aprimorado da semântica nula do banco de dados</span><span class="sxs-lookup"><span data-stu-id="d5261-138">Improved handling of database null semantics</span></span>

<span data-ttu-id="d5261-139">Os bancos de dados relacionais normalmente tratam nulo como um valor desconhecido e, portanto, não são iguais a nenhum outro nulo.</span><span class="sxs-lookup"><span data-stu-id="d5261-139">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="d5261-140">C#, por outro lado, trata NULL como um valor definido que compara igual a qualquer outro nulo.</span><span class="sxs-lookup"><span data-stu-id="d5261-140">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="d5261-141">EF Core, por padrão, traduz as consultas para que elas C# usem a semântica nula.</span><span class="sxs-lookup"><span data-stu-id="d5261-141">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="d5261-142">EF Core 5,0 melhora muito a eficiência dessas traduções.</span><span class="sxs-lookup"><span data-stu-id="d5261-142">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="d5261-143">A documentação é controlada pelo problema [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="d5261-143">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="d5261-144">Propriedades do indexador</span><span class="sxs-lookup"><span data-stu-id="d5261-144">Indexer properties</span></span>

<span data-ttu-id="d5261-145">EF Core 5,0 dá suporte ao C# mapeamento de propriedades do indexador.</span><span class="sxs-lookup"><span data-stu-id="d5261-145">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="d5261-146">Isso permite que as entidades atuem como pacotes de propriedades, em que as colunas são mapeadas para as propriedades nomeadas na bolsa.</span><span class="sxs-lookup"><span data-stu-id="d5261-146">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="d5261-147">A documentação é controlada pelo problema [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="d5261-147">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="d5261-148">Geração de restrições de verificação para mapeamentos de enumeração</span><span class="sxs-lookup"><span data-stu-id="d5261-148">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="d5261-149">EF Core migrações 5,0 agora podem gerar restrições de verificação para mapeamentos de propriedade de enumeração.</span><span class="sxs-lookup"><span data-stu-id="d5261-149">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="d5261-150">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d5261-150">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="d5261-151">A documentação é controlada pelo problema [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="d5261-151">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="d5261-152">Isrelacional</span><span class="sxs-lookup"><span data-stu-id="d5261-152">IsRelational</span></span>

<span data-ttu-id="d5261-153">Um novo método `IsRelational` foi adicionado além do `IsSqlServer`existente, `IsSqlite`e `IsInMemory`.</span><span class="sxs-lookup"><span data-stu-id="d5261-153">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="d5261-154">Isso pode ser usado para testar se DbContext está usando qualquer provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="d5261-154">This can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="d5261-155">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d5261-155">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="d5261-156">A documentação é controlada pelo problema [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="d5261-156">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="d5261-157">Cosmos simultaneidade otimista com ETags</span><span class="sxs-lookup"><span data-stu-id="d5261-157">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="d5261-158">O provedor de banco de dados Azure Cosmos DB agora dá suporte à simultaneidade otimista usando ETags.</span><span class="sxs-lookup"><span data-stu-id="d5261-158">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="d5261-159">Use o construtor de modelos no OnModelCreating para configurar uma ETag:</span><span class="sxs-lookup"><span data-stu-id="d5261-159">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="d5261-160">Em seguida, SaveChanges lançará um `DbUpdateConcurrencyException` em um conflito de simultaneidade, que [pode ser tratado](https://docs.microsoft.com/ef/core/saving/concurrency) para implementar repetições, etc.</span><span class="sxs-lookup"><span data-stu-id="d5261-160">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>


<span data-ttu-id="d5261-161">A documentação é controlada pelo problema [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="d5261-161">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="d5261-162">Conversões de consulta para mais construções DateTime</span><span class="sxs-lookup"><span data-stu-id="d5261-162">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="d5261-163">As consultas que contêm a nova construção DateTime agora são traduzidas.</span><span class="sxs-lookup"><span data-stu-id="d5261-163">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="d5261-164">Além disso, as seguintes funções de SQL Server agora são mapeadas:</span><span class="sxs-lookup"><span data-stu-id="d5261-164">In addition, the following SQL Server functions are now mapped:</span></span>
* <span data-ttu-id="d5261-165">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="d5261-165">DateDiffWeek</span></span>
* <span data-ttu-id="d5261-166">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="d5261-166">DateFromParts</span></span>

<span data-ttu-id="d5261-167">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d5261-167">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="d5261-168">A documentação é controlada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d5261-168">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="d5261-169">Conversões de consulta para mais constructos de matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="d5261-169">Query translations for more byte array constructs</span></span>

<span data-ttu-id="d5261-170">As consultas que usam Contains, Length, SequenceEqual, etc. nas propriedades byte [] agora são convertidas em SQL.</span><span class="sxs-lookup"><span data-stu-id="d5261-170">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="d5261-171">A documentação preliminar está incluída no [status semanal do EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="d5261-171">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="d5261-172">A documentação adicional é acompanhada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d5261-172">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="d5261-173">Conversão de consulta para reversa</span><span class="sxs-lookup"><span data-stu-id="d5261-173">Query translation for Reverse</span></span>

<span data-ttu-id="d5261-174">As consultas que usam `Reverse` agora são traduzidas.</span><span class="sxs-lookup"><span data-stu-id="d5261-174">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="d5261-175">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d5261-175">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="d5261-176">A documentação é controlada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d5261-176">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="d5261-177">Conversão de consulta para operadores bit a bit</span><span class="sxs-lookup"><span data-stu-id="d5261-177">Query translation for bitwise operators</span></span>

<span data-ttu-id="d5261-178">Agora, as consultas que usam operadores de bit que são são traduzidas em mais casos, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d5261-178">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="d5261-179">A documentação é controlada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d5261-179">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="d5261-180">Conversão de consulta para cadeias de caracteres em Cosmos</span><span class="sxs-lookup"><span data-stu-id="d5261-180">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="d5261-181">As consultas que usam os métodos de cadeia de caracteres contém, StartsWith e EndsWith agora são traduzidas ao usar o provedor de Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d5261-181">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="d5261-182">A documentação é controlada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d5261-182">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
