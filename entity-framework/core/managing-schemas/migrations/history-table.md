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
<a name="custom-migrations-history-table"></a><span data-ttu-id="21d42-102">Tabela de histórico de migrações personalizadas</span><span class="sxs-lookup"><span data-stu-id="21d42-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="21d42-103">Por padrão, o EF Core mantém o controle de quais migrações foram aplicadas ao banco de dados, gravando-os em uma tabela chamada `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="21d42-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="21d42-104">Por vários motivos, você talvez queira personalizar essa tabela para atender melhor às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="21d42-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21d42-105">Se você personalizar a tabela de histórico de migrações *depois de* a aplicação de migrações, você é responsável por atualizar a tabela existente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21d42-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="21d42-106">Nome do esquema e tabela</span><span class="sxs-lookup"><span data-stu-id="21d42-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="21d42-107">Você pode alterar o esquema e o nome da tabela usando o `MigrationsHistoryTable()` método no `OnConfiguring()` (ou `ConfigureServices()` no ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="21d42-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="21d42-108">Aqui está um exemplo que usa o provedor do EF Core do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="21d42-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="21d42-109">Outras alterações</span><span class="sxs-lookup"><span data-stu-id="21d42-109">Other changes</span></span>
-------------
<span data-ttu-id="21d42-110">Para configurar aspectos adicionais da tabela, substituição e substituir específicas do provedor `IHistoryRepository` service.</span><span class="sxs-lookup"><span data-stu-id="21d42-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="21d42-111">Aqui está um exemplo de alterar o nome da coluna para MigrationId *Id* no SQL Server.</span><span class="sxs-lookup"><span data-stu-id="21d42-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="21d42-112">`SqlServerHistoryRepository` está dentro de um namespace interno e pode mudar em versões futuras.</span><span class="sxs-lookup"><span data-stu-id="21d42-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
