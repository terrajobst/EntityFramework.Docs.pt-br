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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Suporte a tabelas com otimização de memória no provedor de banco de dados SQL Server EF Core

As [tabelas com otimização de memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) são um recurso de SQL Server em que a tabela inteira reside na memória. Uma segunda cópia dos dados da tabela é mantida em disco, mas apenas para fins de durabilidade. Os dados em tabelas com otimização de memória só são lidos no disco durante a recuperação do banco. Por exemplo, após a reinicialização do servidor.

## <a name="configuring-a-memory-optimized-table"></a>Configurando uma tabela com otimização de memória

Você pode especificar que a tabela para a qual uma entidade está mapeada tem otimização de memória. Ao usar o EF Core para criar e manter um banco de dados com base em seu modelo (com migrações ou `Database.EnsureCreated()`), uma tabela com otimização de memória será criada para essas entidades.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
