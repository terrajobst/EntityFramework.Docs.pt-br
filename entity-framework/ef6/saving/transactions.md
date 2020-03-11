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
# <a name="working-with-transactions"></a>Trabalhando com transações
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Este documento descreverá o uso de transações em EF6, incluindo os aprimoramentos que adicionamos desde EF5 para facilitar o trabalho com transações.  

## <a name="what-ef-does-by-default"></a>O que o EF faz por padrão  

Em todas as versões do Entity Framework, sempre que você executar **SaveChanges ()** para inserir, atualizar ou excluir no banco de dados, a estrutura encapsulará essa operação em uma transação. Essa transação dura apenas o tempo suficiente para executar a operação e, em seguida, é concluída. Quando você executa outra operação, uma nova transação é iniciada.  

A partir do EF6 **Database. ExecuteSqlCommand ()** por padrão, o comando será encapsulado em uma transação se ainda não estiver presente. Há sobrecargas desse método que permitem que você substitua esse comportamento, se desejar. Além disso, na execução EF6 de procedimentos armazenados incluídos no modelo por meio de APIs como **ObjectContext. ExecuteFunction ()** faz o mesmo (exceto que o comportamento padrão não pode ser substituído no momento).  

Em ambos os casos, o nível de isolamento da transação é qualquer nível de isolamento que o provedor de banco de dados considera sua configuração padrão. Por padrão, por exemplo, em SQL Server isso é leitura confirmada.  

Entity Framework não encapsula consultas em uma transação.  

Essa funcionalidade padrão é adequada para muitos usuários e, nesse caso, não há necessidade de fazer nada diferente no EF6; Basta escrever o código como você fez sempre.  

No entanto, alguns usuários exigem maior controle sobre suas transações – isso é abordado nas seções a seguir.  

## <a name="how-the-apis-work"></a>Como as APIs funcionam  

Antes de EF6 Entity Framework insistido de abrir a própria conexão de banco de dados (ela emitiu uma exceção se foi passada uma conexão que já estava aberta). Como uma transação só pode ser iniciada em uma conexão aberta, isso significava que a única maneira de um usuário poder encapsular várias operações em uma transação era usar um [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) ou usar a propriedade **ObjectContext. Connection** e começar a chamar **Open ()** e **BeginTransaction ()** diretamente no objeto **EntityConnection** retornado. Além disso, as chamadas à API que entraram em contato com o banco de dados falharão se você tiver iniciado uma transação na conexão de banco de dados subjacente por conta própria.  

> [!NOTE]
> A limitação de aceitar somente conexões fechadas foi removida no Entity Framework 6. Para obter detalhes, consulte [Gerenciamento de conexão](~/ef6/fundamentals/connection-management.md).  

A partir do EF6, a estrutura agora fornece:  

1. **Database. BeginTransaction ()** : um método mais fácil para um usuário iniciar e concluir transações em um DbContext existente – permitindo que várias operações sejam combinadas dentro da mesma transação e, portanto, todas confirmadas ou todas revertidas como uma. Ele também permite que o usuário especifique mais facilmente o nível de isolamento para a transação.  
2. **Database. UseTransaction ()** : que permite que o DbContext use uma transação que foi iniciada fora do Entity Framework.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Combinando várias operações em uma transação dentro do mesmo contexto  

**Database. BeginTransaction ()** tem duas substituições – uma que usa um [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) explícito e um que não usa argumentos e usa o IsolationLevel padrão do provedor de banco de dados subjacente. Ambas as substituições retornam um objeto **DbContextTransaction** que fornece os métodos **Commit ()** e **Rollback ()** que executam Commit e Rollback na transação de armazenamento subjacente.  

O **DbContextTransaction** deve ser descartado depois de ser confirmado ou revertido. Uma maneira fácil de fazer isso é o **uso (...) {...}** a sintaxe que chamará **Dispose ()** automaticamente quando o bloco Using for concluído:  

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
> Iniciar uma transação requer que a conexão de armazenamento subjacente esteja aberta. Portanto, chamar Database. BeginTransaction () abrirá a conexão se ela ainda não estiver aberta. Se DbContextTransaction abrir a conexão, ela será fechada quando Dispose () for chamado.  

### <a name="passing-an-existing-transaction-to-the-context"></a>Passando uma transação existente para o contexto  

Às vezes, você gostaria de uma transação que é ainda mais ampla no escopo e que inclui operações no mesmo banco de dados, mas fora do EF completamente. Para fazer isso, você deve abrir a conexão e iniciar a transação por conta própria e, em seguida, informar ao EF a) para usar a conexão de banco de dados já aberta e b) para usar a transação existente nessa conexão.  

