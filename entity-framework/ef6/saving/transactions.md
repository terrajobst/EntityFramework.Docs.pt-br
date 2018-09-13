---
title: Trabalhando com transações - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7197733ab25c8475746e7863963384730919e3ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489772"
---
# <a name="working-with-transactions"></a>Trabalhando com transações
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Este documento descreverá o uso de transações no EF6, incluindo os aprimoramentos que adicionamos desde o EF5 para facilitar o trabalho com transações.  

## <a name="what-ef-does-by-default"></a>O que o EF faz por padrão  

Todas as versões do Entity Framework, sempre que você executar **SaveChanges ()** inserir, atualizar ou excluir, no banco de dados, o framework irá encapsular essa operação em uma transação. Esta transação dura apenas por tempo suficiente para executar a operação e, em seguida, conclui. Quando você executar outra operação de tal uma nova transação é iniciada.  

Começando com o EF6 **Database.ExecuteSqlCommand()** por padrão irá encapsular o comando em uma transação se uma já não estava presente. Há sobrecargas do método que permitem que você substituir esse comportamento, se desejar. Além disso, no EF6 a execução de procedimentos armazenados incluídos no modelo por meio de APIs, como **ObjectContext.ExecuteFunction()** faz o mesmo (exceto que o comportamento padrão não é possível no momento ser substituído).  

Em ambos os casos, o nível de isolamento da transação é qualquer nível de isolamento, o provedor de banco de dados considera sua configuração padrão. Por padrão, por exemplo, no SQL Server trata de READ COMMITTED.  

Entity Framework não quebra consultas em uma transação.  

Essa funcionalidade padrão é adequada para muitos usuários e se portanto, não é necessário fazer algo diferente no EF6; Basta escreva o código como sempre fez.  

No entanto, alguns usuários exigem maior controle sobre suas transações – isso é abordado nas seções a seguir.  

## <a name="how-the-apis-work"></a>Como funcionam as APIs  

Antes do EF6 Entity Framework insistiam em dizer sobre como abrir a conexão de banco de dados em si (ele gerava uma exceção se ela foi passada uma conexão que já está aberto). Uma vez que uma transação só pode ser iniciada em uma conexão aberta, isso significava que a única maneira de um usuário pode encapsular várias operações em uma transação foi por usar um [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) ou use o  **ObjectContext.Connection** propriedade e chamar start **Open ()** e **BeginTransaction()** diretamente no retornado **EntityConnection** objeto. Além disso, chamadas de API que contatar o banco de dados falhará se você tinha iniciou uma transação em que a conexão de banco de dados subjacentes por conta própria.  

> [!NOTE]
> A limitação de aceitar somente conexões fechadas foi removida no Entity Framework 6. Para obter detalhes, consulte [gerenciamento de Conexão](~/ef6/fundamentals/connection-management.md).  

Começando com o EF6 o framework agora fornece:  

1. **Database.BeginTransaction()** : um método mais fácil para um usuário iniciar e concluir as transações em si dentro de um DbContext existente – permitindo que várias operações a serem combinados na mesma transação e, portanto, todos os confirmados ou todos revertida como um. Ele também permite que o usuário especifique mais facilmente o nível de isolamento da transação.  
2. **Database.UseTransaction()** : o que permite que o DbContext usar uma transação que foi iniciada fora do Entity Framework.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Combinando várias operações em uma transação no mesmo contexto  

**Database.BeginTransaction()** tem duas substituições – uma que usa uma explícita [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) e outro que não leva argumentos e usa o padrão IsolationLevel do provedor de banco de dados subjacente. Ambas as substituições de retornam um **DbContextTransaction** objeto que fornece **Commit ()** e **Rollback ()** métodos que realizam commit e rollback no armazenamento subjacente transação.  

O **DbContextTransaction** destina-se a ser descartado depois que ele foi confirmado ou revertido. Uma maneira fácil de fazer isso é o **using(...) {...}** a sintaxe que chamará automaticamente **Dispose ()** conclui que o uso do bloco:  

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
> A partir de uma transação requer que a conexão de armazenamento subjacente esteja aberta. Por isso a chamada Database.BeginTransaction() abrirá a conexão se já não estiver aberto. Se DbContextTransaction aberto a conexão, em seguida, ele será fechá-lo quando é chamada de Dispose ().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Passando uma transação existente para o contexto  

Às vezes, você gostaria de uma transação que é ainda mais amplo em escopo e que inclui operações no mesmo banco de dados, mas fora do EF completamente. Para fazer isso abra a conexão e iniciar a transação por conta própria e, em seguida, informar ao EF a) para usar a conexão de banco de dados já aberto e b) para usar a transação existente em que a conexão.  

