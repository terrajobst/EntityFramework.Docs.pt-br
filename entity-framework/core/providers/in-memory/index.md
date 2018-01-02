---
title: "Provedor de Banco de Dados de InMemory – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a>Provedor de Banco de Dados na Memória do EF Core

Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória. Isso é útil ao testar o código que usa o Entity Framework Core. O provedor é mantido como parte do [projeto EntityFramework GitHub](https://github.com/aspnet/EntityFramework).

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
