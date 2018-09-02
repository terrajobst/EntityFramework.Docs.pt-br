---
title: Introdução ao .NET Framework – Banco de dados existente – EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: edcdc0b76394c4d604cf43fc170424e474532b17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993412"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Introdução ao EF Core no .NET Framework com um banco de dados existente

Neste tutorial, você compila um aplicativo de console que realiza o acesso básico a dados em um banco de dados do Microsoft SQL Server usando o Entity Framework. Você criará um modelo do Entity Framework realizando a engenharia reversa de um banco de dados existente.

[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017 versão 15.7 ou posterior](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Criar banco de dados de blog

Este tutorial usa um banco de dados de **blog** em sua instância do LocalDb como o banco de dados existente. Se você já tiver criado o banco de dados de **blog** como parte de outro tutorial, ignore estas etapas.

* Abrir o Visual Studio

* **Ferramentas > Conectar ao Banco de Dados...**

* Selecione **Microsoft SQL Server** e clique em **Continuar**

* Insira **(localdb)\mssqllocaldb** como o **Nome do Servidor**

* Insira **mestre** como o **Nome do Banco de Dados** e clique em **OK**

* O banco de dados mestre agora é exibido em **Conexões de Dados** no **Gerenciador de Servidores**

* Clique com botão direito do mouse no banco de dados em **Gerenciador de Servidores** e selecione **Nova Consulta**

* Copie o script listado abaixo para o editor de consultas

* Clique com o botão direito do mouse no editor de consultas e selecione **Executar**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Criar um novo projeto

* Abra o Visual Studio 2017

* **Arquivo > Novo > Projeto...**

* No menu à esquerda, selecione **Instalado > Visual C# > Área de Trabalho do Windows**

* Selecione o modelo de projeto **Aplicativo de console (.NET Framework)**

* Verifique se o projeto direciona o **.NET Framework 4.6.1** ou posterior

* Nomeie o projeto *ConsoleApp.ExistingDb* e clique em **OK**

## <a name="install-entity-framework"></a>Instalar o Entity Framework

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este tutorial usa o SQL Server. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**

* Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Na próxima etapa, você usará o Entity Framework Tools para realizar a engenharia reversa do banco de dados. Portanto, instale também o pacote de ferramentas.

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-the-model"></a>Realizar engenharia reversa do modelo

Agora é hora de criar o modelo EF com base em um banco de dados existente.

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**

* Execute o seguinte comando para criar um modelo do banco de dados existente

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> É possível especificar as tabelas para as quais gerar entidades adicionado o argumento `-Tables` ao comando acima. Por exemplo, `-Tables Blog,Post`.

O processo de engenharia reversa criou classes de entidade (`Blog` e `Post`) e um contexto derivado (`BloggingContext`) com base no esquema do banco de dados existente.

As classes de entidade são objetos C# simples que representam os dados que você vai consultar e salvar. Veja as classes de entidade `Blog` e `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Para habilitar o carregamento lento, crie propriedades de navegação `virtual` (Blog.Post e Post.Blog).

O contexto representa uma sessão com o banco de dados. Ele tem métodos que você pode usar para consultar e salvar instâncias das classes de entidade.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Usar o modelo

Agora é possível usar o modelo para executar o acesso a dados.

* Abra *Program.cs*

* Substitua o conteúdo do arquivo pelo seguinte código

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Depurar > Iniciar Sem Depuração

  Repare que um blog é salvo no banco de dados, e os detalhes de todos os blogs são impressos no console.

  ![imagem](_static/output-existing-db.png)

## <a name="additional-resources"></a>Recursos adicionais

* [EF Core no .NET Framework com um novo banco de dados](xref:core/get-started/full-dotnet/new-db)
* [EF Core no .NET Core com um novo banco de dados, SQLite](xref:core/get-started/netcore/new-db-sqlite), um tutorial do EF de console de plataforma cruzada.
