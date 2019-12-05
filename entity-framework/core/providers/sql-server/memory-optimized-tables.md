---
title: Provedor de banco de dados Microsoft SQL Server-tabelas com otimização de memória-EF Core
description: Como usar tabelas com otimização de memória com o provedor de banco de dados SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824638"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="64df6-103">Suporte a tabelas com otimização de memória no provedor de banco de dados SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="64df6-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="64df6-104">As [tabelas com otimização de memória](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) são um recurso de SQL Server em que a tabela inteira reside na memória.</span><span class="sxs-lookup"><span data-stu-id="64df6-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="64df6-105">Uma segunda cópia dos dados da tabela é mantida em disco, mas apenas para fins de durabilidade.</span><span class="sxs-lookup"><span data-stu-id="64df6-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="64df6-106">Os dados das tabelas com otimização de memória são lidos apenas no disco durante a recuperação do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64df6-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="64df6-107">Por exemplo, após a reinicialização de um servidor.</span><span class="sxs-lookup"><span data-stu-id="64df6-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="64df6-108">Configurando uma tabela com otimização de memória</span><span class="sxs-lookup"><span data-stu-id="64df6-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="64df6-109">Você pode especificar que a tabela para a qual uma entidade está mapeada tem otimização de memória.</span><span class="sxs-lookup"><span data-stu-id="64df6-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="64df6-110">Ao usar EF Core para criar e manter um banco de dados com base em seu modelo (com [migrações](xref:core/managing-schemas/migrations/index) ou [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), uma tabela com otimização de memória será criada para essas entidades.</span><span class="sxs-lookup"><span data-stu-id="64df6-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
