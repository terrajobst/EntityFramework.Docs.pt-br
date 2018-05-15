---
title: Operações de migrações personalizado - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
---
<a name="custom-migrations-operations"></a><span data-ttu-id="cc541-102">Operações de migrações personalizado</span><span class="sxs-lookup"><span data-stu-id="cc541-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="cc541-103">A API MigrationBuilder permite que você execute vários tipos diferentes de operações durante a migração, mas é distantes completa.</span><span class="sxs-lookup"><span data-stu-id="cc541-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="cc541-104">No entanto, a API também é extensível que permite definir suas próprias operações.</span><span class="sxs-lookup"><span data-stu-id="cc541-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="cc541-105">Para estender a API de duas maneiras: usando o `Sql()` método, ou definindo personalizado `MigrationOperation` objetos.</span><span class="sxs-lookup"><span data-stu-id="cc541-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="cc541-106">Para ilustrar, vamos dar uma olhada na implementação de uma operação que cria um usuário de banco de dados usando cada abordagem.</span><span class="sxs-lookup"><span data-stu-id="cc541-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="cc541-107">Em nosso migrações, queremos permitem escrever o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="cc541-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="cc541-108">Usando MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="cc541-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="cc541-109">A maneira mais fácil de implementar uma operação personalizada é definir um método de extensão que chama `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="cc541-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="cc541-110">Aqui está um exemplo que gera o Transact-SQL apropriado.</span><span class="sxs-lookup"><span data-stu-id="cc541-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="cc541-111">Se suas migrações precisam oferecer suporte a vários provedores de banco de dados, você pode usar o `MigrationBuilder.ActiveProvider` propriedade.</span><span class="sxs-lookup"><span data-stu-id="cc541-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="cc541-112">Aqui está um exemplo de suporte do Microsoft SQL Server e PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="cc541-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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
}
```

<span data-ttu-id="cc541-113">Essa abordagem funciona somente se você souber que cada provedor onde sua operação personalizada será aplicada.</span><span class="sxs-lookup"><span data-stu-id="cc541-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="cc541-114">Usando um MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="cc541-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="cc541-115">Para desassociar a operação personalizada do SQL, você pode definir suas próprias `MigrationOperation` para representá-lo.</span><span class="sxs-lookup"><span data-stu-id="cc541-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="cc541-116">A operação é então passada para o provedor para que ele possa determinar o SQL apropriado para gerar.</span><span class="sxs-lookup"><span data-stu-id="cc541-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="cc541-117">Com essa abordagem, o método de extensão só precisa adicionar uma dessas operações para `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="cc541-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="cc541-118">Essa abordagem requer que cada provedor saber como gerar SQL para essa operação em seus `IMigrationsSqlGenerator` serviço.</span><span class="sxs-lookup"><span data-stu-id="cc541-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="cc541-119">Aqui está um exemplo gerador do SQL Server para lidar com a nova operação de substituição.</span><span class="sxs-lookup"><span data-stu-id="cc541-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="cc541-120">Substitua o serviço de gerador de sql de migrações padrão atualizado.</span><span class="sxs-lookup"><span data-stu-id="cc541-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
