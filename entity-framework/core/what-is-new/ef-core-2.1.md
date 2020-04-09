---
title: Novidades no EF Core 2.1 – EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ba3a26bcd76cd0b9615b13f32456e7280afe533a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417478"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="807d5-102">Novos recursos no EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="807d5-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="807d5-103">Além de várias correções de bugs e pequenas melhorias funcionais e de desempenho, o EF Core 2.1 inclui alguns recursos novos e interessantes:</span><span class="sxs-lookup"><span data-stu-id="807d5-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="807d5-104">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="807d5-104">Lazy loading</span></span>

<span data-ttu-id="807d5-105">Agora o EF Core contém os blocos de construção necessários para que qualquer pessoa crie classes de entidade que podem carregar suas propriedades de navegação sob demanda.</span><span class="sxs-lookup"><span data-stu-id="807d5-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="807d5-106">Também criamos um novo pacote, o Microsoft.EntityFrameworkCore.Proxies, que faz uso desses blocos de construção para gerar classes de proxy de carregamento lento com base em classes de entidade minimamente modificadas (por exemplo, classes com propriedades de navegação virtual).</span><span class="sxs-lookup"><span data-stu-id="807d5-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="807d5-107">Leia a [seção sobre carregamento lento](xref:core/querying/related-data#lazy-loading) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="807d5-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="807d5-108">Parâmetros em construtores de entidade</span><span class="sxs-lookup"><span data-stu-id="807d5-108">Parameters in entity constructors</span></span>

<span data-ttu-id="807d5-109">Como um dos blocos de construção necessários para o carregamento lento, habilitamos a criação de entidades que recebem parâmetros em seus construtores.</span><span class="sxs-lookup"><span data-stu-id="807d5-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="807d5-110">Você pode usar parâmetros para injetar valores de propriedade, delegados de carregamento lento e serviços.</span><span class="sxs-lookup"><span data-stu-id="807d5-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="807d5-111">Leia a [seção sobre construtor de entidade com parâmetros](xref:core/modeling/constructors) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="807d5-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="807d5-112">Conversões de valor</span><span class="sxs-lookup"><span data-stu-id="807d5-112">Value conversions</span></span>

<span data-ttu-id="807d5-113">Até agora, o EF Core só podia mapear propriedades de tipos com suporte nativo do provedor de banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="807d5-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="807d5-114">Os valores eram copiados entre colunas e propriedades sem qualquer transformação.</span><span class="sxs-lookup"><span data-stu-id="807d5-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="807d5-115">Começando com o EF Core 2.1, agora é possível aplicar conversões de valor para transformar os valores obtidos das colunas antes que eles sejam aplicados às propriedades e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="807d5-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="807d5-116">Há diversas conversões que podem ser aplicadas por convenção, conforme necessário, bem como uma API de configuração explícita que permite o registro de conversões personalizadas entre colunas e propriedades.</span><span class="sxs-lookup"><span data-stu-id="807d5-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="807d5-117">Algumas das aplicações desse recurso são:</span><span class="sxs-lookup"><span data-stu-id="807d5-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="807d5-118">Armazenamento de enumerações como cadeias de caracteres</span><span class="sxs-lookup"><span data-stu-id="807d5-118">Storing enums as strings</span></span>
- <span data-ttu-id="807d5-119">Mapeamento de inteiros sem sinal com o SQL Server</span><span class="sxs-lookup"><span data-stu-id="807d5-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="807d5-120">Criptografia e descriptografia automática de valores de propriedade</span><span class="sxs-lookup"><span data-stu-id="807d5-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="807d5-121">Leia a [seção sobre conversões de valor](xref:core/modeling/value-conversions) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="807d5-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="807d5-122">Conversão de GroupBy do LINQ</span><span class="sxs-lookup"><span data-stu-id="807d5-122">LINQ GroupBy translation</span></span>

<span data-ttu-id="807d5-123">Antes da versão 2.1, o operador GroupBy do LINQ, no EF Core, seria sempre avaliado na memória.</span><span class="sxs-lookup"><span data-stu-id="807d5-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="807d5-124">Agora há suporte para convertê-lo para a cláusula GROUP BY do SQL na maioria dos casos comuns.</span><span class="sxs-lookup"><span data-stu-id="807d5-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="807d5-125">Este exemplo mostra uma consulta com GroupBy usada para calcular várias funções de agregação:</span><span class="sxs-lookup"><span data-stu-id="807d5-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => o.Amount)
        });
