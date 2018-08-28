---
title: Provedor de Banco de Dados SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993996"
---
# <a name="sqlite-ef-core-database-provider"></a>Provedor de Banco de Dados EF Core SQLite

Este provedor de banco de dados permite que o Entity Framework Core seja usado com SQLite. O provedor é mantido como parte do [projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar o

Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Introdução

Os recursos a seguir ajudará você a começar a usar esse provedor.
* [SQLite Local em UWP](../../get-started/uwp/getting-started.md)

* [Aplicativo .NET Core para Novo Banco de Dados SQLite](../../get-started/netcore/new-db-sqlite.md)

* [Aplicativo de Exemplo Unicorn Clicker](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Aplicativo de Exemplo Unicorn Packer](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* SQLite (3.7 em diante)

## <a name="supported-platforms"></a>Plataformas compatíveis

* .NET Framework (4.5.1 em diante)

* .NET Core

* Mono (4.2.0 em diante)

* Plataforma Universal do Windows

## <a name="limitations"></a>Limitações

Consulte [Limitações do SQLite](limitations.md) para algumas limitações importantes do provedor SQLite.
