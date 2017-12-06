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
<a name="custom-migrations-history-table"></a><span data-ttu-id="810e3-102">Tabela de histórico de migrações personalizado</span><span class="sxs-lookup"><span data-stu-id="810e3-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="810e3-103">Por padrão, EF Core mantém o controle de quais migrações foram aplicadas ao banco de dados registrando-os em uma tabela chamada `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="810e3-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="810e3-104">Por várias razões, convém personalizar essa tabela para atender melhor às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="810e3-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="810e3-105">Se você personalizar a tabela de histórico de migrações *depois* aplicar migrações, você é responsável por atualizar a tabela existente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="810e3-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="810e3-106">Nome do esquema e tabela</span><span class="sxs-lookup"><span data-stu-id="810e3-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="810e3-107">Você pode alterar o esquema e o nome de tabela usando o `MigrationsHistoryTable()` método `OnConfiguring()` (ou `ConfigureServices()` no ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="810e3-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="810e3-108">Aqui está um exemplo que usa o provedor EF núcleo do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="810e3-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="810e3-109">Outras alterações</span><span class="sxs-lookup"><span data-stu-id="810e3-109">Other changes</span></span>
-------------
<span data-ttu-id="810e3-110">Para configurar aspectos adicionais da tabela, substituir e substitua específicas do provedor `IHistoryRepository` serviço.</span><span class="sxs-lookup"><span data-stu-id="810e3-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="810e3-111">Aqui está um exemplo de como alterar o nome da coluna MigrationId para *Id* no SQL Server.</span><span class="sxs-lookup"><span data-stu-id="810e3-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="810e3-112">`SqlServerHistoryRepository`está dentro de um namespace interno e pode mudar em versões futuras.</span><span class="sxs-lookup"><span data-stu-id="810e3-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
