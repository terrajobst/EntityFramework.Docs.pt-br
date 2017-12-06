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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Tabelas com otimização de memória oferecem suporte no provedor de banco de dados do SQL Server EF Core

> [!NOTE]  
>
> Esse recurso foi introduzido no EF Core 1.1.

[Tabelas com otimização de memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) são um recurso do SQL Server em que a tabela inteira reside na memória. Uma segunda cópia dos dados da tabela é mantida em disco, mas apenas para fins de durabilidade. Os dados das tabelas com otimização de memória são lidos apenas no disco durante a recuperação do banco de dados. Por exemplo, após a reinicialização de um servidor.

## <a name="configuring-a-memory-optimized-table"></a>Configurando uma tabela com otimização de memória

Você pode especificar que a tabela a para que uma entidade é mapeada otimização de memória. Ao usar EF principal para criar e manter um banco de dados com base em seu modelo (com migrações ou `Database.EnsureCreated()`), uma tabela com otimização de memória será criada para essas entidades.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
