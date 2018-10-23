---
title: Introdução ao UWP – Novo banco de dados – EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315614"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introdução ao EF Core na UWP (Plataforma Universal do Windows) com um novo banco de dados

Neste tutorial, você criará um aplicativo UWP (Plataforma Universal do Windows) que executa acesso a dados básicos em um banco de dados local do SQLite usando o Entity Framework Core.

[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Pré-requisitos

* [Windows 10 Fall Creators Update (10.0; build 16299) ou posterior](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 versão 15.7 ou posterior](https://www.visualstudio.com/downloads/) com a carga de trabalho de **Desenvolvimento da Plataforma Universal do Windows**.

* [SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/core).

> [!IMPORTANT]
> Este tutorial usa os comandos de [migrações](xref:core/managing-schemas/migrations/index) do Entity Framework Core para criar e atualizar o esquema do banco de dados.
> Esses comandos não funcionam diretamente com projetos UWP.
> Por esse motivo, o modelo de dados do aplicativo é colocado em um projeto de biblioteca compartilhada, e um aplicativo de console do .NET Core é usado para executar os comandos.

## <a name="create-a-library-project-to-hold-the-data-model"></a>Criar um projeto de biblioteca para armazenar o modelo de dados

* Abrir o Visual Studio

* **Arquivo > Novo > Projeto**

* No menu à esquerda, selecione **Instalado > Visual C# > .NET Standard**.

* Selecione o modelo **Biblioteca de Classes (.NET Standard)**.

* Nomeie o projeto *Blogging.Model*.

* Nomeie a solução *Blogging*.

* Clique em **OK**.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>Instalar o tempo de execução do Entity Framework Core no projeto de modelo de dados

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este tutorial usa o SQLite. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.

* Certifique-se de que o projeto de biblioteca *Blogging.Model* foi selecionado como o **Projeto padrão** no console do Gerenciador de pacotes.

* Execute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

## <a name="create-the-data-model"></a>Criar o modelo de dados

Agora é hora de definir o *DbContext* e as classes de entidade que compõem o modelo.

* Exclua *Class1.cs*.

* Crie *Models.cs* com o seguinte código:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>Criar um novo projeto de console para executar os comandos de migrações

* No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução e escolha **Adicionar > Novo Projeto**.

* No menu à esquerda, selecione **Instalado > Visual C# > .NET Core**.

* Selecione o modelo de projeto **Aplicativo de console (.NET Core)**.

* Nomeie o projeto *Blogging.Migrations.Startup* e clique em **OK**.

* Adicione uma referência de projeto do projeto *Blogging.Migrations.Startup* ao projeto *Blogging.Model*.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>Instalar as ferramentas do Entity Framework Core no projeto de inicialização de migrações

Para habilitar os comandos de migração do EF Core no Console do Gerenciador de pacotes, instale o pacote de ferramentas do EF Core no aplicativo de console.

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`

## <a name="create-the-initial-migration"></a>Criar a migração inicial

 Crie a migração inicial, especificando o aplicativo de console como o projeto de inicialização.

* Execute `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`

Este comando realiza scaffolding de uma migração que cria o conjunto inicial de tabelas de banco de dados para seu modelo de dados.

## <a name="create-the-uwp-project"></a>Criar o projeto UWP

* No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução e escolha **Adicionar > Novo Projeto**.

* No menu à esquerda, selecione **Instalado > Visual C# > Universal do Windows**.

* Selecione o modelo de projeto **Aplicativo em Branco (Universal do Windows)**.

* Nomeie o projeto *Blogging.UWP* e clique em **OK**

> [!IMPORTANT]
> Defina as versões de destino e mínima pelo menos para **Windows 10 Fall Creators Update (10.0; build 16299.0)**.
> As versões anteriores do Windows 10 não são compatíveis com o .NET Standard 2.0, exigido pelo Entity Framework Core.

## <a name="add-code-to-create-the-database-on-application-startup"></a>Adicione código para criar o banco de dados na inicialização do aplicativo

Como você quer que o banco de dados seja criado no dispositivo no qual o aplicativo é executado, adicione código para aplicar quaisquer migrações pendentes ao banco de dados local na inicialização do aplicativo. Na primeira execução do aplicativo, isso será responsável pela criação do banco de dados local.

* Adicione uma referência de projeto do projeto *Blogging.UWP* ao projeto *Blogging.Model*.

* Abra *App.xaml.cs*.

* Adicione o código realçado para aplicar quaisquer migrações pendentes.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Se você alterar o modelo, use o comando `Add-Migration` para realizar o scaffolding de uma nova migração e aplicar as alterações correspondentes no banco de dados. Quaisquer migrações pendentes serão aplicadas ao banco de dados local em cada dispositivo quando o aplicativo for iniciado.
>
>O EF Core usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.

## <a name="use-the-data-model"></a>Usar o modelo de dados

Agora é possível usar o EF Core para executar o acesso a dados.

* Abra *MainPage.xaml*.

* Adicione o manipulador de carregamento de página e o conteúdo da interface do usuário realçado abaixo

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Agora, adicione o código para conectar a interface do usuário com o banco de dados

* Abra *MainPage.xaml.cs*.

* Adicione o código realçado da seguinte lista:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

Agora, você pode executar o aplicativo para vê-lo em ação.

* No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto *Blogging.UWP* e selecione **Implantar**.

* Defina *Blogging.UWP* como projeto de inicialização.

* **Depurar > Iniciar sem depuração**

  O aplicativo é compilado e executado.

* Insira uma URL e clique no botão **Adicionar**

  ![imagem](_static/create.png)

  ![imagem](_static/list.png)

  Pronto! Agora você tem um aplicativo UWP simples executando o Entity Framework Core.

## <a name="next-steps"></a>Próximas etapas

Para obter informações de compatibilidade e desempenho que você deve saber ao usar o EF Core com UWP, veja [Implementações .NET compatíveis com o EF Core](../../platforms/index.md#universal-windows-platform).

Confira outros artigos nesta documentação para saber mais sobre os recursos do Entity Framework Core.
