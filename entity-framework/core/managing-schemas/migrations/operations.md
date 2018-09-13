---
title: Operações de migrações personalizadas – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: dcf11c44dcc9f6008b8290a89dd8c042e5ec5771
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489124"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="beaa5-102">Operações de migrações personalizadas</span><span class="sxs-lookup"><span data-stu-id="beaa5-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="beaa5-103">A API MigrationBuilder permite que você execute muitos tipos diferentes de operações durante a migração, mas ele está longe de ser completa.</span><span class="sxs-lookup"><span data-stu-id="beaa5-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="beaa5-104">No entanto, a API também é extensível, permitindo que você defina suas próprias operações.</span><span class="sxs-lookup"><span data-stu-id="beaa5-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="beaa5-105">Há duas maneiras de estender a API: usando o `Sql()` método, ou definindo personalizado `MigrationOperation` objetos.</span><span class="sxs-lookup"><span data-stu-id="beaa5-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="beaa5-106">Para ilustrar, vamos dar uma olhada na implementação de uma operação que cria um usuário de banco de dados usando cada abordagem.</span><span class="sxs-lookup"><span data-stu-id="beaa5-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="beaa5-107">Em nosso migrações, queremos permitir escrever o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="beaa5-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="beaa5-108">Usando MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="beaa5-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="beaa5-109">A maneira mais fácil de implementar uma operação personalizada é definir um método de extensão que chama `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="beaa5-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="beaa5-110">Aqui está um exemplo que gera o Transact-SQL apropriado.</span><span class="sxs-lookup"><span data-stu-id="beaa5-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="beaa5-111">Se suas migrações precisam dar suporte a vários provedores de banco de dados, você pode usar o `MigrationBuilder.ActiveProvider` propriedade.</span><span class="sxs-lookup"><span data-stu-id="beaa5-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="beaa5-112">Aqui está um exemplo de suporte do Microsoft SQL Server e o PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="beaa5-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="beaa5-113">Essa abordagem funciona somente se você souber que cada provedor de onde sua operação personalizada será aplicada.</span><span class="sxs-lookup"><span data-stu-id="beaa5-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="beaa5-114">Usando um MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="beaa5-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="beaa5-115">Para desacoplar a operação personalizada do SQL, você pode definir seus próprios `MigrationOperation` para representá-lo.</span><span class="sxs-lookup"><span data-stu-id="beaa5-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="beaa5-116">A operação é então passada para o provedor para que possa determinar apropriados do SQL para gerar.</span><span class="sxs-lookup"><span data-stu-id="beaa5-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="beaa5-117">Com essa abordagem, o método de extensão só precisa adicionar uma dessas operações para `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="beaa5-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="beaa5-118">Essa abordagem requer que cada provedor de saber como gerar o SQL para essa operação em seus `IMigrationsSqlGenerator` service.</span><span class="sxs-lookup"><span data-stu-id="beaa5-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="beaa5-119">Aqui está um exemplo, substituindo o gerador do SQL Server para lidar com a nova operação.</span><span class="sxs-lookup"><span data-stu-id="beaa5-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="beaa5-120">Substitua o serviço gerador de sql de migrações de padrão com um atualizado.</span><span class="sxs-lookup"><span data-stu-id="beaa5-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
