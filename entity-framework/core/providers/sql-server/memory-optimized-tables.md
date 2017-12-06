---
title: "Microsoft SQL Server Database provedor - tabelas com otimização de memória - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="2b4d5-102">Tabelas com otimização de memória oferecem suporte no provedor de banco de dados do SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="2b4d5-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="2b4d5-103">Esse recurso foi introduzido no EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="2b4d5-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="2b4d5-104">[Tabelas com otimização de memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) são um recurso do SQL Server em que a tabela inteira reside na memória.</span><span class="sxs-lookup"><span data-stu-id="2b4d5-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="2b4d5-105">Uma segunda cópia dos dados da tabela é mantida em disco, mas apenas para fins de durabilidade.</span><span class="sxs-lookup"><span data-stu-id="2b4d5-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="2b4d5-106">Os dados das tabelas com otimização de memória são lidos apenas no disco durante a recuperação do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2b4d5-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="2b4d5-107">Por exemplo, após a reinicialização de um servidor.</span><span class="sxs-lookup"><span data-stu-id="2b4d5-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="2b4d5-108">Configurando uma tabela com otimização de memória</span><span class="sxs-lookup"><span data-stu-id="2b4d5-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="2b4d5-109">Você pode especificar que a tabela a para que uma entidade é mapeada otimização de memória.</span><span class="sxs-lookup"><span data-stu-id="2b4d5-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="2b4d5-110">Ao usar EF principal para criar e manter um banco de dados com base em seu modelo (com migrações ou `Database.EnsureCreated()`), uma tabela com otimização de memória será criada para essas entidades.</span><span class="sxs-lookup"><span data-stu-id="2b4d5-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
