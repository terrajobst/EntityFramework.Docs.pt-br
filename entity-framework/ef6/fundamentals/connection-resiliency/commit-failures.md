---
title: Tratamento de falhas de confirmação de transação-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417366"
---
# <a name="handling-transaction-commit-failures"></a>Tratamento de falhas de confirmação de transação
> [!NOTE]
> **Somente EF 6.1 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 6,1. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Como parte do 6,1, estamos introduzindo um novo recurso de resiliência de conexão para o EF: a capacidade de detectar e recuperar automaticamente quando falhas de conexão transitórias afetam a confirmação de confirmações de transação. Os detalhes completos do cenário são mais bem descritos na postagem do blog [conectividade do banco de dados SQL e o problema de Idempotência](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  Em suma, o cenário é que quando uma exceção é gerada durante uma confirmação de transação, há duas causas possíveis:  

1. A confirmação da transação falhou no servidor
2. A confirmação da transação foi bem-sucedida no servidor, mas um problema de conectividade impediu que a notificação de êxito chegasse ao cliente  

Quando a primeira situação ocorre no aplicativo ou o usuário pode repetir a operação, mas quando a segunda situação ocorre, as tentativas devem ser evitadas e o aplicativo pode ser recuperado automaticamente. O desafio é que, sem a capacidade de detectar qual foi o motivo real de uma exceção ser relatada durante a confirmação, o aplicativo não pode escolher o curso certo de ação. O novo recurso do EF 6,1 permite que o EF Verifique com o banco de dados se a transação foi bem-sucedida e faça o curso certo de ação de forma transparente.  

## <a name="using-the-feature"></a>Usar o recurso  

Para habilitar o recurso, você precisa incluir uma chamada para [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) no construtor de seu **DbConfiguration**. Se você não estiver familiarizado com o **DbConfiguration**, consulte [Configuração baseada em código](~/ef6/fundamentals/configuring/code-based.md). Esse recurso pode ser usado em combinação com as repetições automáticas que introduzimos no EF6, que ajudam na situação em que a transação realmente não pôde ser confirmada no servidor devido a uma falha transitória:  

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

Quando o recurso estiver habilitado, o EF adicionará automaticamente uma nova tabela ao banco de dados chamada **__Transactions**. Uma nova linha é inserida nessa tabela toda vez que uma transação é criada pelo EF e essa linha é verificada quanto à existência se ocorrer uma falha de transação durante a confirmação.  

Embora o EF faça um melhor esforço para remover linhas da tabela quando elas não são mais necessárias, a tabela pode aumentar se o aplicativo sair prematuramente e, por esse motivo, talvez seja necessário limpar a tabela manualmente em alguns casos.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Como lidar com falhas de confirmação com versões anteriores

Antes do EF 6,1, não havia um mecanismo para lidar com falhas de confirmação no produto EF. Há várias maneiras de lidar com essa situação que pode ser aplicada às versões anteriores do EF6:  

* Opção 1-não fazer nada  

  A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição realmente ocorrer.  

* Opção 2 – usar o banco de dados para redefinir o estado  

  1. Descartar o DbContext atual  
  2. Criar um novo DbContext e restaurar o estado do seu aplicativo do banco de dados  
  3. Informar ao usuário que a última operação pode não ter sido concluída com êxito  

* Opção 3-rastrear manualmente a transação  

  1. Adicione uma tabela não controlada ao banco de dados usado para acompanhar o status das transações.  
  2. Insira uma linha na tabela no início de cada transação.  
  3. Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.  
     - Se a linha estiver presente, continue normalmente, pois a transação foi confirmada com êxito  
     - Se a linha estiver ausente, use uma estratégia de execução para repetir a operação atual.  
  4. Se a confirmação for bem-sucedida, exclua a linha correspondente para evitar o crescimento da tabela.  

[Esta postagem de blog](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contém o código de exemplo para realizar isso em SQL Azure.  
