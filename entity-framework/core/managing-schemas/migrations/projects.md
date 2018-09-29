---
title: Migrações com vários projetos – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447138"
---
<a name="using-a-separate-project"></a><span data-ttu-id="4b2c3-102">Usando um projeto separado</span><span class="sxs-lookup"><span data-stu-id="4b2c3-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="4b2c3-103">Você pode desejar armazenar suas migrações em um assembly diferente que o outro contendo seu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="4b2c3-104">Você também pode usar essa estratégia para manter vários conjuntos de migrações, por exemplo, um para desenvolvimento e outro para atualizações de versão para versão.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="4b2c3-105">Para fazer isso...</span><span class="sxs-lookup"><span data-stu-id="4b2c3-105">To do this...</span></span>

1. <span data-ttu-id="4b2c3-106">Criar uma nova biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-106">Create a new class library.</span></span>

2. <span data-ttu-id="4b2c3-107">Adicione uma referência ao seu assembly de DbContext.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="4b2c3-108">Mova as migrações e arquivos de instantâneo do modelo para a biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="4b2c3-109">Se você não tiver nenhuma migração existente, gerá-lo no projeto que contém o DbContext e em seguida, movê-lo.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="4b2c3-110">Isso é importante porque se o assembly de migrações não contiver uma migração existente, o comando Add-Migration será não é possível localizar o DbContext.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="4b2c3-111">Configure o conjunto de migrações:</span><span class="sxs-lookup"><span data-stu-id="4b2c3-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="4b2c3-112">Adicione uma referência ao seu assembly de migrações do assembly de inicialização.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="4b2c3-113">Se isso faz com que uma dependência circular, atualize o caminho de saída da biblioteca de classes:</span><span class="sxs-lookup"><span data-stu-id="4b2c3-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="4b2c3-114">Se você fez tudo corretamente, você poderá adicionar novas migrações ao projeto.</span><span class="sxs-lookup"><span data-stu-id="4b2c3-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
