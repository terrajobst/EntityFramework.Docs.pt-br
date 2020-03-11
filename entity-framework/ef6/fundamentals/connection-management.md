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
# <a name="connection-management"></a><span data-ttu-id="76569-102">Gerenciamento de Conexão</span><span class="sxs-lookup"><span data-stu-id="76569-102">Connection management</span></span>
<span data-ttu-id="76569-103">Esta página descreve o comportamento de Entity Framework em relação à passagem de conexões com o contexto e à funcionalidade da API do **banco de dados. Connection. Open ()** .</span><span class="sxs-lookup"><span data-stu-id="76569-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="76569-104">Passando conexões para o contexto</span><span class="sxs-lookup"><span data-stu-id="76569-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="76569-105">Comportamento para EF5 e versões anteriores</span><span class="sxs-lookup"><span data-stu-id="76569-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="76569-106">Há dois construtores que aceitam conexões:</span><span class="sxs-lookup"><span data-stu-id="76569-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="76569-107">É possível usá-los, mas você precisa contornar algumas limitações:</span><span class="sxs-lookup"><span data-stu-id="76569-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="76569-108">Se você passar uma conexão aberta para qualquer uma delas, na primeira vez em que a estrutura tentar usá-la, uma InvalidOperationException será gerada dizendo que não é possível abrir novamente uma conexão já aberta.</span><span class="sxs-lookup"><span data-stu-id="76569-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="76569-109">O sinalizador contextOwnsConnection é interpretado para significar se a conexão de armazenamento subjacente deve ser descartada quando o contexto é Descartado.</span><span class="sxs-lookup"><span data-stu-id="76569-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="76569-110">Mas, independentemente dessa configuração, a conexão da loja sempre será fechada quando o contexto for descartado.</span><span class="sxs-lookup"><span data-stu-id="76569-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="76569-111">Portanto, se você tiver mais de um DbContext com a mesma conexão, o contexto que for descartado primeiro fechará a conexão (da mesma forma, se você misturou uma conexão ADO.NET existente com um DbContext, DbContext sempre fechará a conexão quando ela for descartada) .</span><span class="sxs-lookup"><span data-stu-id="76569-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="76569-112">É possível contornar a primeira limitação acima, passando uma conexão fechada e apenas executando o código que a abriria depois que todos os contextos tiverem sido criados:</span><span class="sxs-lookup"><span data-stu-id="76569-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="76569-113">A segunda limitação apenas significa que você precisa descartar qualquer um de seus objetos DbContext até estar pronto para que a conexão seja fechada.</span><span class="sxs-lookup"><span data-stu-id="76569-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="76569-114">Comportamento no EF6 e em versões futuras</span><span class="sxs-lookup"><span data-stu-id="76569-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="76569-115">No EF6 e nas versões futuras, o DbContext tem os mesmos dois construtores, mas não requer mais que a conexão passada para o Construtor seja fechada quando for recebida.</span><span class="sxs-lookup"><span data-stu-id="76569-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="76569-116">Agora, isso é possível:</span><span class="sxs-lookup"><span data-stu-id="76569-116">So this is now possible:</span></span>  

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

<span data-ttu-id="76569-117">O sinalizador contextOwnsConnection agora controla se a conexão é fechada e descartada quando o DbContext é Descartado.</span><span class="sxs-lookup"><span data-stu-id="76569-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="76569-118">Portanto, no exemplo acima, a conexão não é fechada quando o contexto é Descartado (linha 32) como seria nas versões anteriores do EF, mas sim quando a própria conexão é descartada (linha 40).</span><span class="sxs-lookup"><span data-stu-id="76569-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="76569-119">É claro que ainda é possível que o DbContext assuma o controle da conexão (basta definir contextOwnsConnection como true ou usar um dos outros construtores), se desejar.</span><span class="sxs-lookup"><span data-stu-id="76569-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="76569-120">Há algumas considerações adicionais ao usar transações com esse novo modelo.</span><span class="sxs-lookup"><span data-stu-id="76569-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="76569-121">Para obter detalhes, consulte [trabalhando com transações](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="76569-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="76569-122">Database. Connection. Open ()</span><span class="sxs-lookup"><span data-stu-id="76569-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="76569-123">Comportamento para EF5 e versões anteriores</span><span class="sxs-lookup"><span data-stu-id="76569-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="76569-124">No EF5 e em versões anteriores, há um bug de que o **ObjectContext. Connection. State** não foi atualizado para refletir o estado verdadeiro da conexão de armazenamento subjacente.</span><span class="sxs-lookup"><span data-stu-id="76569-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="76569-125">Por exemplo, se você executou o código a seguir, poderá retornar o status **Closed** mesmo que, na verdade, a conexão de armazenamento subjacente esteja **aberta**.</span><span class="sxs-lookup"><span data-stu-id="76569-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="76569-126">Separadamente, se você abrir a conexão de banco de dados chamando Database. Connection. Open (), ela será aberta até a próxima vez que você executar uma consulta ou chamar qualquer coisa que exija uma conexão de banco de dados (por exemplo, SaveChanges ()), mas depois disso, o armazenamento subjacente a conexão será fechada.</span><span class="sxs-lookup"><span data-stu-id="76569-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="76569-127">O contexto, em seguida, reabrirá e fechará novamente a conexão sempre que outra operação de banco de dados for necessária:</span><span class="sxs-lookup"><span data-stu-id="76569-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="76569-128">Comportamento no EF6 e em versões futuras</span><span class="sxs-lookup"><span data-stu-id="76569-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="76569-129">Para EF6 e versões futuras, tomamos a abordagem de que, se o código de chamada optar por abrir a conexão chamando o contexto. Database. Connection. Open (), em seguida, ele tem um bom motivo para fazer isso e a estrutura assumirá que ele deseja controlar a abertura e o fechamento da conexão e não mais fechará a conexão automaticamente.</span><span class="sxs-lookup"><span data-stu-id="76569-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="76569-130">Isso pode potencialmente levar a conexões abertas por um longo tempo, portanto, use com cuidado.</span><span class="sxs-lookup"><span data-stu-id="76569-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="76569-131">Também atualizamos o código para que ObjectContext. Connection. State agora Mantenha o controle do estado da conexão subjacente corretamente.</span><span class="sxs-lookup"><span data-stu-id="76569-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
