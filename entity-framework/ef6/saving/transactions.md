---
title: Trabalhando com transações-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419681"
---
# <a name="working-with-transactions"></a><span data-ttu-id="8ffdc-102">Trabalhando com transações</span><span class="sxs-lookup"><span data-stu-id="8ffdc-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="8ffdc-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8ffdc-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="8ffdc-105">Este documento descreverá o uso de transações em EF6, incluindo os aprimoramentos que adicionamos desde EF5 para facilitar o trabalho com transações.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="8ffdc-106">O que o EF faz por padrão</span><span class="sxs-lookup"><span data-stu-id="8ffdc-106">What EF does by default</span></span>  

<span data-ttu-id="8ffdc-107">Em todas as versões do Entity Framework, sempre que você executar **SaveChanges ()** para inserir, atualizar ou excluir no banco de dados, a estrutura encapsulará essa operação em uma transação.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="8ffdc-108">Essa transação dura apenas o tempo suficiente para executar a operação e, em seguida, é concluída.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="8ffdc-109">Quando você executa outra operação, uma nova transação é iniciada.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="8ffdc-110">A partir do EF6 **Database. ExecuteSqlCommand ()** por padrão, o comando será encapsulado em uma transação se ainda não estiver presente.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="8ffdc-111">Há sobrecargas desse método que permitem que você substitua esse comportamento, se desejar.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="8ffdc-112">Além disso, na execução EF6 de procedimentos armazenados incluídos no modelo por meio de APIs como **ObjectContext. ExecuteFunction ()** faz o mesmo (exceto que o comportamento padrão não pode ser substituído no momento).</span><span class="sxs-lookup"><span data-stu-id="8ffdc-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="8ffdc-113">Em ambos os casos, o nível de isolamento da transação é qualquer nível de isolamento que o provedor de banco de dados considera sua configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="8ffdc-114">Por padrão, por exemplo, em SQL Server isso é leitura confirmada.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="8ffdc-115">Entity Framework não encapsula consultas em uma transação.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="8ffdc-116">Essa funcionalidade padrão é adequada para muitos usuários e, nesse caso, não há necessidade de fazer nada diferente no EF6; Basta escrever o código como você fez sempre.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="8ffdc-117">No entanto, alguns usuários exigem maior controle sobre suas transações – isso é abordado nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="8ffdc-118">Como as APIs funcionam</span><span class="sxs-lookup"><span data-stu-id="8ffdc-118">How the APIs work</span></span>  

