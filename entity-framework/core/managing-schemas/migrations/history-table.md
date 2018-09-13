---
title: Tabela de histórico de migrações personalizadas – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488810"
---
<a name="custom-migrations-history-table"></a>Tabela de histórico de migrações personalizadas
===============================
Por padrão, o EF Core mantém o controle de quais migrações foram aplicadas ao banco de dados, gravando-os em uma tabela chamada `__EFMigrationsHistory`. Por vários motivos, você talvez queira personalizar essa tabela para atender melhor às suas necessidades.

> [!IMPORTANT]
> Se você personalizar a tabela de histórico de migrações *depois de* a aplicação de migrações, você é responsável por atualizar a tabela existente no banco de dados.

<a name="schema-and-table-name"></a>Nome do esquema e tabela
----------------------
Você pode alterar o esquema e o nome da tabela usando o `MigrationsHistoryTable()` método no `OnConfiguring()` (ou `ConfigureServices()` no ASP.NET Core). Aqui está um exemplo que usa o provedor do EF Core do SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Outras alterações
-------------
Para configurar aspectos adicionais da tabela, substituição e substituir específicas do provedor `IHistoryRepository` service. Aqui está um exemplo de alterar o nome da coluna para MigrationId *Id* no SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` está dentro de um namespace interno e pode mudar em versões futuras.

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
