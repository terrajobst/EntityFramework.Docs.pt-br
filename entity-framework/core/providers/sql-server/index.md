---
title: Provedor de Banco de Dados do Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995663"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Provedor de Banco de Dados EF Core do Microsoft SQL Server

Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Microsoft SQL Server (incluindo o SQL Azure). O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar o

Instale o [pacote NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Introdução

Os recursos a seguir ajudará você a começar a usar esse provedor.
* [Introdução ao .NET Framework (Console, WinForms, WPF etc.)](../../get-started/full-dotnet/index.md)

* [Introdução ao ASP.NET Core](../../get-started/aspnetcore/index.md)

* [Aplicativo de exemplo UnicornStore](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* Microsoft SQL Server (2008 em diante)

## <a name="supported-platforms"></a>Plataformas compatíveis

* .NET Framework (4.5.1 em diante)

* .NET Core

* Mono (4.2.0 em diante)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