```

<span data-ttu-id="807d5-126">A conversão de SQL correspondente tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="807d5-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="807d5-127">Propagação de dados</span><span class="sxs-lookup"><span data-stu-id="807d5-127">Data Seeding</span></span>

<span data-ttu-id="807d5-128">Com a nova versão, será possível fornecer os dados iniciais para popular um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="807d5-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="807d5-129">Diferentemente do EF6, a propagação de dados está associada a um tipo de entidade que faz parte da configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="807d5-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="807d5-130">Assim, as migrações do EF Core podem calcular automaticamente quais operações inserir, atualizar ou excluir precisam ser aplicadas ao atualizar o banco de dados para uma nova versão do modelo.</span><span class="sxs-lookup"><span data-stu-id="807d5-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="807d5-131">Por exemplo, você pode usar isso para configurar dados de semente para uma Postagem no `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="807d5-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="807d5-132">Leia a [seção sobre propagação de dados](xref:core/modeling/data-seeding) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="807d5-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="807d5-133">Tipos de consultas</span><span class="sxs-lookup"><span data-stu-id="807d5-133">Query types</span></span>

<span data-ttu-id="807d5-134">Agora um modelo do EF Core pode incluir tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="807d5-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="807d5-135">Diferentemente dos tipos de entidade, os tipos de consulta não têm chaves definidas neles e não podem ser inseridos, excluídos nem atualizados (ou seja, eles são somente leitura), mas podem ser retornados diretamente pelas consultas.</span><span class="sxs-lookup"><span data-stu-id="807d5-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (that is, they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="807d5-136">Estes são alguns dos cenários de uso para tipos de consulta:</span><span class="sxs-lookup"><span data-stu-id="807d5-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="807d5-137">Mapeamento para exibições sem chaves primárias</span><span class="sxs-lookup"><span data-stu-id="807d5-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="807d5-138">Mapeamento para tabelas sem chaves primárias</span><span class="sxs-lookup"><span data-stu-id="807d5-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="807d5-139">Mapeamento para consultas definidas no modelo</span><span class="sxs-lookup"><span data-stu-id="807d5-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="807d5-140">Servir como o tipo de retorno para consultas `FromSql()`</span><span class="sxs-lookup"><span data-stu-id="807d5-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="807d5-141">Leia a [seção sobre tipos de consulta](xref:core/modeling/keyless-entity-types) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="807d5-141">Read the [section on query types](xref:core/modeling/keyless-entity-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="807d5-142">Include para tipos derivados</span><span class="sxs-lookup"><span data-stu-id="807d5-142">Include for derived types</span></span>

<span data-ttu-id="807d5-143">Agora será possível especificar propriedades de navegação definidas somente em tipos derivados ao escrever expressões para o método `Include`.</span><span class="sxs-lookup"><span data-stu-id="807d5-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="807d5-144">Para obter a versão fortemente tipada do `Include`, há suporte tanto para o uso de uma conversão explícita quanto do operador `as`.</span><span class="sxs-lookup"><span data-stu-id="807d5-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="807d5-145">Agora também há suporte para referenciar os nomes da propriedade de navegação definida nos tipos derivados na versão de cadeia de caracteres do `Include`:</span><span class="sxs-lookup"><span data-stu-id="807d5-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="807d5-146">Leia a [seção sobre Include com tipos derivados](xref:core/querying/related-data#include-on-derived-types) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="807d5-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="807d5-147">Suporte para System.Transactions</span><span class="sxs-lookup"><span data-stu-id="807d5-147">System.Transactions support</span></span>

<span data-ttu-id="807d5-148">Adicionamos a capacidade de trabalhar com recursos System.Transactions, como TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="807d5-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="807d5-149">Isso funcionará no .NET Framework e no .NET Core ao usar os provedores de banco de dados que sejam compatíveis.</span><span class="sxs-lookup"><span data-stu-id="807d5-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="807d5-150">Leia a [seção sobre System.Transactions](xref:core/saving/transactions#using-systemtransactions) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="807d5-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="807d5-151">Melhor ordenação de colunas na migração inicial</span><span class="sxs-lookup"><span data-stu-id="807d5-151">Better column ordering in initial migration</span></span>

<span data-ttu-id="807d5-152">Com base nos comentários dos clientes, atualizamos as migrações para que gerem inicialmente colunas para tabelas na mesma ordem em que as propriedades são declaradas nas classes.</span><span class="sxs-lookup"><span data-stu-id="807d5-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="807d5-153">Observe que o EF Core não pode alterar a ordem quando novos membros são adicionados após a criação inicial da tabela.</span><span class="sxs-lookup"><span data-stu-id="807d5-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="807d5-154">Otimização de subconsultas correlacionadas</span><span class="sxs-lookup"><span data-stu-id="807d5-154">Optimization of correlated subqueries</span></span>

<span data-ttu-id="807d5-155">Aprimoramos nossa conversão de consulta para evitar a execução de consultas SQL "N + 1" em muitos cenários comuns, nos quais o uso de uma propriedade de navegação na projeção leva à associação de dados da consulta raiz com os dados de uma subconsulta correlacionada.</span><span class="sxs-lookup"><span data-stu-id="807d5-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="807d5-156">A otimização requer o buffer dos resultados da subconsulta, e é necessário que você modifique a consulta para aceitar o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="807d5-156">The optimization requires buffering the results from the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="807d5-157">Por exemplo, a consulta a seguir normalmente é convertida em uma consulta para os Clientes, além de N (em que "N" é o número retornado de clientes) consultas separadas para os Pedidos:</span><span class="sxs-lookup"><span data-stu-id="807d5-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="807d5-158">Ao incluir `ToList()` no lugar certo, você indica que o buffer é adequado para os Pedidos, permitindo a otimização:</span><span class="sxs-lookup"><span data-stu-id="807d5-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="807d5-159">Observe que essa consulta será convertida em apenas duas consultas SQL: uma para os Clientes e o outra para os Pedidos.</span><span class="sxs-lookup"><span data-stu-id="807d5-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="807d5-160">Atributo [Owned]</span><span class="sxs-lookup"><span data-stu-id="807d5-160">[Owned] attribute</span></span>

<span data-ttu-id="807d5-161">Agora é possível configurar [tipos de entidade de propriedade](xref:core/modeling/owned-entities) simplesmente anotando o tipo com `[Owned]` e, em seguida, certificando-se de que a entidade de proprietário seja adicionada ao modelo:</span><span class="sxs-lookup"><span data-stu-id="807d5-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="807d5-162">Ferramenta de linha de comando dotnet-ef incluída no SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="807d5-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="807d5-163">Os comandos _dotnet-ef_ agora fazem parte do SDK do .NET Core, portanto, não será mais necessário usar DotNetCliToolReference no projeto para usar migrações ou para fazer o scaffold de um DbContext de um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="807d5-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="807d5-164">Consulte a seção sobre [instalação das ferramentas](xref:core/miscellaneous/cli/dotnet#installing-the-tools) para obter mais detalhes de como habilitar as ferramentas de linha de comando para diferentes versões do SDK do .NET Core e EF Core.</span><span class="sxs-lookup"><span data-stu-id="807d5-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="807d5-165">Pacote Microsoft.EntityFrameworkCore.Abstractions</span><span class="sxs-lookup"><span data-stu-id="807d5-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>

<span data-ttu-id="807d5-166">O novo pacote contém atributos e interfaces para você usar em projetos e destacar recursos EF Core sem tomar uma dependência no EF Core como um todo.</span><span class="sxs-lookup"><span data-stu-id="807d5-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="807d5-167">Por exemplo, o atributo [Owned] e a interface ILazyLoader estão localizadas aqui.</span><span class="sxs-lookup"><span data-stu-id="807d5-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="807d5-168">Eventos de alteração de estado</span><span class="sxs-lookup"><span data-stu-id="807d5-168">State change events</span></span>

<span data-ttu-id="807d5-169">Novos eventos `Tracked` e `StateChanged` no `ChangeTracker` podem ser usados para escrever lógica que reage a entidades inserindo o DbContext ou alterando o estado dele.</span><span class="sxs-lookup"><span data-stu-id="807d5-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="807d5-170">Analisador de parâmetros de SQL bruto</span><span class="sxs-lookup"><span data-stu-id="807d5-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="807d5-171">Um novo analisador de código foi incluído no EF Core que detecta usos potencialmente não seguros de nossas APIs de SQL bruto, como `FromSql` ou `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="807d5-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="807d5-172">Por exemplo, para a consulta a seguir, você verá um aviso, porque o _minAge_ não está parametrizado:</span><span class="sxs-lookup"><span data-stu-id="807d5-172">For example, for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="807d5-173">Compatibilidade do provedor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="807d5-173">Database provider compatibility</span></span>

<span data-ttu-id="807d5-174">É recomendável que você use o EF Core 2.1 com provedores que foram atualizados ou pelo menos testados para funcionar com o EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="807d5-174">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="807d5-175">Se você encontrar qualquer incompatibilidade inesperada ou qualquer problema nos novos recursos, ou se você tiver comentários sobre eles, informe usando [nosso rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="807d5-175">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
