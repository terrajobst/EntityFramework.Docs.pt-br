---
title: "Migrações com vários provedores - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a>Migrações com vários provedores
==================================
O [EF principais ferramentas] [ 1] scaffold somente as migrações para o provedor ativo. Às vezes, no entanto, convém usar mais de um provedor (por exemplo, Microsoft SQL Server e SQLite) com o DbContext. Há duas maneiras para lidar com isso com migrações. Você pode manter dois conjuntos de migrações – para cada provedor – ou mesclagem-los em um único conjunto que funciona em ambos.

<a name="two-migration-sets"></a>Dois conjuntos de migração
------------------
Na primeira abordagem, você deve gerar duas migrações para cada alteração de modelo.

Uma forma de fazer isso é colocar cada conjunto de migração [em um assembly separado] [ 2] e alternar manualmente o provedor ativo (e o conjunto de migrações) entre duas migrações de adicionar.

É outra abordagem que facilita o trabalho com as ferramentas criar um novo tipo que deriva de sua DbContext e substitui o provedor ativo. Esse tipo é usado no design de tempo ao adicionar ou aplicação de migrações.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Como cada conjunto de migração usa seus próprios tipos DbContext, essa abordagem não requer o uso de um conjunto separado de migrações.

Ao adicionar nova migração, especifica os tipos de contexto.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Você não precisa especificar o diretório de saída para migrações subsequentes, como eles são criados como irmãos para o último.

<a name="one-migration-set"></a>Migração de um conjunto
-----------------
Se você não gosta ter dois conjuntos de migrações, você pode combiná-los manualmente em um único conjunto pode ser aplicado a ambos os provedores.

As anotações podem coexistir como um provedor ignora quaisquer anotações que não entende. Por exemplo, uma coluna de chave primária que funciona com o Microsoft SQL Server e SQLite pode parecer com isso.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Se operações somente podem ser aplicadas em um provedor (ou estiverem diferente entre os provedores), use o `ActiveProvider` propriedade dizer qual provedor está ativo.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
