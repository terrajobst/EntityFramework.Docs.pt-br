---
title: Testando com InMemory - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="37559-102">Testando com InMemory</span><span class="sxs-lookup"><span data-stu-id="37559-102">Testing with InMemory</span></span>

<span data-ttu-id="37559-103">O provedor InMemory é útil quando você deseja testar componentes usando algo que aproxima a conexão com o banco de dados real, sem a sobrecarga de operações de banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="37559-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="37559-104">Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="37559-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="37559-105">InMemory não é um banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="37559-105">InMemory is not a relational database</span></span>

<span data-ttu-id="37559-106">Provedores de banco de dados principal EF não precisa ser bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="37559-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="37559-107">InMemory é projetado para ser um banco de dados de uso geral para teste e não foi projetado para simular um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="37559-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="37559-108">Alguns exemplos incluem:</span><span class="sxs-lookup"><span data-stu-id="37559-108">Some examples of this include:</span></span>
* <span data-ttu-id="37559-109">InMemory permitirá salvar os dados que possam violar as restrições de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="37559-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="37559-110">Se você usar DefaultValueSql(string) para uma propriedade em seu modelo, isso é uma API de banco de dados relacional e não terá efeito ao executar em InMemory.</span><span class="sxs-lookup"><span data-stu-id="37559-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="37559-111">Para muitos fins de teste essas diferenças não serão importante.</span><span class="sxs-lookup"><span data-stu-id="37559-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="37559-112">No entanto, se você quiser testar em relação a algo que se comporta como um banco de dados relacional true, considere usar [modo na memória de SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="37559-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="37559-113">Exemplo de cenário de teste</span><span class="sxs-lookup"><span data-stu-id="37559-113">Example testing scenario</span></span>

<span data-ttu-id="37559-114">Considere o seguinte serviço que permite que o código do aplicativo executar algumas operações relacionadas à blogs.</span><span class="sxs-lookup"><span data-stu-id="37559-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="37559-115">Ele usa internamente um `DbContext` que se conecta a um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="37559-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="37559-116">Seria útil trocar este contexto para se conectar a um banco de dados InMemory para que podemos escrever testes eficientes para esse serviço sem a necessidade de modificar o código ou muito trabalho para criar um teste duplo do contexto.</span><span class="sxs-lookup"><span data-stu-id="37559-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="37559-117">Prepare seu contexto</span><span class="sxs-lookup"><span data-stu-id="37559-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="37559-118">Evite configurar dois provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="37559-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="37559-119">Os testes que serão externamente, configurar o contexto para usar o provedor de InMemory.</span><span class="sxs-lookup"><span data-stu-id="37559-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="37559-120">Se você estiver configurando um provedor de banco de dados, substituindo `OnConfiguring` em seu contexto, em seguida, você precisa adicionar código condicional para garantir que você configure apenas o provedor de banco de dados se um já não tiver sido configurado.</span><span class="sxs-lookup"><span data-stu-id="37559-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="37559-121">Se você estiver usando o ASP.NET Core, em seguida, não será preciso esse código como o provedor de banco de dados já está configurado fora do contexto (em Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="37559-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="37559-122">Adicione um construtor para teste</span><span class="sxs-lookup"><span data-stu-id="37559-122">Add a constructor for testing</span></span>

<span data-ttu-id="37559-123">É a maneira mais simples para habilitar o teste em relação a um banco de dados diferente modificar o contexto para expor um construtor que aceite um `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="37559-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="37559-124">`DbContextOptions<TContext>`informa o contexto de todas as suas configurações, como o banco de dados para se conectar ao.</span><span class="sxs-lookup"><span data-stu-id="37559-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="37559-125">Este é o mesmo objeto que é criado ao executar o método OnConfiguring em seu contexto.</span><span class="sxs-lookup"><span data-stu-id="37559-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="37559-126">Testes de gravação</span><span class="sxs-lookup"><span data-stu-id="37559-126">Writing tests</span></span>

<span data-ttu-id="37559-127">A chave de teste com esse provedor é a capacidade para informar o contexto para usar o provedor InMemory e controlar o escopo do banco de dados na memória.</span><span class="sxs-lookup"><span data-stu-id="37559-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="37559-128">Normalmente você deseja limpar um banco de dados para cada método de teste.</span><span class="sxs-lookup"><span data-stu-id="37559-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="37559-129">Aqui está um exemplo de uma classe de teste que usa o banco de dados de InMemory.</span><span class="sxs-lookup"><span data-stu-id="37559-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="37559-130">Cada método de teste especifica um nome de banco de dados exclusivo, que significa que cada método tem seu próprio banco de dados de InMemory.</span><span class="sxs-lookup"><span data-stu-id="37559-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="37559-131">Para usar o `.UseInMemoryDatabase()` método de extensão, o pacote Nuget de referência `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="37559-131">To use the `.UseInMemoryDatabase()` extension method, reference the Nuget package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
