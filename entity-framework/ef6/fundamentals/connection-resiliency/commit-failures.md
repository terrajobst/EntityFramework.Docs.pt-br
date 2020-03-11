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
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="68e9b-102">Tratamento de falhas de confirmação de transação</span><span class="sxs-lookup"><span data-stu-id="68e9b-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="68e9b-103">**Somente EF 6.1 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="68e9b-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="68e9b-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="68e9b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="68e9b-105">Como parte do 6,1, estamos introduzindo um novo recurso de resiliência de conexão para o EF: a capacidade de detectar e recuperar automaticamente quando falhas de conexão transitórias afetam a confirmação de confirmações de transação.</span><span class="sxs-lookup"><span data-stu-id="68e9b-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="68e9b-106">Os detalhes completos do cenário são mais bem descritos na postagem do blog [conectividade do banco de dados SQL e o problema de Idempotência](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="68e9b-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="68e9b-107">Em suma, o cenário é que quando uma exceção é gerada durante uma confirmação de transação, há duas causas possíveis:</span><span class="sxs-lookup"><span data-stu-id="68e9b-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="68e9b-108">A confirmação da transação falhou no servidor</span><span class="sxs-lookup"><span data-stu-id="68e9b-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="68e9b-109">A confirmação da transação foi bem-sucedida no servidor, mas um problema de conectividade impediu que a notificação de êxito chegasse ao cliente</span><span class="sxs-lookup"><span data-stu-id="68e9b-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="68e9b-110">Quando a primeira situação ocorre no aplicativo ou o usuário pode repetir a operação, mas quando a segunda situação ocorre, as tentativas devem ser evitadas e o aplicativo pode ser recuperado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="68e9b-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="68e9b-111">O desafio é que, sem a capacidade de detectar qual foi o motivo real de uma exceção ser relatada durante a confirmação, o aplicativo não pode escolher o curso certo de ação.</span><span class="sxs-lookup"><span data-stu-id="68e9b-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="68e9b-112">O novo recurso do EF 6,1 permite que o EF Verifique com o banco de dados se a transação foi bem-sucedida e faça o curso certo de ação de forma transparente.</span><span class="sxs-lookup"><span data-stu-id="68e9b-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="68e9b-113">Usar o recurso</span><span class="sxs-lookup"><span data-stu-id="68e9b-113">Using the feature</span></span>  

<span data-ttu-id="68e9b-114">Para habilitar o recurso, você precisa incluir uma chamada para [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) no construtor de seu **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="68e9b-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="68e9b-115">Se você não estiver familiarizado com o **DbConfiguration**, consulte [Configuração baseada em código](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="68e9b-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="68e9b-116">Esse recurso pode ser usado em combinação com as repetições automáticas que introduzimos no EF6, que ajudam na situação em que a transação realmente não pôde ser confirmada no servidor devido a uma falha transitória:</span><span class="sxs-lookup"><span data-stu-id="68e9b-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

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

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="68e9b-117">Como as transações são controladas</span><span class="sxs-lookup"><span data-stu-id="68e9b-117">How transactions are tracked</span></span>  

<span data-ttu-id="68e9b-118">Quando o recurso estiver habilitado, o EF adicionará automaticamente uma nova tabela ao banco de dados chamada **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="68e9b-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="68e9b-119">Uma nova linha é inserida nessa tabela toda vez que uma transação é criada pelo EF e essa linha é verificada quanto à existência se ocorrer uma falha de transação durante a confirmação.</span><span class="sxs-lookup"><span data-stu-id="68e9b-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="68e9b-120">Embora o EF faça um melhor esforço para remover linhas da tabela quando elas não são mais necessárias, a tabela pode aumentar se o aplicativo sair prematuramente e, por esse motivo, talvez seja necessário limpar a tabela manualmente em alguns casos.</span><span class="sxs-lookup"><span data-stu-id="68e9b-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="68e9b-121">Como lidar com falhas de confirmação com versões anteriores</span><span class="sxs-lookup"><span data-stu-id="68e9b-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="68e9b-122">Antes do EF 6,1, não havia um mecanismo para lidar com falhas de confirmação no produto EF.</span><span class="sxs-lookup"><span data-stu-id="68e9b-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="68e9b-123">Há várias maneiras de lidar com essa situação que pode ser aplicada às versões anteriores do EF6:</span><span class="sxs-lookup"><span data-stu-id="68e9b-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="68e9b-124">Opção 1-não fazer nada</span><span class="sxs-lookup"><span data-stu-id="68e9b-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="68e9b-125">A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição realmente ocorrer.</span><span class="sxs-lookup"><span data-stu-id="68e9b-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="68e9b-126">Opção 2 – usar o banco de dados para redefinir o estado</span><span class="sxs-lookup"><span data-stu-id="68e9b-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="68e9b-127">Descartar o DbContext atual</span><span class="sxs-lookup"><span data-stu-id="68e9b-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="68e9b-128">Criar um novo DbContext e restaurar o estado do seu aplicativo do banco de dados</span><span class="sxs-lookup"><span data-stu-id="68e9b-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="68e9b-129">Informar ao usuário que a última operação pode não ter sido concluída com êxito</span><span class="sxs-lookup"><span data-stu-id="68e9b-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="68e9b-130">Opção 3-rastrear manualmente a transação</span><span class="sxs-lookup"><span data-stu-id="68e9b-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="68e9b-131">Adicione uma tabela não controlada ao banco de dados usado para acompanhar o status das transações.</span><span class="sxs-lookup"><span data-stu-id="68e9b-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="68e9b-132">Insira uma linha na tabela no início de cada transação.</span><span class="sxs-lookup"><span data-stu-id="68e9b-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="68e9b-133">Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="68e9b-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="68e9b-134">Se a linha estiver presente, continue normalmente, pois a transação foi confirmada com êxito</span><span class="sxs-lookup"><span data-stu-id="68e9b-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="68e9b-135">Se a linha estiver ausente, use uma estratégia de execução para repetir a operação atual.</span><span class="sxs-lookup"><span data-stu-id="68e9b-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="68e9b-136">Se a confirmação for bem-sucedida, exclua a linha correspondente para evitar o crescimento da tabela.</span><span class="sxs-lookup"><span data-stu-id="68e9b-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="68e9b-137">[Esta postagem de blog](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contém o código de exemplo para realizar isso em SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="68e9b-137">[This blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
