---
title: Introdução ao ASP.NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: fbc1a00d6d6d0624bcbbfa1e51f4e21a915baaaa
ms.sourcegitcommit: f277883a5ed28eba57d14aaaf17405bc1ae9cf94
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/18/2019
ms.locfileid: "65874573"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Introdução ao EF Core no ASP.NET Core com um novo banco de dados

Neste tutorial, você compila um aplicativo MVC do ASP.NET Core que realiza acesso básico a dados usando o Entity Framework Core. O tutorial usa as migrações para criar o banco de dados do modelo de dados.

Você pode seguir o tutorial usando o Visual Studio 2017 no Windows ou usando a CLI do .NET Core no Windows, macOS ou Linux.

Exiba o exemplo deste artigo no GitHub:
* [Visual Studio 2017 com SQL Server](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [CLI do .NET Core com SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Pré-requisitos

Instale o software a seguir:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 versão 15.7 ou posterior](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:
  * **Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)
  * **Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)
* [SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core).

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* [SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core).

---

## <a name="create-a-new-project"></a>Criar um novo projeto

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Abra o Visual Studio 2017
* **Arquivo > Novo > Projeto**
* No menu à esquerda, selecione **Instalado > Visual C# > .NET Core**.
* Selecione **Aplicativo Web ASP.NET Core**.
* Insira **EFGetStarted.AspNetCore.NewDb** como o nome e clique em **OK**.
* Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:
  * Garanta que **.NET Core** e **ASP.NET Core 2.1** estejam selecionados nas listas suspensas
  * Selecione o modelo de projeto **Aplicativo Web (Model-View-Controller)**
  * Verifique se **Autenticação** está definida como **Sem Autenticação**
  * Clique em **OK**

Aviso: se você usar **Contas de Usuário Individuais** em vez de **Nenhum** para **Autenticação**, um modelo do Entity Framework Core será adicionado ao seu projeto no `Models\IdentityModel.cs`. Com as técnicas que você aprende neste tutorial, é possível optar por adicionar um segundo modelo ou estender o modelo existente para conter as classes de entidade.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* Execute o seguinte comando para criar um projeto do MVC:

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Altere para o diretório do projeto. Os próximos comandos que você inserir precisam ser executados no novo projeto.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Instalar o Entity Framework Core

Para instalar o EF Core, instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Para este tutorial, não é necessário instalar um pacote de provedor, porque o tutorial usa o SQL Server. O pacote de provedor do SQL Server está incluído no [metapacote Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Este tutorial usa SQLite porque ele é executado em todas as plataformas que dão suporte a .NET Core.

* Execute o seguinte comando para instalar o provedor SQLite:

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Criar o modelo

Defina uma classe de contexto e classes de entidade que compõem o modelo.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Clique com o botão direito do mouse na pasta **Modelos** e selecione **Adicionar > Classe**.
* Insira **Model.cs** como o nome e clique em **OK**.
* Substitua o conteúdo do arquivo pelo seguinte código:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* Na pasta **Modelos**, crie **Model.cs** com o seguinte código:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Um aplicativo de produção normalmente colocaria cada classe em um arquivo separado. Para simplificar, este tutorial coloca todas essas classes em um só arquivo.

## <a name="register-the-context-with-dependency-injection"></a>Registrar o contexto com a injeção de dependência

Para disponibilizar o `BloggingContext` aos controladores MVC, registre-o como um serviço em `Startup.cs`.

Os serviços (como `BloggingContext`) são registrados com a [injeção de dependência](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante a inicialização do aplicativo para que eles possam ser fornecidos automaticamente aos componentes que os consomem (como os controladores MVC) por meio de parâmetros e propriedades do construtor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Em **Startup.cs**, adicione as seguintes instruções `using`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Adicione o seguinte código realçado ao método `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* Em **Startup.cs**, adicione as seguintes instruções `using`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Adicione o seguinte código realçado ao método `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

Um aplicativo normalmente colocaria a cadeia de conexão em um arquivo de configuração ou variável de ambiente. Para simplificar, este tutorial define isso no código. Para saber mais, confira [Cadeias de conexão](../../miscellaneous/connection-strings.md).

## <a name="create-the-database"></a>Criar o banco de dados

As etapas a seguir usam [migrações](xref:core/managing-schemas/migrations/index) para criar um banco de dados.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**
* Execute os seguintes comandos:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Se você receber um erro indicando `The term 'add-migration' is not recognized as the name of a cmdlet`, feche e reabra o Visual Studio.

  O comando `Add-Migration` realiza o scaffolding de uma migração e cria o conjunto inicial de tabelas para o modelo. O comando `Update-Database` cria o banco de dados e aplica a nova migração a ele.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* Execute os seguintes comandos:

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  O comando `migrations` realiza o scaffolding de uma migração e cria o conjunto inicial de tabelas para o modelo. O comando `database update` cria o banco de dados e aplica a nova migração a ele.

---

## <a name="create-a-controller"></a>Criar um controlador

Gere um controlador e os modos de exibição para a entidade `Blog`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador**.
* Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Adicionar**.
* Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**.
* Clique em **Adicionar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* Execute os seguintes comandos:

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  Os comandos `tool install` e `add package` instalam as ferramentas que podem realizar o scaffolding de controladores e exibições. O `restore` comando garante que todos os pacotes do projeto foram baixados e o `aspnet-codegenerator` comando realiza o scaffolding.
---

O mecanismo de scaffolding cria os seguintes arquivos:

* Um controlador (*Controllers/BlogsController.cs*)
* Exibições das Razor Pages Criar, Excluir, Detalhes, Editar e Índice (_Views/Blogs/*.cshtml_)

## <a name="run-the-application"></a>Executar o aplicativo

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Depurar** > **Iniciar sem Depuração**

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

```cli
dotnet run
```
---

* Navegue até `/Blogs`

* Use o link **Criar Novo** para criar algumas entradas do blog.

  ![Criar página](_static/create.png)

* Teste os links **Detalhes**, **Editar** e **Excluir**.

  ![Página de índice](_static/index-new-db.png)

## <a name="additional-resources"></a>Recursos adicionais

* [Tutorial: introdução ao EF Core no .NET Core com um novo banco de dados usando o SQLite](xref:core/get-started/netcore/new-db-sqlite)
* [Introdução às Razor Pages no ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start) ou [Introdução ao ASP.NET Core MVC](/aspnet/core/tutorials/first-mvc-app/start-mvc)
* [Tutorial: Razor Pages com o Entity Framework Core no ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro) ou [Introdução ao EF Core em um aplicativo Web do ASP.NET MVC](/aspnet/core/data/ef-mvc/intro)
