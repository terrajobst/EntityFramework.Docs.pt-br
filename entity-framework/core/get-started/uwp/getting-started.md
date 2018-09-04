---
title: Introdução ao UWP – Novo banco de dados – EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996904"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introdução ao EF Core na UWP (Plataforma Universal do Windows) com um novo banco de dados

Neste tutorial, você criará um aplicativo UWP (Plataforma Universal do Windows) que executa acesso a dados básicos em um banco de dados local do SQLite usando o Entity Framework Core.

[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Pré-requisitos

* [Windows 10 Fall Creators Update (10.0; build 16299) ou posterior](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 versão 15.7 ou posterior](https://www.visualstudio.com/downloads/) com a carga de trabalho de **Desenvolvimento da Plataforma Universal do Windows**.

* [SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/core).

## <a name="create-a-model-project"></a>Criar um projeto de modelo

> [!IMPORTANT]
> Devido às limitações nas formas como as ferramentas do .NET Core interagem com os projetos da UWP, o modelo precisa ser colocado em um projeto não UWP para poder executar comandos de migrações no PMC **(Console do Gerenciador de Pacotes)**

* Abrir o Visual Studio

* **Arquivo > Novo > Projeto**

* No menu à esquerda, selecione **Instalado > Visual C# > .NET Standard**.

* Selecione o modelo **Biblioteca de Classes (.NET Standard)**.

* Nomeie o projeto *Blogging.Model*.

* Nomeie a solução *Blogging*.

* Clique em **OK**.

## <a name="install-entity-framework-core"></a>Instalar o Entity Framework Core

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este tutorial usa o SQLite. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.

* Execute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

Mais adiante neste tutorial, você usará algumas ferramentas do Entity Framework Core para manter o banco de dados. Portanto, instale também o pacote de ferramentas.

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Criar o modelo

Agora é hora de definir um contexto e classes de entidade que compõem o modelo.

* Exclua *Class1.cs*.

* Crie *Models.cs* com o seguinte código:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Criar um novo projeto da UWP

* No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução e escolha **Adicionar > Novo Projeto**.

* No menu à esquerda, selecione **Instalado > Visual C# > Universal do Windows**.

* Selecione o modelo de projeto **Aplicativo em Branco (Universal do Windows)**.

* Nomeie o projeto *Blogging.UWP* e clique em **OK**

* Defina as versões de destino e mínima pelo menos para **Windows 10 Fall Creators Update (10.0; build 16299.0)**.

## <a name="create-the-initial-migration"></a>Criar a migração inicial

Agora que você tem um modelo, configure o aplicativo para criar um banco de dados na primeira vez em que ele for executado. Nesta seção, você criará a migração inicial. Na seção a seguir, você deve adicionar o código que se aplica a essa migração quando o aplicativo é iniciado.

Ferramentas de migrações exigem um projeto de inicialização não UWP, portanto, crie-o primeiro.

* No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução e escolha **Adicionar > Novo Projeto**.

* No menu à esquerda, selecione **Instalado > Visual C# > .NET Core**.

* Selecione o modelo de projeto **Aplicativo de console (.NET Core)**.

* Nomeie o projeto *Blogging.Migrations.Startup* e clique em **OK**.

* Adicione uma referência de projeto do projeto *Blogging.Migrations.Startup* ao projeto *Blogging.Model*.

Agora você pode criar sua migração inicial.

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**

* Selecione o projeto *Blogging.Model* como o **Projeto padrão**.

* No **Gerenciador de Soluções**, defina o projeto *Blogging.Migrations.Startup* como o projeto de inicialização.

* Execute `Add-Migration InitialCreate`.

  Este comando realiza scaffolding de uma migração que cria o conjunto inicial de tabelas para seu modelo.

## <a name="create-the-database-on-app-startup"></a>Criar o banco de dados na inicialização do aplicativo

Como você quer que o banco de dados seja criado no dispositivo no qual o aplicativo é executado, adicione código para aplicar quaisquer migrações pendentes ao banco de dados local na inicialização do aplicativo. Na primeira execução do aplicativo, isso será responsável pela criação do banco de dados local.

* Adicione uma referência de projeto do projeto *Blogging.UWP* ao projeto *Blogging.Model*.

* Abra *App.xaml.cs*.

* Adicione o código realçado para aplicar quaisquer migrações pendentes.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Se você alterar o modelo, use o comando `Add-Migration` para realizar o scaffolding de uma nova migração e aplicar as alterações correspondentes no banco de dados. Quaisquer migrações pendentes serão aplicadas ao banco de dados local em cada dispositivo quando o aplicativo for iniciado.
>
>O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.

## <a name="use-the-model"></a>Usar o modelo

Agora é possível usar o modelo para executar o acesso a dados.

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
