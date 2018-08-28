---
title: Tabela de histórico de migrações personalizadas – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 7ee76cadd6fac4ec403918e88460e43067ae5815
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995690"
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
