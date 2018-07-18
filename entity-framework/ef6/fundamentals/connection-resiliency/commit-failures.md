---
title: Tratamento de falhas de confirmação de transação - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
caps.latest.revision: 3
ms.openlocfilehash: 95ac69a005a70f31086bd4d3c0088bd3e82d28e2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39119893"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="a1baf-102">Tratamento de falhas de confirmação de transação</span><span class="sxs-lookup"><span data-stu-id="a1baf-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="a1baf-103">**EF6.1 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="a1baf-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="a1baf-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="a1baf-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="a1baf-105">Como parte de um 6.1, introduzimos um novo recurso de resiliência de conexão para o EF: a capacidade de detectar e recuperar automaticamente quando as falhas de conexão transitórias afetam a confirmação de confirmações de transações.</span><span class="sxs-lookup"><span data-stu-id="a1baf-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="a1baf-106">Os detalhes completos do cenário são mais bem descritos na postagem do blog [conectividade de banco de dados SQL e o problema de idempotência](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="a1baf-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="a1baf-107">Em resumo, o cenário é que, quando uma exceção é gerada durante confirmação de uma transação há duas causas possíveis:</span><span class="sxs-lookup"><span data-stu-id="a1baf-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="a1baf-108">A confirmação de transação que falhou no servidor</span><span class="sxs-lookup"><span data-stu-id="a1baf-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="a1baf-109">A confirmação de transação foi bem-sucedida no servidor, mas um problema de conectividade impediu a notificação de êxito de atingir o cliente</span><span class="sxs-lookup"><span data-stu-id="a1baf-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="a1baf-110">Quando a primeira situação acontece o aplicativo ou o usuário poderá repetir a operação, mas quando a segunda situação ocorre novas tentativas devem ser evitadas e o aplicativo pode se recuperar automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a1baf-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="a1baf-111">O desafio é que, sem a capacidade de detectar qual era o real motivo pelo qual uma exceção foi relatado durante a confirmação, o aplicativo não é possível escolher o certo curso de ação.</span><span class="sxs-lookup"><span data-stu-id="a1baf-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="a1baf-112">O novo recurso no EF 6.1 permite que o EF Verifique novamente com o banco de dados se a transação foi bem-sucedida e faça o curso certo da ação de forma transparente.</span><span class="sxs-lookup"><span data-stu-id="a1baf-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="a1baf-113">Usando o recurso</span><span class="sxs-lookup"><span data-stu-id="a1baf-113">Using the feature</span></span>  

<span data-ttu-id="a1baf-114">Para habilitar o recurso, você precisa incluir uma chamada para [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) no construtor da sua **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="a1baf-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="a1baf-115">Se você estiver familiarizado com **DbConfiguration**, consulte [configuração do código com base em](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="a1baf-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="a1baf-116">Esse recurso pode ser usado em combinação com as novas tentativas automáticas que introduzimos no EF6, que ajudam na situação em que a transação, na verdade, não foi confirmada no servidor devido a uma falha transitória:</span><span class="sxs-lookup"><span data-stu-id="a1baf-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

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

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="a1baf-117">Como as transações são controladas</span><span class="sxs-lookup"><span data-stu-id="a1baf-117">How transactions are tracked</span></span>  

<span data-ttu-id="a1baf-118">Quando o recurso está habilitado, o EF adicionará automaticamente uma nova tabela no banco de dados chamado **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="a1baf-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="a1baf-119">Uma nova linha é inserida nessa tabela sempre que uma transação é criada pelo EF e que a linha é verificada quanto a existência se ocorrer uma falha de transação durante a confirmação.</span><span class="sxs-lookup"><span data-stu-id="a1baf-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="a1baf-120">Embora o EF fará o melhor para remover linhas da tabela, quando eles não são mais necessários, a tabela pode crescer se o aplicativo é encerrado prematuramente e por esse motivo, que talvez seja necessário limpar a tabela manualmente em alguns casos.</span><span class="sxs-lookup"><span data-stu-id="a1baf-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="a1baf-121">Como lidar com falhas de confirmação com versões anteriores</span><span class="sxs-lookup"><span data-stu-id="a1baf-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="a1baf-122">Antes do EF 6.1 não havia mecanismo para lidar com falhas de confirmação no produto EF.</span><span class="sxs-lookup"><span data-stu-id="a1baf-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="a1baf-123">Há várias maneiras de lidar com essa situação que pode ser aplicada a versões anteriores do EF6:</span><span class="sxs-lookup"><span data-stu-id="a1baf-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

### <a name="option-1---do-nothing"></a><span data-ttu-id="a1baf-124">Opção 1 - fazer nada</span><span class="sxs-lookup"><span data-stu-id="a1baf-124">Option 1 - Do nothing</span></span>  

<span data-ttu-id="a1baf-125">A probabilidade de uma falha de conexão durante a confirmação da transação é baixa, portanto, pode ser aceitável para o aplicativo falhar apenas se essa condição ocorrer, na verdade.</span><span class="sxs-lookup"><span data-stu-id="a1baf-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

## <a name="option-2---use-the-database-to-reset-state"></a><span data-ttu-id="a1baf-126">Opção 2 – usar o banco de dados para redefinir o estado</span><span class="sxs-lookup"><span data-stu-id="a1baf-126">Option 2 - Use the database to reset state</span></span>  

1. <span data-ttu-id="a1baf-127">Descartar o DbContext atual</span><span class="sxs-lookup"><span data-stu-id="a1baf-127">Discard the current DbContext</span></span>  
2. <span data-ttu-id="a1baf-128">Criar um novo DbContext e restaurar o estado do seu aplicativo do banco de dados</span><span class="sxs-lookup"><span data-stu-id="a1baf-128">Create a new DbContext and restore the state of your application from the database</span></span>  
3. <span data-ttu-id="a1baf-129">Informar ao usuário que a última operação talvez não tenham sido concluída com êxito</span><span class="sxs-lookup"><span data-stu-id="a1baf-129">Inform the user that the last operation might not have been completed successfully</span></span>  

## <a name="option-3---manually-track-the-transaction"></a><span data-ttu-id="a1baf-130">Opção 3 - controlar manualmente a transação</span><span class="sxs-lookup"><span data-stu-id="a1baf-130">Option 3 - Manually track the transaction</span></span>  

1. <span data-ttu-id="a1baf-131">Adicione uma tabela de não rastreados no banco de dados usado para acompanhar o status das transações.</span><span class="sxs-lookup"><span data-stu-id="a1baf-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
2. <span data-ttu-id="a1baf-132">Inserir uma linha na tabela no início de cada transação.</span><span class="sxs-lookup"><span data-stu-id="a1baf-132">Insert a row into the table at the beginning of each transaction.</span></span>  
3. <span data-ttu-id="a1baf-133">Se a conexão falhar durante a confirmação, verifique a presença da linha correspondente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a1baf-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
    - <span data-ttu-id="a1baf-134">Se a linha estiver presente, continue normalmente, conforme a transação foi confirmada com êxito</span><span class="sxs-lookup"><span data-stu-id="a1baf-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
    - <span data-ttu-id="a1baf-135">Se a linha estiver ausente, use uma estratégia de execução para repetir a operação atual.</span><span class="sxs-lookup"><span data-stu-id="a1baf-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
4. <span data-ttu-id="a1baf-136">Se a confirmação for bem-sucedida, exclua a linha correspondente para evitar o crescimento da tabela.</span><span class="sxs-lookup"><span data-stu-id="a1baf-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="a1baf-137">[Esta postagem de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contém código de exemplo para realizar isso no SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="a1baf-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
