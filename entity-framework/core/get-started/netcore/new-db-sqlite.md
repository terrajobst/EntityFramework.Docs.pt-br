---
title: Introdução ao .NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
description: Introdução ao .NET Core usando o Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: ec20040917a2bca8177924b6905b1cd79e5cd9da
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834728"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Introdução ao EF Core no aplicativo de console do .NET Core com um novo banco de dados

Neste tutorial, você criará um aplicativo de console do .NET Core que executa acesso a dados em um banco de dados SQLite usando o Entity Framework Core. Você usará migrações para criar o banco de dados com base no modelo. Veja [ASP.NET Core – Novo banco de dados](xref:core/get-started/aspnetcore/new-db) para ter uma versão do Visual Studio usando o ASP.NET Core MVC.

Exiba o exemplo deste artigo no GitHub (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite)).

## <a name="prerequisites"></a>Pré-requisitos

* [SDK do .NET Core 2.1](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Criar um novo projeto

* Crie um novo projeto de console:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>Alterar o diretório atual

Em etapas subsequentes, é preciso emitir comandos `dotnet` com relação ao aplicativo.

* Podemos alterar o diretório atual para o diretório do aplicativo como este:

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a>Instalar o Entity Framework Core

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este passo a passo usa SQLite. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* Instalar Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Execute `dotnet restore` para instalar os novos pacotes.

## <a name="create-the-model"></a>Criar o modelo

Defina um contexto e classes de entidade que compõem seu modelo.

* Crie um novo arquivo *Model.cs* com o seguinte conteúdo.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Dica: em um aplicativo real, você coloca cada classe em um arquivo separado e coloca a cadeia de conexão em um arquivo de configuração ou na variável de ambiente. Para simplificar o tutorial, tudo está contido em um arquivo.

## <a name="create-the-database"></a>Criar o banco de dados

Assim que você tiver um modelo, use as [migrações](xref:core/managing-schemas/migrations/index) para criar um banco de dados.

* Execute `dotnet ef migrations add InitialCreate` para realizar scaffolding de uma migração e criar o conjunto inicial de tabelas para o modelo.
* Execute `dotnet ef database update` para aplicar a nova migração ao banco de dados. Este comando cria o banco de dados antes de aplicar as migrações.

O banco de dados SQLite *blogging.db** está no diretório do projeto.

## <a name="use-the-model"></a>Usar o modelo

* Abra *Program.cs* e substitua o conteúdo pelo código a seguir:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Teste o aplicativo do console. Veja a [observação do Visual Studio](#vs) para executar o aplicativo do Visual Studio.

  `dotnet run`

  Um blog é salvo no banco de dados, e os detalhes de todos os blogs são exibidos no console.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Alteração do modelo:

- Se você fizer alterações no modelo, poderá usar o comando `dotnet ef migrations add` para realizar scaffolding de uma nova [migração](xref:core/managing-schemas/migrations/index). Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), é possível usar o comando `dotnet ef database update` para aplicar as alterações do esquema no banco de dados.
- O EF Core usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.
- O mecanismo de banco de dados do SQLite não dá suporte a determinadas alterações de esquema que têm suporte na maioria dos outros bancos de dados relacionais. Por exemplo, não há suporte para a operação `DropColumn`. As migrações do EF Core geram código para essas operações. Mas se você tentar aplicá-las a um banco de dados ou gerar um script, o EF Core gerará exceções. Veja [Limitações do SQLite](../../providers/sqlite/limitations.md). Para novos desenvolvimentos, considere descartar o banco de dados e criar um novo em vez de usar migrações quando o modelo for alterado.

<a name="vs"></a>
### <a name="run-from-visual-studio"></a>Executar usando o Visual Studio

Para executar este exemplo do Visual Studio, você deve definir o diretório de trabalho manualmente para ser a raiz do projeto. Se você não definir o diretório de trabalho, o seguinte `Microsoft.Data.Sqlite.SqliteException` será gerado: `SQLite Error 1: 'no such table: Blogs'`.

Para definir o diretório de trabalho:

* No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Propriedades**.
* Selecione a guia **Depurar** no painel esquerdo.
* Defina o **Diretório de trabalho** como o diretório do projeto.
* Salve as alterações.

## <a name="additional-resources"></a>Recursos adicionais

* [Tutorial: Introdução ao EF Core no ASP.NET Core com um novo banco de dados usando o SQLite](xref:core/get-started/aspnetcore/new-db)
* [Tutorial: introdução ao Razor Pages no ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Tutorial: Razor Pages com o Entity Framework Core no ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
