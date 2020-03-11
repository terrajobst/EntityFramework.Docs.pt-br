---
title: Gerenciamento de conexões-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417983"
---
# <a name="connection-management"></a>Gerenciamento de Conexão
Esta página descreve o comportamento de Entity Framework em relação à passagem de conexões com o contexto e à funcionalidade da API do **banco de dados. Connection. Open ()** .  

## <a name="passing-connections-to-the-context"></a>Passando conexões para o contexto  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportamento para EF5 e versões anteriores  

Há dois construtores que aceitam conexões:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

É possível usá-los, mas você precisa contornar algumas limitações:  

1. Se você passar uma conexão aberta para qualquer uma delas, na primeira vez em que a estrutura tentar usá-la, uma InvalidOperationException será gerada dizendo que não é possível abrir novamente uma conexão já aberta.  
2. O sinalizador contextOwnsConnection é interpretado para significar se a conexão de armazenamento subjacente deve ser descartada quando o contexto é Descartado. Mas, independentemente dessa configuração, a conexão da loja sempre será fechada quando o contexto for descartado. Portanto, se você tiver mais de um DbContext com a mesma conexão, o contexto que for descartado primeiro fechará a conexão (da mesma forma, se você misturou uma conexão ADO.NET existente com um DbContext, DbContext sempre fechará a conexão quando ela for descartada) .  

É possível contornar a primeira limitação acima, passando uma conexão fechada e apenas executando o código que a abriria depois que todos os contextos tiverem sido criados:  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

A segunda limitação apenas significa que você precisa descartar qualquer um de seus objetos DbContext até estar pronto para que a conexão seja fechada.  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportamento no EF6 e em versões futuras  

No EF6 e nas versões futuras, o DbContext tem os mesmos dois construtores, mas não requer mais que a conexão passada para o Construtor seja fechada quando for recebida. Agora, isso é possível:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

O sinalizador contextOwnsConnection agora controla se a conexão é fechada e descartada quando o DbContext é Descartado. Portanto, no exemplo acima, a conexão não é fechada quando o contexto é Descartado (linha 32) como seria nas versões anteriores do EF, mas sim quando a própria conexão é descartada (linha 40).  

É claro que ainda é possível que o DbContext assuma o controle da conexão (basta definir contextOwnsConnection como true ou usar um dos outros construtores), se desejar.  

> [!NOTE]
> Há algumas considerações adicionais ao usar transações com esse novo modelo. Para obter detalhes, consulte [trabalhando com transações](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database. Connection. Open ()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportamento para EF5 e versões anteriores  

No EF5 e em versões anteriores, há um bug de que o **ObjectContext. Connection. State** não foi atualizado para refletir o estado verdadeiro da conexão de armazenamento subjacente. Por exemplo, se você executou o código a seguir, poderá retornar o status **Closed** mesmo que, na verdade, a conexão de armazenamento subjacente esteja **aberta**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Separadamente, se você abrir a conexão de banco de dados chamando Database. Connection. Open (), ela será aberta até a próxima vez que você executar uma consulta ou chamar qualquer coisa que exija uma conexão de banco de dados (por exemplo, SaveChanges ()), mas depois disso, o armazenamento subjacente a conexão será fechada. O contexto, em seguida, reabrirá e fechará novamente a conexão sempre que outra operação de banco de dados for necessária:  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportamento no EF6 e em versões futuras  

Para EF6 e versões futuras, tomamos a abordagem de que, se o código de chamada optar por abrir a conexão chamando o contexto. Database. Connection. Open (), em seguida, ele tem um bom motivo para fazer isso e a estrutura assumirá que ele deseja controlar a abertura e o fechamento da conexão e não mais fechará a conexão automaticamente.  

> [!NOTE]
> Isso pode potencialmente levar a conexões abertas por um longo tempo, portanto, use com cuidado.  

Também atualizamos o código para que ObjectContext. Connection. State agora Mantenha o controle do estado da conexão subjacente corretamente.  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
