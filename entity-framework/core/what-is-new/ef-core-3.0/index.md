---
title: Novos recursos no Entity Framework Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebc676930ffc396aa70bb8afb91cf5a0cd43e04d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413191"
---
# <a name="new-features-in-entity-framework-core-30"></a><span data-ttu-id="8a4de-102">Novos recursos no Entity Framework Core 3.0</span><span class="sxs-lookup"><span data-stu-id="8a4de-102">New features in Entity Framework Core 3.0</span></span>

<span data-ttu-id="8a4de-103">A lista a seguir inclui os principais novos recursos no EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="8a4de-103">The following list includes the major new features in EF Core 3.0.</span></span>

<span data-ttu-id="8a4de-104">Por ser uma versão principal, o EF Core 3.0 também contém várias [alterações de falha](xref:core/what-is-new/ef-core-3.0/breaking-changes), que são melhorias de API que podem ter um impacto negativo em aplicativos existentes.</span><span class="sxs-lookup"><span data-stu-id="8a4de-104">As a major release, EF Core 3.0 also contains several [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-overhaul"></a><span data-ttu-id="8a4de-105">Visão geral do LINQ</span><span class="sxs-lookup"><span data-stu-id="8a4de-105">LINQ overhaul</span></span>

<span data-ttu-id="8a4de-106">O LINQ permite que você escreva consultas de banco de dados usando a linguagem .NET de sua escolha, aproveitando informações de tipo avançado para oferecer IntelliSense e verificação do tipo em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="8a4de-106">LINQ enables you to write database queries using your .NET language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="8a4de-107">Mas o LINQ também permite escrever um número ilimitado de consultas complicadas que contêm expressões arbitrárias (chamadas de método ou operações).</span><span class="sxs-lookup"><span data-stu-id="8a4de-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="8a4de-108">O modo de lidar com todas essas combinações é o principal desafio para os provedores LINQ.</span><span class="sxs-lookup"><span data-stu-id="8a4de-108">How to handle all those combinations is the main challenge for LINQ providers.</span></span>

<span data-ttu-id="8a4de-109">No EF Core 3.0, rearquitetamos nosso provedor LINQ para permitir a conversão de mais padrões de consulta em SQL, gerando consultas eficazes em mais casos e impedindo que consultas ineficazes não sejam detectadas.</span><span class="sxs-lookup"><span data-stu-id="8a4de-109">In EF Core 3.0, we rearchitected our LINQ provider to enable translating more query patterns into SQL, generating efficient queries in more cases, and preventing inefficient queries from going undetected.</span></span> <span data-ttu-id="8a4de-110">O novo provedor LINQ é a base sobre a qual será possível oferecer novos recursos de consulta e melhorias de desempenho em versões futuras, sem interromper aplicativos e provedores de dados existentes.</span><span class="sxs-lookup"><span data-stu-id="8a4de-110">The new LINQ provider is the foundation over which we'll be able to offer new query capabilities and performance improvements in future releases, without breaking existing applications and data providers.</span></span>

### <a name="restricted-client-evaluation"></a><span data-ttu-id="8a4de-111">Avaliação de cliente restrita</span><span class="sxs-lookup"><span data-stu-id="8a4de-111">Restricted client evaluation</span></span>

<span data-ttu-id="8a4de-112">A alteração de design mais importante tem a ver com a forma de lidarmos com expressões LINQ que não podem ser convertidas em parâmetros ou convertidas em SQL.</span><span class="sxs-lookup"><span data-stu-id="8a4de-112">The most important design change has to do with how we handle LINQ expressions that cannot be converted to parameters or translated to SQL.</span></span>

<span data-ttu-id="8a4de-113">Em versões anteriores, o EF Core identificava quais partes de uma consulta poderiam ser convertidas em SQL e executava o restante da consulta no cliente.</span><span class="sxs-lookup"><span data-stu-id="8a4de-113">In previous versions, EF Core identified what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="8a4de-114">Esse tipo de execução do lado do cliente é recomendável em algumas situações. Porém, em muitos outros casos, pode resultar em consultas ineficazes.</span><span class="sxs-lookup"><span data-stu-id="8a4de-114">This type of client-side execution is desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>

<span data-ttu-id="8a4de-115">Por exemplo, se o EF Core 2.2 não pudesse converter um predicado em uma chamada `Where()`, ele executava uma instrução SQL sem um filtro, transferia todas as linhas do banco de dados e, em seguida, filtrava-as na memória:</span><span class="sxs-lookup"><span data-stu-id="8a4de-115">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed an SQL statement without a filter, transferred all the rows from the database, and then filtered them in-memory:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

<span data-ttu-id="8a4de-116">Isso pode ser aceitável caso o banco de dados contenha um número pequeno de linhas, mas pode resultar em problemas significativos de desempenho ou até mesmo em falha de aplicativo se o banco de dados contiver um grande número de linhas.</span><span class="sxs-lookup"><span data-stu-id="8a4de-116">That may be acceptable if the database contains a small number of rows but can result in significant performance issues or even application failure if the database contains a large number of rows.</span></span>

<span data-ttu-id="8a4de-117">No EF Core 3.0, restringimos a avaliação do cliente para acontecer somente na projeção de nível superior (essencialmente, a última chamada para `Select()`).</span><span class="sxs-lookup"><span data-stu-id="8a4de-117">In EF Core 3.0, we've restricted client evaluation to only happen on the top-level projection (essentially, the last call to `Select()`).</span></span>
<span data-ttu-id="8a4de-118">Quando o EF Core 3.0 detecta expressões que não podem ser convertidas em nenhum outro lugar da consulta, ele lança uma exceção de runtime.</span><span class="sxs-lookup"><span data-stu-id="8a4de-118">When EF Core 3.0 detects expressions that can't be translated anywhere else in the query, it throws a runtime exception.</span></span>

<span data-ttu-id="8a4de-119">Para avaliar uma condição de predicado no cliente como no exemplo anterior, os desenvolvedores agora precisam alternar explicitamente a avaliação da consulta para o LINQ to Objects:</span><span class="sxs-lookup"><span data-stu-id="8a4de-119">To evaluate a predicate condition on the client as in the previous example, developers now need to explicitly switch evaluation of the query to LINQ to Objects:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

<span data-ttu-id="8a4de-120">Confira a [documentação sobre alterações de falha](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) para saber mais sobre como isso pode afetar os aplicativos existentes.</span><span class="sxs-lookup"><span data-stu-id="8a4de-120">See the [breaking changes documentation](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) for more details about how this can affect existing applications.</span></span>

### <a name="single-sql-statement-per-linq-query"></a><span data-ttu-id="8a4de-121">Única instrução SQL por consulta LINQ</span><span class="sxs-lookup"><span data-stu-id="8a4de-121">Single SQL statement per LINQ query</span></span>

<span data-ttu-id="8a4de-122">Outro aspecto do design que mudou significativamente no 3.0 é que agora sempre geramos uma única instrução SQL por consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="8a4de-122">Another aspect of the design that changed significantly in 3.0 is that we now always generate a single SQL statement per LINQ query.</span></span> <span data-ttu-id="8a4de-123">Em versões anteriores, costumávamos gerar várias instruções SQL em determinados casos, chamadas `Include()` convertidas nas propriedades de navegação de coleção e consultas convertidas que seguiam certos padrões com subconsultas.</span><span class="sxs-lookup"><span data-stu-id="8a4de-123">In previous versions, we used to generate multiple SQL statements in certain cases, translated `Include()` calls on collection navigation properties and translated queries that followed certain patterns with subqueries.</span></span> <span data-ttu-id="8a4de-124">Embora isso fosse conveniente em alguns casos e, para `Include()`, inclusive ajudasse a evitar o envio de dados redundantes, a implementação era complexa e resultava em comportamentos extremamente ineficazes (consultas N+1).</span><span class="sxs-lookup"><span data-stu-id="8a4de-124">Although this was in some cases convenient, and for `Include()` it even helped avoid sending redundant data over the wire, the implementation was complex, and it resulted in some extremely inefficient behaviors (N+1 queries).</span></span> <span data-ttu-id="8a4de-125">Havia situações em que os dados retornados em várias consultas eram potencialmente inconsistentes.</span><span class="sxs-lookup"><span data-stu-id="8a4de-125">There were situations in which the data returned across multiple queries was potentially inconsistent.</span></span>

<span data-ttu-id="8a4de-126">De modo semelhante à avaliação do cliente, se o EF Core 3.0 não puder converter uma consulta LINQ em uma única instrução SQL, ele lançará uma exceção de tempo de exceção.</span><span class="sxs-lookup"><span data-stu-id="8a4de-126">Similarly to client evaluation, if EF Core 3.0 can't translate a LINQ query into a single SQL statement, it throws a runtime exception.</span></span> <span data-ttu-id="8a4de-127">No entanto, agora o EF Core é capaz de converter muitos dos padrões comuns que costumavam gerar várias consultas em uma única consulta com JOINs.</span><span class="sxs-lookup"><span data-stu-id="8a4de-127">But we made EF Core capable of translating many of the common patterns that used to generate multiple queries to a single query with JOINs.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="8a4de-128">Suporte do Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8a4de-128">Cosmos DB support</span></span>

<span data-ttu-id="8a4de-129">O provedor do Cosmos DB para o EF Core permite que desenvolvedores familiarizados com o modelo de programação EF conduzam facilmente o Azure Cosmos DB como um banco de dados de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a4de-129">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span> <span data-ttu-id="8a4de-130">A meta é tornar algumas das vantagens do Cosmos DB ainda mais acessíveis para os desenvolvedores de .NET, tais como a distribuição global, disponibilidade "Always On", escalabilidade elástica e baixa latência.</span><span class="sxs-lookup"><span data-stu-id="8a4de-130">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span> <span data-ttu-id="8a4de-131">O provedor ativa a maioria dos recursos do EF Core, como controle automático de alterações, LINQ e conversões de valores, em relação à API do SQL no Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8a4de-131">The provider enables most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

<span data-ttu-id="8a4de-132">Confira a [documentação do provedor do Cosmos DB](xref:core/providers/cosmos/index) para saber mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="8a4de-132">See the [Cosmos DB provider documentation](xref:core/providers/cosmos/index) for more details.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="8a4de-133">Suporte do C# 8.0</span><span class="sxs-lookup"><span data-stu-id="8a4de-133">C# 8.0 support</span></span>

<span data-ttu-id="8a4de-134">O EF Core 3.0 aproveita alguns dos [novos recursos do C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span><span class="sxs-lookup"><span data-stu-id="8a4de-134">EF Core 3.0 takes advantage of a couple of the [new features in C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="8a4de-135">Fluxos assíncronos</span><span class="sxs-lookup"><span data-stu-id="8a4de-135">Asynchronous streams</span></span>

<span data-ttu-id="8a4de-136">Os resultados de consulta assíncronos agora são expostos por meio da nova interface padrão `IAsyncEnumerable<T>` e podem ser consumidos usando `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="8a4de-136">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders =
    from o in context.Orders
    where o.Status == OrderStatus.Pending
    select o;

await foreach(var o in orders.AsAsyncEnumerable())
{
    Process(o);
}
```

<span data-ttu-id="8a4de-137">Confira os [fluxos assíncronos na documentação do C#](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) para saber mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="8a4de-137">See the [asynchronous streams in the C# documentation](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) for more details.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="8a4de-138">Tipos de referência anuláveis</span><span class="sxs-lookup"><span data-stu-id="8a4de-138">Nullable reference types</span></span>

<span data-ttu-id="8a4de-139">Quando este novo recurso é habilitado no código, o EF Core examina a nulidade das propriedades de tipo de referência e aplica-a às colunas e relacionamentos correspondentes no banco de dados: as propriedades de tipos de referências que não permite valor nulo são tratadas como se tivessem o atributo de anotação de dados `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="8a4de-139">When this new feature is enabled in your code, EF Core examines the nullability of reference type properties and applies it to corresponding columns and relationships in the database: properties of non-nullable references types are treated as if they had the `[Required]` data annotation attribute.</span></span>

<span data-ttu-id="8a4de-140">Por exemplo, na seguinte classe, as propriedades marcadas como do tipo `string?` serão configuradas como opcionais, enquanto `string` serão configuradas como obrigatória:</span><span class="sxs-lookup"><span data-stu-id="8a4de-140">For example, in the following class, properties marked as of type `string?` will be configured as optional, whereas `string` will be configured as required:</span></span>

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

<span data-ttu-id="8a4de-141">Confira [Trabalhar com tipos de referência que permitem valor nulo](xref:core/miscellaneous/nullable-reference-types) na documentação do EF Core para saber mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="8a4de-141">See [Working with nullable reference types](xref:core/miscellaneous/nullable-reference-types) in the EF Core documentation for more details.</span></span>

## <a name="interception-of-database-operations"></a><span data-ttu-id="8a4de-142">Interceptação de operações de banco de dados</span><span class="sxs-lookup"><span data-stu-id="8a4de-142">Interception of database operations</span></span>

<span data-ttu-id="8a4de-143">A nova API de interceptação do EF Core 3.0 permite fornecer a lógica personalizada a ser invocada automaticamente sempre que operações de banco de dados de baixo nível ocorrerem como parte da operação normal do EF Core.</span><span class="sxs-lookup"><span data-stu-id="8a4de-143">The new interception API in EF Core 3.0 allows providing custom logic to be invoked automatically whenever low-level database operations occur as part of the normal operation of EF Core.</span></span> <span data-ttu-id="8a4de-144">Por exemplo, ao abrir conexões, confirmar transações ou executar comandos.</span><span class="sxs-lookup"><span data-stu-id="8a4de-144">For example, when opening connections, committing transactions, or executing commands.</span></span>

<span data-ttu-id="8a4de-145">Da mesma forma que os recursos de interceptação que existiam no EF 6, os interceptadores permitem que você intercepte operações antes ou depois que elas ocorram.</span><span class="sxs-lookup"><span data-stu-id="8a4de-145">Similarly to the interception features that existed in EF 6, interceptors allow you to intercept operations before or after they happen.</span></span> <span data-ttu-id="8a4de-146">Ao interceptá-las antes que elas aconteçam, você poderá ignorar a execução e fornecer resultados alternativos baseados na lógica de interceptação.</span><span class="sxs-lookup"><span data-stu-id="8a4de-146">When you intercept them before they happen, you are allowed to by-pass execution and supply alternate results from the interception logic.</span></span>

<span data-ttu-id="8a4de-147">Por exemplo, para manipular o texto do comando, você pode criar um `IDbCommandInterceptor`:</span><span class="sxs-lookup"><span data-stu-id="8a4de-147">For example, to manipulate command text, you can create an `IDbCommandInterceptor`:</span></span>

``` csharp
public class HintCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult result)
    {
        // Manipulate the command text, etc. here...
        command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
        return result;
    }
}
```

<span data-ttu-id="8a4de-148">E registrá-lo com  `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="8a4de-148">And register it with your `DbContext`:</span></span>

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="8a4de-149">Engenharia reversa de exibições de banco de dados</span><span class="sxs-lookup"><span data-stu-id="8a4de-149">Reverse engineering of database views</span></span>

<span data-ttu-id="8a4de-150">Os tipos de consulta, que representam dados que podem ser lidos do banco de dados, mas não atualizados, foram renomeados para [tipos de entidade sem chave](xref:core/modeling/keyless-entity-types).</span><span class="sxs-lookup"><span data-stu-id="8a4de-150">Query types, which represent data that can be read from the database but not updated, have been renamed to [keyless entity types](xref:core/modeling/keyless-entity-types).</span></span>
<span data-ttu-id="8a4de-151">Como eles são extremamente adequados para mapear exibições de banco de dados na maioria dos cenários, o EF Core agora cria automaticamente tipos de entidade sem chave ao fazer engenharia reversa de exibições de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8a4de-151">As they are an excellent fit for mapping database views in most scenarios, EF Core now automatically creates keyless entity types when reverse engineering database views.</span></span>

<span data-ttu-id="8a4de-152">Por exemplo, usando a [ferramenta de linha de comando dotnet ef](xref:core/miscellaneous/cli/dotnet), você pode digitar:</span><span class="sxs-lookup"><span data-stu-id="8a4de-152">For example, using the [dotnet ef command-line tool](xref:core/miscellaneous/cli/dotnet) you can type:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="8a4de-153">E a ferramenta fará scaffolding dos tipos automaticamente para exibições e tabelas sem chaves:</span><span class="sxs-lookup"><span data-stu-id="8a4de-153">And the tool will now automatically scaffold types for views and tables without keys:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Names>(entity =>
    {
        entity.HasNoKey();
        entity.ToView("Names");
    });

    modelBuilder.Entity<Things>(entity =>
    {
        entity.HasNoKey();
    });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="8a4de-154">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="8a4de-154">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="8a4de-155">No EF Core 3.0 em diante, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, será possível adicionar um `Order` sem um `OrderDetails` e todas as propriedades `OrderDetails`, com exceção da chave primária, serão mapeadas para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="8a4de-155">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="8a4de-156">Ao realizar consultas, o EF Core definirá `OrderDetails` como `null` se uma de suas propriedades necessárias não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="8a4de-156">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="8a4de-157">EF 6.3 no .NET Core</span><span class="sxs-lookup"><span data-stu-id="8a4de-157">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="8a4de-158">Esse não é realmente um recurso do EF Core 3.0, mas achamos que ele é importante para muitos dos nossos clientes atuais.</span><span class="sxs-lookup"><span data-stu-id="8a4de-158">This isn't really an EF Core 3.0 feature, but we think it is important to many of our current customers.</span></span>

<span data-ttu-id="8a4de-159">Reconhecemos que muitos aplicativos existentes usam versões anteriores do EF e que a portabilidade deles para o EF Core somente para aproveitar o .NET Core pode exigir um esforço significativo.</span><span class="sxs-lookup"><span data-stu-id="8a4de-159">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can require a significant effort.</span></span>
<span data-ttu-id="8a4de-160">Por esse motivo, decidimos portar a versão mais recente do EF 6 para execução no .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="8a4de-160">For that reason, we decided to port the newest version of EF 6 to run on .NET Core 3.0.</span></span>

<span data-ttu-id="8a4de-161">Para obter mais detalhes, confira [Novidades no EF 6](xref:ef6/what-is-new/index).</span><span class="sxs-lookup"><span data-stu-id="8a4de-161">For more details, see [what's new in EF 6](xref:ef6/what-is-new/index).</span></span>

## <a name="postponed-features"></a><span data-ttu-id="8a4de-162">Recursos adiados</span><span class="sxs-lookup"><span data-stu-id="8a4de-162">Postponed features</span></span>

<span data-ttu-id="8a4de-163">Alguns recursos originalmente planejados para o EF Core 3.0 foram adiados para futuras versões:</span><span class="sxs-lookup"><span data-stu-id="8a4de-163">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span>

- <span data-ttu-id="8a4de-164">A habilidade de ignorar partes de um modelo em migrações, controladas como [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="8a4de-164">Ability to ignore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="8a4de-165">Entidades de recipiente de propriedades, controladas como dois problemas separados: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sobre entidades de tipo compartilhado e [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sobre suporte a mapeamento de propriedades indexadas.</span><span class="sxs-lookup"><span data-stu-id="8a4de-165">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
