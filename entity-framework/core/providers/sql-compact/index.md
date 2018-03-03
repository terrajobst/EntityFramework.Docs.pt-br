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
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Provedor de Banco de Dados EF Core do Microsoft SQL Server Compact Edition

Esse provedor de banco de dados permite que o Entity Framework Core seja usado com o SQL Server Compact Edition. O provedor é mantido como parte do [projeto GitHub ErikEJ/EntityFramework.SqlServerCompact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).

> [!NOTE]  
> Este provedor não é mantido como parte do projeto do Entity Framework Core. Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.

## <a name="install"></a>Instalar o

Para trabalhar com o SQL Server Compact Edition 4.0, instale o [pacote NuGet EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

Para trabalhar com o SQL Server Compact Edition 3.5, instale o [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>Introdução

Consulte a [documentação de introdução no site do projeto](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>Plataformas compatíveis

* .NET Framework (4.5.1 em diante)
