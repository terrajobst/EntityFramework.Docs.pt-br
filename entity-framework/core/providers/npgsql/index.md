---
title: "Provedor de Banco de Dados Npgsql para PostgreSQL – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a>Provedor de Banco de Dados de EF Core Npgsql para PostgreSQL

Este provedor de banco de dados permite que o Entity Framework Core seja usado com PostgreSQL. O provedor é mantido como parte do [projeto Npgsql](http://www.npgsql.org).

> [!NOTE]  
> Este provedor não é mantido como parte do projeto do Entity Framework Core. Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.

## <a name="install"></a>Instalar o

Instale o [pacote NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a>Introdução

Consulte a [documentação do Npgsql](http://www.npgsql.org/efcore/index.html) para começar.

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* PostgreSQL

## <a name="supported-platforms"></a>Plataformas compatíveis

* .NET Framework (4.5.1 em diante)

* .NET Core

* Mono (4.2.0 em diante)