<span data-ttu-id="8ffdc-119">Antes de EF6 Entity Framework insistido de abrir a própria conexão de banco de dados (ela emitiu uma exceção se foi passada uma conexão que já estava aberta).</span><span class="sxs-lookup"><span data-stu-id="8ffdc-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="8ffdc-120">Como uma transação só pode ser iniciada em uma conexão aberta, isso significava que a única maneira de um usuário poder encapsular várias operações em uma transação era usar um [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) ou usar a propriedade **ObjectContext. Connection** e começar a chamar **Open ()** e **BeginTransaction ()** diretamente no objeto **EntityConnection** retornado.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="8ffdc-121">Além disso, as chamadas à API que entraram em contato com o banco de dados falharão se você tiver iniciado uma transação na conexão de banco de dados subjacente por conta própria.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="8ffdc-122">A limitação de aceitar somente conexões fechadas foi removida no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="8ffdc-123">Para obter detalhes, consulte [Gerenciamento de conexão](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="8ffdc-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="8ffdc-124">A partir do EF6, a estrutura agora fornece:</span><span class="sxs-lookup"><span data-stu-id="8ffdc-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="8ffdc-125">**Database. BeginTransaction ()** : um método mais fácil para um usuário iniciar e concluir transações em um DbContext existente – permitindo que várias operações sejam combinadas dentro da mesma transação e, portanto, todas confirmadas ou todas revertidas como uma.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="8ffdc-126">Ele também permite que o usuário especifique mais facilmente o nível de isolamento para a transação.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="8ffdc-127">**Database. UseTransaction ()** : que permite que o DbContext use uma transação que foi iniciada fora do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="8ffdc-128">Combinando várias operações em uma transação dentro do mesmo contexto</span><span class="sxs-lookup"><span data-stu-id="8ffdc-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="8ffdc-129">**Database. BeginTransaction ()** tem duas substituições – uma que usa um [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) explícito e um que não usa argumentos e usa o IsolationLevel padrão do provedor de banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="8ffdc-130">Ambas as substituições retornam um objeto **DbContextTransaction** que fornece os métodos **Commit ()** e **Rollback ()** que executam Commit e Rollback na transação de armazenamento subjacente.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="8ffdc-131">O **DbContextTransaction** deve ser descartado depois de ser confirmado ou revertido.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="8ffdc-132">Uma maneira fácil de fazer isso é o **uso (...) {...}**</span><span class="sxs-lookup"><span data-stu-id="8ffdc-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="8ffdc-133">a sintaxe que chamará **Dispose ()** automaticamente quando o bloco Using for concluído:</span><span class="sxs-lookup"><span data-stu-id="8ffdc-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="8ffdc-134">Iniciar uma transação requer que a conexão de armazenamento subjacente esteja aberta.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="8ffdc-135">Portanto, chamar Database. BeginTransaction () abrirá a conexão se ela ainda não estiver aberta.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="8ffdc-136">Se DbContextTransaction abrir a conexão, ela será fechada quando Dispose () for chamado.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="8ffdc-137">Passando uma transação existente para o contexto</span><span class="sxs-lookup"><span data-stu-id="8ffdc-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="8ffdc-138">Às vezes, você gostaria de uma transação que é ainda mais ampla no escopo e que inclui operações no mesmo banco de dados, mas fora do EF completamente.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="8ffdc-139">Para fazer isso, você deve abrir a conexão e iniciar a transação por conta própria e, em seguida, informar ao EF a) para usar a conexão de banco de dados já aberta e b) para usar a transação existente nessa conexão.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="8ffdc-140">Para fazer isso, você deve definir e usar um construtor em sua classe de contexto que herda de um dos construtores DbContext que utilizam i) um parâmetro de conexão existente e II) o booliano contextOwnsConnection.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="8ffdc-141">O sinalizador contextOwnsConnection deve ser definido como false quando chamado neste cenário.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="8ffdc-142">Isso é importante, pois informa Entity Framework que não deve fechar a conexão quando ela é feita com ela (por exemplo, consulte a linha 4 abaixo):</span><span class="sxs-lookup"><span data-stu-id="8ffdc-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="8ffdc-143">Além disso, você deve iniciar a transação por conta própria (incluindo o IsolationLevel se quiser evitar a configuração padrão) e permitir que Entity Framework saiba que existe uma transação existente já iniciada na conexão (consulte a linha 33 abaixo).</span><span class="sxs-lookup"><span data-stu-id="8ffdc-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="8ffdc-144">Em seguida, você está livre para executar operações de banco de dados diretamente na própria SqlConnection ou no DbContext.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="8ffdc-145">Todas essas operações são executadas dentro de uma transação.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="8ffdc-146">Você assume a responsabilidade por confirmar ou reverter a transação e chamar Dispose () nela, bem como para fechar e descartar a conexão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="8ffdc-147">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ffdc-147">For example:</span></span>  

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
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
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
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="8ffdc-148">Limpando a transação</span><span class="sxs-lookup"><span data-stu-id="8ffdc-148">Clearing up the transaction</span></span>

<span data-ttu-id="8ffdc-149">Você pode passar NULL para Database. UseTransaction () para limpar o conhecimento de Entity Framework da transação atual.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="8ffdc-150">Entity Framework não será confirmada nem reverterá a transação existente quando você fizer isso, portanto, use com cuidado e somente se tiver certeza de que deseja fazer isso.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="8ffdc-151">Erros em UseTransaction</span><span class="sxs-lookup"><span data-stu-id="8ffdc-151">Errors in UseTransaction</span></span>

<span data-ttu-id="8ffdc-152">Você verá uma exceção de Database. UseTransaction () se passar uma transação quando:</span><span class="sxs-lookup"><span data-stu-id="8ffdc-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="8ffdc-153">Entity Framework já tem uma transação existente</span><span class="sxs-lookup"><span data-stu-id="8ffdc-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="8ffdc-154">Entity Framework já está operando dentro de um TransactionScope</span><span class="sxs-lookup"><span data-stu-id="8ffdc-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="8ffdc-155">O objeto de conexão na transação passada é nulo.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="8ffdc-156">Ou seja, a transação não está associada a uma conexão – geralmente é um sinal de que a transação já foi concluída</span><span class="sxs-lookup"><span data-stu-id="8ffdc-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="8ffdc-157">O objeto de conexão na transação passada não corresponde à conexão do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="8ffdc-158">Usando transações com outros recursos</span><span class="sxs-lookup"><span data-stu-id="8ffdc-158">Using transactions with other features</span></span>  

