---
title: Resiliência de conexão e lógica de repetição-EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824842"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="076ef-102">Resiliência de conexão e lógica de repetição</span><span class="sxs-lookup"><span data-stu-id="076ef-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="076ef-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="076ef-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="076ef-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="076ef-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="076ef-105">Os aplicativos que se conectam a um servidor de banco de dados sempre foram vulneráveis a quebras de conexão devido a falhas de back-end e instabilidade de rede.</span><span class="sxs-lookup"><span data-stu-id="076ef-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="076ef-106">No entanto, em um ambiente baseado em LAN que trabalha com servidores de banco de dados dedicados, esses erros são raros o suficiente para que a lógica extra para lidar com essas falhas não seja geralmente necessária.</span><span class="sxs-lookup"><span data-stu-id="076ef-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="076ef-107">Com o aumento dos servidores de banco de dados baseados em nuvem, como o banco de dados SQL do Windows Azure e as conexões em redes menos confiáveis, agora é mais comum que as interrupções de conexão ocorram.</span><span class="sxs-lookup"><span data-stu-id="076ef-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="076ef-108">Isso pode ser devido a técnicas defensivas que os bancos de dados de nuvem usam para garantir a imparcialidade do serviço, como a limitação de conexão ou a instabilidade na rede, causando tempos limite intermitentes e outros erros transitórios.</span><span class="sxs-lookup"><span data-stu-id="076ef-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="076ef-109">A resiliência da conexão refere-se à capacidade do EF de repetir automaticamente os comandos que falharem devido a essas quebras de conexão.</span><span class="sxs-lookup"><span data-stu-id="076ef-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="076ef-110">Estratégias de execução</span><span class="sxs-lookup"><span data-stu-id="076ef-110">Execution Strategies</span></span>  

<span data-ttu-id="076ef-111">A repetição de conexão é encarregada por uma implementação da interface IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="076ef-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="076ef-112">As implementações do IDbExecutionStrategy serão responsáveis por aceitar uma operação e, se ocorrer uma exceção, determinando se uma repetição é apropriada e tentando novamente se for.</span><span class="sxs-lookup"><span data-stu-id="076ef-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="076ef-113">Há quatro estratégias de execução fornecidas com o EF:</span><span class="sxs-lookup"><span data-stu-id="076ef-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="076ef-114">**DefaultExecutionStrategy**: essa estratégia de execução não repete nenhuma operação, é o padrão para bancos de dados diferentes do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="076ef-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="076ef-115">**DefaultSqlExecutionStrategy**: essa é uma estratégia de execução interna que é usada por padrão.</span><span class="sxs-lookup"><span data-stu-id="076ef-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="076ef-116">No entanto, essa estratégia não é repetida. no entanto, ela encapsulará as exceções que poderiam ser transitórias para informar os usuários de que eles podem querer habilitar a resiliência da conexão.</span><span class="sxs-lookup"><span data-stu-id="076ef-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="076ef-117">**DbExecutionStrategy**: essa classe é adequada como uma classe base para outras estratégias de execução, incluindo suas próprias personalizadas.</span><span class="sxs-lookup"><span data-stu-id="076ef-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="076ef-118">Ele implementa uma política de repetição exponencial, em que a nova tentativa inicial ocorre com um atraso zero e o atraso aumenta exponencialmente até que a contagem máxima de repetições seja atingida.</span><span class="sxs-lookup"><span data-stu-id="076ef-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="076ef-119">Essa classe tem um método abstrato ShouldRetryOn que pode ser implementado em estratégias de execução derivadas para controlar quais exceções devem ser repetidas.</span><span class="sxs-lookup"><span data-stu-id="076ef-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="076ef-120">**SqlAzureExecutionStrategy**: essa estratégia de execução herda de DbExecutionStrategy e tentará novamente as exceções conhecidas como possivelmente transitórias ao trabalhar com o banco de dados SQL do Azure.</span><span class="sxs-lookup"><span data-stu-id="076ef-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="076ef-121">As estratégias de execução 2 e 4 estão incluídas no provedor do SQL Server que acompanha o EF, que está no assembly do EntityFramework. SqlServer e foi projetado para trabalhar com SQL Server.</span><span class="sxs-lookup"><span data-stu-id="076ef-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="076ef-122">Habilitando uma estratégia de execução</span><span class="sxs-lookup"><span data-stu-id="076ef-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="076ef-123">A maneira mais fácil de dizer ao EF para usar uma estratégia de execução é com o método SetExecutionStrategy da classe [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) :</span><span class="sxs-lookup"><span data-stu-id="076ef-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="076ef-124">Esse código informa ao EF para usar o SqlAzureExecutionStrategy ao se conectar a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="076ef-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="076ef-125">Configurando a estratégia de execução</span><span class="sxs-lookup"><span data-stu-id="076ef-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="076ef-126">O construtor de SqlAzureExecutionStrategy pode aceitar dois parâmetros, MaxRetryCount e MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="076ef-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="076ef-127">Contagem de MaxRetry é o número máximo de vezes que a estratégia tentará novamente.</span><span class="sxs-lookup"><span data-stu-id="076ef-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="076ef-128">O MaxDelay é um TimeSpan que representa o atraso máximo entre as tentativas que serão usadas pela estratégia de execução.</span><span class="sxs-lookup"><span data-stu-id="076ef-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="076ef-129">Para definir o número máximo de repetições como 1 e o atraso máximo para 30 segundos, você deve executar o seguinte:</span><span class="sxs-lookup"><span data-stu-id="076ef-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execute the following:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

