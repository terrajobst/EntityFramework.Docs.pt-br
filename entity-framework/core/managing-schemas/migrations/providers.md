---
title: Migrações com vários provedores – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: efe95893f7dbfc8e5c4775e86d58abb32eee3c83
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502104"
---
# <a name="migrations-with-multiple-providers"></a>Migrações com vários provedores

As [ferramentas de EF Core][1] apenas Scaffold migrações para o provedor ativo. Às vezes, no entanto, talvez você queira usar mais de um provedor (por exemplo Microsoft SQL Server e SQLite) com o DbContext. Há duas maneiras de lidar com isso com migrações. Você pode manter dois conjuntos de migrações, um para cada provedor, ou mesclá-los em um único conjunto que pode funcionar em ambos.

## <a name="two-migration-sets"></a>Dois conjuntos de migração

Na primeira abordagem, você gera duas migrações para cada alteração de modelo.

Uma maneira de fazer isso é colocar cada conjunto de migração [em um assembly separado][2] e alternar manualmente o provedor ativo (e o assembly de migrações) entre adicionar as duas migrações.

Outra abordagem que facilita o trabalho com as ferramentas é criar um novo tipo que deriva de seu DbContext e substitui o provedor ativo. Esse tipo é usado em tempo de design ao adicionar ou aplicar migrações.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Como cada conjunto de migração usa seus próprios tipos DbContext, essa abordagem não requer o uso de um assembly de migrações separado.

Ao adicionar uma nova migração, especifique os tipos de contexto.

### <a name="net-core-clitabdotnet-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> Você não precisa especificar o diretório de saída para migrações subsequentes, pois elas são criadas como irmãos para a última.

## <a name="one-migration-set"></a>Um conjunto de migração

Se você não gostar de ter dois conjuntos de migrações, poderá combiná-los manualmente em um único conjunto que possa ser aplicado aos dois provedores.

As anotações podem coexistir, pois um provedor ignora todas as anotações que ele não entende. Por exemplo, uma coluna de chave primária que funciona com ambos Microsoft SQL Server e SQLite pode ser parecida com esta.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Se as operações só puderem ser aplicadas em um provedor (ou elas forem diferentes entre os provedores), use a propriedade `ActiveProvider` para informar qual provedor está ativo.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
