---
title: Usando um projeto de migrações separado-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812035"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="bbef1-102">Usando um projeto de migrações separado</span><span class="sxs-lookup"><span data-stu-id="bbef1-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="bbef1-103">Talvez você queira armazenar suas migrações em um assembly diferente daquele que contém seu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bbef1-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="bbef1-104">Você também pode usar essa estratégia para manter vários conjuntos de migrações, por exemplo, um para desenvolvimento e outro para atualizações de lançamento em lançamento.</span><span class="sxs-lookup"><span data-stu-id="bbef1-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="bbef1-105">Para fazer isso...</span><span class="sxs-lookup"><span data-stu-id="bbef1-105">To do this...</span></span>

1. <span data-ttu-id="bbef1-106">Criar uma nova biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="bbef1-106">Create a new class library.</span></span>

2. <span data-ttu-id="bbef1-107">Adicione uma referência ao seu assembly DbContext.</span><span class="sxs-lookup"><span data-stu-id="bbef1-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="bbef1-108">Mova os arquivos de instantâneos de modelo e migrações para a biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="bbef1-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="bbef1-109">Se você não tiver migrações existentes, gere uma no projeto que contém DbContext e, em seguida, mova-a.</span><span class="sxs-lookup"><span data-stu-id="bbef1-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="bbef1-110">Isso é importante porque, se o assembly de migrações não contiver uma migração existente, o comando Add-Migration não conseguirá localizar o DbContext.</span><span class="sxs-lookup"><span data-stu-id="bbef1-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="bbef1-111">Configure o assembly de migrações:</span><span class="sxs-lookup"><span data-stu-id="bbef1-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="bbef1-112">Adicione uma referência ao assembly de migrações do assembly de inicialização.</span><span class="sxs-lookup"><span data-stu-id="bbef1-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="bbef1-113">Se isso causar uma dependência circular, atualize o caminho de saída da biblioteca de classes:</span><span class="sxs-lookup"><span data-stu-id="bbef1-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="bbef1-114">Se você fez tudo corretamente, deve ser capaz de adicionar novas migrações ao projeto.</span><span class="sxs-lookup"><span data-stu-id="bbef1-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
