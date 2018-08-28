---
title: Tratamento de falhas de confirmação de transação - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: a22a651851bc46e2bf1fe652b3b9a921ec22b70b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996831"
---
# <a name="handling-transaction-commit-failures"></a>Tratamento de falhas de confirmação de transação
> [!NOTE]
> **EF6.1 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 6.1. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Como parte de um 6.1, introduzimos um novo recurso de resiliência de conexão para o EF: a capacidade de detectar e recuperar automaticamente quando as falhas de conexão transitórias afetam a confirmação de confirmações de transações. Os detalhes completos do cenário são mais bem descritos na postagem do blog [conectividade de banco de dados SQL e o problema de idempotência](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  Em resumo, o cenário é que, quando uma exceção é gerada durante confirmação de uma transação há duas causas possíveis:  

1. A confirmação de transação que falhou no servidor
2. A confirmação de transação foi bem-sucedida no servidor, mas um problema de conectividade impediu a notificação de êxito de atingir o cliente  

Quando a primeira situação acontece o aplicativo ou o usuário poderá repetir a operação, mas quando a segunda situação ocorre novas tentativas devem ser evitadas e o aplicativo pode se recuperar automaticamente. O desafio é que, sem a capacidade de detectar qual era o real motivo pelo qual uma exceção foi relatado durante a confirmação, o aplicativo não é possível escolher o certo curso de ação. O novo recurso no EF 6.1 permite que o EF Verifique novamente com o banco de dados se a transação foi bem-sucedida e faça o curso certo da ação de forma transparente.  

## <a name="using-the-feature"></a>Usando o recurso  

Para habilitar o recurso, você precisa incluir uma chamada para [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) no construtor da sua **DbConfiguration**. Se você estiver familiarizado com **DbConfiguration**, consulte [configuração do código com base em](~/ef6/fundamentals/configuring/code-based.md). Esse recurso pode ser usado em combinação com as novas tentativas automáticas que introduzimos no EF6, que ajudam na situação em que a transação, na verdade, não foi confirmada no servidor devido a uma falha transitória:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a>Como as transações são controladas  

Quando o recurso está habilitado, o EF adicionará automaticamente uma nova tabela no banco de dados chamado **__Transactions**. Uma nova linha é inserida nessa tabela sempre que uma transação é criada pelo EF e que a linha é verificada quanto a existência se ocorrer uma falha de transação durante a confirmação.  

Embora o EF fará o melhor para remover linhas da tabela, quando eles não são mais necessários, a tabela pode crescer se o aplicativo é encerrado prematuramente e por esse motivo, que talvez seja necessário limpar a tabela manualmente em alguns casos.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Como lidar com falhas de confirmação com versões anteriores

Antes do EF 6.1 não havia mecanismo para lidar com falhas de confirmação no produto EF. Há várias maneiras de lidar com essa situação que pode ser aplicada a versões anteriores do EF6:  

### <a name="option-1---do-nothing"></a>Opção 1 - fazer nada  

A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição ocorrer, na verdade.  

## <a name="option-2---use-the-database-to-reset-state"></a>Opção 2 – usar o banco de dados para redefinir o estado  

1. Descartar o DbContext atual  
2. Criar um novo DbContext e restaurar o estado do seu aplicativo do banco de dados  
3. Informar ao usuário que a última operação talvez não tenham sido concluída com êxito  

## <a name="option-3---manually-track-the-transaction"></a>Opção 3 - controlar manualmente a transação  

1. Adicione uma tabela de não rastreados no banco de dados usado para acompanhar o status das transações.  
2. Inserir uma linha na tabela no início de cada transação.  
3. Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.  
    - Se a linha estiver presente, continue normalmente, conforme a transação foi confirmada com êxito  
    - Se a linha estiver ausente, use uma estratégia de execução para repetir a operação atual.  
4. Se a confirmação for bem-sucedida, exclua a linha correspondente para evitar o crescimento da tabela.  

[Esta postagem de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contém código de exemplo para realizar isso no SQL Azure.  
