---
title: Provedor de banco de dados Microsoft SQL Server-tabelas com otimização de memória-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813492"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="3a034-102">Suporte a tabelas com otimização de memória no provedor de banco de dados SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="3a034-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="3a034-103">As [tabelas com otimização de memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) são um recurso de SQL Server em que a tabela inteira reside na memória.</span><span class="sxs-lookup"><span data-stu-id="3a034-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="3a034-104">Uma segunda cópia dos dados da tabela é mantida em disco, mas apenas para fins de durabilidade.</span><span class="sxs-lookup"><span data-stu-id="3a034-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="3a034-105">Os dados em tabelas com otimização de memória só são lidos no disco durante a recuperação do banco.</span><span class="sxs-lookup"><span data-stu-id="3a034-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="3a034-106">Por exemplo, após a reinicialização do servidor.</span><span class="sxs-lookup"><span data-stu-id="3a034-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="3a034-107">Configurando uma tabela com otimização de memória</span><span class="sxs-lookup"><span data-stu-id="3a034-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="3a034-108">Você pode especificar que a tabela para a qual uma entidade está mapeada tem otimização de memória.</span><span class="sxs-lookup"><span data-stu-id="3a034-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="3a034-109">Ao usar o EF Core para criar e manter um banco de dados com base em seu modelo (com migrações ou `Database.EnsureCreated()`), uma tabela com otimização de memória será criada para essas entidades.</span><span class="sxs-lookup"><span data-stu-id="3a034-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
