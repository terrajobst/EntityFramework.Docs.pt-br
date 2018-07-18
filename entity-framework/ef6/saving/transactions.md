---
title: Trabalhando com transações - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
caps.latest.revision: 3
ms.openlocfilehash: 4238c88cc149458ed11b96a0bf9aaed9aac40b2d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120099"
---
# <a name="working-with-transactions"></a><span data-ttu-id="c45db-102">Trabalhando com transações</span><span class="sxs-lookup"><span data-stu-id="c45db-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="c45db-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c45db-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c45db-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="c45db-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c45db-105">Este documento descreverá o uso de transações no EF6, incluindo os aprimoramentos que adicionamos desde o EF5 para facilitar o trabalho com transações.</span><span class="sxs-lookup"><span data-stu-id="c45db-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="c45db-106">O que o EF faz por padrão</span><span class="sxs-lookup"><span data-stu-id="c45db-106">What EF does by default</span></span>  

<span data-ttu-id="c45db-107">Todas as versões do Entity Framework, sempre que você executar **SaveChanges ()** inserir, atualizar ou excluir, no banco de dados, o framework irá encapsular essa operação em uma transação.</span><span class="sxs-lookup"><span data-stu-id="c45db-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="c45db-108">Esta transação dura apenas por tempo suficiente para executar a operação e, em seguida, conclui.</span><span class="sxs-lookup"><span data-stu-id="c45db-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="c45db-109">Quando você executar outra operação de tal uma nova transação é iniciada.</span><span class="sxs-lookup"><span data-stu-id="c45db-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="c45db-110">Começando com o EF6 **Database.ExecuteSqlCommand()** por padrão irá encapsular o comando em uma transação se uma já não estava presente.</span><span class="sxs-lookup"><span data-stu-id="c45db-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="c45db-111">Há sobrecargas do método que permitem que você substituir esse comportamento, se desejar.</span><span class="sxs-lookup"><span data-stu-id="c45db-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="c45db-112">Além disso, no EF6 a execução de procedimentos armazenados incluídos no modelo por meio de APIs, como **ObjectContext.ExecuteFunction()** faz o mesmo (exceto que o comportamento padrão não é possível no momento ser substituído).</span><span class="sxs-lookup"><span data-stu-id="c45db-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="c45db-113">Em ambos os casos, o nível de isolamento da transação é qualquer nível de isolamento, o provedor de banco de dados considera sua configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="c45db-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="c45db-114">Por padrão, por exemplo, no SQL Server trata de READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="c45db-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="c45db-115">Entity Framework não quebra consultas em uma transação.</span><span class="sxs-lookup"><span data-stu-id="c45db-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="c45db-116">Essa funcionalidade padrão é adequada para muitos usuários e se portanto, não é necessário fazer algo diferente no EF6; Basta escreva o código como sempre fez.</span><span class="sxs-lookup"><span data-stu-id="c45db-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="c45db-117">No entanto, alguns usuários exigem maior controle sobre suas transações – isso é abordado nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="c45db-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="c45db-118">Como funcionam as APIs</span><span class="sxs-lookup"><span data-stu-id="c45db-118">How the APIs work</span></span>  