<span data-ttu-id="8ffdc-159">Esta seção detalha como as transações acima interagem com:</span><span class="sxs-lookup"><span data-stu-id="8ffdc-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="8ffdc-160">Resiliência de conexão</span><span class="sxs-lookup"><span data-stu-id="8ffdc-160">Connection resiliency</span></span>  
- <span data-ttu-id="8ffdc-161">Métodos assíncronos</span><span class="sxs-lookup"><span data-stu-id="8ffdc-161">Asynchronous methods</span></span>  
- <span data-ttu-id="8ffdc-162">Transações TransactionScope</span><span class="sxs-lookup"><span data-stu-id="8ffdc-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="8ffdc-163">Resiliência da conexão</span><span class="sxs-lookup"><span data-stu-id="8ffdc-163">Connection Resiliency</span></span>  

<span data-ttu-id="8ffdc-164">O novo recurso de resiliência de conexão não funciona com transações iniciadas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="8ffdc-165">Para obter detalhes, consulte [repetindo estratégias de execução](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="8ffdc-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="8ffdc-166">Programação assíncrona</span><span class="sxs-lookup"><span data-stu-id="8ffdc-166">Asynchronous Programming</span></span>  

<span data-ttu-id="8ffdc-167">A abordagem descrita nas seções anteriores não precisa de mais opções ou configurações para trabalhar com os métodos de [consulta assíncrona e salvar](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="8ffdc-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="8ffdc-168">Mas lembre-se de que, dependendo do que você faz dentro dos métodos assíncronos, isso pode resultar em transações de longa execução, o que pode, por sua vez, causar deadlocks ou bloqueios que são ruins para o desempenho do aplicativo geral.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="8ffdc-169">Transações TransactionScope</span><span class="sxs-lookup"><span data-stu-id="8ffdc-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="8ffdc-170">Antes de EF6 a maneira recomendada de fornecer transações de escopo maiores era usar um objeto TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="8ffdc-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="8ffdc-171">A SqlConnection e a Entity Framework usariam a transação de TransactionScope de ambiente e, portanto, são confirmadas juntas.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="8ffdc-172">A partir do TransactionScope do .NET 4.5.1 foi atualizado para também funcionar com métodos assíncronos por meio do uso da enumeração [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) :</span><span class="sxs-lookup"><span data-stu-id="8ffdc-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="8ffdc-173">Ainda há algumas limitações para a abordagem TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="8ffdc-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="8ffdc-174">Requer o .NET 4.5.1 ou superior para trabalhar com métodos assíncronos.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="8ffdc-175">Ele não pode ser usado em cenários de nuvem, a menos que você tenha certeza de que tem apenas uma conexão (cenários de nuvem não dão suporte a transações distribuídas).</span><span class="sxs-lookup"><span data-stu-id="8ffdc-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="8ffdc-176">Ele não pode ser combinado com a abordagem Database. UseTransaction () das seções anteriores.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="8ffdc-177">Ele gerará exceções se você emitir qualquer DDL e não tiver habilitado transações distribuídas por meio do serviço MSDTC.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="8ffdc-178">Vantagens da abordagem TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="8ffdc-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="8ffdc-179">Ele atualizará automaticamente uma transação local para uma transação distribuída se você fizer mais de uma conexão com um determinado banco de dados ou combinar uma conexão com um banco de dados com uma conexão com um banco de dados diferente dentro da mesma transação (Observação: você deve ter o serviço MSDTC configurado para permitir que as transações distribuídas para isso funcionem).</span><span class="sxs-lookup"><span data-stu-id="8ffdc-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="8ffdc-180">Facilidade de codificação.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-180">Ease of coding.</span></span> <span data-ttu-id="8ffdc-181">Se você preferir que a transação seja ambiente e lidada com implicitamente em segundo plano em vez de explicitamente sob controle, a abordagem TransactionScope pode ser melhor.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="8ffdc-182">Em resumo, com as APIs New Database. BeginTransaction () e Database. UseTransaction () acima, a abordagem TransactionScope não é mais necessária para a maioria dos usuários.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="8ffdc-183">Se você continuar a usar o TransactionScope, lembre-se das limitações acima.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="8ffdc-184">É recomendável usar a abordagem descrita nas seções anteriores, em vez disso, sempre que possível.</span><span class="sxs-lookup"><span data-stu-id="8ffdc-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
