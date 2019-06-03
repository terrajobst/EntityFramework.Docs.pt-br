---
title: Referência de ferramentas do Entity Framework Core – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 13e80f740bc5ce3404e8dba40b65ec872c5e3f90
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452259"
---
# <a name="entity-framework-core-tools-reference"></a>Referência de ferramentas do Entity Framework Core

As ferramentas do Entity Framework Core ajudam com tarefas de desenvolvimento de tempo de design. São usados principalmente para gerenciar Migrações e criar o scaffolding de um `DbContext` e tipos de entidade por engenharia reversa do esquema de um banco de dados.

* As [ferramentas do Console do Gerenciador de Pacotes do EF Core](powershell.md) são executadas no [Console do Gerenciador de Pacotes](https://docs.microsoft.com/nuget/tools/package-manager-console) no Visual Studio. Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.

* As [ferramentas de CLI (interface de linha de comando) do EF Core .NET](dotnet.md) são uma extensão das [ferramentas da CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/) multiplataforma. Essas ferramentas exigem um projeto de SDK do .NET Core (com `Sdk="Microsoft.NET.Sdk"` ou semelhantes no arquivo de projeto).

Ambas as ferramentas expõem a mesma funcionalidade. Se você está desenvolvendo no Visual Studio, é recomendável usar as ferramentas do **Console do Gerenciador de Pacotes**, pois elas oferecem uma experiência mais integrada.

## <a name="next-steps"></a>Próximas etapas

* [Referência de ferramentas do Console do Gerenciador de Pacotes do EF Core](powershell.md)
* [Referência de ferramentas de CLI do .NET Core EF](dotnet.md)
