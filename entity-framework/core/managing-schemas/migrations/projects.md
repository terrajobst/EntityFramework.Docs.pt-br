---
title: Usando um projeto de migrações separado-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 89b7f50fe750c2953aa75efcdffcb1a5199ce90c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416811"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="dde85-102">Usando um projeto de migrações separado</span><span class="sxs-lookup"><span data-stu-id="dde85-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="dde85-103">Talvez você queira armazenar suas migrações em um assembly diferente daquele que contém seu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="dde85-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="dde85-104">Você também pode usar essa estratégia para manter vários conjuntos de migrações, por exemplo, um para desenvolvimento e outro para atualizações de lançamento em lançamento.</span><span class="sxs-lookup"><span data-stu-id="dde85-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="dde85-105">Para fazer isto...</span><span class="sxs-lookup"><span data-stu-id="dde85-105">To do this...</span></span>

1. <span data-ttu-id="dde85-106">Criar uma nova biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="dde85-106">Create a new class library.</span></span>

2. <span data-ttu-id="dde85-107">Adicione uma referência ao seu assembly DbContext.</span><span class="sxs-lookup"><span data-stu-id="dde85-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="dde85-108">Mova os arquivos de instantâneos de modelo e migrações para a biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="dde85-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="dde85-109">Se você não tiver migrações existentes, gere uma no projeto que contém DbContext e, em seguida, mova-a.</span><span class="sxs-lookup"><span data-stu-id="dde85-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="dde85-110">Isso é importante porque, se o assembly de migrações não contiver uma migração existente, o comando Add-Migration não conseguirá localizar o DbContext.</span><span class="sxs-lookup"><span data-stu-id="dde85-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="dde85-111">Configure o assembly de migrações:</span><span class="sxs-lookup"><span data-stu-id="dde85-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="dde85-112">Adicione uma referência ao assembly de migrações do assembly de inicialização.</span><span class="sxs-lookup"><span data-stu-id="dde85-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="dde85-113">Se isso causar uma dependência circular, atualize o caminho de saída da biblioteca de classes:</span><span class="sxs-lookup"><span data-stu-id="dde85-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="dde85-114">Se você fez tudo corretamente, deve ser capaz de adicionar novas migrações ao projeto.</span><span class="sxs-lookup"><span data-stu-id="dde85-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-cli"></a>[<span data-ttu-id="dde85-115">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="dde85-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studio"></a>[<span data-ttu-id="dde85-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dde85-116">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
