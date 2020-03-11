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
# <a name="connection-resiliency"></a>Resiliência da conexão

A resiliência de conexão repete automaticamente os comandos de banco de dados com falha. O recurso pode ser usado com qualquer banco de dados fornecendo uma "estratégia de execução", que encapsula a lógica necessária para detectar falhas e repetir comandos. Os provedores de EF Core podem fornecer estratégias de execução adaptadas às suas condições específicas de falha do banco de dados e às políticas de repetição ideais.

Por exemplo, o provedor de SQL Server inclui uma estratégia de execução que é especificamente adaptada para SQL Server (incluindo SQL Azure). Ele reconhece os tipos de exceção que podem ser repetidos e tem padrões sensíveis para tentativas máximas, atraso entre repetições, etc.

Uma estratégia de execução é especificada ao configurar as opções para seu contexto. Normalmente, isso está no método `OnConfiguring` do contexto derivado:

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

ou em `Startup.cs` para um aplicativo ASP.NET Core:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a>Estratégia de execução personalizada

Há um mecanismo para registrar uma estratégia de execução personalizada de sua preferência se você quiser alterar qualquer um dos padrões.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Estratégias e transações de execução

Uma estratégia de execução que repete automaticamente as falhas precisa ser capaz de reproduzir cada operação em um bloco de repetição que falha. Quando novas tentativas são habilitadas, cada operação executada por meio de EF Core se torna sua própria operação passível. Ou seja, cada consulta e cada chamada para `SaveChanges()` será repetida como uma unidade se ocorrer uma falha transitória.

No entanto, se o seu código iniciar uma transação usando `BeginTransaction()` você estiver definindo seu próprio grupo de operações que precisam ser tratadas como uma unidade, e tudo dentro da transação precisará ser reproduzido caso ocorra uma falha. Você receberá uma exceção como a seguinte se tentar fazer isso ao usar uma estratégia de execução:

> InvalidOperationException: a estratégia de execução configurada ' SqlServerRetryingExecutionStrategy ' não oferece suporte a transações iniciadas pelo usuário. Use a estratégia de execução retornada por 'DbContext.Database.CreateExecutionStrategy()' para executar todas as operações na transação como uma unidade repetível.

A solução é invocar manualmente a estratégia de execução com um delegado representando tudo o que precisa ser executado. Se ocorrer uma falha transitória, a estratégia de execução invocará o representante novamente.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

Essa abordagem também pode ser usada com transações de ambiente.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Falha na confirmação da transação e o problema de Idempotência

Em geral, quando há uma falha de conexão, a transação atual é revertida. No entanto, se a conexão for descartada enquanto a transação estiver sendo confirmada, o estado resultante da transação será desconhecido. 

Por padrão, a estratégia de execução tentará novamente a operação como se a transação fosse revertida, mas se não for o caso, isso resultará em uma exceção se o novo estado do banco de dados for incompatível ou puder levar à **corrupção de dados** se a operação não depender de um estado específico, por exemplo, ao inserir uma nova linha com valores de chave gerados automaticamente.

Há várias maneiras de lidar com isso.

### <a name="option-1---do-almost-nothing"></a>Opção 1-fazer (quase) nada

A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição realmente ocorrer.

No entanto, você precisa evitar o uso de chaves geradas pela loja para garantir que uma exceção seja lançada em vez de adicionar uma linha duplicada. Considere usar um valor GUID gerado pelo cliente ou um gerador de valor do lado do cliente.

### <a name="option-2---rebuild-application-state"></a>Opção 2 – recriar o estado do aplicativo

1. Descartar a `DbContext`atual.
2. Crie um novo `DbContext` e restaure o estado do seu aplicativo a partir do banco de dados.
3. Informe ao usuário que a última operação pode não ter sido concluída com êxito.

### <a name="option-3---add-state-verification"></a>Opção 3 – adicionar verificação de estado

Para a maioria das operações que alteram o estado do banco de dados, é possível adicionar código que verifica se foi bem-sucedido. O EF fornece um método de extensão para tornar isso mais fácil `IExecutionStrategy.ExecuteInTransaction`.

Esse método começa e confirma uma transação e também aceita uma função no parâmetro `verifySucceeded` que é invocado quando ocorre um erro transitório durante a confirmação da transação.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Aqui `SaveChanges` é invocado com `acceptAllChangesOnSuccess` definido como `false` para evitar a alteração do estado da entidade `Blog` para `Unchanged` se `SaveChanges` tiver sucesso. Isso permite repetir a mesma operação se a confirmação falhar e a transação for revertida.

### <a name="option-4---manually-track-the-transaction"></a>Opção 4-rastrear manualmente a transação

Se você precisar usar chaves geradas pelo repositório ou precisar de uma maneira genérica de lidar com falhas de confirmação que não dependem da operação executada, cada transação poderá receber uma ID verificada quando a confirmação falhar.

1. Adicione uma tabela ao banco de dados usado para acompanhar o status das transações.
2. Insira uma linha na tabela no início de cada transação.
3. Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.
4. Se a confirmação for bem-sucedida, exclua a linha correspondente para evitar o crescimento da tabela.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Certifique-se de que o contexto usado para a verificação tenha uma estratégia de execução definida, pois a conexão provavelmente falhará novamente durante a verificação se ela falhar durante a confirmação da transação.
