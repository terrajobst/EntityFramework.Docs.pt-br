---
title: Operações de migrações personalizadas – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: 93de6ee1b2eda1875188ace6eda299260fbcc1fe
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028077"
---
<a name="custom-migrations-operations"></a>Operações de migrações personalizadas
============================
A API MigrationBuilder permite que você execute muitos tipos diferentes de operações durante a migração, mas ele está longe de ser completa. No entanto, a API também é extensível, permitindo que você defina suas próprias operações. Há duas maneiras de estender a API: usando o `Sql()` método, ou definindo personalizado `MigrationOperation` objetos.

Para ilustrar, vamos dar uma olhada na implementação de uma operação que cria um usuário de banco de dados usando cada abordagem. Em nosso migrações, queremos permitir escrever o código a seguir:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Usando MigrationBuilder.Sql()
----------------------------
A maneira mais fácil de implementar uma operação personalizada é definir um método de extensão que chama `MigrationBuilder.Sql()`.
Aqui está um exemplo que gera o Transact-SQL apropriado.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Se suas migrações precisam dar suporte a vários provedores de banco de dados, você pode usar o `MigrationBuilder.ActiveProvider` propriedade. Aqui está um exemplo de suporte do Microsoft SQL Server e o PostgreSQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }

    return migrationBuilder;
}
```

Essa abordagem funciona somente se você souber que cada provedor de onde sua operação personalizada será aplicada.

<a name="using-a-migrationoperation"></a>Usando um MigrationOperation
---------------------------
Para desacoplar a operação personalizada do SQL, você pode definir seus próprios `MigrationOperation` para representá-lo. A operação é então passada para o provedor para que possa determinar apropriados do SQL para gerar.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Com essa abordagem, o método de extensão só precisa adicionar uma dessas operações para `MigrationBuilder.Operations`.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

Essa abordagem requer que cada provedor de saber como gerar o SQL para essa operação em seus `IMigrationsSqlGenerator` service. Aqui está um exemplo, substituindo o gerador do SQL Server para lidar com a nova operação.

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

Substitua o serviço gerador de sql de migrações de padrão com um atualizado.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
