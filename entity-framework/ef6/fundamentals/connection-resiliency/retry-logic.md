---
title: Conexão resiliência e lógica de repetição - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 47181292873009c7bce2047787503258ffa35d9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997479"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="45543-102">Conexão resiliência e lógica de repetição</span><span class="sxs-lookup"><span data-stu-id="45543-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="45543-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="45543-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="45543-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="45543-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="45543-105">Aplicativos que se conectam a um servidor de banco de dados sempre foram vulneráveis a interrupções de conexão devido a falhas de back-end e instabilidade de rede.</span><span class="sxs-lookup"><span data-stu-id="45543-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="45543-106">No entanto, em um ambiente de LAN com base em trabalhar com servidores de banco de dados dedicado, esses erros são bastante raros que lógica extra para lidar com essas falhas geralmente não é necessária.</span><span class="sxs-lookup"><span data-stu-id="45543-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="45543-107">Com o aumento da nuvem com base conexões e servidores de banco de dados como banco de dados SQL do Windows Azure em redes menos confiáveis agora é mais comum para quebras de conexão ocorra.</span><span class="sxs-lookup"><span data-stu-id="45543-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="45543-108">Isso pode ser devido a técnicas de defesa que bancos de dados de uso para garantir a integridade do serviço, como as limitações de conexão ou à instabilidade na rede, causando tempos limite de intermitente e outros erros transitórios de nuvem.</span><span class="sxs-lookup"><span data-stu-id="45543-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="45543-109">Resiliência de Conexão refere-se à capacidade para o EF repetir automaticamente a todos os comandos que falharem devido a essas quebras de conexão.</span><span class="sxs-lookup"><span data-stu-id="45543-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="45543-110">Estratégias de execução</span><span class="sxs-lookup"><span data-stu-id="45543-110">Execution Strategies</span></span>  

<span data-ttu-id="45543-111">Repetição de Conexão é tratada por uma implementação da interface IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="45543-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="45543-112">Implementações do IDbExecutionStrategy será responsáveis por aceitar uma operação e, se ocorrer uma exceção, determinando se uma repetição é apropriada e tentar novamente se ele for.</span><span class="sxs-lookup"><span data-stu-id="45543-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="45543-113">Há quatro estratégias de execução que são fornecidos com o EF:</span><span class="sxs-lookup"><span data-stu-id="45543-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="45543-114">**DefaultExecutionStrategy**: essa estratégia de execução não tenta novamente todas as operações, é o padrão para bancos de dados diferente do sql server.</span><span class="sxs-lookup"><span data-stu-id="45543-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="45543-115">**DefaultSqlExecutionStrategy**: isso é uma estratégia de execução interna que é usada por padrão.</span><span class="sxs-lookup"><span data-stu-id="45543-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="45543-116">Essa estratégia não é repetida, no entanto, ele vai encapsular quaisquer exceções que poderiam ser transitórias para informar os usuários que desejam habilitar a resiliência de conexão.</span><span class="sxs-lookup"><span data-stu-id="45543-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="45543-117">**DbExecutionStrategy**: esta classe é adequada como uma classe base para outras estratégias de execução, incluindo seus próprios conectores personalizados.</span><span class="sxs-lookup"><span data-stu-id="45543-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="45543-118">Ele implementa uma política de repetição exponencial, onde a repetição inicial acontece com zero atraso e o atraso aumenta exponencialmente até que a contagem máxima de novas tentativas for atingida.</span><span class="sxs-lookup"><span data-stu-id="45543-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="45543-119">Essa classe tem um método abstrato ShouldRetryOn que pode ser implementado em estratégias de execução derivada para controlar quais exceções devem ser recuperadas.</span><span class="sxs-lookup"><span data-stu-id="45543-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="45543-120">**SqlAzureExecutionStrategy**: essa estratégia de execução herda DbExecutionStrategy e tentará novamente no exceções que são conhecidas como transitório, possivelmente, ao trabalhar com o banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="45543-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="45543-121">Estratégias de execução 2 e 4 são incluídas no provedor do Sql Server que é fornecido com o EF, que está no assembly EntityFramework. SQLServer e são projetadas para trabalhar com o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="45543-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="45543-122">Habilitando uma estratégia de execução</span><span class="sxs-lookup"><span data-stu-id="45543-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="45543-123">A maneira mais fácil de informar ao EF para usar uma estratégia de execução é com o método SetExecutionStrategy a [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) classe:</span><span class="sxs-lookup"><span data-stu-id="45543-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="45543-124">Esse código diz ao EF para usar o SqlAzureExecutionStrategy ao se conectar ao SQL Server.</span><span class="sxs-lookup"><span data-stu-id="45543-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="45543-125">Configurando a estratégia de execução</span><span class="sxs-lookup"><span data-stu-id="45543-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="45543-126">O construtor do SqlAzureExecutionStrategy pode aceitar dois parâmetros, MaxRetryCount e MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="45543-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="45543-127">Contagem de MaxRetry é o número máximo de vezes que a estratégia será tentada.</span><span class="sxs-lookup"><span data-stu-id="45543-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="45543-128">O MaxDelay é um TimeSpan que representa o atraso máximo entre novas tentativas que usará a estratégia de execução.</span><span class="sxs-lookup"><span data-stu-id="45543-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="45543-129">Para definir o número máximo de tentativas para 1 e o atraso máximo para 30 segundos, você faria execue o seguinte:</span><span class="sxs-lookup"><span data-stu-id="45543-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

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

