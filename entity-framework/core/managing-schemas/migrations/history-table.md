---
title: "Tabela de histórico de migrações personalizado - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a>Tabela de histórico de migrações personalizado
===============================
Por padrão, EF Core mantém o controle de quais migrações foram aplicadas ao banco de dados registrando-os em uma tabela chamada `__EFMigrationsHistory`. Por várias razões, convém personalizar essa tabela para atender melhor às suas necessidades.

> [!IMPORTANT]
> Se você personalizar a tabela de histórico de migrações *depois* aplicar migrações, você é responsável por atualizar a tabela existente no banco de dados.

<a name="schema-and-table-name"></a>Nome do esquema e tabela
----------------------
Você pode alterar o esquema e o nome de tabela usando o `MigrationsHistoryTable()` método `OnConfiguring()` (ou `ConfigureServices()` no ASP.NET Core). Aqui está um exemplo que usa o provedor EF núcleo do SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Outras alterações
-------------
Para configurar aspectos adicionais da tabela, substituir e substitua específicas do provedor `IHistoryRepository` serviço. Aqui está um exemplo de como alterar o nome da coluna MigrationId para *Id* no SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`está dentro de um namespace interno e pode mudar em versões futuras.

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
