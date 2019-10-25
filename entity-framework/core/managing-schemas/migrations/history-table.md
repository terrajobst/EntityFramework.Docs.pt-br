---
title: Tabela de histórico de migrações personalizadas-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812079"
---
# <a name="custom-migrations-history-table"></a>Tabela de histórico de migrações personalizadas

Por padrão, EF Core controla quais migrações foram aplicadas ao banco de dados, gravando-as em uma tabela chamada `__EFMigrationsHistory`. Por vários motivos, talvez você queira personalizar essa tabela para atender melhor às suas necessidades.

> [!IMPORTANT]
> Se você personalizar a tabela de histórico de migrações *depois* de aplicar as migrações, você será responsável por atualizar a tabela existente no banco de dados.

## <a name="schema-and-table-name"></a>Nome do esquema e da tabela

Você pode alterar o nome do esquema e da tabela usando o método `MigrationsHistoryTable()` em `OnConfiguring()` (ou `ConfigureServices()` em ASP.NET Core). Aqui está um exemplo usando o provedor de EF Core de SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a>Outras alterações

Para configurar aspectos adicionais da tabela, substitua e substitua o serviço de `IHistoryRepository` específico do provedor. Aqui está um exemplo de alteração do nome da coluna Migrationid para *ID* em SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` está dentro de um namespace interno e pode ser alterado em versões futuras.

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
