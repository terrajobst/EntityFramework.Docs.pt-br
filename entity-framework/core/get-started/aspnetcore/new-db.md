---
title: Introdução ao ASP.NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Introdução ao EF Core no ASP.NET Core com um novo banco de dados

Neste passo a passo, você compilará um aplicativo MVC do ASP.NET Core que executa acesso básico aos dados usando o Entity Framework Core. Você usará as migrações para criar o banco de dados de seu modelo do EF Core. Veja [Recursos adicionais](#additional-resources) para conferir mais tutoriais do Entity Framework Core.

Este tutorial exige:
* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:
  * **Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)
  * **Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)
* [SDK do .NET Core 2.0](https://www.microsoft.com/net/download/core).

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) deste artigo no GitHub.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Criar um novo projeto no Visual Studio 2017

* **Arquivo > Novo > Projeto**
* No menu à esquerda, selecione **Instalado > Modelos > Visual C# > .Net Core**.
* Selecione **Aplicativo Web ASP.NET Core**.
* Insira **EFGetStarted.AspNetCore.NewDb** como o nome e clique em **OK**.
* Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:
  * Certifique-se de que as opções **.NET Core** e **ASP.NET Core 2.0** estejam selecionadas nas listas suspensas
  * Selecione o modelo de projeto **Aplicativo Web (Model-View-Controller)**
  * Certifique-se de que **Autenticação** esteja definida como **Sem Autenticação**
  * Clique em **OK**

Aviso: se você usar **Contas de Usuário Individuais** em vez de **Nenhum** para **Autenticação**, um modelo do Entity Framework Core será adicionado ao seu projeto no `Models\IdentityModel.cs`. Com as técnicas que você aprenderá neste passo a passo, você pode optar por adicionar um segundo modelo ou estender esse modelo existente para conter as classes de entidade.

## <a name="install-entity-framework-core"></a>Instalar o Entity Framework Core

Instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar. Este passo a passo usa o SQL Server. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**

* Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Usaremos Entity Framework Core Tools para criar um banco de dados de se modelo do EF Core. Portanto, também instalaremos o pacote de ferramentas:

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`

Usaremos algumas ferramentas de scaffolding do ASP.NET Core para criar controladores e exibições posteriormente. Portanto, também instalaremos o pacote de design:

* Execute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="create-the-model"></a>Criar o modelo

Defina um contexto e classes de entidade que compõem o modelo:

* Clique com o botão direito do mouse na pasta **Modelos** e selecione **Adicionar > Classe**.
* Insira **Model.cs** como o nome e clique em **OK**.
* Substitua o conteúdo do arquivo pelo seguinte código:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Observação: em um aplicativo real, você normalmente colocaria cada classe de seu modelo em um arquivo separado. Para simplificar, estamos colocando todas as classes em um arquivo para este tutorial.

## <a name="register-your-context-with-dependency-injection"></a>Registre seu contexto com injeção de dependência

Serviços (como `BloggingContext`) são registrados com [injeção de dependência](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante a inicialização do aplicativo. Agora, os componentes que exigem esses serviços (como os controladores de MVC) os recebem por meio de propriedades ou parâmetros do construtor.

Para que os controladores MVC usem o `BloggingContext`, vamos registrá-lo como um serviço.

* Abra **Startup.cs**
* Adicione as seguintes instruções `using`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Adicione o método `AddDbContext` para registrá-lo como um serviço:

* Adicione o seguinte código ao método `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Observação: um aplicativo real normalmente colocaria a cadeia de conexão em um arquivo de configuração. Para simplificar, definimos isso no código. Para saber mais, confira [Cadeias de conexão](../../miscellaneous/connection-strings.md).

## <a name="create-your-database"></a>Criar seu banco de dados

Assim que você tiver um modelo, use as [migrações](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para criar um banco de dados.

* Abra o PMC:

  **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**
* Execute `Add-Migration InitialCreate` para realizar scaffolding de uma migração a fim de criar o conjunto inicial de tabelas para seu modelo. Se você receber um erro indicando `The term 'add-migration' is not recognized as the name of a cmdlet`, feche e reabra o Visual Studio.
* Execute `Update-Database` para aplicar a nova migração ao banco de dados. Este comando cria o banco de dados antes de aplicar as migrações.

## <a name="create-a-controller"></a>Criar um controlador

Habilite o scaffolding no projeto:

* Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador**.
* Selecione **Dependências Mínimas** e clique em **Adicionar**
* Você pode ignorar ou excluir o arquivo *ScaffoldingReadMe.txt*.

Agora que o scaffolding está habilitado, é possível realizar o scaffolding de um controlador para a entidade `Blog`.

* Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador**.
* Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Ok**.
* Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**.
* Clique em **Adicionar**.


## <a name="run-the-application"></a>Executar o aplicativo

Pressione F5 para executar e testar o aplicativo.

* Navegue até `/Blogs`
* Use o link de criação para criar algumas entradas do blog. Teste os links de detalhes e exclusão.

![imagem](_static/create.png)

![imagem](_static/index-new-db.png)

## <a name="additional-resources"></a>Recursos adicionais

* [EF – Novo banco de dados com SQLite](xref:core/get-started/netcore/new-db-sqlite) – um tutorial EF de console de plataforma cruzada.
* [Introdução ao ASP.NET Core MVC no Mac ou Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introdução ao ASP.NET Core MVC com o Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Introdução ao ASP.NET Core e ao Entity Framework Core usando o Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
