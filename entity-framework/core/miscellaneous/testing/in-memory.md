---
title: Testando com InMemory – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: f814c8955e155688bb5e8d34b9c9f6d24dcc6601
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900256"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="a0603-102">Testando com InMemory</span><span class="sxs-lookup"><span data-stu-id="a0603-102">Testing with InMemory</span></span>

<span data-ttu-id="a0603-103">O provedor InMemory é útil quando você deseja testar componentes usando algo que se aproxime da conexão ao banco de dados real, sem a sobrecarga de operações de banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="a0603-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="a0603-104">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="a0603-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="a0603-105">InMemory não é um banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="a0603-105">InMemory is not a relational database</span></span>

<span data-ttu-id="a0603-106">Provedores de banco de dados do EF Core não precisa ser bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="a0603-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="a0603-107">InMemory é projetado para ser um banco de dados de uso geral para teste e não foi projetado para simular um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="a0603-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="a0603-108">Alguns exemplos incluem:</span><span class="sxs-lookup"><span data-stu-id="a0603-108">Some examples of this include:</span></span>

* <span data-ttu-id="a0603-109">InMemory permitirá que você salve os dados que possam violar as restrições de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="a0603-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="a0603-110">Se você usar DefaultValueSql(string) para uma propriedade em seu modelo, isso é uma API de banco de dados relacional e não terá efeito ao executar em InMemory.</span><span class="sxs-lookup"><span data-stu-id="a0603-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="a0603-111">[Simultaneidade por meio da versão de linha/carimbo de hora](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` ou `IsRowVersion`) não tem suporte.</span><span class="sxs-lookup"><span data-stu-id="a0603-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="a0603-112">Não [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) será lançada se uma atualização é feita usando um token de simultaneidade antigo.</span><span class="sxs-lookup"><span data-stu-id="a0603-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="a0603-113">Para muitos fins de teste, essas diferenças não serão relevante.</span><span class="sxs-lookup"><span data-stu-id="a0603-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="a0603-114">No entanto, se você quiser testar em relação a algo que se comporta como um verdadeiro banco de dados relacional, considere usar [modo na memória do SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="a0603-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="a0603-115">Cenário de teste de exemplo</span><span class="sxs-lookup"><span data-stu-id="a0603-115">Example testing scenario</span></span>

<span data-ttu-id="a0603-116">Considere o seguinte serviço que permite que o código do aplicativo executar algumas operações relacionadas a blogs.</span><span class="sxs-lookup"><span data-stu-id="a0603-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="a0603-117">Ele usa internamente um `DbContext` que se conecta a um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a0603-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="a0603-118">Seria útil trocar neste contexto, como se conectar a um banco de dados InMemory para que podemos escrever testes eficientes para esse serviço sem precisar modificar o código ou fazer muito trabalho para criar um teste duplo do contexto.</span><span class="sxs-lookup"><span data-stu-id="a0603-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="a0603-119">Prepare seu contexto</span><span class="sxs-lookup"><span data-stu-id="a0603-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="a0603-120">Evite configurar dois provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="a0603-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="a0603-121">Em seus testes você pretende configurar o contexto para usar o provedor InMemory externamente.</span><span class="sxs-lookup"><span data-stu-id="a0603-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="a0603-122">Se você estiver configurando um provedor de banco de dados por meio da substituição `OnConfiguring` em seu contexto, em seguida, você precisará adicionar algum código condicional para garantir que apenas configurar o provedor de banco de dados se um já não tiver sido configurado.</span><span class="sxs-lookup"><span data-stu-id="a0603-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="a0603-123">Se você estiver usando o ASP.NET Core, em seguida, você não deve precisar esse código, pois o provedor de banco de dados já estiver configurado fora do contexto (em Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="a0603-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="a0603-124">Adicione um construtor para teste</span><span class="sxs-lookup"><span data-stu-id="a0603-124">Add a constructor for testing</span></span>

<span data-ttu-id="a0603-125">A maneira mais simples de habilitar o teste em relação a outro banco de dados é modificar o contexto para expor um construtor que aceita um `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="a0603-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="a0603-126">`DbContextOptions<TContext>` informa o contexto de todas as suas configurações, como qual banco de dados para se conectar ao.</span><span class="sxs-lookup"><span data-stu-id="a0603-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="a0603-127">Isso é o mesmo objeto que é criado, executando o método OnConfiguring em seu contexto.</span><span class="sxs-lookup"><span data-stu-id="a0603-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="a0603-128">Escrever testes</span><span class="sxs-lookup"><span data-stu-id="a0603-128">Writing tests</span></span>

<span data-ttu-id="a0603-129">A chave para teste com esse provedor é a capacidade de informar o contexto para usar o provedor InMemory e controlar o escopo do banco de dados na memória.</span><span class="sxs-lookup"><span data-stu-id="a0603-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="a0603-130">Normalmente você deseja limpar um banco de dados para cada método de teste.</span><span class="sxs-lookup"><span data-stu-id="a0603-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="a0603-131">Aqui está um exemplo de uma classe de teste que usa o banco de dados InMemory.</span><span class="sxs-lookup"><span data-stu-id="a0603-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="a0603-132">Cada método de teste especifica um nome de banco de dados exclusivo, que significa que cada método possui seu próprio banco de dados InMemory.</span><span class="sxs-lookup"><span data-stu-id="a0603-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="a0603-133">Para usar o `.UseInMemoryDatabase()` método de extensão, o pacote NuGet de referência `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="a0603-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
