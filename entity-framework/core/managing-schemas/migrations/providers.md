---
title: Migrações com vários provedores – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: 384ad27e405adc2bccb5e96aae30e5bd7ac556be
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824711"
---
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="ab312-102">Migrações com vários provedores</span><span class="sxs-lookup"><span data-stu-id="ab312-102">Migrations with Multiple Providers</span></span>

<span data-ttu-id="ab312-103">As [ferramentas de EF Core][1] apenas Scaffold migrações para o provedor ativo.</span><span class="sxs-lookup"><span data-stu-id="ab312-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="ab312-104">Às vezes, no entanto, talvez você queira usar mais de um provedor (por exemplo Microsoft SQL Server e SQLite) com o DbContext.</span><span class="sxs-lookup"><span data-stu-id="ab312-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="ab312-105">Há duas maneiras de lidar com isso com migrações.</span><span class="sxs-lookup"><span data-stu-id="ab312-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="ab312-106">Você pode manter dois conjuntos de migrações, um para cada provedor, ou mesclá-los em um único conjunto que pode funcionar em ambos.</span><span class="sxs-lookup"><span data-stu-id="ab312-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

## <a name="two-migration-sets"></a><span data-ttu-id="ab312-107">Dois conjuntos de migração</span><span class="sxs-lookup"><span data-stu-id="ab312-107">Two migration sets</span></span>

<span data-ttu-id="ab312-108">Na primeira abordagem, você gera duas migrações para cada alteração de modelo.</span><span class="sxs-lookup"><span data-stu-id="ab312-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="ab312-109">Uma maneira de fazer isso é colocar cada conjunto de migração [em um assembly separado][2] e alternar manualmente o provedor ativo (e o assembly de migrações) entre adicionar as duas migrações.</span><span class="sxs-lookup"><span data-stu-id="ab312-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="ab312-110">Outra abordagem que facilita o trabalho com as ferramentas é criar um novo tipo que deriva de seu DbContext e substitui o provedor ativo.</span><span class="sxs-lookup"><span data-stu-id="ab312-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="ab312-111">Esse tipo é usado em tempo de design ao adicionar ou aplicar migrações.</span><span class="sxs-lookup"><span data-stu-id="ab312-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="ab312-112">Como cada conjunto de migração usa seus próprios tipos DbContext, essa abordagem não requer o uso de um assembly de migrações separado.</span><span class="sxs-lookup"><span data-stu-id="ab312-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="ab312-113">Ao adicionar uma nova migração, especifique os tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="ab312-113">When adding new migration, specify the context types.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="ab312-114">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="ab312-114">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="ab312-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab312-115">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> <span data-ttu-id="ab312-116">Você não precisa especificar o diretório de saída para migrações subsequentes, pois elas são criadas como irmãos para a última.</span><span class="sxs-lookup"><span data-stu-id="ab312-116">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="one-migration-set"></a><span data-ttu-id="ab312-117">Um conjunto de migração</span><span class="sxs-lookup"><span data-stu-id="ab312-117">One migration set</span></span>

<span data-ttu-id="ab312-118">Se você não gostar de ter dois conjuntos de migrações, poderá combiná-los manualmente em um único conjunto que possa ser aplicado aos dois provedores.</span><span class="sxs-lookup"><span data-stu-id="ab312-118">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="ab312-119">As anotações podem coexistir, pois um provedor ignora todas as anotações que ele não entende.</span><span class="sxs-lookup"><span data-stu-id="ab312-119">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="ab312-120">Por exemplo, uma coluna de chave primária que funciona com ambos Microsoft SQL Server e SQLite pode ser parecida com esta.</span><span class="sxs-lookup"><span data-stu-id="ab312-120">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="ab312-121">Se as operações só puderem ser aplicadas em um provedor (ou elas forem diferentes entre os provedores), use a propriedade `ActiveProvider` para informar qual provedor está ativo.</span><span class="sxs-lookup"><span data-stu-id="ab312-121">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
