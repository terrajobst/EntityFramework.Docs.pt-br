---
title: Introdução ao .NET Framework – Novo banco de dados – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614422"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Introdução ao EF Core no .NET Framework com um novo banco de dados

Neste tutorial, você compila um aplicativo de console que realiza o acesso básico a dados em um banco de dados do Microsoft SQL Server usando o Entity Framework. Você usará migrações para criar o banco de dados com base em um modelo.

[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017 versão 15.7 ou posterior](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Criar um novo projeto

* Abra o Visual Studio 2017

* **Arquivo > Novo > Projeto...**

* No menu à esquerda, selecione **Instalado > Visual C# > Área de Trabalho do Windows**

* Selecione o modelo de projeto **Aplicativo de console (.NET Framework)**

* Verifique se o projeto direciona o **.NET Framework 4.6.1** ou posterior

* Nomeie o projeto *ConsoleApp.NewDb* e clique em **OK**

## <a name="install-entity-framework"></a>Instalar o Entity Framework

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este tutorial usa o SQL Server. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes

* Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Mais adiante neste tutorial, você usará o Entity Framework Tools para manter o banco de dados. Portanto, instale também o pacote de ferramentas.

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Criar o modelo

Agora é hora de definir um contexto e classes de entidade que compõem o modelo.

* **Projeto > Adicionar classe...**

* Insira *Model.cs* como o nome e clique em **OK**

* Substitua o conteúdo do arquivo pelo seguinte código

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> Em um aplicativo real, você colocaria cada classe em um arquivo separado e colocaria a cadeia de conexão em um arquivo de configuração ou na variável de ambiente. Para simplificar, tudo está em um único arquivo de código para este tutorial.

## <a name="create-the-database"></a>Criar o banco de dados

Agora que você tem um modelo, é possível usar as migrações para criar um banco de dados.

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**

* Execute `Add-Migration InitialCreate` para gerar uma migração e criar o conjunto inicial de tabelas para o modelo.

* Execute `Update-Database` para aplicar a nova migração ao banco de dados. Como o banco de dados ainda não existe, ele será criado antes que a migração seja aplicada.

> [!TIP]  
> Se você fizer alterações no modelo, use o comando `Add-Migration` para gerar uma nova migração para fazer as alterações correspondentes de esquema no banco de dados. Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), use o comando `Update-Database` para aplicar as alterações no banco de dados.
>
> O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.

## <a name="use-the-model"></a>Usar o modelo

Agora é possível usar o modelo para executar o acesso a dados.

* Abra *Program.cs*

* Substitua o conteúdo do arquivo pelo seguinte código

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **Depurar > Iniciar sem depuração**

  Repare que um blog é salvo no banco de dados, e os detalhes de todos os blogs são impressos no console.

  ![imagem](_static/output-new-db.png)

## <a name="additional-resources"></a>Recursos adicionais

* [EF Core no .NET Framework com um banco de dados existente](xref:core/get-started/full-dotnet/existing-db)
* [EF Core no .NET Core com um novo banco de dados, SQLite](xref:core/get-started/netcore/new-db-sqlite), um tutorial do EF de console de plataforma cruzada.