<span data-ttu-id="45543-130">A SqlAzureExecutionStrategy tentará instantaneamente na primeira vez que uma falha temporária ocorre, mas atrasará mais entre cada repetição até que o limite de repetição de ambos o máximo for excedida, ou o tempo total atinge o atraso máximo.</span><span class="sxs-lookup"><span data-stu-id="45543-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="45543-131">As estratégias de execução tentará apenas um número limitado de exceções que são geralmente tansient, você ainda precisará lidar com outros erros, bem como capturar a exceção RetryLimitExceeded para o caso em que um erro não é transitório ou levar muito tempo resolver em si.</span><span class="sxs-lookup"><span data-stu-id="45543-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

## <a name="limitations"></a><span data-ttu-id="45543-132">Limitações</span><span class="sxs-lookup"><span data-stu-id="45543-132">Limitations</span></span>  

<span data-ttu-id="45543-133">Há alguns conhecidos de limitações ao usar uma estratégia de execução tentando novamente:</span><span class="sxs-lookup"><span data-stu-id="45543-133">There are some known of limitations when using a retrying execution strategy:</span></span>  

### <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="45543-134">Não há suporte para consultas de streaming</span><span class="sxs-lookup"><span data-stu-id="45543-134">Streaming queries are not supported</span></span>  

<span data-ttu-id="45543-135">Por padrão, EF6 e versões posteriores armazenará em buffer os resultados da consulta em vez de streaming-los.</span><span class="sxs-lookup"><span data-stu-id="45543-135">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="45543-136">Se você quiser ter resultados transmitidos você pode usar o método AsStreaming para alterar um LINQ para consulta de entidades para streaming.</span><span class="sxs-lookup"><span data-stu-id="45543-136">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="45543-137">Não há suporte para o fluxo quando uma estratégia de execução tentando novamente é registrada.</span><span class="sxs-lookup"><span data-stu-id="45543-137">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="45543-138">Essa limitação existe porque a conexão foi possível descartar metade do caminho pelos resultados que estão sendo retornados.</span><span class="sxs-lookup"><span data-stu-id="45543-138">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="45543-139">Quando isso ocorrer, o EF precisa executar novamente a consulta inteira, mas não tem nenhuma maneira confiável de saber quais resultados já foram retornados (dados podem ter sido alterado desde a consulta inicial foi enviada, os resultados podem vir de volta em uma ordem diferente, os resultados não podem ter um identificador exclusivo etc.).</span><span class="sxs-lookup"><span data-stu-id="45543-139">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

### <a name="user-initiated-transactions-not-supported"></a><span data-ttu-id="45543-140">As transações que não tem suportadas iniciada pelo usuário</span><span class="sxs-lookup"><span data-stu-id="45543-140">User initiated transactions not supported</span></span>  

<span data-ttu-id="45543-141">Quando você tiver configurado uma estratégia de execução que resulta em tentativas, há algumas limitações em torno do uso de transações.</span><span class="sxs-lookup"><span data-stu-id="45543-141">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

#### <a name="whats-supported-efs-default-transaction-behavior"></a><span data-ttu-id="45543-142">O que é suportado: comportamento de transação de padrão do EF</span><span class="sxs-lookup"><span data-stu-id="45543-142">What's Supported: EF's default transaction behavior</span></span>  

