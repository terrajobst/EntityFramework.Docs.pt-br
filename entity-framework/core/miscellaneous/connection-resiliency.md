---
title: Resiliência de Conexão – EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: d6e31cf2b9b783ea503703536d159b34bf2e18c0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997184"
---
# <a name="connection-resiliency"></a><span data-ttu-id="1a8f6-102">Resiliência da conexão</span><span class="sxs-lookup"><span data-stu-id="1a8f6-102">Connection Resiliency</span></span>

<span data-ttu-id="1a8f6-103">Resiliência de Conexão automaticamente repete comandos de banco de dados com falha.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="1a8f6-104">O recurso pode ser usado com qualquer banco de dados, fornecendo uma "estratégia de execução," que encapsula a lógica necessária para detectar falhas e repetir comandos.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="1a8f6-105">Provedores do EF Core podem fornecer estratégias de execução adaptadas às suas condições de falha do banco de dados específico e as políticas de repetição ideal.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="1a8f6-106">Por exemplo, o provedor SQL Server inclui uma estratégia de execução especificamente projetado para o SQL Server (incluindo SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="1a8f6-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="1a8f6-107">Ele está ciente dos tipos de exceção que podem ser repetidos e tem padrões específicos para o número máximo de tentativas, atraso entre as repetições, etc.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="1a8f6-108">Uma estratégia de execução é especificada ao configurar as opções para seu contexto.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="1a8f6-109">Isso é normalmente na `OnConfiguring` método de seu contexto derivado ou no `Startup.cs` para um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="1a8f6-110">Estratégia de execução personalizado</span><span class="sxs-lookup"><span data-stu-id="1a8f6-110">Custom execution strategy</span></span>

<span data-ttu-id="1a8f6-111">Há um mecanismo para registrar uma estratégia de execução personalizado de sua preferência, se você quiser alterar qualquer um dos padrões.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="1a8f6-112">Transações e estratégias de execução</span><span class="sxs-lookup"><span data-stu-id="1a8f6-112">Execution strategies and transactions</span></span>

<span data-ttu-id="1a8f6-113">Uma estratégia de execução repete automaticamente em falhas precisa ser capaz de reproduzir cada operação em um bloco de repetição que falha.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="1a8f6-114">Quando novas tentativas são habilitadas, cada operação executada através do EF Core se torna sua própria operação repetível.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="1a8f6-115">Ou seja, cada consulta e cada chamada para `SaveChanges()` serão repetidas como uma unidade se ocorrer uma falha temporária.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-115">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="1a8f6-116">No entanto, se seu código inicia uma transação usando `BeginTransaction()` você está definindo seu próprio grupo de operações que precisam ser tratados como uma unidade, e tudo dentro da transação precisarão ser reproduzidos deverá ocorrer de uma falha.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-116">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="1a8f6-117">Se você tentar fazer isso usando uma estratégia de execução, você receberá uma exceção semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="1a8f6-117">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="1a8f6-118">InvalidOperationException: A estratégia de execução configurada 'SqlServerRetryingExecutionStrategy' não dá suporte a transações iniciadas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-118">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="1a8f6-119">Use a estratégia de execução retornada por 'DbContext.Database.CreateExecutionStrategy()' para executar todas as operações na transação como uma unidade repetível.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-119">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="1a8f6-120">A solução é invocar manualmente a estratégia de execução com um delegado que representa tudo que precisa para ser executado.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-120">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="1a8f6-121">Se ocorrer uma falha temporária, a estratégia de execução invocará o delegado novamente.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-121">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="1a8f6-122">Falha de confirmação de transação e o problema de idempotência</span><span class="sxs-lookup"><span data-stu-id="1a8f6-122">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="1a8f6-123">Em geral, quando houver uma falha de conexão a transação atual é revertida.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-123">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="1a8f6-124">No entanto, se a conexão for descartada enquanto a transação está sendo confirmada resultante estado da transação é desconhecido.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-124">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="1a8f6-125">Consulte este [postagem de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-125">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="1a8f6-126">Por padrão, a estratégia de execução tentará repetir a operação como se a transação foi revertida, mas se não for o caso isso resultará em uma exceção se o novo estado do banco de dados é incompatível ou pode levar à **corrupção de dados** se a operação não se baseia em um estado específico, por exemplo, ao inserir uma nova linha com valores de chave gerada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-126">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="1a8f6-127">Há várias maneiras de lidar com isso.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-127">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="1a8f6-128">Opção 1 - fazer nada (quase)</span><span class="sxs-lookup"><span data-stu-id="1a8f6-128">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="1a8f6-129">A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição ocorrer, na verdade.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-129">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="1a8f6-130">No entanto, você precisa evitar o uso de chaves geradas pelo repositório para garantir que uma exceção é gerada em vez de adicionar uma linha duplicada.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-130">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="1a8f6-131">Considere o uso de um valor GUID gerado pelo cliente ou um gerador de valor do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-131">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="1a8f6-132">Opção 2 - estado de recompilação do aplicativo</span><span class="sxs-lookup"><span data-stu-id="1a8f6-132">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="1a8f6-133">Descartar atual `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-133">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="1a8f6-134">Criar um novo `DbContext` e restaurar o estado do seu aplicativo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-134">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="1a8f6-135">Informar ao usuário que a última operação talvez não tenham sido concluída com êxito.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-135">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="1a8f6-136">Opção 3 - adicionar verificação de estado</span><span class="sxs-lookup"><span data-stu-id="1a8f6-136">Option 3 - Add state verification</span></span>

<span data-ttu-id="1a8f6-137">Para a maioria das operações que alteram o estado do banco de dados, é possível adicionar código que verifica se ela foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-137">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="1a8f6-138">O EF fornece um método de extensão para facilitar essa tarefa - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-138">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="1a8f6-139">Esse método inicia e confirma uma transação e também aceita uma função no `verifySucceeded` parâmetro que é invocado quando ocorre um erro transitório durante a confirmação da transação.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-139">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="1a8f6-140">Aqui `SaveChanges` é invocado com `acceptAllChangesOnSuccess` definido como `false` para evitar a alteração do estado da `Blog` entidade `Unchanged` se `SaveChanges` for bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-140">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="1a8f6-141">Isso permite que repetir a mesma operação, se houver uma falha e a transação é revertida.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-141">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="1a8f6-142">Opção 4 - controlar manualmente a transação</span><span class="sxs-lookup"><span data-stu-id="1a8f6-142">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="1a8f6-143">Se você precisar usar chaves geradas pelo repositório ou precisa de uma maneira genérica de lidar com falhas de confirmação que não depende da operação executada cada transação podiam ser atribuída a uma ID que é verificada durante a confirmação falhará.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-143">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="1a8f6-144">Adicione uma tabela no banco de dados usado para acompanhar o status das transações.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-144">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="1a8f6-145">Inserir uma linha na tabela no início de cada transação.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-145">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="1a8f6-146">Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-146">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="1a8f6-147">Se a confirmação for bem-sucedida, exclua a linha correspondente para evitar o crescimento da tabela.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-147">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="1a8f6-148">Certifique-se de que o contexto usado para a verificação tem uma estratégia de execução definida como a conexão é a probabilidade de falhar novamente durante a verificação se falhou durante a confirmação da transação.</span><span class="sxs-lookup"><span data-stu-id="1a8f6-148">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
