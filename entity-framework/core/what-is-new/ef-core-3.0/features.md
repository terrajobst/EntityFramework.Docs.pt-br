---
title: Novos recursos no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005565"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="48c2d-102">Novos recursos incluídos no EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="48c2d-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="48c2d-103">A lista a seguir inclui os principais novos recursos planejados para o EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="48c2d-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="48c2d-104">EF Core 3,0 é uma versão principal e também contém várias [alterações significativas](xref:core/what-is-new/ef-core-3.0/breaking-changes), que são melhorias de API que podem ter impacto negativo sobre os aplicativos existentes.</span><span class="sxs-lookup"><span data-stu-id="48c2d-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="48c2d-105">Melhorias do LINQ</span><span class="sxs-lookup"><span data-stu-id="48c2d-105">LINQ improvements</span></span> 

<span data-ttu-id="48c2d-106">O LINQ permite que você grave consultas de banco de dados sem deixar a linguagem de sua escolha, tirando proveito de informações de tipo avançadas para oferecer verificação de tipo IntelliSense e de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="48c2d-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="48c2d-107">Mas o LINQ também permite que você grave um número ilimitado de consultas complicadas que contêm expressões arbitrárias (chamadas de método ou operações).</span><span class="sxs-lookup"><span data-stu-id="48c2d-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="48c2d-108">Lidar com todas essas combinações sempre foi um desafio significativo para os provedores de LINQ.</span><span class="sxs-lookup"><span data-stu-id="48c2d-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="48c2d-109">No EF Core 3,0, reescrevemos nossa implementação de LINQ para permitir a tradução de mais expressões no SQL, a fim de gerar consultas eficientes em mais casos, para evitar que consultas ineficientes não sejam detectadas e para facilitar a introdução de uma nova consulta gradualmente. recursos e desempenho improvementswithout interrompendo aplicativos e provedores de dados existentes.</span><span class="sxs-lookup"><span data-stu-id="48c2d-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="48c2d-110">Avaliação do cliente</span><span class="sxs-lookup"><span data-stu-id="48c2d-110">Client evaluation</span></span>

<span data-ttu-id="48c2d-111">A principal alteração de design no EF Core 3,0 tem a ver com como ele lida com expressões LINQ que ele não pode converter em SQL ou parâmetros:</span><span class="sxs-lookup"><span data-stu-id="48c2d-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="48c2d-112">Nas primeiras versões, EF Core simplesmente descobrir quais partes de uma consulta poderiam ser convertidas em SQL e executou o restante da consulta no cliente.</span><span class="sxs-lookup"><span data-stu-id="48c2d-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="48c2d-113">Esse tipo de execução do lado do cliente pode ser desejável em algumas situações, mas em muitos outros casos ele pode resultar em consultas ineficientes.</span><span class="sxs-lookup"><span data-stu-id="48c2d-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="48c2d-114">Por exemplo, se EF Core 2,2 não pôde converter um predicado em uma `Where()` chamada, ele executou uma instrução SQL sem um filtro, leu todas as linhas do banco de dados e as filtrou na memória.</span><span class="sxs-lookup"><span data-stu-id="48c2d-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="48c2d-115">Isso pode ser aceitável se o banco de dados contiver um pequeno número de linhas, mas pode resultar em problemas de desempenho significativos ou até mesmo falha de aplicativo se o banco de dados contiver um grande número ou linhas.</span><span class="sxs-lookup"><span data-stu-id="48c2d-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="48c2d-116">No EF Core 3,0, a avaliação do cliente foi restrita para acontecer apenas na projeção de nível superior (a última `Select()`chamada para).</span><span class="sxs-lookup"><span data-stu-id="48c2d-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="48c2d-117">Quando EF Core 3,0 detecta expressões que não podem ser convertidas em qualquer outro lugar na consulta, ela gera uma exceção de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="48c2d-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="48c2d-118">Suporte do Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="48c2d-118">Cosmos DB support</span></span> 

<span data-ttu-id="48c2d-119">O provedor Cosmos DB para EF Core permite que os desenvolvedores familiarizados com o modelo de programação do EF destinam-se facilmente a Azure Cosmos DB como um banco de dados de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="48c2d-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="48c2d-120">A meta é tornar algumas das vantagens do Cosmos DB ainda mais acessíveis para os desenvolvedores de .NET, tais como a distribuição global, disponibilidade "Always On", escalabilidade elástica e baixa latência.</span><span class="sxs-lookup"><span data-stu-id="48c2d-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="48c2d-121">O provedor ativará a maioria dos recursos do EF Core, como controle automático de alterações, LINQ e conversões de valores, em relação à API do SQL no Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="48c2d-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="48c2d-122">Suporte do C# 8.0</span><span class="sxs-lookup"><span data-stu-id="48c2d-122">C# 8.0 support</span></span>