<span data-ttu-id="076ef-130">O SqlAzureExecutionStrategy tentará instantaneamente na primeira vez que ocorrer uma falha transitória, mas atrasará mais tempo entre cada repetição até que o limite máximo de tentativas seja excedido ou o tempo total atinja o atraso máximo.</span><span class="sxs-lookup"><span data-stu-id="076ef-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="076ef-131">As estratégias de execução tentarão apenas um número limitado de exceções que normalmente são transitórias, você ainda precisará lidar com outros erros, bem como capturar a exceção RetryLimitExceeded para o caso em que um erro não é transitório ou leva muito tempo para ser resolvido próprio.</span><span class="sxs-lookup"><span data-stu-id="076ef-131">The execution strategies will only retry a limited number of exceptions that are usually transient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="076ef-132">Há algumas limitações conhecidas ao usar uma estratégia de execução de repetição:</span><span class="sxs-lookup"><span data-stu-id="076ef-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="076ef-133">Não há suporte para consultas de streaming</span><span class="sxs-lookup"><span data-stu-id="076ef-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="076ef-134">Por padrão, o EF6 e a versão posterior armazenarão em buffer os resultados da consulta em vez de transmiti-los.</span><span class="sxs-lookup"><span data-stu-id="076ef-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="076ef-135">Se desejar que os resultados sejam transmitidos, você poderá usar o método de transmissão para alterar uma consulta de LINQ to Entities para streaming.</span><span class="sxs-lookup"><span data-stu-id="076ef-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="076ef-136">Não há suporte para streaming quando uma estratégia de execução de repetição é registrada.</span><span class="sxs-lookup"><span data-stu-id="076ef-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="076ef-137">Essa limitação existe porque a conexão pode encaixar a parte por meio dos resultados retornados.</span><span class="sxs-lookup"><span data-stu-id="076ef-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="076ef-138">Quando isso ocorre, o EF precisa executar novamente a consulta inteira, mas não tem uma maneira confiável de saber quais resultados já foram retornados (os dados podem ter sido alterados desde que a consulta inicial foi enviada, os resultados podem voltar em uma ordem diferente, os resultados podem não ter um identificador exclusivo , etc.).</span><span class="sxs-lookup"><span data-stu-id="076ef-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="076ef-139">Não há suporte para transações iniciadas pelo usuário</span><span class="sxs-lookup"><span data-stu-id="076ef-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="076ef-140">Quando você configurou uma estratégia de execução que resulta em novas tentativas, há algumas limitações em relação ao uso de transações.</span><span class="sxs-lookup"><span data-stu-id="076ef-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="076ef-141">Por padrão, o EF executará qualquer atualização de banco de dados em uma transação.</span><span class="sxs-lookup"><span data-stu-id="076ef-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="076ef-142">Você não precisa fazer nada para habilitar isso, o EF sempre faz isso automaticamente.</span><span class="sxs-lookup"><span data-stu-id="076ef-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="076ef-143">Por exemplo, no código a seguir, SaveChanges é executado automaticamente em uma transação.</span><span class="sxs-lookup"><span data-stu-id="076ef-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="076ef-144">Se o SaveChanges falhar após a inserção de um dos novos sites, a transação seria revertida e nenhuma alteração foi aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="076ef-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="076ef-145">O contexto também é deixado em um estado que permite que SaveChanges seja chamado novamente para tentar aplicar as alterações novamente.</span><span class="sxs-lookup"><span data-stu-id="076ef-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="076ef-146">Quando não estiver usando uma estratégia de execução de repetição, você poderá encapsular várias operações em uma única transação.</span><span class="sxs-lookup"><span data-stu-id="076ef-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="076ef-147">Por exemplo, o código a seguir encapsula duas chamadas SaveChanges em uma única transação.</span><span class="sxs-lookup"><span data-stu-id="076ef-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="076ef-148">Se qualquer parte de uma operação falhar, nenhuma das alterações será aplicada.</span><span class="sxs-lookup"><span data-stu-id="076ef-148">If any part of either operation fails then none of the changes are applied.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

<span data-ttu-id="076ef-149">Isso não é suportado quando se usa uma estratégia de execução de repetição porque o EF não reconhece nenhuma operação anterior e como repeti-las.</span><span class="sxs-lookup"><span data-stu-id="076ef-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="076ef-150">Por exemplo, se o segundo SaveChanges falhar, o EF não terá mais as informações necessárias para tentar novamente a primeira chamada SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="076ef-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="solution-manually-call-execution-strategy"></a><span data-ttu-id="076ef-151">Solução: chamar a estratégia de execução manualmente</span><span class="sxs-lookup"><span data-stu-id="076ef-151">Solution: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="076ef-152">A solução é usar manualmente a estratégia de execução e dar a ela o conjunto inteiro de lógica a ser executado, para que possa tentar novamente tudo se uma das operações falhar.</span><span class="sxs-lookup"><span data-stu-id="076ef-152">The solution is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="076ef-153">Quando uma estratégia de execução derivada de DbExecutionStrategy estiver sendo executada, suspenderá a estratégia de execução implícita usada no SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="076ef-153">When an execution strategy derived from DbExecutionStrategy is running it will suspend the implicit execution strategy used in SaveChanges.</span></span>  

<span data-ttu-id="076ef-154">Observe que os contextos devem ser construídos dentro do bloco de código a ser repetido.</span><span class="sxs-lookup"><span data-stu-id="076ef-154">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="076ef-155">Isso garante que estamos começando com um estado limpo para cada repetição.</span><span class="sxs-lookup"><span data-stu-id="076ef-155">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });
```  
