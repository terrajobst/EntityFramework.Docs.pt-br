---
title: Resiliência de Conexão – EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: d5101d0622ddc2c90ddded16b9ec6cc4eb814c36
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283830"
---
# <a name="connection-resiliency"></a>Resiliência da conexão

Resiliência de Conexão automaticamente repete comandos de banco de dados com falha. O recurso pode ser usado com qualquer banco de dados, fornecendo uma "estratégia de execução," que encapsula a lógica necessária para detectar falhas e repetir comandos. Provedores do EF Core podem fornecer estratégias de execução adaptadas às suas condições de falha do banco de dados específico e as políticas de repetição ideal.

Por exemplo, o provedor SQL Server inclui uma estratégia de execução especificamente projetado para o SQL Server (incluindo SQL Azure). Ele está ciente dos tipos de exceção que podem ser repetidos e tem padrões específicos para o número máximo de tentativas, atraso entre as repetições, etc.

Uma estratégia de execução é especificada ao configurar as opções para seu contexto. Isso é normalmente na `OnConfiguring` método de seu contexto derivado ou no `Startup.cs` para um aplicativo ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Estratégia de execução personalizado

Há um mecanismo para registrar uma estratégia de execução personalizado de sua preferência, se você quiser alterar qualquer um dos padrões.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Transações e estratégias de execução

Uma estratégia de execução repete automaticamente em falhas precisa ser capaz de reproduzir cada operação em um bloco de repetição que falha. Quando novas tentativas são habilitadas, cada operação executada através do EF Core se torna sua própria operação repetível. Ou seja, cada consulta e cada chamada para `SaveChanges()` serão repetidas como uma unidade se ocorrer uma falha temporária.

No entanto, se seu código inicia uma transação usando `BeginTransaction()` você está definindo seu próprio grupo de operações que precisam ser tratados como uma unidade, e tudo dentro da transação precisarão ser reproduzidos deverá ocorrer de uma falha. Se você tentar fazer isso usando uma estratégia de execução, você receberá uma exceção semelhante ao seguinte:

> InvalidOperationException: A estratégia de execução configurada 'SqlServerRetryingExecutionStrategy' não dá suporte a transações iniciadas pelo usuário. Use a estratégia de execução retornada por 'DbContext.Database.CreateExecutionStrategy()' para executar todas as operações na transação como uma unidade repetível.

A solução é invocar manualmente a estratégia de execução com um delegado que representa tudo que precisa para ser executado. Se ocorrer uma falha temporária, a estratégia de execução invocará o delegado novamente.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Falha de confirmação de transação e o problema de idempotência

Em geral, quando houver uma falha de conexão a transação atual é revertida. No entanto, se a conexão for descartada enquanto a transação está sendo confirmada resultante estado da transação é desconhecido. Consulte este [postagem de blog](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) para obter mais detalhes.

Por padrão, a estratégia de execução tentará repetir a operação como se a transação foi revertida, mas se não for o caso isso resultará em uma exceção se o novo estado do banco de dados é incompatível ou pode levar à **corrupção de dados** se a operação não se baseia em um estado específico, por exemplo, ao inserir uma nova linha com valores de chave gerada automaticamente.

Há várias maneiras de lidar com isso.

### <a name="option-1---do-almost-nothing"></a>Opção 1 - fazer nada (quase)

A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição ocorrer, na verdade.

No entanto, você precisa evitar o uso de chaves geradas pelo repositório para garantir que uma exceção é gerada em vez de adicionar uma linha duplicada. Considere o uso de um valor GUID gerado pelo cliente ou um gerador de valor do lado do cliente.

### <a name="option-2---rebuild-application-state"></a>Opção 2 - estado de recompilação do aplicativo

1. Descartar atual `DbContext`.
2. Criar um novo `DbContext` e restaurar o estado do seu aplicativo do banco de dados.
3. Informar ao usuário que a última operação talvez não tenham sido concluída com êxito.

### <a name="option-3---add-state-verification"></a>Opção 3 - adicionar verificação de estado

Para a maioria das operações que alteram o estado do banco de dados, é possível adicionar código que verifica se ela foi bem-sucedida. O EF fornece um método de extensão para facilitar essa tarefa - `IExecutionStrategy.ExecuteInTransaction`.

Esse método inicia e confirma uma transação e também aceita uma função no `verifySucceeded` parâmetro que é invocado quando ocorre um erro transitório durante a confirmação da transação.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Aqui `SaveChanges` é invocado com `acceptAllChangesOnSuccess` definido como `false` para evitar a alteração do estado da `Blog` entidade `Unchanged` se `SaveChanges` for bem-sucedida. Isso permite que repetir a mesma operação, se houver uma falha e a transação é revertida.

### <a name="option-4---manually-track-the-transaction"></a>Opção 4 - controlar manualmente a transação

Se você precisar usar chaves geradas pelo repositório ou precisa de uma maneira genérica de lidar com falhas de confirmação que não depende da operação executada cada transação podiam ser atribuída a uma ID que é verificada durante a confirmação falhará.

1. Adicione uma tabela no banco de dados usado para acompanhar o status das transações.
2. Inserir uma linha na tabela no início de cada transação.
3. Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.
4. Se a confirmação for bem-sucedida, exclua a linha correspondente para evitar o crescimento da tabela.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Certifique-se de que o contexto usado para a verificação tem uma estratégia de execução definida como a conexão é a probabilidade de falhar novamente durante a verificação se falhou durante a confirmação da transação.