<span data-ttu-id="c45db-119">Antes do EF6 Entity Framework insistiam em dizer sobre como abrir a conexão de banco de dados em si (ele gerava uma exceção se ela foi passada uma conexão que já está aberto).</span><span class="sxs-lookup"><span data-stu-id="c45db-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="c45db-120">Uma vez que uma transação só pode ser iniciada em uma conexão aberta, isso significava que a única maneira de um usuário pode encapsular várias operações em uma transação foi por usar um [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) ou use o  **ObjectContext.Connection** propriedade e chamar start **Open ()** e **BeginTransaction()** diretamente no retornado **EntityConnection** objeto.</span><span class="sxs-lookup"><span data-stu-id="c45db-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="c45db-121">Além disso, chamadas de API que contatar o banco de dados falhará se você tinha iniciou uma transação em que a conexão de banco de dados subjacentes por conta própria.</span><span class="sxs-lookup"><span data-stu-id="c45db-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="c45db-122">A limitação de aceitar somente conexões fechadas foi removida no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c45db-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="c45db-123">Para obter detalhes, consulte [gerenciamento de Conexão](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="c45db-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="c45db-124">Começando com o EF6 o framework agora fornece:</span><span class="sxs-lookup"><span data-stu-id="c45db-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="c45db-125">**Database.BeginTransaction()** : um método mais fácil para um usuário iniciar e concluir as transações em si dentro de um DbContext existente – permitindo que várias operações a serem combinados na mesma transação e, portanto, todos os confirmados ou todos revertida como um.</span><span class="sxs-lookup"><span data-stu-id="c45db-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="c45db-126">Ele também permite que o usuário especifique mais facilmente o nível de isolamento da transação.</span><span class="sxs-lookup"><span data-stu-id="c45db-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="c45db-127">**Database.UseTransaction()** : o que permite que o DbContext usar uma transação que foi iniciada fora do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c45db-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="c45db-128">Combinando várias operações em uma transação no mesmo contexto</span><span class="sxs-lookup"><span data-stu-id="c45db-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="c45db-129">**Database.BeginTransaction()** tem duas substituições – uma que usa uma explícita [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) e outro que não leva argumentos e usa o padrão IsolationLevel do provedor de banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="c45db-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="c45db-130">Ambas as substituições de retornam um **DbContextTransaction** objeto que fornece **Commit ()** e **Rollback ()** métodos que realizam commit e rollback no armazenamento subjacente transação.</span><span class="sxs-lookup"><span data-stu-id="c45db-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="c45db-131">O **DbContextTransaction** destina-se a ser descartado depois que ele foi confirmado ou revertido.</span><span class="sxs-lookup"><span data-stu-id="c45db-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="c45db-132">Uma maneira fácil de fazer isso é o **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="c45db-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="c45db-133">a sintaxe que chamará automaticamente **Dispose ()** conclui que o uso do bloco:</span><span class="sxs-lookup"><span data-stu-id="c45db-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    try
                    {
                        context.Database.ExecuteSqlCommand(
                            @"UPDATE Blogs SET Rating = 5" +
                                " WHERE Name LIKE '%Entity Framework%'"
                            );

                        var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        context.SaveChanges();

                        dbContextTransaction.Commit();
                    }
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="c45db-134">A partir de uma transação requer que a conexão de armazenamento subjacente esteja aberta.</span><span class="sxs-lookup"><span data-stu-id="c45db-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="c45db-135">Por isso a chamada Database.BeginTransaction() abrirá a conexão se já não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="c45db-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="c45db-136">Se DbContextTransaction aberto a conexão, em seguida, ele será fechá-lo quando é chamada de Dispose ().</span><span class="sxs-lookup"><span data-stu-id="c45db-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="c45db-137">Passando uma transação existente para o contexto</span><span class="sxs-lookup"><span data-stu-id="c45db-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="c45db-138">Às vezes, você gostaria de uma transação que é ainda mais amplo em escopo e que inclui operações no mesmo banco de dados, mas fora do EF completamente.</span><span class="sxs-lookup"><span data-stu-id="c45db-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="c45db-139">Para fazer isso abra a conexão e iniciar a transação por conta própria e, em seguida, informar ao EF a) para usar a conexão de banco de dados já aberto e b) para usar a transação existente em que a conexão.</span><span class="sxs-lookup"><span data-stu-id="c45db-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="c45db-140">Para fazer isso, você deve definir e usar um construtor na classe de contexto que herda de um dos construtores DbContext que levam a i) um parâmetro de conexão existente e ii) o contextOwnsConnection boolean.</span><span class="sxs-lookup"><span data-stu-id="c45db-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="c45db-141">O sinalizador dos contextOwnsConnection deve ser definido como false quando chamado nesse cenário.</span><span class="sxs-lookup"><span data-stu-id="c45db-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="c45db-142">Isso é importante, pois ele informa ao Entity Framework que ele não deve fechar a conexão quando tiver terminado com ele (por exemplo, consulte a linha 4 abaixo):</span><span class="sxs-lookup"><span data-stu-id="c45db-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="c45db-143">Além disso, você deve iniciar a transação por conta própria (incluindo o IsolationLevel se você quiser evitar a configuração padrão) e permitir que o Entity Framework sabe que há uma transação existente for um novato na conexão (consulte a linha 33 abaixo).</span><span class="sxs-lookup"><span data-stu-id="c45db-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="c45db-144">Em seguida, você é livre para executar operações de banco de dados diretamente no próprio SqlConnection ou no DbContext.</span><span class="sxs-lookup"><span data-stu-id="c45db-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="c45db-145">Todas essas operações são executadas dentro de uma transação.</span><span class="sxs-lookup"><span data-stu-id="c45db-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="c45db-146">Você assume a responsabilidade para confirmar ou reverter a transação e para chamar Dispose (), bem como para fechar e descarte a conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c45db-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="c45db-147">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c45db-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   try
                   {
                       var sqlCommand = new SqlCommand();
                       sqlCommand.Connection = conn;
                       sqlCommand.Transaction = sqlTxn;
                       sqlCommand.CommandText =
                           @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                       sqlCommand.ExecuteNonQuery();

                       using (var context =  
                         new BloggingContext(conn, contextOwnsConnection: false))
                        {
                            context.Database.UseTransaction(sqlTxn);

                            var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                            foreach (var post in query)
                            {
                                post.Title += "[Cool Blog]";
                            }
                           context.SaveChanges();
                        }

                        sqlTxn.Commit();
                    }
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="c45db-148">Limpar a transação</span><span class="sxs-lookup"><span data-stu-id="c45db-148">Clearing up the transaction</span></span>

