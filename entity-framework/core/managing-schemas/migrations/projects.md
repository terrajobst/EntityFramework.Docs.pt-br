---
title: Migrações com vários projetos – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997981"
---
<a name="using-a-separate-project"></a><span data-ttu-id="c9b2a-102">Usando um projeto separado</span><span class="sxs-lookup"><span data-stu-id="c9b2a-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="c9b2a-103">Você pode desejar armazenar suas migrações em um assembly diferente que o outro contendo seu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="c9b2a-104">Você também pode usar essa estratégia para manter vários conjuntos de migrações, por exemplo, um para desenvolvimento e outro para atualizações de versão para versão.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="c9b2a-105">Para fazer isso...</span><span class="sxs-lookup"><span data-stu-id="c9b2a-105">To do this...</span></span>

1. <span data-ttu-id="c9b2a-106">Criar uma nova biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-106">Create a new class library.</span></span>

2. <span data-ttu-id="c9b2a-107">Adicione uma referência ao seu assembly de DbContext.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="c9b2a-108">Mova as migrações e arquivos de instantâneo do modelo para a biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="c9b2a-109">Se você não tiver nenhuma migração existente, gerá-lo no projeto que contém o DbContext e em seguida, movê-lo.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="c9b2a-110">Isso é importante porque se o assembly de migrações não contiver uma migração existente, o comando Add-Migration será não é possível localizar o DbContext.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="c9b2a-111">Configure o conjunto de migrações:</span><span class="sxs-lookup"><span data-stu-id="c9b2a-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="c9b2a-112">Adicione uma referência ao seu assembly de migrações do assembly de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="c9b2a-113">Se isso faz com que uma dependência circular, atualize o caminho de saída da biblioteca de classes:</span><span class="sxs-lookup"><span data-stu-id="c9b2a-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="c9b2a-114">Se você fez tudo corretamente, você poderá adicionar novas migrações ao projeto.</span><span class="sxs-lookup"><span data-stu-id="c9b2a-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
