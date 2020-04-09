---
title: O que há de novo no EF Core 5.0
description: Visão geral dos novos recursos no EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634276"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="3aae3-103">O que há de novo no EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="3aae3-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="3aae3-104">O EF Core 5.0 está atualmente em desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="3aae3-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="3aae3-105">Esta página conterá uma visão geral das alterações interessantes introduzidas em cada visualização.</span><span class="sxs-lookup"><span data-stu-id="3aae3-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="3aae3-106">Esta página não duplica o [plano para o EF Core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="3aae3-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="3aae3-107">O plano descreve os temas gerais do EF Core 5.0, incluindo tudo o que estamos planejando incluir antes de enviar o lançamento final.</span><span class="sxs-lookup"><span data-stu-id="3aae3-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="3aae3-108">Adicionaremos links daqui à documentação oficial à medida que for publicada.</span><span class="sxs-lookup"><span data-stu-id="3aae3-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-2"></a><span data-ttu-id="3aae3-109">Preview 2</span><span class="sxs-lookup"><span data-stu-id="3aae3-109">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="3aae3-110">Use um atributo C# para especificar um campo de backup de propriedades</span><span class="sxs-lookup"><span data-stu-id="3aae3-110">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="3aae3-111">Um atributo C# agora pode ser usado para especificar o campo de backup de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="3aae3-111">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="3aae3-112">Este atributo permite que o EF Core ainda escreva e leia a partir do campo de respaldo como normalmente aconteceria, mesmo quando o campo de backup não pode ser encontrado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="3aae3-112">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="3aae3-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3aae3-113">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="3aae3-114">A documentação é rastreada por [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-114">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="3aae3-115">Mapeamento discriminatório completo</span><span class="sxs-lookup"><span data-stu-id="3aae3-115">Complete discriminator mapping</span></span>

<span data-ttu-id="3aae3-116">O EF Core usa uma coluna discriminatória para [mapeamento TPH de uma hierarquia de herança](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="3aae3-116">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="3aae3-117">Alguns aprimoramentos de desempenho são possíveis desde que o EF Core conheça todos os valores possíveis para o discriminador.</span><span class="sxs-lookup"><span data-stu-id="3aae3-117">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="3aae3-118">O EF Core 5.0 agora implementa esses aprimoramentos.</span><span class="sxs-lookup"><span data-stu-id="3aae3-118">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="3aae3-119">Por exemplo, versões anteriores do EF Core sempre gerariam esse SQL para uma consulta retornando todos os tipos em uma hierarquia:</span><span class="sxs-lookup"><span data-stu-id="3aae3-119">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="3aae3-120">O EF Core 5.0 agora gerará o seguinte quando um mapeamento discriminatório completo for configurado:</span><span class="sxs-lookup"><span data-stu-id="3aae3-120">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="3aae3-121">Será o comportamento padrão começando com a pré-visualização 3.</span><span class="sxs-lookup"><span data-stu-id="3aae3-121">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="3aae3-122">Melhorias de desempenho no Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="3aae3-122">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="3aae3-123">Fizemos duas melhorias de desempenho para o SQLIte:</span><span class="sxs-lookup"><span data-stu-id="3aae3-123">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="3aae3-124">Recuperar dados binários e de seqüência com GetBytes, GetChars e GetTextReader agora é mais eficiente, fazendo uso de SqliteBlob e streams.</span><span class="sxs-lookup"><span data-stu-id="3aae3-124">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="3aae3-125">A inicialização do SqliteConnection agora é preguiçosa.</span><span class="sxs-lookup"><span data-stu-id="3aae3-125">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="3aae3-126">Essas melhorias estão no ADO.NET provedor Microsoft.Data.Sqlite e, portanto, também melhoram o desempenho fora do EF Core.</span><span class="sxs-lookup"><span data-stu-id="3aae3-126">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="3aae3-127">Preview 1</span><span class="sxs-lookup"><span data-stu-id="3aae3-127">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="3aae3-128">Registro simples</span><span class="sxs-lookup"><span data-stu-id="3aae3-128">Simple logging</span></span>

<span data-ttu-id="3aae3-129">Este recurso adiciona funcionalidade `Database.Log` semelhante à do EF6.</span><span class="sxs-lookup"><span data-stu-id="3aae3-129">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="3aae3-130">Ou seja, ele fornece uma maneira simples de obter logs da EF Core sem a necessidade de configurar qualquer tipo de estrutura de registro externo.</span><span class="sxs-lookup"><span data-stu-id="3aae3-130">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="3aae3-131">A documentação preliminar está incluída no status semanal da [EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="3aae3-131">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="3aae3-132">A documentação adicional é rastreada por [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-132">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="3aae3-133">Maneira simples de gerar SQL gerado</span><span class="sxs-lookup"><span data-stu-id="3aae3-133">Simple way to get generated SQL</span></span>

<span data-ttu-id="3aae3-134">O EF Core 5.0 introduz o `ToQueryString` método de extensão, que retornará o SQL que o EF Core irá gerar ao executar uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="3aae3-134">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="3aae3-135">A documentação preliminar está incluída no status semanal da [EF para 9 de janeiro de 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="3aae3-135">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="3aae3-136">A documentação adicional é rastreada por [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-136">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="3aae3-137">Use um atributo C# para indicar que uma entidade não tem chave</span><span class="sxs-lookup"><span data-stu-id="3aae3-137">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="3aae3-138">Um tipo de entidade agora pode ser configurado como não tendo nenhuma chave usando o novo `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="3aae3-138">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="3aae3-139">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3aae3-139">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="3aae3-140">A documentação é rastreada por [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-140">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="3aae3-141">A seqüência de conexão ou conexão pode ser alterada no DbContext inicializado</span><span class="sxs-lookup"><span data-stu-id="3aae3-141">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="3aae3-142">Agora é mais fácil criar uma instância DbContext sem qualquer conexão ou seqüência de conexão.</span><span class="sxs-lookup"><span data-stu-id="3aae3-142">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="3aae3-143">Além disso, a seqüência de conexão ou conexão agora pode ser mutada na instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="3aae3-143">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="3aae3-144">Esse recurso permite que a mesma instância de contexto se conecte dinamicamente a diferentes bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="3aae3-144">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="3aae3-145">A documentação é rastreada por [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-145">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="3aae3-146">Proxies de rastreamento de alterações</span><span class="sxs-lookup"><span data-stu-id="3aae3-146">Change-tracking proxies</span></span>

<span data-ttu-id="3aae3-147">O EF Core agora pode gerar proxies de tempo de execução que implementam automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="3aae3-147">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="3aae3-148">Em seguida, eles relatam alterações de valor nas propriedades da entidade diretamente para o EF Core, evitando a necessidade de varredura para alterações.</span><span class="sxs-lookup"><span data-stu-id="3aae3-148">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="3aae3-149">No entanto, os proxies vêm com seu próprio conjunto de limitações, então eles não são para todos.</span><span class="sxs-lookup"><span data-stu-id="3aae3-149">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="3aae3-150">A documentação é rastreada por [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-150">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="3aae3-151">Visualizações aprimoradas de depuração</span><span class="sxs-lookup"><span data-stu-id="3aae3-151">Enhanced debug views</span></span>

<span data-ttu-id="3aae3-152">As visualizações de depuração são uma maneira fácil de olhar para os internos do EF Core quando problemas de depuração.</span><span class="sxs-lookup"><span data-stu-id="3aae3-152">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="3aae3-153">Uma visão dedepuração para o Modelo foi implementada há algum tempo.</span><span class="sxs-lookup"><span data-stu-id="3aae3-153">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="3aae3-154">Para o EF Core 5.0, facilitamos a visualização do modelo e adicionamos uma nova visão de depuração para entidades rastreadas no gestor estadual.</span><span class="sxs-lookup"><span data-stu-id="3aae3-154">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="3aae3-155">A documentação preliminar está incluída no status semanal da [EF para 12 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="3aae3-155">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="3aae3-156">A documentação adicional é rastreada por [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-156">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="3aae3-157">Melhor manuseio da semântica nula do banco de dados</span><span class="sxs-lookup"><span data-stu-id="3aae3-157">Improved handling of database null semantics</span></span>

<span data-ttu-id="3aae3-158">As bases de dados relacionais normalmente tratam null como um valor desconhecido e, portanto, não é igual a qualquer outro NULL.</span><span class="sxs-lookup"><span data-stu-id="3aae3-158">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="3aae3-159">Enquanto C# trata nulo como um valor definido, que se compara a qualquer outro nulo.</span><span class="sxs-lookup"><span data-stu-id="3aae3-159">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="3aae3-160">O EF Core por padrão traduz consultas para que eles usem a semântica nula C#.</span><span class="sxs-lookup"><span data-stu-id="3aae3-160">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="3aae3-161">O EF Core 5.0 melhora muito a eficiência dessas traduções.</span><span class="sxs-lookup"><span data-stu-id="3aae3-161">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="3aae3-162">A documentação é rastreada por [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-162">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="3aae3-163">Propriedades do indexador</span><span class="sxs-lookup"><span data-stu-id="3aae3-163">Indexer properties</span></span>

<span data-ttu-id="3aae3-164">O EF Core 5.0 suporta o mapeamento das propriedades do indexador C#.</span><span class="sxs-lookup"><span data-stu-id="3aae3-164">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="3aae3-165">Essas propriedades permitem que as entidades atuem como sacos de propriedades onde as colunas são mapeadas para nomear propriedades no saco.</span><span class="sxs-lookup"><span data-stu-id="3aae3-165">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="3aae3-166">A documentação é rastreada por [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-166">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="3aae3-167">Geração de restrições de verificação para mapeamentos de enum</span><span class="sxs-lookup"><span data-stu-id="3aae3-167">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="3aae3-168">As migrações do EF Core 5.0 agora podem gerar restrições check para mapeamentos de propriedades enum.</span><span class="sxs-lookup"><span data-stu-id="3aae3-168">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="3aae3-169">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3aae3-169">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="3aae3-170">A documentação é rastreada por [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-170">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="3aae3-171">IsRelational</span><span class="sxs-lookup"><span data-stu-id="3aae3-171">IsRelational</span></span>

<span data-ttu-id="3aae3-172">Um `IsRelational` novo método foi adicionado, além `IsSqlServer` `IsSqlite`do `IsInMemory`existente, e .</span><span class="sxs-lookup"><span data-stu-id="3aae3-172">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="3aae3-173">Este método pode ser usado para testar se o DbContext está usando algum provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="3aae3-173">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="3aae3-174">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3aae3-174">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="3aae3-175">A documentação é rastreada por [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-175">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="3aae3-176">Cosmos otimista selada com ETags</span><span class="sxs-lookup"><span data-stu-id="3aae3-176">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="3aae3-177">O provedor de banco de dados Azure Cosmos DB agora suporta concorrência otimista usando ETags.</span><span class="sxs-lookup"><span data-stu-id="3aae3-177">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="3aae3-178">Use o construtor de modelos em OnModelCriando para configurar um ETag:</span><span class="sxs-lookup"><span data-stu-id="3aae3-178">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="3aae3-179">SaveChanges, em `DbUpdateConcurrencyException` seguida, lançará um conflito de si0, que [pode ser manipulado](https://docs.microsoft.com/ef/core/saving/concurrency) para implementar repetições, etc.</span><span class="sxs-lookup"><span data-stu-id="3aae3-179">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="3aae3-180">A documentação é rastreada por [emissão #2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="3aae3-180">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="3aae3-181">Tradução de consulta para mais construções DateTime</span><span class="sxs-lookup"><span data-stu-id="3aae3-181">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="3aae3-182">As consultas que contêm nova construção DateTime são agora traduzidas.</span><span class="sxs-lookup"><span data-stu-id="3aae3-182">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="3aae3-183">Além disso, as seguintes funções do SQL Server são agora mapeadas:</span><span class="sxs-lookup"><span data-stu-id="3aae3-183">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="3aae3-184">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="3aae3-184">DateDiffWeek</span></span>
* <span data-ttu-id="3aae3-185">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="3aae3-185">DateFromParts</span></span>

<span data-ttu-id="3aae3-186">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3aae3-186">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="3aae3-187">A documentação é rastreada por [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-187">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="3aae3-188">Tradução de consultas para mais construções de matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="3aae3-188">Query translations for more byte array constructs</span></span>

<span data-ttu-id="3aae3-189">As consultas usando Contains, Length, SequenceEqual, etc. em byte[] propriedades são agora traduzidas para SQL.</span><span class="sxs-lookup"><span data-stu-id="3aae3-189">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="3aae3-190">A documentação preliminar está incluída no status semanal da [EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="3aae3-190">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="3aae3-191">A documentação adicional é rastreada por [emissão #2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="3aae3-191">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="3aae3-192">Tradução de consulta para Reverter</span><span class="sxs-lookup"><span data-stu-id="3aae3-192">Query translation for Reverse</span></span>

<span data-ttu-id="3aae3-193">As consultas `Reverse` que usam agora são traduzidas.</span><span class="sxs-lookup"><span data-stu-id="3aae3-193">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="3aae3-194">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3aae3-194">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="3aae3-195">A documentação é rastreada por [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-195">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="3aae3-196">Tradução de consulta para operadores bitwise</span><span class="sxs-lookup"><span data-stu-id="3aae3-196">Query translation for bitwise operators</span></span>

<span data-ttu-id="3aae3-197">Consultas usando operadores bitwise agora são traduzidas em mais casos Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3aae3-197">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="3aae3-198">A documentação é rastreada por [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-198">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="3aae3-199">Tradução de consulta para strings no Cosmos</span><span class="sxs-lookup"><span data-stu-id="3aae3-199">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="3aae3-200">As consultas que usam os métodos de string Contains, StartsWith e EndsWith são agora traduzidas ao usar o provedor Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3aae3-200">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="3aae3-201">A documentação é rastreada por [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)de emissão .</span><span class="sxs-lookup"><span data-stu-id="3aae3-201">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