<span data-ttu-id="c45db-149">Você pode passar nulo para Database.UseTransaction() para limpar dados de conhecimento do Entity Framework da transação atual.</span><span class="sxs-lookup"><span data-stu-id="c45db-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="c45db-150">Entity Framework irá nem confirmação nem reverter a transação existente quando você fizer isso, portanto, use com cuidado e somente se você tiver certeza, isso é o que você deseja fazer.</span><span class="sxs-lookup"><span data-stu-id="c45db-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="c45db-151">Erros no UseTransaction</span><span class="sxs-lookup"><span data-stu-id="c45db-151">Errors in UseTransaction</span></span>

<span data-ttu-id="c45db-152">Você verá uma exceção de Database.UseTransaction() se você passar uma transação quando:</span><span class="sxs-lookup"><span data-stu-id="c45db-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="c45db-153">O Entity Framework já tem uma transação existente</span><span class="sxs-lookup"><span data-stu-id="c45db-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="c45db-154">Entity Framework já está operando em um TransactionScope</span><span class="sxs-lookup"><span data-stu-id="c45db-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="c45db-155">O objeto de conexão na transação passado é nulo.</span><span class="sxs-lookup"><span data-stu-id="c45db-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="c45db-156">Ou seja, a transação não está associada uma conexão – normalmente, isso é um sinal de que essa transação já foi concluída.</span><span class="sxs-lookup"><span data-stu-id="c45db-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="c45db-157">O objeto de conexão na transação passada não corresponde a conexão do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c45db-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="c45db-158">Usando transações com outros recursos</span><span class="sxs-lookup"><span data-stu-id="c45db-158">Using transactions with other features</span></span>  

<span data-ttu-id="c45db-159">Esta seção fornece detalhes sobre como as transações acima interagem com:</span><span class="sxs-lookup"><span data-stu-id="c45db-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="c45db-160">Resiliência da conexão</span><span class="sxs-lookup"><span data-stu-id="c45db-160">Connection resiliency</span></span>  
- <span data-ttu-id="c45db-161">Métodos assíncronos</span><span class="sxs-lookup"><span data-stu-id="c45db-161">Asynchronous methods</span></span>  
- <span data-ttu-id="c45db-162">Transações de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="c45db-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="c45db-163">Resiliência da conexão</span><span class="sxs-lookup"><span data-stu-id="c45db-163">Connection Resiliency</span></span>  

