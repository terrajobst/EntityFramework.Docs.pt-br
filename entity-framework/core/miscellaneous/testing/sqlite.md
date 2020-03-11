---
title: Testando com o SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417298"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="f04dd-102">Testar com SQLite</span><span class="sxs-lookup"><span data-stu-id="f04dd-102">Testing with SQLite</span></span>

<span data-ttu-id="f04dd-103">O SQLite tem um modo na memória que permite usar o SQLite para gravar testes em relação a um banco de dados relacional, sem a sobrecarga das operações reais do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f04dd-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="f04dd-104">Você pode exibir o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) deste artigo no github</span><span class="sxs-lookup"><span data-stu-id="f04dd-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="f04dd-105">Cenário de teste de exemplo</span><span class="sxs-lookup"><span data-stu-id="f04dd-105">Example testing scenario</span></span>

<span data-ttu-id="f04dd-106">Considere o seguinte serviço que permite que o código do aplicativo execute algumas operações relacionadas a Blogs.</span><span class="sxs-lookup"><span data-stu-id="f04dd-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="f04dd-107">Internamente, ele usa um `DbContext` que se conecta a um banco de dados SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f04dd-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="f04dd-108">Seria útil trocar esse contexto para se conectar a um banco de dados SQLite na memória para que possamos escrever testes eficientes para esse serviço sem precisar modificar o código ou fazer muito trabalho para criar um duplo de teste do contexto.</span><span class="sxs-lookup"><span data-stu-id="f04dd-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="f04dd-109">Prepare seu contexto</span><span class="sxs-lookup"><span data-stu-id="f04dd-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="f04dd-110">Evite configurar dois provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="f04dd-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="f04dd-111">Em seus testes, você vai configurar externamente o contexto para usar o provedor de inmemory.</span><span class="sxs-lookup"><span data-stu-id="f04dd-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="f04dd-112">Se você estiver configurando um provedor de banco de dados substituindo `OnConfiguring` em seu contexto, será necessário adicionar algum código condicional para garantir que você só configure o provedor de banco de dados se ainda não tiver sido configurado.</span><span class="sxs-lookup"><span data-stu-id="f04dd-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="f04dd-113">Se você estiver usando ASP.NET Core, não deverá precisar desse código, pois o provedor de banco de dados está configurado fora do contexto (em Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="f04dd-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="f04dd-114">Adicionar um construtor para teste</span><span class="sxs-lookup"><span data-stu-id="f04dd-114">Add a constructor for testing</span></span>

<span data-ttu-id="f04dd-115">A maneira mais simples de habilitar os testes em relação a um banco de dados diferente é modificar o contexto para expor um construtor que aceite um `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="f04dd-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="f04dd-116">`DbContextOptions<TContext>` informa ao contexto todas as suas configurações, como a qual banco de dados se conectar.</span><span class="sxs-lookup"><span data-stu-id="f04dd-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="f04dd-117">Esse é o mesmo objeto criado com a execução do método onconfiguring em seu contexto.</span><span class="sxs-lookup"><span data-stu-id="f04dd-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="f04dd-118">Escrevendo testes</span><span class="sxs-lookup"><span data-stu-id="f04dd-118">Writing tests</span></span>

<span data-ttu-id="f04dd-119">A chave para testar esse provedor é a capacidade de informar ao contexto para usar o SQLite e controlar o escopo do banco de dados na memória.</span><span class="sxs-lookup"><span data-stu-id="f04dd-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="f04dd-120">O escopo do banco de dados é controlado abrindo e fechando a conexão.</span><span class="sxs-lookup"><span data-stu-id="f04dd-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="f04dd-121">O banco de dados tem o escopo definido para a duração em que a conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="f04dd-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="f04dd-122">Normalmente, você deseja um banco de dados limpo para cada método de teste.</span><span class="sxs-lookup"><span data-stu-id="f04dd-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="f04dd-123">Para usar `SqliteConnection()` e o método de extensão de `.UseSqlite()`, referencie o pacote NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="f04dd-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
