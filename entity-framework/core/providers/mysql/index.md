---
title: "Provedor de Banco de Dados MySQL – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: c151845c8b08ef6a668b352f15545752156b0a9d
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="mysql-ef-core-database-provider"></a>Provedor de Banco de Dados EF Core MySQL

Este provedor de banco de dados permite que o Entity Framework Core seja usado com MySQL. O provedor é mantido como parte do [projeto MySQL](http://dev.mysql.com).

> [!WARNING]  
> Esse provedor é a versão de pré-lançamento.

> [!NOTE]  
> Este provedor não é mantido como parte do projeto do Entity Framework Core. Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.

## <a name="install"></a>Instalar o

Instale o [pacote NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>Introdução

Consulte [Introdução ao provedor MySQL EF Core e Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* MySQL

## <a name="supported-platforms"></a>Plataformas compatíveis

* .NET Framework (4.5.1 em diante)

* .NET Core
