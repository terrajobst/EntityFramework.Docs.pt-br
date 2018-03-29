---
title: Transações - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: fe4c0d6ad7ccb2e97dc94fbf2eb26a41e7fbcb19
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="using-transactions"></a><span data-ttu-id="e5fa2-102">Usando transações</span><span class="sxs-lookup"><span data-stu-id="e5fa2-102">Using Transactions</span></span>

<span data-ttu-id="e5fa2-103">Transações permitem que várias operações de banco de dados a serem processadas de forma atômica.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="e5fa2-104">Se a transação for confirmada, todas as operações são aplicadas com êxito no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="e5fa2-105">Se a transação for revertida, nenhuma das operações são aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="e5fa2-106">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="e5fa2-107">Comportamento de transação padrão</span><span class="sxs-lookup"><span data-stu-id="e5fa2-107">Default transaction behavior</span></span>

<span data-ttu-id="e5fa2-108">Por padrão, se o provedor de banco de dados oferece suporte a transações, todas as alterações em uma única chamada para `SaveChanges()` são aplicados em uma transação.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="e5fa2-109">Se qualquer uma das alterações falhar, em seguida, a transação é revertida e nenhuma das alterações são aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="e5fa2-110">Isso significa que `SaveChanges()` é garantido completamente ser bem-sucedida, ou deixe o banco de dados não modificado se ocorrer um erro.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="e5fa2-111">Para a maioria dos aplicativos, esse comportamento padrão é suficiente.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="e5fa2-112">Você deve controlar transações somente manualmente se seus requisitos de aplicativo julguem necessário.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="e5fa2-113">Controle de transações</span><span class="sxs-lookup"><span data-stu-id="e5fa2-113">Controlling transactions</span></span>

<span data-ttu-id="e5fa2-114">Você pode usar o `DbContext.Database` API para iniciar, confirmar e reverter transações.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="e5fa2-115">O exemplo a seguir mostra duas `SaveChanges()` operações e um LINQ de consulta que está sendo executada em uma única transação.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="e5fa2-116">Nem todos os provedores de banco de dados oferecer suporte a transações.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-116">Not all database providers support transactions.</span></span> <span data-ttu-id="e5fa2-117">Alguns provedores podem gerar ou não-operacional quando transações são chamadas de APIs.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="e5fa2-118">Transação de contexto entre (bancos de dados relacionais somente)</span><span class="sxs-lookup"><span data-stu-id="e5fa2-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="e5fa2-119">Você também pode compartilhar uma transação entre várias instâncias de contexto.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="e5fa2-120">Essa funcionalidade está disponível somente ao usar um provedor de banco de dados relacional, porque ele requer o uso de `DbTransaction` e `DbConnection`, que são específicos para bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="e5fa2-121">Para compartilhar uma transação, os contextos devem compartilhar ambos um `DbConnection` e um `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="e5fa2-122">Permitir a conexão a ser fornecido externamente</span><span class="sxs-lookup"><span data-stu-id="e5fa2-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="e5fa2-123">Compartilhando um `DbConnection` requer a capacidade de transmitir uma conexão em um contexto ao construir a ele.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="e5fa2-124">A maneira mais fácil para permitir `DbConnection` a ser fornecido externamente é parar de usar o `DbContext.OnConfiguring` método para configurar o contexto e criar externamente `DbContextOptions` e passá-los para o construtor de contexto.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="e5fa2-125">`DbContextOptionsBuilder` é a API usada na `DbContext.OnConfiguring` para configurar o contexto, você agora vai usá-lo externamente para criar `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="e5fa2-126">Uma alternativa é continuar usando `DbContext.OnConfiguring`, mas aceitam uma `DbConnection` que é salvo e, em seguida, usado em `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a><span data-ttu-id="e5fa2-127">Compartilhamento de conexão e de transação</span><span class="sxs-lookup"><span data-stu-id="e5fa2-127">Share connection and transaction</span></span>

<span data-ttu-id="e5fa2-128">Agora você pode criar várias instâncias de contexto que compartilham a mesma conexão.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="e5fa2-129">Em seguida, use o `DbContext.Database.UseTransaction(DbTransaction)` API para inscrever-se os dois contextos na mesma transação.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="e5fa2-130">Usando DbTransactions externo (bancos de dados relacionais somente)</span><span class="sxs-lookup"><span data-stu-id="e5fa2-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="e5fa2-131">Se você estiver usando várias tecnologias de acesso a dados para acessar um banco de dados relacional, pode ser que você deseja compartilhar uma transação entre operações executadas por essas tecnologias diferentes.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="e5fa2-132">O exemplo a seguir mostra como executar uma operação do SqlClient do ADO.NET e uma operação de Entity Framework Core na mesma transação.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="e5fa2-133">Usando System. Transactions</span><span class="sxs-lookup"><span data-stu-id="e5fa2-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="e5fa2-134">Esse recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e5fa2-135">É possível usar as transações ambiente se você precisar coordenar em um escopo mais amplo.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="e5fa2-136">Também é possível se inscrever em uma transação explícita.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="e5fa2-137">Limitações de System. Transactions</span><span class="sxs-lookup"><span data-stu-id="e5fa2-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="e5fa2-138">Núcleo EF depende de provedores de banco de dados para implementar o suporte para System. Transactions.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="e5fa2-139">Embora o suporte é muito comum entre os provedores ADO.NET para .NET Framework, a API só foi adicionado recentemente ao .NET Core e suporte, portanto, não estará tão difundido.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="e5fa2-140">Se um provedor não implementa o suporte para System. Transactions, é possível que chamadas para essas APIs serão ignoradas completamente.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="e5fa2-141">SqlClient para .NET Core dá suporte a ele em 2.1 em diante.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="e5fa2-142">SqlClient para .NET Core 2.0 lançará uma exceção de você tentar usar o recurso.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="e5fa2-143">É recomendável que você teste que a API funcione corretamente com seu provedor antes de você confiar para o gerenciamento de transações.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="e5fa2-144">Você é incentivado a entrar em contato com o mantenedor do provedor de banco de dados se não existir.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="e5fa2-145">A partir da versão 2.1, a implementação de System. Transactions no núcleo do .NET não dá suporte para transações distribuídas, portanto você não pode usar `TransactionScope` ou `CommitableTransaction` para coordenar transações em vários gerenciadores de recursos.</span><span class="sxs-lookup"><span data-stu-id="e5fa2-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
