---
title: "Migrações com vários projetos - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="using-a-separate-project"></a><span data-ttu-id="906e3-102">Usando um projeto separado</span><span class="sxs-lookup"><span data-stu-id="906e3-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="906e3-103">Você pode desejar armazenar suas migrações em um conjunto diferente de uma contendo o `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="906e3-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="906e3-104">Você também pode usar essa estratégia para manter vários conjuntos de migrações, por exemplo, um para o desenvolvimento e outro para atualizações de versão para versão.</span><span class="sxs-lookup"><span data-stu-id="906e3-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="906e3-105">Para fazer isso...</span><span class="sxs-lookup"><span data-stu-id="906e3-105">To do this...</span></span>

1. <span data-ttu-id="906e3-106">Criar uma nova biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="906e3-106">Create a new class library.</span></span>

2. <span data-ttu-id="906e3-107">Adicione uma referência ao seu assembly de DbContext.</span><span class="sxs-lookup"><span data-stu-id="906e3-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="906e3-108">Mova os arquivos de instantâneo de modelo e migrações para a biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="906e3-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="906e3-109">Se você não adicionou nenhum, adicione uma ao projeto DbContext e movê-lo.</span><span class="sxs-lookup"><span data-stu-id="906e3-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="906e3-110">Configure o assembly de migrações:</span><span class="sxs-lookup"><span data-stu-id="906e3-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="906e3-111">Adicione uma referência ao seu assembly de migrações do assembly de inicialização.</span><span class="sxs-lookup"><span data-stu-id="906e3-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="906e3-112">Se isso faz com que uma dependência circular, atualize o caminho de saída da biblioteca de classes:</span><span class="sxs-lookup"><span data-stu-id="906e3-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="906e3-113">Se você fez tudo corretamente, você poderá adicionar novas migrações ao projeto.</span><span class="sxs-lookup"><span data-stu-id="906e3-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
