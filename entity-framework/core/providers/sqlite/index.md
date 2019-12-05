---
title: Provedor de Banco de Dados SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 433dedbe1e0e11bf2fd83e259e321ece32b2c188
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824795"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="cc838-102">Provedor de Banco de Dados EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="cc838-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="cc838-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com SQLite.</span><span class="sxs-lookup"><span data-stu-id="cc838-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="cc838-104">O provedor é mantido como parte do [projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="cc838-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="cc838-105">Instalar o</span><span class="sxs-lookup"><span data-stu-id="cc838-105">Install</span></span>

<span data-ttu-id="cc838-106">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="cc838-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="cc838-107">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc838-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="cc838-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc838-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="cc838-109">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="cc838-109">Supported Database Engines</span></span>

* <span data-ttu-id="cc838-110">SQLite (3.7 em diante)</span><span class="sxs-lookup"><span data-stu-id="cc838-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="cc838-111">Limitações</span><span class="sxs-lookup"><span data-stu-id="cc838-111">Limitations</span></span>

<span data-ttu-id="cc838-112">Consulte [Limitações do SQLite](limitations.md) para algumas limitações importantes do provedor SQLite.</span><span class="sxs-lookup"><span data-stu-id="cc838-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
