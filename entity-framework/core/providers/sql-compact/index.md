---
title: "Provedor de Banco de Dados do Microsoft SQL Server Compact Edition – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="012ea-102">Provedor de Banco de Dados EF Core do Microsoft SQL Server Compact Edition</span><span class="sxs-lookup"><span data-stu-id="012ea-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="012ea-103">Esse provedor de banco de dados permite que o Entity Framework Core seja usado com o SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="012ea-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="012ea-104">O provedor é mantido como parte do [projeto GitHub ErikEJ/EntityFramework.SqlServerCompact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="012ea-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="012ea-105">Este provedor não é mantido como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="012ea-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="012ea-106">Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="012ea-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="012ea-107">Instalar o</span><span class="sxs-lookup"><span data-stu-id="012ea-107">Install</span></span>

<span data-ttu-id="012ea-108">Para trabalhar com o SQL Server Compact Edition 4.0, instale o [pacote NuGet EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span><span class="sxs-lookup"><span data-stu-id="012ea-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="012ea-109">Para trabalhar com o SQL Server Compact Edition 3.5, instale o [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span><span class="sxs-lookup"><span data-stu-id="012ea-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="012ea-110">Introdução</span><span class="sxs-lookup"><span data-stu-id="012ea-110">Get Started</span></span>

<span data-ttu-id="012ea-111">Consulte a [documentação de introdução no site do projeto](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span><span class="sxs-lookup"><span data-stu-id="012ea-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="012ea-112">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="012ea-112">Supported Database Engines</span></span>

* <span data-ttu-id="012ea-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="012ea-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="012ea-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="012ea-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="012ea-115">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="012ea-115">Supported Platforms</span></span>

* <span data-ttu-id="012ea-116">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="012ea-116">.NET Framework (4.5.1 onwards)</span></span>
