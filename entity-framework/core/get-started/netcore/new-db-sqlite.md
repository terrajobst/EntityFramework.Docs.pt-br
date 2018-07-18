---
title: Introdução ao .NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Introdução ao .NET Core usando o Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911496"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Introdução ao EF Core no aplicativo de console do .NET Core com um novo banco de dados

Neste passo a passo, você criará um aplicativo de console do .NET Core que executa acesso a dados em um banco de dados SQLite usando o Entity Framework Core. Você usará migrações para criar o banco de dados com base no modelo. Veja [ASP.NET Core – Novo banco de dados](xref:core/get-started/aspnetcore/new-db) para ter uma versão do Visual Studio usando o ASP.NET Core MVC.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) deste artigo no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

[O SDK do .NET Core](https://www.microsoft.com/net/core) 2.1

## <a name="create-a-new-project"></a>Criar um novo projeto

* Crie um novo projeto de console:

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a>Instalar o Entity Framework Core

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este passo a passo usa SQLite. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* Instalar Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Execute `dotnet restore` para instalar os novos pacotes.

## <a name="create-the-model"></a>Criar o modelo

Defina um contexto e classes de entidade que compõem seu modelo.

* Crie um novo arquivo *Model.cs* com o seguinte conteúdo.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Dica: em um aplicativo real, você coloca cada classe em um arquivo separado e coloca a cadeia de conexão em um arquivo de configuração. Para simplificar o tutorial, tudo está contido em um arquivo.

## <a name="create-the-database"></a>Criar o banco de dados

Assim que você tiver um modelo, use as [migrações](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para criar um banco de dados.

* Execute `dotnet ef migrations add InitialCreate` para realizar scaffolding de uma migração e criar o conjunto inicial de tabelas para o modelo.
* Execute `dotnet ef database update` para aplicar a nova migração ao banco de dados. Este comando cria o banco de dados antes de aplicar as migrações.

O banco de dados SQLite *blogging.db** está no diretório do projeto.

## <a name="use-your-model"></a>Usar o modelo

* Abra *Program.cs* e substitua o conteúdo pelo código a seguir:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Teste o aplicativo:

  `dotnet run`

  Um blog é salvo no banco de dados, e os detalhes de todos os blogs são exibidos no console.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Alteração do modelo:

- Se você fizer alterações em seu modelo, use o comando `dotnet ef migrations add` para realizar o scaffolding de uma nova [migração](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para fazer as alterações correspondentes de esquema no banco de dados. Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), use o comando `dotnet ef database update` para aplicar as alterações no banco de dados.
- O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.
- O SQLite não oferece suporte a todas as migrações (alterações de esquema) devido às limitações no SQLite. Veja [Limitações do SQLite](../../providers/sqlite/limitations.md). Para novos desenvolvimentos, considere descartar o banco de dados e criar um novo em vez de usar migrações quando o modelo for alterado.

## <a name="additional-resources"></a>Recursos adicionais

* [.Net Core – Novo banco de dados com SQLite](xref:core/get-started/netcore/new-db-sqlite) – um tutorial EF de console de plataforma cruzada.
* [Introdução ao ASP.NET Core MVC no Mac ou Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introdução ao ASP.NET Core MVC com o Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Introdução ao ASP.NET Core e ao Entity Framework Core usando o Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
