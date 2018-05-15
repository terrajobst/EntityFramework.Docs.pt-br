---
title: Introdução ao .NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Introdução ao .NET Core usando o Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: fcace3c0f259b1a456d9ca1086e6a1549c070d57
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Introdução ao EF Core no aplicativo de console do .NET Core com um novo banco de dados

Neste passo a passo, você criará um aplicativo de console do .NET Core que executa acesso a dados básicos em um banco de dados SQLite usando o Entity Framework Core. Você usará as migrações para criar o banco de dados de seu modelo. Veja [ASP.NET Core – Novo banco de dados](xref:core/get-started/aspnetcore/new-db) para ter uma versão do Visual Studio usando o ASP.NET Core MVC.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) deste artigo no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

Você precisará atender aos pré-requisitos a seguir para concluir este passo a passo:
* Um sistema operacional que dá suporte ao .NET Core.
* [O SDK do .NET Core](https://www.microsoft.com/net/core) 2.0 (embora as instruções possam ser usadas para criar um aplicativo com uma versão anterior, com poucas modificações).

## <a name="create-a-new-project"></a>Criar um novo projeto

* Crie uma nova pasta `ConsoleApp.SQLite` para seu projeto e use o comando `dotnet` para preenchê-la com um aplicativo .NET Core.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Instalar o Entity Framework Core

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este passo a passo usa SQLite. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* Instalar Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Edite manualmente `ConsoleApp.SQLite.csproj` para adicionar um DotNetCliToolReference ao Microsoft.EntityFrameworkCore.Tools.DotNet:

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

`ConsoleApp.SQLite.csproj` deve conter os seguintes:

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Observação: os números de versão usados acima estavam corretos no momento da publicação.

*  Execute `dotnet restore` para instalar os novos pacotes.

## <a name="create-the-model"></a>Criar o modelo

Defina um contexto e classes de entidade que compõem seu modelo.

* Crie um novo arquivo *Model.cs* com o seguinte conteúdo.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Dica: em um aplicativo real, você colocaria cada classe em um arquivo separado, e colocaria a cadeia de conexão em um arquivo de configuração. Para simplificar o tutorial, estamos colocando tudo em um arquivo.

## <a name="create-the-database"></a>Criar o banco de dados

Assim que você tiver um modelo, use as [migrações](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para criar um banco de dados.

* Execute `dotnet ef migrations add InitialCreate` para realizar scaffolding de uma migração e criar o conjunto inicial de tabelas para o modelo.
* Execute `dotnet ef database update` para aplicar a nova migração ao banco de dados. Este comando cria o banco de dados antes de aplicar as migrações.

> [!NOTE]  
> Ao usar caminhos relativos com o SQLite, o caminho será relativo ao assembly principal do aplicativo. Neste exemplo, o binário principal é `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, portanto, o banco de dados SQLite estará em `bin/Debug/netcoreapp2.0/blogging.db`.

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
