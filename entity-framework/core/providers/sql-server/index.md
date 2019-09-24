---
title: Provedor de Banco de Dados do Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 70e7f3d4bc1f57f1b23d9b3e0bd6264236ddbd27
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149201"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="5c8e8-102">Provedor de Banco de Dados EF Core do Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="5c8e8-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="5c8e8-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Microsoft SQL Server (incluindo o SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="5c8e8-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="5c8e8-104">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="5c8e8-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="5c8e8-105">Instalar o</span><span class="sxs-lookup"><span data-stu-id="5c8e8-105">Install</span></span>

<span data-ttu-id="5c8e8-106">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="5c8e8-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="supported-database-engines"></a><span data-ttu-id="5c8e8-107">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="5c8e8-107">Supported Database Engines</span></span>

* <span data-ttu-id="5c8e8-108">Microsoft SQL Server (2012 em diante)</span><span class="sxs-lookup"><span data-stu-id="5c8e8-108">Microsoft SQL Server (2012 onwards)</span></span>
