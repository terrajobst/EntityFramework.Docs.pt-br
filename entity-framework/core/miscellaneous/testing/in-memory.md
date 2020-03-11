---
title: Testando com inmemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416505"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="fc26e-102">Testar com InMemory</span><span class="sxs-lookup"><span data-stu-id="fc26e-102">Testing with InMemory</span></span>

<span data-ttu-id="fc26e-103">O provedor de inmemory é útil quando você deseja testar os componentes usando algo que aproxima a conexão com o banco de dados real, sem a sobrecarga das operações reais do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc26e-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="fc26e-104">Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="fc26e-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="fc26e-105">Inmemory não é um banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="fc26e-105">InMemory is not a relational database</span></span>

<span data-ttu-id="fc26e-106">Os provedores de banco de dados EF Core não precisam ser bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="fc26e-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="fc26e-107">A inmemory foi projetada para ser um banco de dados de uso geral para teste e não é projetada para imitar um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="fc26e-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="fc26e-108">Alguns exemplos disso incluem:</span><span class="sxs-lookup"><span data-stu-id="fc26e-108">Some examples of this include:</span></span>

* <span data-ttu-id="fc26e-109">A inmemory permitirá que você salve os dados que violariam as restrições de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="fc26e-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="fc26e-110">Se você usar DefaultValueSql (cadeia de caracteres) para uma propriedade em seu modelo, essa será uma API de banco de dados relacional e não terá nenhum efeito durante a execução na inmemory.</span><span class="sxs-lookup"><span data-stu-id="fc26e-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="fc26e-111">Não há suporte para a [simultaneidade via versão de carimbo de data/hora/linha](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` ou `IsRowVersion`).</span><span class="sxs-lookup"><span data-stu-id="fc26e-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="fc26e-112">Nenhum [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) será gerado se uma atualização for feita usando um token de simultaneidade antigo.</span><span class="sxs-lookup"><span data-stu-id="fc26e-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="fc26e-113">Para muitos fins de teste, essas diferenças não importam.</span><span class="sxs-lookup"><span data-stu-id="fc26e-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="fc26e-114">No entanto, se você quiser testar algo que se comporta mais como um banco de dados relacional verdadeiro, considere o uso [do SQLite no modo de memória](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="fc26e-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="fc26e-115">Cenário de teste de exemplo</span><span class="sxs-lookup"><span data-stu-id="fc26e-115">Example testing scenario</span></span>

<span data-ttu-id="fc26e-116">Considere o seguinte serviço que permite que o código do aplicativo execute algumas operações relacionadas a Blogs.</span><span class="sxs-lookup"><span data-stu-id="fc26e-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="fc26e-117">Internamente, ele usa um `DbContext` que se conecta a um banco de dados SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc26e-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="fc26e-118">Seria útil trocar esse contexto para se conectar a um banco de dados de inmemory, de forma que possamos escrever testes eficientes para esse serviço sem precisar modificar o código ou fazer muito trabalho para criar um duplo de teste do contexto.</span><span class="sxs-lookup"><span data-stu-id="fc26e-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="fc26e-119">Prepare seu contexto</span><span class="sxs-lookup"><span data-stu-id="fc26e-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="fc26e-120">Evite configurar dois provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="fc26e-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="fc26e-121">Em seus testes, você vai configurar externamente o contexto para usar o provedor de inmemory.</span><span class="sxs-lookup"><span data-stu-id="fc26e-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="fc26e-122">Se você estiver configurando um provedor de banco de dados substituindo `OnConfiguring` em seu contexto, será necessário adicionar algum código condicional para garantir que você só configure o provedor de banco de dados se ainda não tiver sido configurado.</span><span class="sxs-lookup"><span data-stu-id="fc26e-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="fc26e-123">Se você estiver usando ASP.NET Core, não deverá precisar desse código, pois o provedor de banco de dados já está configurado fora do contexto (em Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="fc26e-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="fc26e-124">Adicionar um construtor para teste</span><span class="sxs-lookup"><span data-stu-id="fc26e-124">Add a constructor for testing</span></span>

<span data-ttu-id="fc26e-125">A maneira mais simples de habilitar os testes em relação a um banco de dados diferente é modificar o contexto para expor um construtor que aceite um `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="fc26e-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="fc26e-126">`DbContextOptions<TContext>` informa ao contexto todas as suas configurações, como a qual banco de dados se conectar.</span><span class="sxs-lookup"><span data-stu-id="fc26e-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="fc26e-127">Esse é o mesmo objeto criado com a execução do método onconfiguring em seu contexto.</span><span class="sxs-lookup"><span data-stu-id="fc26e-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="fc26e-128">Escrevendo testes</span><span class="sxs-lookup"><span data-stu-id="fc26e-128">Writing tests</span></span>

<span data-ttu-id="fc26e-129">A chave para testar esse provedor é a capacidade de informar ao contexto para usar o provedor de inmemory e controlar o escopo do banco de dados na memória.</span><span class="sxs-lookup"><span data-stu-id="fc26e-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="fc26e-130">Normalmente, você deseja um banco de dados limpo para cada método de teste.</span><span class="sxs-lookup"><span data-stu-id="fc26e-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="fc26e-131">Aqui está um exemplo de uma classe de teste que usa o banco de dados de inmemory.</span><span class="sxs-lookup"><span data-stu-id="fc26e-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="fc26e-132">Cada método de teste especifica um nome de banco de dados exclusivo, o que significa que cada método tem seu próprio banco de dados de inmemory.</span><span class="sxs-lookup"><span data-stu-id="fc26e-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="fc26e-133">Para usar o método de extensão `.UseInMemoryDatabase()`, faça referência ao pacote NuGet [Microsoft. EntityFrameworkCore. inmemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="fc26e-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
