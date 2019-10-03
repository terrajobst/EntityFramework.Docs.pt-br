---
title: Criar e descartar APIs-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
ms.openlocfilehash: 88c1403d2fae740ad78bb7c41d404b0dd91e86ae
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813438"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="6a41b-102">Criar e remover APIs</span><span class="sxs-lookup"><span data-stu-id="6a41b-102">Create and Drop APIs</span></span>

<span data-ttu-id="6a41b-103">Os métodos EnsureCreated e EnsureDeleted fornecem uma alternativa leve às [migrações](migrations/index.md) para gerenciar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6a41b-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="6a41b-104">Esses métodos são úteis em cenários quando os dados são transitórios e podem ser descartados quando o esquema é alterado.</span><span class="sxs-lookup"><span data-stu-id="6a41b-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="6a41b-105">Por exemplo, durante o protótipo, em testes ou para caches locais.</span><span class="sxs-lookup"><span data-stu-id="6a41b-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="6a41b-106">Alguns provedores (especialmente aqueles não relacionais) não oferecem suporte a migrações.</span><span class="sxs-lookup"><span data-stu-id="6a41b-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="6a41b-107">Para esses provedores, a EnsureCreated geralmente é a maneira mais fácil de inicializar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6a41b-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="6a41b-108">EnsureCreated e migrações não funcionam bem juntos.</span><span class="sxs-lookup"><span data-stu-id="6a41b-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="6a41b-109">Se você estiver usando migrações, não use EnsureCreated para inicializar o esquema.</span><span class="sxs-lookup"><span data-stu-id="6a41b-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="6a41b-110">A transição de EnsureCreated para migrações não é uma experiência direta.</span><span class="sxs-lookup"><span data-stu-id="6a41b-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="6a41b-111">A maneira mais simples de fazer isso é descartar o banco de dados e recriá-lo usando as migrações.</span><span class="sxs-lookup"><span data-stu-id="6a41b-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="6a41b-112">Se você antecipar o uso de migrações no futuro, é melhor começar com as migrações em vez de usar EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="6a41b-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="6a41b-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="6a41b-113">EnsureDeleted</span></span>

<span data-ttu-id="6a41b-114">O método EnsureDeleted removerá o banco de dados se ele existir.</span><span class="sxs-lookup"><span data-stu-id="6a41b-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="6a41b-115">Se você não tiver as permissões apropriadas, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="6a41b-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="6a41b-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="6a41b-116">EnsureCreated</span></span>

<span data-ttu-id="6a41b-117">O EnsureCreated criará o banco de dados se ele não existir e inicializará o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6a41b-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="6a41b-118">Se existir alguma tabela (incluindo tabelas para outra classe DbContext), o esquema não será inicializado.</span><span class="sxs-lookup"><span data-stu-id="6a41b-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="6a41b-119">As versões assíncronas desses métodos também estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="6a41b-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="6a41b-120">Script SQL</span><span class="sxs-lookup"><span data-stu-id="6a41b-120">SQL Script</span></span>

<span data-ttu-id="6a41b-121">Para obter o SQL usado pelo EnsureCreated, você pode usar o método GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="6a41b-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="6a41b-122">Várias classes DbContext</span><span class="sxs-lookup"><span data-stu-id="6a41b-122">Multiple DbContext classes</span></span>

<span data-ttu-id="6a41b-123">EnsureCreated só funciona quando nenhuma tabela está presente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6a41b-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="6a41b-124">Se necessário, você pode escrever seu próprio cheque para ver se o esquema precisa ser inicializado e usar o serviço IRelationalDatabaseCreator subjacente para inicializar o esquema.</span><span class="sxs-lookup"><span data-stu-id="6a41b-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
