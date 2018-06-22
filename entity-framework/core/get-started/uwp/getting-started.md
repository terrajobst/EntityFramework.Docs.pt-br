---
title: Introdução ao UWP – Novo banco de dados – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049829"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introdução ao EF Core na UWP (Plataforma Universal do Windows) com um novo banco de dados

> [!NOTE]
> Este tutorial usa o EF Core 2.0.1 (lançado junto com o ASP.NET Core e o .NET Core SDK 2.0.3). O EF Core 2.0.0 não tem algumas correções de bug essenciais necessárias para uma boa experiência da UWP.

Neste passo a passo, você compilará um aplicativo UWP (Plataforma Universal do Windows) que executa acesso a dados básicos em um banco de dados local do SQLite usando o Entity Framework.

> [!IMPORTANT]
> **Considere evitar tipos anônimos em consultas do LINQ na UWP**. A implantação de um aplicativo da UWP na loja de aplicativos exige a compilação do aplicativo com o .NET Native. Consultas com tipos anônimos tem um desempenho pior no .NET Native.

> [!TIP]
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) deste artigo no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

Os itens a seguir são necessários para concluir este passo a passo:

* [Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) versão 15.4 ou posterior com a carga de trabalho de **Desenvolvimento da Plataforma Universal do Windows**.

## <a name="create-a-new-model-project"></a>Criar um novo projeto de modelo

> [!WARNING]
> Devido às limitações nas formas como as ferramentas do .NET Core interagem com os projetos da UWP, o modelo precisa ser colocado em um projeto não UWP para poder executar comandos de migrações no Console do Gerenciador de Pacotes

* Abrir o Visual Studio

* Arquivo > Novo > Projeto...

* No menu à esquerda, selecione Modelos > Visual C#

* Selecione o modelo de projeto **Biblioteca de Classes (.NET Standard)**

* Dê um nome ao projeto e clique em **OK**

## <a name="install-entity-framework"></a>Instalar o Entity Framework

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este passo a passo usa SQLite. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes

* Execute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

Mais adiante neste passo a passo, usaremos também algumas Entity Framework Tools para manter o banco de dados. Portanto, também instalaremos o pacote de ferramentas.

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`

* Edite o arquivo .csproj e substitua `<TargetFramework>netstandard2.0</TargetFramework>` por `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`

## <a name="create-your-model"></a>Criar seu modelo

Agora é hora de definir um contexto e classes de entidade que compõem seu modelo.

* Projeto > Adicionar Classe...

* Insira *Model.cs* como o nome e clique em **OK**

* Substitua o conteúdo do arquivo pelo seguinte código

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Criar um novo projeto da UWP

* Abrir o Visual Studio

* Arquivo > Novo > Projeto...

* No menu à esquerda, selecione Modelos > Visual C# > Windows Universal

* Selecione o modelo de projeto **Aplicativo em Branco (Universal do Windows)**

* Dê um nome ao projeto e clique em **OK**

* Defina as versões de destino e mínima como, pelo menos, `Windows 10 Fall Creators Update (10.0; build 16299.0)`

## <a name="create-your-database"></a>Criar seu banco de dados

Agora que você tem um modelo, use as migrações para criar um banco de dados para você.

* Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes

* Selecione o projeto de modelo como o projeto Padrão e defina-o como o projeto de inicialização

* Execute `Add-Migration MyFirstMigration` para realizar scaffolding de uma migração a fim de criar o conjunto inicial de tabelas para seu modelo.

Como queremos que o banco de dados seja criado no dispositivo no qual o aplicativo é executado, adicionaremos alguns códigos para aplicar quaisquer migrações pendentes ao banco de dados local na inicialização do aplicativo. Na primeira execução do aplicativo, isso será responsável pela criação do banco de dados local para nós.

* Clique com o botão direito do mouse no **App.xaml** no **Gerenciador de Soluções** e selecione **Exibir Código**

* Adicione o using realçado ao início do arquivo

* Adicione o código realçado para aplicar quaisquer migrações pendentes

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Se você fizer alterações futuras em seu modelo, use o comando `Add-Migration` para realizar o scaffolding de uma nova migração e aplicar as alterações correspondentes no banco de dados. Quaisquer migrações pendentes serão aplicadas ao banco de dados local em cada dispositivo quando o aplicativo for iniciado.
>
>O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.

## <a name="use-your-model"></a>Usar o modelo

Agora você pode usar o modelo para executar o acesso aos dados.

* Abra *MainPage.xaml*

* Adicione o manipulador de carregamento de página e o conteúdo da interface do usuário realçado abaixo

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

Agora, adicionaremos o código para conectar a interface do usuário com o banco de dados

* Clique com o botão direito do mouse no **MainPage.xaml** no **Gerenciador de Soluções** e selecione **Exibir Código**

* Adicione o código realçado da seguinte lista

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

Agora, você pode executar o aplicativo para vê-lo em ação.

* Depurar > Iniciar Sem Depuração

* O aplicativo será compilado e iniciado

* Insira uma URL e clique no botão **Adicionar**

![imagem](_static/create.png)

![imagem](_static/list.png)

## <a name="next-steps"></a>Próximas etapas

> [!TIP]
> O desempenho do `SaveChanges()` pode ser melhorado com a implementação de [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) em seus tipos de entidade e usando `ChangeTrackingStrategy.ChangingAndChangedNotifications`.

Pronto! Agora você tem um aplicativo UWP simples executando o Entity Framework.

Verifique outros artigos nesta documentação para saber mais sobre os recursos do Entity Framework.
