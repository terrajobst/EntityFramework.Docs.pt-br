---
title: Migrações com vários provedores – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997967"
---
<a name="migrations-with-multiple-providers"></a>Migrações com vários provedores
==================================
O [ferramentas do EF Core] [ 1] apenas criar o scaffolding de migrações para o provedor de Active Directory. Às vezes, no entanto, você talvez queira usar mais de um provedor (por exemplo, Microsoft SQL Server e SQLite) com seu DbContext. Há duas maneiras de lidar com isso com as migrações. Você pode manter dois conjuntos de migrações – uma para cada provedor – ou mesclagem-los em um único conjunto de que possa trabalhar em ambos.

<a name="two-migration-sets"></a>Dois conjuntos de migração
------------------
A primeira abordagem, você gera duas migrações de cada alteração de modelo.

Uma forma de fazer isso é colocar cada conjunto de migração [em um assembly separado] [ 2] e mudar manualmente o provedor de Active Directory (e o conjunto de migrações) entre as duas migrações de adicionar.

É outra abordagem que facilita o trabalho com as ferramentas criar um novo tipo que deriva de seu DbContext e substitui o provedor de Active Directory. Esse tipo é usado no design de tempo durante a adição ou a aplicação de migrações.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Como cada conjunto de migração usa seus próprios tipos de DbContext, essa abordagem não exige o uso de um assembly de migrações separadas.

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
> Você não precisa especificar o diretório de saída para migrações subsequentes, pois eles são criados como irmãos para o último deles.

<a name="one-migration-set"></a>Conjunto de uma migração
-----------------
Se você não gostar de ter dois conjuntos de migrações, você pode combiná-los manualmente em um único conjunto pode ser aplicado a ambos os provedores.

As anotações podem coexistir, pois um provedor ignora todas as anotações que não entende. Por exemplo, uma coluna de chave primária que funciona com o Microsoft SQL Server e SQLite pode parecer com isso.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Se operações somente podem ser aplicadas em um provedor (ou eles são diferente entre provedores), use o `ActiveProvider` propriedade informar qual provedor está ativo.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
