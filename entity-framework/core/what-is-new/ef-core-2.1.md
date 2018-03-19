---
title: "Novidades no EF Core 2.1 – EF Core"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: bb1e691e0f22bd36467d58c02bde22c63067207e
ms.sourcegitcommit: fcaeaf085171dfb5c080ec42df1d1df8dfe204fb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="da5ca-102">Novos recursos no EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="da5ca-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="da5ca-103">Essa versão ainda está em versão prévia.</span><span class="sxs-lookup"><span data-stu-id="da5ca-103">This release is still in preview.</span></span>

<span data-ttu-id="da5ca-104">Além de várias pequenas melhorias e mais de uma centena de correções de bugs do produto, o EF Core 2.1 inclui vários novos recursos:</span><span class="sxs-lookup"><span data-stu-id="da5ca-104">Besides numerous small improvements and more than a hundred product bug fixes, EF Core 2.1 includes several new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="da5ca-105">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="da5ca-105">Lazy loading</span></span>
<span data-ttu-id="da5ca-106">Agora o EF Core contém os blocos de construção necessários para que qualquer pessoa crie classes de entidade que podem carregar suas propriedades de navegação sob demanda.</span><span class="sxs-lookup"><span data-stu-id="da5ca-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="da5ca-107">Também criamos um novo pacote, o Microsoft.EntityFrameworkCore.Proxies, que faz uso desses blocos de construção para gerar classes de proxy de carregamento lento com base em classes de entidade minimamente modificadas (por exemplo, classes com propriedades de navegação virtual).</span><span class="sxs-lookup"><span data-stu-id="da5ca-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="da5ca-108">Leia a [seção sobre carregamento lento](xref:core/querying/related-data#lazy-loading) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="da5ca-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="da5ca-109">Parâmetros em construtores de entidade</span><span class="sxs-lookup"><span data-stu-id="da5ca-109">Parameters in entity constructors</span></span>
<span data-ttu-id="da5ca-110">Como um dos blocos de construção necessários para o carregamento lento, habilitamos a criação de entidades que recebem parâmetros em seus construtores.</span><span class="sxs-lookup"><span data-stu-id="da5ca-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="da5ca-111">Você pode usar parâmetros para injetar valores de propriedade, delegados de carregamento lento e serviços.</span><span class="sxs-lookup"><span data-stu-id="da5ca-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="da5ca-112">Leia a [seção sobre construtor de entidade com parâmetros](xref:core/modeling/constructors) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="da5ca-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="da5ca-113">Conversões de valor</span><span class="sxs-lookup"><span data-stu-id="da5ca-113">Value conversions</span></span>
<span data-ttu-id="da5ca-114">Até agora, o EF Core só podia mapear propriedades de tipos com suporte nativo do provedor de banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="da5ca-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="da5ca-115">Os valores eram copiados entre colunas e propriedades sem qualquer transformação.</span><span class="sxs-lookup"><span data-stu-id="da5ca-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="da5ca-116">Começando com o EF Core 2.1, agora é possível aplicar conversões de valor para transformar os valores obtidos das colunas antes que eles sejam aplicados às propriedades e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="da5ca-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="da5ca-117">Há diversas conversões que podem ser aplicadas por convenção, conforme necessário, bem como uma API de configuração explícita que permite o registro de conversões personalizadas entre colunas e propriedades.</span><span class="sxs-lookup"><span data-stu-id="da5ca-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="da5ca-118">Algumas das aplicações desse recurso são:</span><span class="sxs-lookup"><span data-stu-id="da5ca-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="da5ca-119">Armazenamento de enumerações como cadeias de caracteres</span><span class="sxs-lookup"><span data-stu-id="da5ca-119">Storing enums as strings</span></span>
- <span data-ttu-id="da5ca-120">Mapeamento de inteiros sem sinal com o SQL Server</span><span class="sxs-lookup"><span data-stu-id="da5ca-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="da5ca-121">Criptografia e descriptografia automática de valores de propriedade</span><span class="sxs-lookup"><span data-stu-id="da5ca-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="da5ca-122">Leia a [seção sobre conversões de valor](xref:core/modeling/value-conversions) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="da5ca-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="da5ca-123">Conversão de GroupBy do LINQ</span><span class="sxs-lookup"><span data-stu-id="da5ca-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="da5ca-124">Antes da versão 2.1, o operador GroupBy do LINQ, no EF Core, era sempre avaliado na memória.</span><span class="sxs-lookup"><span data-stu-id="da5ca-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="da5ca-125">Agora há suporte para convertê-lo para a cláusula GROUP BY do SQL na maioria dos casos comuns.</span><span class="sxs-lookup"><span data-stu-id="da5ca-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="da5ca-126">Este exemplo mostra uma consulta com GroupBy usada para calcular várias funções de agregação:</span><span class="sxs-lookup"><span data-stu-id="da5ca-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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
          Avg = g.Average(o => Amount)
        });
