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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Tabelas com otimização de memória oferecem suporte no provedor de banco de dados do SQL Server EF Core

> [!NOTE]  
>
> Essa funcionalidade foi introduzida no EF Core 1.1.

[Tabelas com otimização de memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) são um recurso do SQL Server em que a tabela inteira reside na memória. Uma segunda cópia dos dados da tabela é mantida no disco, mas apenas para fins de durabilidade. Dados em tabelas com otimização de memória é somente leitura do disco durante a recuperação de banco de dados. Por exemplo, depois de um servidor reinicie.

## <a name="configuring-a-memory-optimized-table"></a>Configurando uma tabela com otimização de memória

Você pode especificar que a tabela para a qual uma entidade está mapeada tem otimização de memória. Ao usar o EF Core para criar e manter um banco de dados com base em seu modelo (com migrações ou `Database.EnsureCreated()`), uma tabela com otimização de memória será criada para essas entidades.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