<span data-ttu-id="c45db-164">O novo recurso de resiliência de Conexão não funciona com transações iniciadas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="c45db-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="c45db-165">Para obter detalhes, consulte [limitações com estratégias de repetição de execução](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span><span class="sxs-lookup"><span data-stu-id="c45db-165">For details, see [Limitations with Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="c45db-166">Programação assíncrona</span><span class="sxs-lookup"><span data-stu-id="c45db-166">Asynchronous Programming</span></span>  

<span data-ttu-id="c45db-167">A abordagem descrita nas seções anteriores precisa sem opções ou configurações adicionais para trabalhar com o [assíncrona consultar e salvar os métodos](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="c45db-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="c45db-168">Mas lembre-se de que, dependendo do que você fizer em métodos assíncronos, isso pode resultar em transações de longa execução – o que por sua vez podem causar deadlocks ou bloqueio que é ruim para o desempenho do aplicativo geral.</span><span class="sxs-lookup"><span data-stu-id="c45db-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="c45db-169">Transações de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="c45db-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="c45db-170">Antes do EF6 era usar um objeto TransactionScope a maneira recomendada de fornecer transações maiores de escopo:</span><span class="sxs-lookup"><span data-stu-id="c45db-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

<span data-ttu-id="c45db-171">A SqlConnection e Entity Framework seriam usar a transação de ambiente TransactionScope e, portanto, ser confirmadas em conjunto.</span><span class="sxs-lookup"><span data-stu-id="c45db-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="c45db-172">Começando com o .NET 4.5.1 TransactionScope foi atualizado para também funcionam com métodos assíncronos por meio do uso do [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeração:</span><span class="sxs-lookup"><span data-stu-id="c45db-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

<span data-ttu-id="c45db-173">Ainda há algumas limitações para a abordagem de TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="c45db-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="c45db-174">Requer o .NET 4.5.1 ou posterior para trabalhar com métodos assíncronos.</span><span class="sxs-lookup"><span data-stu-id="c45db-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="c45db-175">Ele não pode ser usado em cenários de nuvem, a menos que você tiver certeza de uma e apenas uma conexão (cenários de nuvem não dão suporte a transações distribuídas).</span><span class="sxs-lookup"><span data-stu-id="c45db-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="c45db-176">Ele não pode ser combinado com a abordagem Database.UseTransaction() das seções anteriores.</span><span class="sxs-lookup"><span data-stu-id="c45db-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="c45db-177">Se você executar nenhum DDL e não tiver habilitado a transações distribuídas por meio do serviço MSDTC, ele gerará exceções.</span><span class="sxs-lookup"><span data-stu-id="c45db-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="c45db-178">Vantagens da abordagem TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="c45db-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="c45db-179">Ele irá atualizar automaticamente uma transação local a uma transação distribuída se você fizer mais de uma conexão para um determinado banco de dados ou combina uma conexão para um banco de dados com uma conexão a um banco de dados diferente na mesma transação (Observação: você deve ter o serviço MSDTC configurado para permitir transações distribuídas para que isso funcione).</span><span class="sxs-lookup"><span data-stu-id="c45db-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="c45db-180">Facilidade de codificação.</span><span class="sxs-lookup"><span data-stu-id="c45db-180">Ease of coding.</span></span> <span data-ttu-id="c45db-181">Se você preferir a transação ambiente e distribuídas com implicitamente em segundo plano em vez de explicitamente em você controlar, em seguida, a abordagem de TransactionScope pode ser mais adequada é melhor.</span><span class="sxs-lookup"><span data-stu-id="c45db-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="c45db-182">Em resumo, com as novas APIs de Database.UseTransaction() acima e Database.BeginTransaction() a abordagem de TransactionScope não é necessária para a maioria dos usuários.</span><span class="sxs-lookup"><span data-stu-id="c45db-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="c45db-183">Se você continuar a usar o TransactionScope, em seguida, esteja ciente das limitações acima.</span><span class="sxs-lookup"><span data-stu-id="c45db-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="c45db-184">É recomendável usar a abordagem descrita nas seções anteriores em vez disso, sempre que possível.</span><span class="sxs-lookup"><span data-stu-id="c45db-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
