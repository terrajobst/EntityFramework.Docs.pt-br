---
title: "Provedor de Banco de Dados do IBM Data Server (DB2) – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>Provedores de Banco de Dados do EF Core do IBM Data Server (DB2)

Este provedor de banco de dados permite que o Entity Framework Core seja usado com IBM Data Server. O provedor é mantido pela IBM.

> [!NOTE]  
> Este provedor não é mantido como parte do projeto do Entity Framework Core. Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.

## <a name="install"></a>Instalar o

Para trabalhar com o IBM Data Server no Windows, instale o [pacote NuGet IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore).

``` powershell
Install-Package IBM.EntityFrameworkCore
```

Para trabalhar com o IBM Data Server no Linux, instale o [pacote NuGet IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>Introdução

[Introdução ao Provedor .NET da IBM para .NET Core](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* zOS
* LUW incluindo IBM dashDB
* IBM I
* Informix

## <a name="supported-platforms"></a>Plataformas compatíveis

* .NET Framework (4.6 em diante)
* .NET Core
