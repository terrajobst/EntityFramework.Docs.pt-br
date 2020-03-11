---
title: Resiliência de conexão-EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 07646e6ead845c38537945a03367ac7f50784236
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416637"
---
# <a name="connection-resiliency"></a><span data-ttu-id="c4190-102">Resiliência da conexão</span><span class="sxs-lookup"><span data-stu-id="c4190-102">Connection Resiliency</span></span>

<span data-ttu-id="c4190-103">A resiliência de conexão repete automaticamente os comandos de banco de dados com falha.</span><span class="sxs-lookup"><span data-stu-id="c4190-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="c4190-104">O recurso pode ser usado com qualquer banco de dados fornecendo uma "estratégia de execução", que encapsula a lógica necessária para detectar falhas e repetir comandos.</span><span class="sxs-lookup"><span data-stu-id="c4190-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="c4190-105">Os provedores de EF Core podem fornecer estratégias de execução adaptadas às suas condições específicas de falha do banco de dados e às políticas de repetição ideais.</span><span class="sxs-lookup"><span data-stu-id="c4190-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="c4190-106">Por exemplo, o provedor de SQL Server inclui uma estratégia de execução que é especificamente adaptada para SQL Server (incluindo SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="c4190-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="c4190-107">Ele reconhece os tipos de exceção que podem ser repetidos e tem padrões sensíveis para tentativas máximas, atraso entre repetições, etc.</span><span class="sxs-lookup"><span data-stu-id="c4190-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="c4190-108">Uma estratégia de execução é especificada ao configurar as opções para seu contexto.</span><span class="sxs-lookup"><span data-stu-id="c4190-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="c4190-109">Normalmente, isso está no método `OnConfiguring` do contexto derivado:</span><span class="sxs-lookup"><span data-stu-id="c4190-109">This is typically in the `OnConfiguring` method of your derived context:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

<span data-ttu-id="c4190-110">ou em `Startup.cs` para um aplicativo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c4190-110">or in `Startup.cs` for an ASP.NET Core application:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a><span data-ttu-id="c4190-111">Estratégia de execução personalizada</span><span class="sxs-lookup"><span data-stu-id="c4190-111">Custom execution strategy</span></span>

<span data-ttu-id="c4190-112">Há um mecanismo para registrar uma estratégia de execução personalizada de sua preferência se você quiser alterar qualquer um dos padrões.</span><span class="sxs-lookup"><span data-stu-id="c4190-112">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="c4190-113">Estratégias e transações de execução</span><span class="sxs-lookup"><span data-stu-id="c4190-113">Execution strategies and transactions</span></span>

<span data-ttu-id="c4190-114">Uma estratégia de execução que repete automaticamente as falhas precisa ser capaz de reproduzir cada operação em um bloco de repetição que falha.</span><span class="sxs-lookup"><span data-stu-id="c4190-114">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="c4190-115">Quando novas tentativas são habilitadas, cada operação executada por meio de EF Core se torna sua própria operação passível.</span><span class="sxs-lookup"><span data-stu-id="c4190-115">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="c4190-116">Ou seja, cada consulta e cada chamada para `SaveChanges()` será repetida como uma unidade se ocorrer uma falha transitória.</span><span class="sxs-lookup"><span data-stu-id="c4190-116">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="c4190-117">No entanto, se o seu código iniciar uma transação usando `BeginTransaction()` você estiver definindo seu próprio grupo de operações que precisam ser tratadas como uma unidade, e tudo dentro da transação precisará ser reproduzido caso ocorra uma falha.</span><span class="sxs-lookup"><span data-stu-id="c4190-117">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="c4190-118">Você receberá uma exceção como a seguinte se tentar fazer isso ao usar uma estratégia de execução:</span><span class="sxs-lookup"><span data-stu-id="c4190-118">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="c4190-119">InvalidOperationException: a estratégia de execução configurada ' SqlServerRetryingExecutionStrategy ' não oferece suporte a transações iniciadas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="c4190-119">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="c4190-120">Use a estratégia de execução retornada por 'DbContext.Database.CreateExecutionStrategy()' para executar todas as operações na transação como uma unidade repetível.</span><span class="sxs-lookup"><span data-stu-id="c4190-120">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="c4190-121">A solução é invocar manualmente a estratégia de execução com um delegado representando tudo o que precisa ser executado.</span><span class="sxs-lookup"><span data-stu-id="c4190-121">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="c4190-122">Se ocorrer uma falha transitória, a estratégia de execução invocará o representante novamente.</span><span class="sxs-lookup"><span data-stu-id="c4190-122">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

