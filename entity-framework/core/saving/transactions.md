---
title: Transações – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 7083a1228420416a1b60d9744ca2dad2339be53f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993599"
---
# <a name="using-transactions"></a><span data-ttu-id="25784-102">Como usar transações</span><span class="sxs-lookup"><span data-stu-id="25784-102">Using Transactions</span></span>

<span data-ttu-id="25784-103">As transações permitem que várias operações de banco de dados sejam processadas de forma atômica.</span><span class="sxs-lookup"><span data-stu-id="25784-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="25784-104">Se a transação for confirmada, todas as operações serão aplicadas com êxito ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25784-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="25784-105">Se a transação for revertida, nenhuma operação será aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25784-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="25784-106">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="25784-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="25784-107">Comportamento de transação padrão</span><span class="sxs-lookup"><span data-stu-id="25784-107">Default transaction behavior</span></span>

<span data-ttu-id="25784-108">Por padrão, se o provedor de banco de dados oferecer suporte a transações, todas as alterações em uma única chamada para `SaveChanges()` serão aplicadas em uma transação.</span><span class="sxs-lookup"><span data-stu-id="25784-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="25784-109">Se qualquer uma das alterações falhar, a transação é revertida e nenhuma das alterações será aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25784-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="25784-110">Isso significa que é garantido que o `SaveChanges()` terá êxito ou sairá do banco de dados sem modificação caso ocorra algum erro.</span><span class="sxs-lookup"><span data-stu-id="25784-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="25784-111">Para a maioria dos aplicativos, esse comportamento padrão é suficiente.</span><span class="sxs-lookup"><span data-stu-id="25784-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="25784-112">Você só deve controlar as transações manualmente se os requisitos do aplicativo considerarem necessário.</span><span class="sxs-lookup"><span data-stu-id="25784-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="25784-113">Como controlar transações</span><span class="sxs-lookup"><span data-stu-id="25784-113">Controlling transactions</span></span>

<span data-ttu-id="25784-114">Você pode usar a API do `DbContext.Database` para iniciar, confirmar e reverter transações.</span><span class="sxs-lookup"><span data-stu-id="25784-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="25784-115">O exemplo a seguir mostra duas operações do `SaveChanges()` e uma consulta LINQ sendo executada em uma única transação.</span><span class="sxs-lookup"><span data-stu-id="25784-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="25784-116">Nem todos os provedores de banco de dados oferecem suporte a transações.</span><span class="sxs-lookup"><span data-stu-id="25784-116">Not all database providers support transactions.</span></span> <span data-ttu-id="25784-117">Alguns provedores podem gerar ou ficar não operacional quando as APIs de transação forem chamadas.</span><span class="sxs-lookup"><span data-stu-id="25784-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="25784-118">Transação entre diferentes contextos (somente bancos de dados relacionais)</span><span class="sxs-lookup"><span data-stu-id="25784-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="25784-119">Você também pode compartilhar uma transação em várias instâncias de contexto.</span><span class="sxs-lookup"><span data-stu-id="25784-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="25784-120">Esta funcionalidade só está disponível ao usar um provedor de banco de dados relacional, porque ele requer o uso do `DbTransaction` e `DbConnection`, que são específicos para bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="25784-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="25784-121">Para compartilhar uma transação, os contextos devem compartilhar um `DbConnection` e um `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="25784-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="25784-122">Permitir que a conexão seja fornecido externamente</span><span class="sxs-lookup"><span data-stu-id="25784-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="25784-123">Compartilhar um `DbConnection` requer a capacidade de aprovar uma conexão em um contexto ao construí-la.</span><span class="sxs-lookup"><span data-stu-id="25784-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="25784-124">A maneira mais fácil de permitir que o `DbConnection` seja fornecido externamente é parar de usar o método `DbContext.OnConfiguring` para configurar o contexto e criar externamente `DbContextOptions` e aprová-los para o construtor de contexto.</span><span class="sxs-lookup"><span data-stu-id="25784-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="25784-125">`DbContextOptionsBuilder` é a API usada no `DbContext.OnConfiguring` para configurar o contexto e agora você irá usá-la externamente para criar `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="25784-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="25784-126">Uma alternativa é continuar usando o `DbContext.OnConfiguring`, mas aceitar um `DbConnection` que seja salvo e, depois, usado no `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="25784-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="25784-127">Compartilhar uma conexão e transação</span><span class="sxs-lookup"><span data-stu-id="25784-127">Share connection and transaction</span></span>

<span data-ttu-id="25784-128">Agora você pode criar várias instâncias de contexto que compartilham a mesma conexão.</span><span class="sxs-lookup"><span data-stu-id="25784-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="25784-129">Em seguida, use a API do `DbContext.Database.UseTransaction(DbTransaction)` para inscrever os dois contextos na mesma transação.</span><span class="sxs-lookup"><span data-stu-id="25784-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="25784-130">Usando DbTransactions externos (somente bancos de dados relacionais)</span><span class="sxs-lookup"><span data-stu-id="25784-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="25784-131">Se você estiver usando várias tecnologias de acesso a dados para acessar um banco de dados relacional, é possível compartilhar uma transação entre operações executadas por essas diferentes tecnologias.</span><span class="sxs-lookup"><span data-stu-id="25784-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="25784-132">O exemplo a seguir mostra como executar uma operação do SqlClient do ADO.NET e uma operação do Entity Framework Core na mesma transação.</span><span class="sxs-lookup"><span data-stu-id="25784-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="25784-133">Como usar System.Transactions</span><span class="sxs-lookup"><span data-stu-id="25784-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="25784-134">Este recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="25784-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="25784-135">É possível usar transações ambientes se você precisar coordenar um escopo mais amplo.</span><span class="sxs-lookup"><span data-stu-id="25784-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="25784-136">Também é possível se inscrever em uma transação explícita.</span><span class="sxs-lookup"><span data-stu-id="25784-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="25784-137">Limitações do System.Transactions</span><span class="sxs-lookup"><span data-stu-id="25784-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="25784-138">O EF Core depende dos provedores de banco de dados para implementar o suporte para System.Transactions.</span><span class="sxs-lookup"><span data-stu-id="25784-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="25784-139">Embora o suporte seja muito comum entre os provedores de ADO.NET para .NET Framework, a API só foi adicionada recentemente ao .NET Core e, portanto, o suporte ainda não está tão difundido.</span><span class="sxs-lookup"><span data-stu-id="25784-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="25784-140">Se um provedor não implementar o suporte para System.Transactions, é possível que as chamadas para essas APIs sejam ignoradas completamente.</span><span class="sxs-lookup"><span data-stu-id="25784-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="25784-141">O SqlClient para .NET Core não oferecerá suporte do 2.1 em diante.</span><span class="sxs-lookup"><span data-stu-id="25784-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="25784-142">O SqlClient para .NET Core 2.0 gerará uma exceção se você tentar usar o recurso.</span><span class="sxs-lookup"><span data-stu-id="25784-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="25784-143">Recomendamos testar se a API funciona corretamente com seu provedor antes de depender dela para o gerenciamento de transações.</span><span class="sxs-lookup"><span data-stu-id="25784-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="25784-144">Caso ela não funcione, fale com o mantenedor do provedor do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25784-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="25784-145">A partir da versão 2.1, a implementação do System.Transactions no .NET Core não incluirá o suporte a transações distribuídas, portanto, você não pode usar `TransactionScope` ou `CommittableTransaction` para coordenar transações em vários gerenciadores de recursos.</span><span class="sxs-lookup"><span data-stu-id="25784-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
