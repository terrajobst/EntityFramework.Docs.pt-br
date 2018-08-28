---
title: Testar com SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996862"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="e1973-102">Testar com SQLite</span><span class="sxs-lookup"><span data-stu-id="e1973-102">Testing with SQLite</span></span>

<span data-ttu-id="e1973-103">SQLite tem um modo na memória que permite que você use o SQLite para escrever testes em relação a um banco de dados relacional, sem a sobrecarga de operações de banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="e1973-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="e1973-104">Você pode exibir este artigo [amostra](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) no GitHub</span><span class="sxs-lookup"><span data-stu-id="e1973-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="e1973-105">Cenário de teste de exemplo</span><span class="sxs-lookup"><span data-stu-id="e1973-105">Example testing scenario</span></span>

<span data-ttu-id="e1973-106">Considere o seguinte serviço que permite que o código do aplicativo executar algumas operações relacionadas a blogs.</span><span class="sxs-lookup"><span data-stu-id="e1973-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="e1973-107">Ele usa internamente um `DbContext` que se conecta a um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e1973-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="e1973-108">Seria útil trocar neste contexto, como se conectar ao banco de dados SQLite na memória para que podemos escrever testes eficientes para esse serviço sem precisar modificar o código ou fazer muito trabalho para criar um teste duplo do contexto.</span><span class="sxs-lookup"><span data-stu-id="e1973-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="e1973-109">Prepare seu contexto</span><span class="sxs-lookup"><span data-stu-id="e1973-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="e1973-110">Evite configurar dois provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="e1973-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="e1973-111">Em seus testes você pretende configurar o contexto para usar o provedor InMemory externamente.</span><span class="sxs-lookup"><span data-stu-id="e1973-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="e1973-112">Se você estiver configurando um provedor de banco de dados por meio da substituição `OnConfiguring` em seu contexto, em seguida, você precisará adicionar algum código condicional para garantir que apenas configurar o provedor de banco de dados se um já não tiver sido configurado.</span><span class="sxs-lookup"><span data-stu-id="e1973-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="e1973-113">Se você estiver usando o ASP.NET Core, em seguida, você não deve precisar esse código, pois o provedor de banco de dados estiver configurado fora do contexto (em Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="e1973-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="e1973-114">Adicione um construtor para teste</span><span class="sxs-lookup"><span data-stu-id="e1973-114">Add a constructor for testing</span></span>

<span data-ttu-id="e1973-115">A maneira mais simples de habilitar o teste em relação a outro banco de dados é modificar o contexto para expor um construtor que aceita um `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="e1973-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="e1973-116">`DbContextOptions<TContext>` informa o contexto de todas as suas configurações, como qual banco de dados para se conectar ao.</span><span class="sxs-lookup"><span data-stu-id="e1973-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="e1973-117">Isso é o mesmo objeto que é criado, executando o método OnConfiguring em seu contexto.</span><span class="sxs-lookup"><span data-stu-id="e1973-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="e1973-118">Escrever testes</span><span class="sxs-lookup"><span data-stu-id="e1973-118">Writing tests</span></span>

<span data-ttu-id="e1973-119">A chave para teste com esse provedor é a capacidade de informar o contexto para usar o SQLite e controlar o escopo do banco de dados na memória.</span><span class="sxs-lookup"><span data-stu-id="e1973-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="e1973-120">O escopo do banco de dados é controlado pelo abrindo e fechando a conexão.</span><span class="sxs-lookup"><span data-stu-id="e1973-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="e1973-121">O banco de dados está no escopo para a duração em que a conexão está aberta.</span><span class="sxs-lookup"><span data-stu-id="e1973-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="e1973-122">Normalmente você deseja limpar um banco de dados para cada método de teste.</span><span class="sxs-lookup"><span data-stu-id="e1973-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
