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
# <a name="custom-migrations-history-table"></a><span data-ttu-id="07983-102">Tabela de histórico de migrações personalizadas</span><span class="sxs-lookup"><span data-stu-id="07983-102">Custom Migrations History Table</span></span>

<span data-ttu-id="07983-103">Por padrão, EF Core controla quais migrações foram aplicadas ao banco de dados, gravando-as em uma tabela chamada `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="07983-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="07983-104">Por vários motivos, talvez você queira personalizar essa tabela para atender melhor às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="07983-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="07983-105">Se você personalizar a tabela de histórico de migrações *depois* de aplicar as migrações, você será responsável por atualizar a tabela existente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="07983-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="07983-106">Nome do esquema e da tabela</span><span class="sxs-lookup"><span data-stu-id="07983-106">Schema and table name</span></span>

<span data-ttu-id="07983-107">Você pode alterar o nome do esquema e da tabela usando o método `MigrationsHistoryTable()` em `OnConfiguring()` (ou `ConfigureServices()` em ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="07983-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="07983-108">Aqui está um exemplo usando o provedor de EF Core de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07983-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a><span data-ttu-id="07983-109">Outras alterações</span><span class="sxs-lookup"><span data-stu-id="07983-109">Other changes</span></span>

<span data-ttu-id="07983-110">Para configurar aspectos adicionais da tabela, substitua e substitua o serviço de `IHistoryRepository` específico do provedor.</span><span class="sxs-lookup"><span data-stu-id="07983-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="07983-111">Aqui está um exemplo de alteração do nome da coluna Migrationid para *ID* em SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07983-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="07983-112">`SqlServerHistoryRepository` está dentro de um namespace interno e pode ser alterado em versões futuras.</span><span class="sxs-lookup"><span data-stu-id="07983-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