Para fazer isso, você deve definir e usar um construtor na classe de contexto que herda de um dos construtores DbContext que levam a i) um parâmetro de conexão existente e ii) o contextOwnsConnection boolean.  

> [!NOTE]
> O sinalizador dos contextOwnsConnection deve ser definido como false quando chamado nesse cenário. Isso é importante, pois ele informa ao Entity Framework que ele não deve fechar a conexão quando tiver terminado com ele (por exemplo, consulte a linha 4 abaixo):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Além disso, você deve iniciar a transação por conta própria (incluindo o IsolationLevel se você quiser evitar a configuração padrão) e permitir que o Entity Framework sabe que há uma transação existente for um novato na conexão (consulte a linha 33 abaixo).  

Em seguida, você é livre para executar operações de banco de dados diretamente no próprio SqlConnection ou no DbContext. Todas essas operações são executadas dentro de uma transação. Você assume a responsabilidade para confirmar ou reverter a transação e para chamar Dispose (), bem como para fechar e descarte a conexão de banco de dados. Por exemplo:  

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

### <a name="clearing-up-the-transaction"></a>Limpar a transação

Você pode passar nulo para Database.UseTransaction() para limpar dados de conhecimento do Entity Framework da transação atual. Entity Framework irá nem confirmação nem reverter a transação existente quando você fizer isso, portanto, use com cuidado e somente se você tiver certeza, isso é o que você deseja fazer.  

### <a name="errors-in-usetransaction"></a>Erros no UseTransaction

Você verá uma exceção de Database.UseTransaction() se você passar uma transação quando:  
- O Entity Framework já tem uma transação existente  
- Entity Framework já está operando em um TransactionScope  
- O objeto de conexão na transação passado é nulo. Ou seja, a transação não está associada uma conexão – normalmente, isso é um sinal de que essa transação já foi concluída.  
- O objeto de conexão na transação passada não corresponde a conexão do Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Usando transações com outros recursos  

Esta seção fornece detalhes sobre como as transações acima interagem com:  

- Resiliência da conexão  
- Métodos assíncronos  
- Transações de TransactionScope  

### <a name="connection-resiliency"></a>Resiliência da conexão  

O novo recurso de resiliência de Conexão não funciona com transações iniciadas pelo usuário. Para obter detalhes, consulte [estratégias de repetição de execução](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Programação assíncrona  

A abordagem descrita nas seções anteriores precisa sem opções ou configurações adicionais para trabalhar com o [assíncrona consultar e salvar os métodos](~/ef6/fundamentals/async.md
). Mas lembre-se de que, dependendo do que você fizer em métodos assíncronos, isso pode resultar em transações de longa execução – o que por sua vez podem causar deadlocks ou bloqueio que é ruim para o desempenho do aplicativo geral.  

### <a name="transactionscope-transactions"></a>Transações de TransactionScope  

Antes do EF6 era usar um objeto TransactionScope a maneira recomendada de fornecer transações maiores de escopo:  

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

A SqlConnection e Entity Framework seriam usar a transação de ambiente TransactionScope e, portanto, ser confirmadas em conjunto.  

Começando com o .NET 4.5.1 TransactionScope foi atualizado para também funcionam com métodos assíncronos por meio do uso do [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeração:  

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

Ainda há algumas limitações para a abordagem de TransactionScope:  

- Requer o .NET 4.5.1 ou posterior para trabalhar com métodos assíncronos.  
- Ele não pode ser usado em cenários de nuvem, a menos que você tiver certeza de uma e apenas uma conexão (cenários de nuvem não dão suporte a transações distribuídas).  
- Ele não pode ser combinado com a abordagem Database.UseTransaction() das seções anteriores.  
- Se você executar nenhum DDL e não tiver habilitado a transações distribuídas por meio do serviço MSDTC, ele gerará exceções.  

Vantagens da abordagem TransactionScope:  

- Ele irá atualizar automaticamente uma transação local a uma transação distribuída se você fizer mais de uma conexão para um determinado banco de dados ou combina uma conexão para um banco de dados com uma conexão a um banco de dados diferente na mesma transação (Observação: você deve ter o serviço MSDTC configurado para permitir transações distribuídas para que isso funcione).  
- Facilidade de codificação. Se você preferir a transação ambiente e distribuídas com implicitamente em segundo plano em vez de explicitamente em você controlar, em seguida, a abordagem de TransactionScope pode ser mais adequada é melhor.  

Em resumo, com as novas APIs de Database.UseTransaction() acima e Database.BeginTransaction() a abordagem de TransactionScope não é necessária para a maioria dos usuários. Se você continuar a usar o TransactionScope, em seguida, esteja ciente das limitações acima. É recomendável usar a abordagem descrita nas seções anteriores em vez disso, sempre que possível.  
