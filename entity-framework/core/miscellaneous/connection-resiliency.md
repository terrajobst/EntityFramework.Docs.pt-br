---
title: Resiliência de Conexão - Core EF
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053526"
---
# <a name="connection-resiliency"></a>Resiliência da conexão

Resiliência de Conexão repete automaticamente os comandos de banco de dados com falha. O recurso pode ser usado com qualquer banco de dados, fornecendo uma "estratégia de execução," que encapsula a lógica necessária para detectar falhas e tente novamente comandos. Provedores de núcleo EF podem fornecer estratégias de execução personalizadas para suas condições de falha do banco de dados específico e políticas de repetição ideal.

Por exemplo, o provedor SQL Server inclui uma estratégia de execução foi especificamente projetada para o SQL Server (incluindo o SQL Azure). Ele está ciente dos tipos de exceção que podem ser repetidos e tem a sensatos padrões para o número máximo de tentativas, atraso entre repetições, etc.

Uma estratégia de execução for especificada ao configurar as opções para o contexto. Isso é normalmente no `OnConfiguring` método de seu contexto derivado ou no `Startup.cs` para um aplicativo ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Estratégia de execução personalizado

Há um mecanismo para registrar uma estratégia personalizada de execução de sua preferência, se você quiser alterar qualquer um dos padrões.

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

Uma estratégia de execução repete automaticamente em caso de falha deve ser capaz de executar cada operação em um bloco de repetição que falha. Quando novas tentativas são habilitadas, cada operação executar via EF Core torna-se sua própria operação repetível, ou seja, cada consulta e cada chamada para `SaveChanges()` será repetida como uma unidade se ocorrer uma falha temporária.

No entanto, se seu código inicia uma transação usando `BeginTransaction()` você está definindo seu próprio grupo de operações que precisam ser tratados como uma unidade, ou seja, tudo dentro da transação deve ser reproduzida deverá ocorrer uma falha. Se você tentar fazer isso ao usar uma estratégia de execução, você receberá uma exceção semelhante à seguinte.

> InvalidOperationException: A estratégia de execução configurada 'SqlServerRetryingExecutionStrategy' não oferece suporte a transações iniciadas pelo usuário. Use a estratégia de execução retornada por 'DbContext.Database.CreateExecutionStrategy()' para executar todas as operações na transação como uma unidade repetível.

A solução é chamar manualmente a estratégia de execução com um delegado que representa tudo o que precisa ser executada. Se ocorrer uma falha transitória, a estratégia de execução invocará o representante novamente.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Falha de confirmação de transação e o problema de idempotência

Em geral, quando há uma falha de conexão a transação atual será revertida. No entanto, se a conexão for descartada enquanto a transação está sendo confirmada resultante estado da transação é desconhecido. Consulte este [postagem de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) para obter mais detalhes.

Por padrão, a estratégia de execução tentará repetir a operação como se a transação foi revertida, mas se não for o caso isso resultará em uma exceção se o novo estado do banco de dados é incompatível ou pode levar a **corrupção de dados** se a operação não se baseia em um estado específico, por exemplo, ao inserir uma nova linha com valores de chave gerada automaticamente.

Há várias maneiras de lidar com isso.

### <a name="option-1---do-almost-nothing"></a>Opção 1 - fazer nada (quase)

A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição ocorrer, na verdade.

No entanto, você precisa evitar o uso de chaves geradas pelo repositório para garantir que uma exceção será lançada em vez de adicionar uma linha duplicada. Considere usar um valor GUID gerado pelo cliente ou um gerador do valor do lado do cliente.

### <a name="option-2---rebuild-application-state"></a>Opção 2 - reconstruir o estado do aplicativo

1. Descartar atual `DbContext`.
2. Criar um novo `DbContext` e restaurar o estado do seu aplicativo do banco de dados.
3. Informe ao usuário que a última operação pode não ter sido concluída com êxito.

### <a name="option-3---add-state-verification"></a>Opção 3 - adicionar estado de verificação

Para a maioria das operações que alteram o estado do banco de dados, é possível adicionar o código que verifica se ela foi bem-sucedida. EF fornece um método de extensão para facilitar essa tarefa - `IExecutionStrategy.ExecuteInTransaction`.

Esse método inicia e confirma uma transação e também aceita uma função no `verifySucceeded` parâmetro que é invocado quando ocorre um erro temporário durante a confirmação da transação.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Aqui `SaveChanges` é invocado com `acceptAllChangesOnSuccess` definida como `false` para evitar alterar o estado do `Blog` entidade `Unchanged` se `SaveChanges` for bem-sucedida. Isso permite que repita a operação mesmo se houver uma falha e a transação será revertida.

### <a name="option-4---manually-track-the-transaction"></a>Opção 4 - controlar manualmente a transação

Se você precisar usar chaves geradas pelo repositório ou precisam de um modo genérico de tratamento de falhas de confirmação que não depende da operação executada cada transação pôde ser atribuída a uma ID que é verificada quando a confirmação falhará.

1. Adicione uma tabela no banco de dados usado para acompanhar o status das transações.
2. Inserir uma linha na tabela no início de cada transação.
3. Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.
4. Se a confirmação for bem-sucedido, exclua a linha correspondente para evitar o crescimento da tabela.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Certifique-se de que o contexto usado para a verificação tiver uma estratégia de execução definida como a conexão é provavelmente falharão novamente durante a verificação se ela teve falha durante a confirmação da transação.
