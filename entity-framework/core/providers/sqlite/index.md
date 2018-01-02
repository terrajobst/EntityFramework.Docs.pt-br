---
title: "Provedor de Banco de Dados SQLite – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a>Provedor de Banco de Dados EF Core SQLite

Este provedor de banco de dados permite que o Entity Framework Core seja usado com SQLite. O provedor é mantido como parte do [projeto EntityFramework GitHub](https://github.com/aspnet/EntityFramework).

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