<span data-ttu-id="45543-143">Por padrão, o EF executará todas as atualizações de banco de dados dentro de uma transação.</span><span class="sxs-lookup"><span data-stu-id="45543-143">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="45543-144">Você não precisa fazer nada para habilitar isso, o EF sempre faz isso automaticamente.</span><span class="sxs-lookup"><span data-stu-id="45543-144">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="45543-145">Por exemplo, no código a seguir SaveChanges é executada automaticamente em uma transação.</span><span class="sxs-lookup"><span data-stu-id="45543-145">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="45543-146">Se SaveChanges falhar depois de inserir uma do novo Site e em seguida, a transação deve ser revertida e nenhuma alteração aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="45543-146">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="45543-147">O contexto também for deixado em um estado que permite que o SaveChanges ser chamado novamente para tentar novamente a aplicação das alterações.</span><span class="sxs-lookup"><span data-stu-id="45543-147">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a><span data-ttu-id="45543-148">O que não tem suporte: transações iniciada pelo usuário</span><span class="sxs-lookup"><span data-stu-id="45543-148">What’s not supported: User initiated transactions</span></span>  

<span data-ttu-id="45543-149">Quando não estiver usando uma estratégia de execução tentando novamente, você pode encapsular várias operações em uma única transação.</span><span class="sxs-lookup"><span data-stu-id="45543-149">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="45543-150">Por exemplo, o seguinte código conclui duas chamadas SaveChanges em uma única transação.</span><span class="sxs-lookup"><span data-stu-id="45543-150">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="45543-151">Se qualquer parte de uma ou outra operação falhar, em seguida, nenhuma alteração será aplicada.</span><span class="sxs-lookup"><span data-stu-id="45543-151">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="45543-152">Isso não é suportado ao usar uma estratégia de execução tentando novamente porque o EF não está ciente de todas as operações anteriores e como repeti-los.</span><span class="sxs-lookup"><span data-stu-id="45543-152">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="45543-153">Por exemplo, se o SaveChanges segunda falha, em seguida, EF não tem as informações necessárias para repetir a primeira chamada SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="45543-153">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

#### <a name="possible-workarounds"></a><span data-ttu-id="45543-154">Possíveis soluções alternativas</span><span class="sxs-lookup"><span data-stu-id="45543-154">Possible workarounds</span></span>  

##### <a name="suspend-execution-strategy"></a><span data-ttu-id="45543-155">Suspender estratégia de execução</span><span class="sxs-lookup"><span data-stu-id="45543-155">Suspend Execution Strategy</span></span>  

<span data-ttu-id="45543-156">Uma solução alternativa é possível suspender a estratégia de execução tentando novamente para o trecho de código que precisa usar um usuário iniciou a transação.</span><span class="sxs-lookup"><span data-stu-id="45543-156">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="45543-157">A maneira mais fácil de fazer isso é adicionar um sinalizador SuspendExecutionStrategy ao seu código com base em classe de configuração e altere o lambda de estratégia de execução para retornar a estratégia de execução padrão (não retying) quando o sinalizador é definido.</span><span class="sxs-lookup"><span data-stu-id="45543-157">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="45543-158">Observe que estamos usando CallContext para armazenar o valor do sinalizador.</span><span class="sxs-lookup"><span data-stu-id="45543-158">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="45543-159">Isso fornece uma funcionalidade semelhante para o armazenamento local de thread, mas é seguro usar com o código assíncrono - incluindo consulta assíncrona e salvar com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="45543-159">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="45543-160">Agora podemos pode suspender a estratégia de execução para a seção de código que usa uma transação iniciada pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="45543-160">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a><span data-ttu-id="45543-161">Chamar manualmente a estratégia de execução</span><span class="sxs-lookup"><span data-stu-id="45543-161">Manually Call Execution Strategy</span></span>  

<span data-ttu-id="45543-162">Outra opção é usar a estratégia de execução manualmente e dê a ele o conjunto inteiro de lógica a ser executado, para que ele pode repetir tudo o que se uma das operações falhar.</span><span class="sxs-lookup"><span data-stu-id="45543-162">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="45543-163">Ainda precisamos suspender a estratégia de execução - usando a técnica mostrada acima - para que nenhum contexto usado dentro do bloco de código com nova tentativa não tentará repetir a ação.</span><span class="sxs-lookup"><span data-stu-id="45543-163">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="45543-164">Observe que nenhum contexto deve ser construído dentro do bloco de código para ser repetida.</span><span class="sxs-lookup"><span data-stu-id="45543-164">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="45543-165">Isso garante que estamos começando com um estado limpo para cada repetição.</span><span class="sxs-lookup"><span data-stu-id="45543-165">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

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

MyConfiguration.SuspendExecutionStrategy = false;
```  