<span data-ttu-id="48c2d-123">EF Core 3,0 aproveita alguns dos novos recursos do C# 8,0:</span><span class="sxs-lookup"><span data-stu-id="48c2d-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="48c2d-124">Fluxos assíncronos</span><span class="sxs-lookup"><span data-stu-id="48c2d-124">Asynchronous streams</span></span>

<span data-ttu-id="48c2d-125">Os resultados da consulta assíncrona agora são expostos usando `IAsyncEnumerable<T>` a nova interface padrão e podem `await foreach`ser consumidos usando o.</span><span class="sxs-lookup"><span data-stu-id="48c2d-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="48c2d-126">Tipos de referência anuláveis</span><span class="sxs-lookup"><span data-stu-id="48c2d-126">Nullable reference types</span></span> 

<span data-ttu-id="48c2d-127">Quando esse novo recurso está habilitado em seu código, EF Core pode motivo da nulidade das propriedades de tipos referência (de tipos primitivos, como cadeia de caracteres ou propriedades de navegação) para decidir a nulidade das colunas e relações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="48c2d-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="48c2d-128">Interceptação</span><span class="sxs-lookup"><span data-stu-id="48c2d-128">Interception</span></span>

<span data-ttu-id="48c2d-129">A nova API de interceptação no EF Core 3,0 permite observar e modificar de forma programática o resultado de operações de banco de dados de nível baixo que ocorrem como parte da operação normal do EF Core, como abrir conexões, initating transações e executar comandos.</span><span class="sxs-lookup"><span data-stu-id="48c2d-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="48c2d-130">Engenharia reversa de exibições de banco de dados</span><span class="sxs-lookup"><span data-stu-id="48c2d-130">Reverse engineering of database views</span></span>

<span data-ttu-id="48c2d-131">Tipos de entidade sem chaves (anteriormente conhecidas como [tipos de consulta](xref:core/modeling/query-types)) representam dados que podem ser lidos do banco de dado, mas não podem ser atualizados.</span><span class="sxs-lookup"><span data-stu-id="48c2d-131">Entity types without keys (previously known as [query types](xref:core/modeling/query-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="48c2d-132">Essa característica torna-se uma ótima opção para mapear exibições de banco de dados na maioria dos cenários, portanto, automatizamos a criação de tipos de entidade sem chaves ao reverter exibições de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="48c2d-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="48c2d-133">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="48c2d-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="48c2d-134">No EF Core 3.0 em diante, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, será possível adicionar um `Order` sem um `OrderDetails` e todas as propriedades `OrderDetails`, com exceção da chave primária, serão mapeadas para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="48c2d-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="48c2d-135">Ao realizar consultas, o EF Core definirá `OrderDetails` como `null` se uma de suas propriedades necessárias não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="48c2d-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a><span data-ttu-id="48c2d-136">EF 6.3 no .NET Core</span><span class="sxs-lookup"><span data-stu-id="48c2d-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="48c2d-137">Entendemos que muitos aplicativos existentes usam versões anteriores do EF e que a portabilidade deles para o EF Core somente para aproveitar o .NET Core pode exigir um esforço significativo.</span><span class="sxs-lookup"><span data-stu-id="48c2d-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="48c2d-138">Por esse motivo, habilitamos a versão newewst do EF 6 para ser executada no .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="48c2d-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="48c2d-139">Há algumas limitações, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="48c2d-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="48c2d-140">Novos provedores precisam funcionar no .NET Core</span><span class="sxs-lookup"><span data-stu-id="48c2d-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="48c2d-141">Suporte espacial com o SQL Server não será habilitado</span><span class="sxs-lookup"><span data-stu-id="48c2d-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="48c2d-142">Recursos adiados</span><span class="sxs-lookup"><span data-stu-id="48c2d-142">Postponed features</span></span>

<span data-ttu-id="48c2d-143">Alguns recursos originalmente planejados para EF Core 3,0 foram adiados para versões futuras:</span><span class="sxs-lookup"><span data-stu-id="48c2d-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="48c2d-144">Capacidade de ingore partes de um modelo em migrações, controladas por [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="48c2d-144">Ability to ingore parts of a model in migrations, tracked by [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="48c2d-145">Entidades do recipiente de propriedades, rastreadas por dois problemas separados: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sobre entidades de tipo compartilhado e [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sobre o suporte a mapeamento de propriedade indexada.</span><span class="sxs-lookup"><span data-stu-id="48c2d-145">Property bag entities, tracked by two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