<span data-ttu-id="c4190-123">Essa abordagem também pode ser usada com transações de ambiente.</span><span class="sxs-lookup"><span data-stu-id="c4190-123">This approach can also be used with ambient transactions.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="c4190-124">Falha na confirmação da transação e o problema de Idempotência</span><span class="sxs-lookup"><span data-stu-id="c4190-124">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="c4190-125">Em geral, quando há uma falha de conexão, a transação atual é revertida.</span><span class="sxs-lookup"><span data-stu-id="c4190-125">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="c4190-126">No entanto, se a conexão for descartada enquanto a transação estiver sendo confirmada, o estado resultante da transação será desconhecido.</span><span class="sxs-lookup"><span data-stu-id="c4190-126">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> 

<span data-ttu-id="c4190-127">Por padrão, a estratégia de execução tentará novamente a operação como se a transação fosse revertida, mas se não for o caso, isso resultará em uma exceção se o novo estado do banco de dados for incompatível ou puder levar à **corrupção de dados** se a operação não depender de um estado específico, por exemplo, ao inserir uma nova linha com valores de chave gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c4190-127">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="c4190-128">Há várias maneiras de lidar com isso.</span><span class="sxs-lookup"><span data-stu-id="c4190-128">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="c4190-129">Opção 1-fazer (quase) nada</span><span class="sxs-lookup"><span data-stu-id="c4190-129">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="c4190-130">A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição realmente ocorrer.</span><span class="sxs-lookup"><span data-stu-id="c4190-130">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="c4190-131">No entanto, você precisa evitar o uso de chaves geradas pela loja para garantir que uma exceção seja lançada em vez de adicionar uma linha duplicada.</span><span class="sxs-lookup"><span data-stu-id="c4190-131">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="c4190-132">Considere usar um valor GUID gerado pelo cliente ou um gerador de valor do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="c4190-132">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="c4190-133">Opção 2 – recriar o estado do aplicativo</span><span class="sxs-lookup"><span data-stu-id="c4190-133">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="c4190-134">Descartar a `DbContext`atual.</span><span class="sxs-lookup"><span data-stu-id="c4190-134">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="c4190-135">Crie um novo `DbContext` e restaure o estado do seu aplicativo a partir do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c4190-135">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="c4190-136">Informe ao usuário que a última operação pode não ter sido concluída com êxito.</span><span class="sxs-lookup"><span data-stu-id="c4190-136">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="c4190-137">Opção 3 – adicionar verificação de estado</span><span class="sxs-lookup"><span data-stu-id="c4190-137">Option 3 - Add state verification</span></span>

<span data-ttu-id="c4190-138">Para a maioria das operações que alteram o estado do banco de dados, é possível adicionar código que verifica se foi bem-sucedido.</span><span class="sxs-lookup"><span data-stu-id="c4190-138">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="c4190-139">O EF fornece um método de extensão para tornar isso mais fácil `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="c4190-139">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="c4190-140">Esse método começa e confirma uma transação e também aceita uma função no parâmetro `verifySucceeded` que é invocado quando ocorre um erro transitório durante a confirmação da transação.</span><span class="sxs-lookup"><span data-stu-id="c4190-140">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="c4190-141">Aqui `SaveChanges` é invocado com `acceptAllChangesOnSuccess` definido como `false` para evitar a alteração do estado da entidade `Blog` para `Unchanged` se `SaveChanges` tiver sucesso.</span><span class="sxs-lookup"><span data-stu-id="c4190-141">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="c4190-142">Isso permite repetir a mesma operação se a confirmação falhar e a transação for revertida.</span><span class="sxs-lookup"><span data-stu-id="c4190-142">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="c4190-143">Opção 4-rastrear manualmente a transação</span><span class="sxs-lookup"><span data-stu-id="c4190-143">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="c4190-144">Se você precisar usar chaves geradas pelo repositório ou precisar de uma maneira genérica de lidar com falhas de confirmação que não dependem da operação executada, cada transação poderá receber uma ID verificada quando a confirmação falhar.</span><span class="sxs-lookup"><span data-stu-id="c4190-144">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="c4190-145">Adicione uma tabela ao banco de dados usado para acompanhar o status das transações.</span><span class="sxs-lookup"><span data-stu-id="c4190-145">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="c4190-146">Insira uma linha na tabela no início de cada transação.</span><span class="sxs-lookup"><span data-stu-id="c4190-146">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="c4190-147">Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c4190-147">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="c4190-148">Se a confirmação for bem-sucedida, exclua a linha correspondente para evitar o crescimento da tabela.</span><span class="sxs-lookup"><span data-stu-id="c4190-148">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="c4190-149">Certifique-se de que o contexto usado para a verificação tenha uma estratégia de execução definida, pois a conexão provavelmente falhará novamente durante a verificação se ela falhar durante a confirmação da transação.</span><span class="sxs-lookup"><span data-stu-id="c4190-149">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