```

<span data-ttu-id="da5ca-127">A conversão de SQL correspondente tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="da5ca-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="da5ca-128">Propagação de dados</span><span class="sxs-lookup"><span data-stu-id="da5ca-128">Data Seeding</span></span>
<span data-ttu-id="da5ca-129">Com a nova versão, será possível fornecer os dados iniciais para popular um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="da5ca-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="da5ca-130">Diferentemente do EF6, a propagação de dados está associada a um tipo de entidade que faz parte da configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="da5ca-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="da5ca-131">Assim, as migrações do EF Core podem calcular automaticamente quais operações inserir, atualizar ou excluir precisam ser aplicadas ao atualizar o banco de dados para uma nova versão do modelo.</span><span class="sxs-lookup"><span data-stu-id="da5ca-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="da5ca-132">Por exemplo, você pode usar isso para configurar dados de semente para uma Postagem no `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="da5ca-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="da5ca-133">Leia a [seção sobre propagação de dados](xref:core/modeling/data-seeding) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="da5ca-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="da5ca-134">Tipos de consulta</span><span class="sxs-lookup"><span data-stu-id="da5ca-134">Query types</span></span>
<span data-ttu-id="da5ca-135">Agora um modelo do EF Core pode incluir tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="da5ca-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="da5ca-136">Diferentemente dos tipos de entidade, os tipos de consulta não têm chaves definidas neles e não podem ser inseridos, excluídos nem atualizados (ou seja, eles são somente leitura), mas podem ser retornados diretamente pelas consultas.</span><span class="sxs-lookup"><span data-stu-id="da5ca-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="da5ca-137">Estes são alguns dos cenários de uso para tipos de consulta:</span><span class="sxs-lookup"><span data-stu-id="da5ca-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="da5ca-138">Mapeamento para exibições sem chaves primárias</span><span class="sxs-lookup"><span data-stu-id="da5ca-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="da5ca-139">Mapeamento para tabelas sem chaves primárias</span><span class="sxs-lookup"><span data-stu-id="da5ca-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="da5ca-140">Mapeamento para consultas definidas no modelo</span><span class="sxs-lookup"><span data-stu-id="da5ca-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="da5ca-141">Servir como o tipo de retorno para consultas `FromSql()`</span><span class="sxs-lookup"><span data-stu-id="da5ca-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="da5ca-142">Leia a [seção sobre tipos de consulta](xref:core/modeling/query-types) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="da5ca-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="da5ca-143">Include para tipos derivados</span><span class="sxs-lookup"><span data-stu-id="da5ca-143">Include for derived types</span></span>
<span data-ttu-id="da5ca-144">Agora será possível especificar propriedades de navegação definidas somente em tipos derivados ao escrever expressões para o método `Include`.</span><span class="sxs-lookup"><span data-stu-id="da5ca-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="da5ca-145">Para obter a versão fortemente tipada do `Include`, há suporte tanto para o uso de uma conversão explícita quanto do operador `as`.</span><span class="sxs-lookup"><span data-stu-id="da5ca-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="da5ca-146">Agora também há suporte para referenciar os nomes da propriedade de navegação definida nos tipos derivados na versão de cadeia de caracteres do `Include`:</span><span class="sxs-lookup"><span data-stu-id="da5ca-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="da5ca-147">Leia a [seção sobre Include com tipos derivados](xref:core/querying/related-data#include-on-derived-types) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="da5ca-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="da5ca-148">Suporte para System.Transactions</span><span class="sxs-lookup"><span data-stu-id="da5ca-148">System.Transactions support</span></span>
<span data-ttu-id="da5ca-149">Adicionamos a capacidade de trabalhar com recursos System.Transactions, como TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="da5ca-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="da5ca-150">Isso funcionará no .NET Framework e no .NET Core ao usar os provedores de banco de dados que sejam compatíveis.</span><span class="sxs-lookup"><span data-stu-id="da5ca-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="da5ca-151">Leia a [seção sobre System.Transactions](xref:core/saving/transactions#using-systemtransactions) para obter mais informações sobre este tópico.</span><span class="sxs-lookup"><span data-stu-id="da5ca-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="da5ca-152">Melhor ordenação de colunas na migração inicial</span><span class="sxs-lookup"><span data-stu-id="da5ca-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="da5ca-153">Com base nos comentários dos clientes, atualizamos as migrações para que gerem inicialmente colunas para tabelas na mesma ordem em que as propriedades são declaradas nas classes.</span><span class="sxs-lookup"><span data-stu-id="da5ca-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="da5ca-154">Observe que o EF Core não pode alterar a ordem quando novos membros são adicionados após a criação inicial da tabela.</span><span class="sxs-lookup"><span data-stu-id="da5ca-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="da5ca-155">Otimização de subconsultas correlacionadas</span><span class="sxs-lookup"><span data-stu-id="da5ca-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="da5ca-156">Aprimoramos nossa conversão de consulta para evitar a execução de consultas SQL "N + 1" em muitos cenários comuns, nos quais o uso de uma propriedade de navegação na projeção leva à associação de dados da consulta raiz com os dados de uma subconsulta correlacionada.</span><span class="sxs-lookup"><span data-stu-id="da5ca-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="da5ca-157">A otimização requer o armazenamento em buffer dos resultados da subconsulta e é necessário que você modifique a consulta para aceitar o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="da5ca-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="da5ca-158">Por exemplo, a consulta a seguir normalmente é convertida em uma consulta para os Clientes, além de N (em que "N" é o número retornado de clientes) consultas separadas para os Pedidos:</span><span class="sxs-lookup"><span data-stu-id="da5ca-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="da5ca-159">Ao incluir `ToList()` no lugar certo, você indica que o buffer é adequado para os Pedidos, permitindo a otimização:</span><span class="sxs-lookup"><span data-stu-id="da5ca-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="da5ca-160">Observe que essa consulta será convertida em apenas duas consultas SQL: uma para os Clientes e o outra para os Pedidos.</span><span class="sxs-lookup"><span data-stu-id="da5ca-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="ownedattribute"></a><span data-ttu-id="da5ca-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="da5ca-161">OwnedAttribute</span></span>

<span data-ttu-id="da5ca-162">Agora é possível configurar [tipos de entidade de propriedade](xref:core/modeling/owned-entities) simplesmente anotando o tipo com `[Owned]` e, em seguida, certificando-se de que a entidade de proprietário seja adicionada ao modelo:</span><span class="sxs-lookup"><span data-stu-id="da5ca-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="database-provider-compatibility"></a><span data-ttu-id="da5ca-163">Compatibilidade do provedor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="da5ca-163">Database provider compatibility</span></span>

<span data-ttu-id="da5ca-164">O EF Core 2.1 foi projetado para ser compatível com provedores de banco de dados criados para o EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="da5ca-164">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0.</span></span> <span data-ttu-id="da5ca-165">Embora alguns dos recursos descritos acima (por exemplo, conversões de valor) exijam um provedor atualizado, outros (por exemplo, carregamento lento) funcionarão com provedores existentes.</span><span class="sxs-lookup"><span data-stu-id="da5ca-165">While some of the features described above (e.g. value conversions) require an updated provider, others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="da5ca-166">Se você encontrar qualquer incompatibilidade inesperada ou qualquer problema nos novos recursos, ou se você tiver comentários sobre eles, informe usando [nosso rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="da5ca-166">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
