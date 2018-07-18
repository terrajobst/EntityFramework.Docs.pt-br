---
title: Gerenciamento de Conexão - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 9588ef85435d3c0218defdc098f1e7150fb7ef72
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120093"
---
# <a name="connection-management"></a><span data-ttu-id="ed9d3-102">Gerenciamento de Conexão</span><span class="sxs-lookup"><span data-stu-id="ed9d3-102">Connection management</span></span>
<span data-ttu-id="ed9d3-103">Esta página descreve o comportamento do Entity Framework em relação a passagem de conexões para o contexto e a funcionalidade dos **Database.Connection.Open()** API.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="ed9d3-104">Conexões de passar para o contexto</span><span class="sxs-lookup"><span data-stu-id="ed9d3-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="ed9d3-105">Comportamento do EF5 e versões anteriores</span><span class="sxs-lookup"><span data-stu-id="ed9d3-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="ed9d3-106">Há dois construtores que aceitam conexões:</span><span class="sxs-lookup"><span data-stu-id="ed9d3-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="ed9d3-107">É possível usá-los, mas você tem que trabalhar em torno de algumas limitações:</span><span class="sxs-lookup"><span data-stu-id="ed9d3-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="ed9d3-108">Se você passar uma conexão aberta para qualquer uma delas, em seguida, na primeira vez em que o framework tenta usá-lo que um InvalidOperationException é gerado indicando que não é possível reabrir uma conexão já aberta.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="ed9d3-109">O sinalizador dos contextOwnsConnection é interpretado como se a conexão de armazenamento subjacente deve ser descartado quando o contexto for descartado.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="ed9d3-110">Mas, independentemente dessa configuração, a conexão de armazenamento sempre é fechado quando o contexto for descartado.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="ed9d3-111">Portanto, se você tiver mais de um DbContext com a mesma conexão seja qual for o contexto for descartado primeiro fechará a conexão (da mesma forma se uma conexão ADO.NET existente com um DbContext tem uma combinação, DbContext sempre fechará a conexão quando ela é descartada) .</span><span class="sxs-lookup"><span data-stu-id="ed9d3-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="ed9d3-112">É possível contornar a limitação primeiro acima passando uma conexão fechada e executar apenas o código que seria abertos depois que todos os contextos foram criados:</span><span class="sxs-lookup"><span data-stu-id="ed9d3-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="ed9d3-113">A segunda limitação apenas significa que você precisa evitar descartar a qualquer um dos seus objetos DbContext até estar pronto para a conexão ser fechada.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="ed9d3-114">Comportamento no EF6 e versões futuras</span><span class="sxs-lookup"><span data-stu-id="ed9d3-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="ed9d3-115">No EF6 e versões futuras DbContext tem os mesmos dois construtores, mas não exige mais que a conexão passada para o construtor ser fechado quando ela é recebida.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="ed9d3-116">Portanto, agora é possível:</span><span class="sxs-lookup"><span data-stu-id="ed9d3-116">So this is now possible:</span></span>  

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

<span data-ttu-id="ed9d3-117">Também o sinalizador dos contextOwnsConnection agora controla ou não a conexão está fechado e descartado quando o DbContext for descartado.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="ed9d3-118">Portanto, no exemplo acima a conexão não for fechada quando o contexto é descartado (linha 32), como era nas versões anteriores do EF, mas em vez disso, quando a conexão em si é descartado (linha 40).</span><span class="sxs-lookup"><span data-stu-id="ed9d3-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="ed9d3-119">Certamente ainda é possível para o DbContext para assumir o controle da conexão (apenas conjunto contextOwnsConnection como verdadeira ou use um dos outros construtores) se desejar.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="ed9d3-120">Há algumas considerações adicionais ao usar transações com esse novo modelo.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="ed9d3-121">Para obter detalhes, consulte [trabalhando com transações](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="ed9d3-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="ed9d3-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="ed9d3-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="ed9d3-123">Comportamento do EF5 e versões anteriores</span><span class="sxs-lookup"><span data-stu-id="ed9d3-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="ed9d3-124">Há um bug no EF5 e versões anteriores, de modo que o **ObjectContext.Connection.State** não foi atualizado para refletir o estado real da conexão de armazenamento subjacente.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="ed9d3-125">Por exemplo, se você executasse o código a seguir você pode ser retornado o status **fechado** mesmo que na verdade subjacente armazenar a conexão é **abrir**.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="ed9d3-126">Separadamente, se você abrir a conexão de banco de dados chamando Database.Connection.Open() será aberto até da próxima vez que você executa uma consulta ou chama algo que requer uma conexão de banco de dados (por exemplo, SaveChanges()), mas após que subjacente armazena conexão será fechada.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="ed9d3-127">O contexto será abrir novamente e Fechar novamente a conexão sempre que outra operação de banco de dados é necessária:</span><span class="sxs-lookup"><span data-stu-id="ed9d3-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="ed9d3-128">Comportamento no EF6 e versões futuras</span><span class="sxs-lookup"><span data-stu-id="ed9d3-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="ed9d3-129">Para versões futuras e EF6 é que nós tiramos a abordagem que se o código de chamada optar por abrir a conexão com o contexto de chamada. Database.Connection.Open() e em seguida, ele tem um bom motivo para fazer isso e o framework assumirá que ele deseja controle sobre a abertura e fechamento da conexão e não fechará automaticamente a conexão.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="ed9d3-130">Potencialmente, isso pode levar a conexões que estão abertas para um longo tempo, portanto, use com cuidado.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="ed9d3-131">Também atualizamos o código para que ObjectContext.Connection.State agora mantém controle sobre o estado da conexão subjacente corretamente.</span><span class="sxs-lookup"><span data-stu-id="ed9d3-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
