---
title: Provedor de Banco de Dados SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e4cbdba46f901831892192a343db2920a5760042
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149261"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="c6b14-102">Provedor de Banco de Dados EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="c6b14-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="c6b14-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com SQLite.</span><span class="sxs-lookup"><span data-stu-id="c6b14-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="c6b14-104">O provedor é mantido como parte do [projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="c6b14-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="c6b14-105">Instalar o</span><span class="sxs-lookup"><span data-stu-id="c6b14-105">Install</span></span>

<span data-ttu-id="c6b14-106">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="c6b14-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a><span data-ttu-id="c6b14-107">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="c6b14-107">Supported Database Engines</span></span>

* <span data-ttu-id="c6b14-108">SQLite (3.7 em diante)</span><span class="sxs-lookup"><span data-stu-id="c6b14-108">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="c6b14-109">Limitações</span><span class="sxs-lookup"><span data-stu-id="c6b14-109">Limitations</span></span>

<span data-ttu-id="c6b14-110">Consulte [Limitações do SQLite](limitations.md) para algumas limitações importantes do provedor SQLite.</span><span class="sxs-lookup"><span data-stu-id="c6b14-110">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
