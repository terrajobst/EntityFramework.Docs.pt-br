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
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="d4930-102">Migrações com vários provedores</span><span class="sxs-lookup"><span data-stu-id="d4930-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="d4930-103">O [ferramentas do EF Core] [ 1] apenas criar o scaffolding de migrações para o provedor de Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d4930-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="d4930-104">Às vezes, no entanto, você talvez queira usar mais de um provedor (por exemplo, Microsoft SQL Server e SQLite) com seu DbContext.</span><span class="sxs-lookup"><span data-stu-id="d4930-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="d4930-105">Há duas maneiras de lidar com isso com as migrações.</span><span class="sxs-lookup"><span data-stu-id="d4930-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="d4930-106">Você pode manter dois conjuntos de migrações – uma para cada provedor – ou mesclagem-los em um único conjunto de que possa trabalhar em ambos.</span><span class="sxs-lookup"><span data-stu-id="d4930-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="d4930-107">Dois conjuntos de migração</span><span class="sxs-lookup"><span data-stu-id="d4930-107">Two migration sets</span></span>
------------------
<span data-ttu-id="d4930-108">A primeira abordagem, você gera duas migrações de cada alteração de modelo.</span><span class="sxs-lookup"><span data-stu-id="d4930-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="d4930-109">Uma forma de fazer isso é colocar cada conjunto de migração [em um assembly separado] [ 2] e mudar manualmente o provedor de Active Directory (e o conjunto de migrações) entre as duas migrações de adicionar.</span><span class="sxs-lookup"><span data-stu-id="d4930-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="d4930-110">É outra abordagem que facilita o trabalho com as ferramentas criar um novo tipo que deriva de seu DbContext e substitui o provedor de Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d4930-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="d4930-111">Esse tipo é usado no design de tempo durante a adição ou a aplicação de migrações.</span><span class="sxs-lookup"><span data-stu-id="d4930-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="d4930-112">Como cada conjunto de migração usa seus próprios tipos de DbContext, essa abordagem não exige o uso de um assembly de migrações separadas.</span><span class="sxs-lookup"><span data-stu-id="d4930-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="d4930-113">Ao adicionar nova migração, especifica os tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="d4930-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="d4930-114">Você não precisa especificar o diretório de saída para migrações subsequentes, pois eles são criados como irmãos para o último deles.</span><span class="sxs-lookup"><span data-stu-id="d4930-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="d4930-115">Conjunto de uma migração</span><span class="sxs-lookup"><span data-stu-id="d4930-115">One migration set</span></span>
-----------------
<span data-ttu-id="d4930-116">Se você não gostar de ter dois conjuntos de migrações, você pode combiná-los manualmente em um único conjunto pode ser aplicado a ambos os provedores.</span><span class="sxs-lookup"><span data-stu-id="d4930-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="d4930-117">As anotações podem coexistir, pois um provedor ignora todas as anotações que não entende.</span><span class="sxs-lookup"><span data-stu-id="d4930-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="d4930-118">Por exemplo, uma coluna de chave primária que funciona com o Microsoft SQL Server e SQLite pode parecer com isso.</span><span class="sxs-lookup"><span data-stu-id="d4930-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="d4930-119">Se operações somente podem ser aplicadas em um provedor (ou eles são diferente entre provedores), use o `ActiveProvider` propriedade informar qual provedor está ativo.</span><span class="sxs-lookup"><span data-stu-id="d4930-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