Para fazer isso, você deve definir e usar um construtor em sua classe de contexto que herda de um dos construtores DbContext que utilizam i) um parâmetro de conexão existente e II) o booliano contextOwnsConnection.  

> [!NOTE]
> O sinalizador contextOwnsConnection deve ser definido como false quando chamado neste cenário. Isso é importante, pois informa Entity Framework que não deve fechar a conexão quando ela é feita com ela (por exemplo, consulte a linha 4 abaixo):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Além disso, você deve iniciar a transação por conta própria (incluindo o IsolationLevel se quiser evitar a configuração padrão) e permitir que Entity Framework saiba que existe uma transação existente já iniciada na conexão (consulte a linha 33 abaixo).  

Em seguida, você está livre para executar operações de banco de dados diretamente na própria SqlConnection ou no DbContext. Todas essas operações são executadas dentro de uma transação. Você assume a responsabilidade por confirmar ou reverter a transação e chamar Dispose () nela, bem como para fechar e descartar a conexão do banco de dados. Por exemplo:  

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

### <a name="clearing-up-the-transaction"></a>Limpando a transação

Você pode passar NULL para Database. UseTransaction () para limpar o conhecimento de Entity Framework da transação atual. Entity Framework não será confirmada nem reverterá a transação existente quando você fizer isso, portanto, use com cuidado e somente se tiver certeza de que deseja fazer isso.  

### <a name="errors-in-usetransaction"></a>Erros em UseTransaction

Você verá uma exceção de Database. UseTransaction () se passar uma transação quando:  
- Entity Framework já tem uma transação existente  
- Entity Framework já está operando dentro de um TransactionScope  
- O objeto de conexão na transação passada é nulo. Ou seja, a transação não está associada a uma conexão – geralmente é um sinal de que a transação já foi concluída  
- O objeto de conexão na transação passada não corresponde à conexão do Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Usando transações com outros recursos  

Esta seção detalha como as transações acima interagem com:  

- Resiliência de conexão  
- Métodos assíncronos  
- Transações TransactionScope  

### <a name="connection-resiliency"></a>Resiliência da conexão  

O novo recurso de resiliência de conexão não funciona com transações iniciadas pelo usuário. Para obter detalhes, consulte [repetindo estratégias de execução](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Programação assíncrona  

A abordagem descrita nas seções anteriores não precisa de mais opções ou configurações para trabalhar com os métodos de [consulta assíncrona e salvar](~/ef6/fundamentals/async.md
). Mas lembre-se de que, dependendo do que você faz dentro dos métodos assíncronos, isso pode resultar em transações de longa execução, o que pode, por sua vez, causar deadlocks ou bloqueios que são ruins para o desempenho do aplicativo geral.  

### <a name="transactionscope-transactions"></a>Transações TransactionScope  

Antes de EF6 a maneira recomendada de fornecer transações de escopo maiores era usar um objeto TransactionScope:  

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

A SqlConnection e a Entity Framework usariam a transação de TransactionScope de ambiente e, portanto, são confirmadas juntas.  

A partir do TransactionScope do .NET 4.5.1 foi atualizado para também funcionar com métodos assíncronos por meio do uso da enumeração [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) :  

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

Ainda há algumas limitações para a abordagem TransactionScope:  

- Requer o .NET 4.5.1 ou superior para trabalhar com métodos assíncronos.  
- Ele não pode ser usado em cenários de nuvem, a menos que você tenha certeza de que tem apenas uma conexão (cenários de nuvem não dão suporte a transações distribuídas).  
- Ele não pode ser combinado com a abordagem Database. UseTransaction () das seções anteriores.  
- Ele gerará exceções se você emitir qualquer DDL e não tiver habilitado transações distribuídas por meio do serviço MSDTC.  

Vantagens da abordagem TransactionScope:  

- Ele atualizará automaticamente uma transação local para uma transação distribuída se você fizer mais de uma conexão com um determinado banco de dados ou combinar uma conexão com um banco de dados com uma conexão com um banco de dados diferente dentro da mesma transação (Observação: você deve ter o serviço MSDTC configurado para permitir que as transações distribuídas para isso funcionem).  
- Facilidade de codificação. Se você preferir que a transação seja ambiente e lidada com implicitamente em segundo plano em vez de explicitamente sob controle, a abordagem TransactionScope pode ser melhor.  

Em resumo, com as APIs New Database. BeginTransaction () e Database. UseTransaction () acima, a abordagem TransactionScope não é mais necessária para a maioria dos usuários. Se você continuar a usar o TransactionScope, lembre-se das limitações acima. É recomendável usar a abordagem descrita nas seções anteriores, em vez disso, sempre que possível.  
