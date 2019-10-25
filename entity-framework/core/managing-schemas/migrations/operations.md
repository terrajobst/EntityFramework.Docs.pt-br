---
title: Operações de migrações personalizadas-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812052"
---
# <a name="custom-migrations-operations"></a><span data-ttu-id="e83c0-102">Operações de migrações personalizadas</span><span class="sxs-lookup"><span data-stu-id="e83c0-102">Custom Migrations Operations</span></span>

<span data-ttu-id="e83c0-103">A API do MigrationBuilder permite que você execute vários tipos diferentes de operações durante uma migração, mas ela está longe de ser exaustiva.</span><span class="sxs-lookup"><span data-stu-id="e83c0-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="e83c0-104">No entanto, a API também é extensível, permitindo que você defina suas próprias operações.</span><span class="sxs-lookup"><span data-stu-id="e83c0-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="e83c0-105">Há duas maneiras de estender a API: usando o método `Sql()` ou definindo objetos de `MigrationOperation` personalizados.</span><span class="sxs-lookup"><span data-stu-id="e83c0-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="e83c0-106">Para ilustrar, vamos dar uma olhada na implementação de uma operação que cria um usuário de banco de dados usando cada abordagem.</span><span class="sxs-lookup"><span data-stu-id="e83c0-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="e83c0-107">Em nossas migrações, queremos habilitar a gravação do código a seguir:</span><span class="sxs-lookup"><span data-stu-id="e83c0-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="e83c0-108">Usando MigrationBuilder. SQL ()</span><span class="sxs-lookup"><span data-stu-id="e83c0-108">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="e83c0-109">A maneira mais fácil de implementar uma operação personalizada é definir um método de extensão que chama `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="e83c0-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="e83c0-110">Aqui está um exemplo que gera o Transact-SQL apropriado.</span><span class="sxs-lookup"><span data-stu-id="e83c0-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="e83c0-111">Se suas migrações precisarem dar suporte a vários provedores de banco de dados, você poderá usar a propriedade `MigrationBuilder.ActiveProvider`.</span><span class="sxs-lookup"><span data-stu-id="e83c0-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="e83c0-112">Aqui está um exemplo que dá suporte a Microsoft SQL Server e PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="e83c0-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="e83c0-113">Essa abordagem só funcionará se você souber todos os provedores em que a operação personalizada será aplicada.</span><span class="sxs-lookup"><span data-stu-id="e83c0-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="e83c0-114">Usando um MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="e83c0-114">Using a MigrationOperation</span></span>

<span data-ttu-id="e83c0-115">Para desacoplar a operação personalizada do SQL, você pode definir seu próprio `MigrationOperation` para representá-la.</span><span class="sxs-lookup"><span data-stu-id="e83c0-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="e83c0-116">Em seguida, a operação é passada ao provedor para que ele possa determinar o SQL apropriado a ser gerado.</span><span class="sxs-lookup"><span data-stu-id="e83c0-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="e83c0-117">Com essa abordagem, o método de extensão precisa apenas adicionar uma dessas operações a `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="e83c0-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="e83c0-118">Essa abordagem requer que cada provedor saiba como gerar SQL para esta operação em seu serviço de `IMigrationsSqlGenerator`.</span><span class="sxs-lookup"><span data-stu-id="e83c0-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="e83c0-119">Aqui está um exemplo que substitui o gerador de SQL Server para manipular a nova operação.</span><span class="sxs-lookup"><span data-stu-id="e83c0-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="e83c0-120">Substitua o serviço SQL Generator de migrações padrão por um atualizado.</span><span class="sxs-lookup"><span data-stu-id="e83c0-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
