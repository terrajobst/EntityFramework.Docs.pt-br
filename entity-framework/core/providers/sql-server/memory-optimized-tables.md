---
title: Microsoft SQL Server banco de dados provedor - tabelas com otimização de memória – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995796"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="0e2c7-102">Tabelas com otimização de memória oferecem suporte no provedor de banco de dados do SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="0e2c7-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="0e2c7-103">Essa funcionalidade foi introduzida no EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="0e2c7-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="0e2c7-104">[Tabelas com otimização de memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) são um recurso do SQL Server em que a tabela inteira reside na memória.</span><span class="sxs-lookup"><span data-stu-id="0e2c7-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="0e2c7-105">Uma segunda cópia dos dados da tabela é mantida no disco, mas apenas para fins de durabilidade.</span><span class="sxs-lookup"><span data-stu-id="0e2c7-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="0e2c7-106">Dados em tabelas com otimização de memória é somente leitura do disco durante a recuperação de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0e2c7-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="0e2c7-107">Por exemplo, depois de um servidor reinicie.</span><span class="sxs-lookup"><span data-stu-id="0e2c7-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="0e2c7-108">Configurando uma tabela com otimização de memória</span><span class="sxs-lookup"><span data-stu-id="0e2c7-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="0e2c7-109">Você pode especificar que a tabela para a qual uma entidade está mapeada tem otimização de memória.</span><span class="sxs-lookup"><span data-stu-id="0e2c7-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="0e2c7-110">Ao usar o EF Core para criar e manter um banco de dados com base em seu modelo (com migrações ou `Database.EnsureCreated()`), uma tabela com otimização de memória será criada para essas entidades.</span><span class="sxs-lookup"><span data-stu-id="0e2c7-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
