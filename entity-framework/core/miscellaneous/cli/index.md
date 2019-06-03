---
title: Referência de ferramentas do Entity Framework Core – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 13e80f740bc5ce3404e8dba40b65ec872c5e3f90
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452259"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="df3b3-102">Referência de ferramentas do Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="df3b3-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="df3b3-103">As ferramentas do Entity Framework Core ajudam com tarefas de desenvolvimento de tempo de design.</span><span class="sxs-lookup"><span data-stu-id="df3b3-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="df3b3-104">São usados principalmente para gerenciar Migrações e criar o scaffolding de um `DbContext` e tipos de entidade por engenharia reversa do esquema de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="df3b3-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="df3b3-105">As [ferramentas do Console do Gerenciador de Pacotes do EF Core](powershell.md) são executadas no [Console do Gerenciador de Pacotes](https://docs.microsoft.com/nuget/tools/package-manager-console) no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df3b3-105">The [EF Core Package Manager Console tools](powershell.md) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="df3b3-106">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="df3b3-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

* <span data-ttu-id="df3b3-107">As [ferramentas de CLI (interface de linha de comando) do EF Core .NET](dotnet.md) são uma extensão das [ferramentas da CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/) multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="df3b3-107">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="df3b3-108">Essas ferramentas exigem um projeto de SDK do .NET Core (com `Sdk="Microsoft.NET.Sdk"` ou semelhantes no arquivo de projeto).</span><span class="sxs-lookup"><span data-stu-id="df3b3-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="df3b3-109">Ambas as ferramentas expõem a mesma funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="df3b3-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="df3b3-110">Se você está desenvolvendo no Visual Studio, é recomendável usar as ferramentas do **Console do Gerenciador de Pacotes**, pois elas oferecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="df3b3-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df3b3-111">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="df3b3-111">Next steps</span></span>

* [<span data-ttu-id="df3b3-112">Referência de ferramentas do Console do Gerenciador de Pacotes do EF Core</span><span class="sxs-lookup"><span data-stu-id="df3b3-112">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="df3b3-113">Referência de ferramentas de CLI do .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="df3b3-113">EF Core .NET CLI tools reference</span></span>](dotnet.md)
