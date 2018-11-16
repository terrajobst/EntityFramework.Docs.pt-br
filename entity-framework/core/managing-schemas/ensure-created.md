---
title: Criar e remover APIs – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688623"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="bf0cc-102">Criar e remover APIs</span><span class="sxs-lookup"><span data-stu-id="bf0cc-102">Create and Drop APIs</span></span>

<span data-ttu-id="bf0cc-103">Os métodos EnsureCreated e EnsureDeleted fornecem uma alternativa leve para [migrações](migrations/index.md) para gerenciar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="bf0cc-104">Esses métodos são úteis em cenários quando os dados são transitórios e podem ser descartados quando o esquema é alterado.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="bf0cc-105">Por exemplo durante a criação de protótipos, testes ou para caches locais.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="bf0cc-106">Alguns provedores (especialmente os não-relacional) não dão suporte a migrações.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="bf0cc-107">Para esses provedores, EnsureCreated costuma ser a maneira mais fácil para inicializar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="bf0cc-108">Migrações e EnsureCreated não funcionam bem juntos.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="bf0cc-109">Se você estiver usando as migrações, não use EnsureCreated para inicializar o esquema.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="bf0cc-110">A transição de EnsureCreated para migrações não é uma experiência perfeita.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="bf0cc-111">A maneira mais simples de fazê-lo é remover o banco de dados e recriá-lo usando as migrações.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="bf0cc-112">Se você antecipar o uso de migrações no futuro, é melhor começar com as migrações em vez de usar EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="bf0cc-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="bf0cc-113">EnsureDeleted</span></span>

<span data-ttu-id="bf0cc-114">O método EnsureDeleted descartará o banco de dados se ele existir.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="bf0cc-115">Se você não tiver as permissões apropriadas, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="bf0cc-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="bf0cc-116">EnsureCreated</span></span>

<span data-ttu-id="bf0cc-117">EnsureCreated criará o banco de dados se ele não existe e inicializar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="bf0cc-118">Se existirem quaisquer tabelas (incluindo tabelas para outra classe DbContext), o esquema não ser inicializado.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="bf0cc-119">Versões assíncronas desses métodos também estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="bf0cc-120">Script SQL</span><span class="sxs-lookup"><span data-stu-id="bf0cc-120">SQL Script</span></span>

<span data-ttu-id="bf0cc-121">Para obter o SQL usado pelo EnsureCreated, você pode usar o método GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="bf0cc-122">Várias classes DbContext</span><span class="sxs-lookup"><span data-stu-id="bf0cc-122">Multiple DbContext classes</span></span>

<span data-ttu-id="bf0cc-123">EnsureCreated só funciona quando não há tabelas estão presentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="bf0cc-124">Se necessário, você pode escrever sua própria verificação para ver se o esquema precisa ser inicializado e usar o serviço IRelationalDatabaseCreator subjacente para inicializar o esquema.</span><span class="sxs-lookup"><span data-stu-id="bf0cc-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
