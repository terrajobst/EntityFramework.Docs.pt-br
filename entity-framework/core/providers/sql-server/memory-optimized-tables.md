---
title: Provedor de banco de dados Microsoft SQL Server-tabelas com otimização de memória-EF Core
description: Como usar tabelas com otimização de memória com o provedor de banco de dados SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417787"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Suporte a tabelas com otimização de memória no provedor de banco de dados SQL Server EF Core

As [tabelas com otimização de memória](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) são um recurso de SQL Server em que a tabela inteira reside na memória. Uma segunda cópia dos dados da tabela é mantida em disco, mas apenas para fins de durabilidade. Os dados das tabelas com otimização de memória são lidos apenas no disco durante a recuperação do banco de dados. Por exemplo, após a reinicialização de um servidor.

## <a name="configuring-a-memory-optimized-table"></a>Configurando uma tabela com otimização de memória

Você pode especificar que a tabela para a qual uma entidade está mapeada tem otimização de memória. Ao usar EF Core para criar e manter um banco de dados com base em seu modelo (com [migrações](xref:core/managing-schemas/migrations/index) ou [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), uma tabela com otimização de memória será criada para essas entidades.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
