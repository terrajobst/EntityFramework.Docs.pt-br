---
title: "Provedor de Banco de Dados de InMemory – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="ef-core-in-memory-database-provider"></a>Provedor de Banco de Dados na Memória do EF Core

Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória. Isso pode ser útil para testes, embora o provedor SQLite, no modo em memória, seja uma substituição de teste mais adequada para bancos de dados relacionais. O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar o

Instale o [pacote NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Introdução

Os recursos a seguir ajudará você a começar a usar esse provedor.
* [Testar com InMemory](../../miscellaneous/testing/in-memory.md)

* [Testes de Aplicativo de Exemplo UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* Banco de dados na memória interno (projetado somente para fins de teste)

## <a name="supported-platforms"></a>Plataformas compatíveis

* .NET Framework (4.5.1 em diante)

* .NET Core

* Mono (4.2.0 em diante)

* Plataforma Universal do Windows
