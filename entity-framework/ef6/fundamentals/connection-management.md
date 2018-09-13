---
title: Gerenciamento de Conexão - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489330"
---
# <a name="connection-management"></a>Gerenciamento de Conexão
Esta página descreve o comportamento do Entity Framework em relação a passagem de conexões para o contexto e a funcionalidade dos **Database.Connection.Open()** API.  

## <a name="passing-connections-to-the-context"></a>Conexões de passar para o contexto  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportamento do EF5 e versões anteriores  

Há dois construtores que aceitam conexões:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

É possível usá-los, mas você tem que trabalhar em torno de algumas limitações:  

1. Se você passar uma conexão aberta para qualquer uma delas, em seguida, na primeira vez em que o framework tenta usá-lo que um InvalidOperationException é gerado indicando que não é possível reabrir uma conexão já aberta.  
2. O sinalizador dos contextOwnsConnection é interpretado como se a conexão de armazenamento subjacente deve ser descartado quando o contexto for descartado. Mas, independentemente dessa configuração, a conexão de armazenamento sempre é fechado quando o contexto for descartado. Portanto, se você tiver mais de um DbContext com a mesma conexão seja qual for o contexto for descartado primeiro fechará a conexão (da mesma forma se uma conexão ADO.NET existente com um DbContext tem uma combinação, DbContext sempre fechará a conexão quando ela é descartada) .  

É possível contornar a limitação primeiro acima passando uma conexão fechada e executar apenas o código que seria abertos depois que todos os contextos foram criados:  

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

A segunda limitação apenas significa que você precisa evitar descartar a qualquer um dos seus objetos DbContext até estar pronto para a conexão ser fechada.  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportamento no EF6 e versões futuras  

No EF6 e versões futuras DbContext tem os mesmos dois construtores, mas não exige mais que a conexão passada para o construtor ser fechado quando ela é recebida. Portanto, agora é possível:  

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

Também o sinalizador dos contextOwnsConnection agora controla ou não a conexão está fechado e descartado quando o DbContext for descartado. Portanto, no exemplo acima a conexão não for fechada quando o contexto é descartado (linha 32), como era nas versões anteriores do EF, mas em vez disso, quando a conexão em si é descartado (linha 40).  

Certamente ainda é possível para o DbContext para assumir o controle da conexão (apenas conjunto contextOwnsConnection como verdadeira ou use um dos outros construtores) se desejar.  

> [!NOTE]
> Há algumas considerações adicionais ao usar transações com esse novo modelo. Para obter detalhes, consulte [trabalhando com transações](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportamento do EF5 e versões anteriores  

Há um bug no EF5 e versões anteriores, de modo que o **ObjectContext.Connection.State** não foi atualizado para refletir o estado real da conexão de armazenamento subjacente. Por exemplo, se você executasse o código a seguir você pode ser retornado o status **fechado** mesmo que na verdade subjacente armazenar a conexão é **abrir**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Separadamente, se você abrir a conexão de banco de dados chamando Database.Connection.Open() será aberto até da próxima vez que você executa uma consulta ou chama algo que requer uma conexão de banco de dados (por exemplo, SaveChanges()), mas após que subjacente armazena conexão será fechada. O contexto será abrir novamente e Fechar novamente a conexão sempre que outra operação de banco de dados é necessária:  

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

### <a name="behavior-in-ef6-and-future-versions"></a>Comportamento no EF6 e versões futuras  

Para versões futuras e EF6 é que nós tiramos a abordagem que se o código de chamada optar por abrir a conexão com o contexto de chamada. Database.Connection.Open() e em seguida, ele tem um bom motivo para fazer isso e o framework assumirá que ele deseja controle sobre a abertura e fechamento da conexão e não fechará automaticamente a conexão.  

> [!NOTE]
> Potencialmente, isso pode levar a conexões que estão abertas para um longo tempo, portanto, use com cuidado.  

Também atualizamos o código para que ObjectContext.Connection.State agora mantém controle sobre o estado da conexão subjacente corretamente.  

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
