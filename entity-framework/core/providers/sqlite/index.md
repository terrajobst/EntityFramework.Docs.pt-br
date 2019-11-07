---
title: Provedor de Banco de Dados SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 4637044b1712280d3cdca6bcca1ca61564ff78ea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656080"
---
# <a name="sqlite-ef-core-database-provider"></a>Provedor de Banco de Dados EF Core SQLite

Este provedor de banco de dados permite que o Entity Framework Core seja usado com SQLite. O provedor é mantido como parte do [projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar o

Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

## <a name="net-core-clitabdotnet-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* SQLite (3.7 em diante)

## <a name="limitations"></a>Limitações

Consulte [Limitações do SQLite](limitations.md) para algumas limitações importantes do provedor SQLite.
