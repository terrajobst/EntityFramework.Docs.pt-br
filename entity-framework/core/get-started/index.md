---
title: Introdução – EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412861"
---
# <a name="getting-started-with-ef-core"></a>Introdução ao EF Core

Neste tutorial, você criará um aplicativo de console do .NET Core que executa acesso a dados em um banco de dados SQLite usando o Entity Framework Core.

Você pode seguir o tutorial usando o Visual Studio no Windows ou usando a CLI do .NET Core no Windows, macOS ou Linux.

[Exiba o exemplo deste artigo no GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).

## <a name="prerequisites"></a>Pré-requisitos

Instale o software a seguir:

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* [SDK do .Net Core](https://www.microsoft.com/net/download/core).

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019 versão 16.3 ou posterior](https://www.visualstudio.com/downloads/) com esta carga de trabalho:
  * **Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)

---

## <a name="create-a-new-project"></a>Criar um novo projeto

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Abrir o Visual Studio
* Clique em **Criar um novo projeto**
* Selecione **Aplicativo de Console (.NET Core)** com a marca **C#** e clique em **Avançar**
* Insira **EFGetStarted** como o nome e clique em **Criar**

---

## <a name="install-entity-framework-core"></a>Instalar o Entity Framework Core

Para instalar o EF Core, instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar. Este tutorial usa SQLite porque ele é executado em todas as plataformas que dão suporte a .NET Core. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../providers/index.md).

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**
* Execute os seguintes comandos:

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

Dica: você também pode instalar pacotes clicando com o botão direito do mouse no projeto e selecionando **Gerenciar Pacotes NuGet**

---

## <a name="create-the-model"></a>Criar o modelo

Defina uma classe de contexto e classes de entidade que compõem o modelo.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* No diretório do projeto, crie **Model.cs** com o seguinte código

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Clique com o botão direito do mouse no projeto e selecione **Adicionar > Classe**
* Insira **Model.cs** como o nome e clique em **Adicionar**
* Substitua o conteúdo do arquivo pelo seguinte código

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

O EF Core também pode fazer a [engenharia reversa](../managing-schemas/scaffolding.md) de um modelo de um banco de dados existente.

Dica: em um aplicativo real, você coloca cada classe em um arquivo separado e coloca a [cadeia de conexão](../miscellaneous/connection-strings.md) em um arquivo de configuração ou na variável de ambiente. Para simplificar o tutorial, tudo está contido em um arquivo.

## <a name="create-the-database"></a>Criar o banco de dados

As etapas a seguir usam [migrações](xref:core/managing-schemas/migrations/index) para criar um banco de dados.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

* Execute os seguintes comandos:

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Isso instala o [dotnet ef](../miscellaneous/cli/dotnet.md) e o pacote de design necessário para executar o comando em um projeto. O comando `migrations` realiza o scaffolding de uma migração e cria o conjunto inicial de tabelas para o modelo. O comando `database update` cria o banco de dados e aplica a nova migração a ele.

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Execute os seguintes comandos no **Console do Gerenciador de Pacotes**

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  Isso instala as [ferramentas do PMC para EF Core](../miscellaneous/cli/powershell.md). O comando `Add-Migration` realiza o scaffolding de uma migração e cria o conjunto inicial de tabelas para o modelo. O comando `Update-Database` cria o banco de dados e aplica a nova migração a ele.

---

## <a name="create-read-update--delete"></a>Criar, ler, atualizar e excluir

* Abra *Program.cs* e substitua o conteúdo pelo código a seguir:

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a>Executar o aplicativo

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

O Visual Studio usa um diretório de trabalho divergente ao executar aplicativos de console do .NET Core. (confira [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) Isso faz com que uma exceção seja gerada: *não existe essa tabela: Blogs*. Para atualizar o diretório de trabalho:

* clique com o botão direito do mouse no projeto e selecione **Editar Arquivo de Projeto**
* Logo abaixo da propriedade *TargetFramework*, adicione o seguinte:

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* Salve o arquivo

Agora, você pode executar o aplicativo:

* **Depurar > Iniciar sem depuração**

---

## <a name="next-steps"></a>Próximas etapas

* Siga o [Tutorial do ASP.NET Core](/aspnet/core/data/ef-rp/intro) para usar o EF Core em um aplicativo Web
* Saiba mais sobre as [expressões de consulta LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
* [Configure seu modelo](xref:core/modeling/index) para especificar configurações como [obrigatório](xref:core/modeling/entity-properties#required-and-optional-properties) e [comprimento máximo](xref:core/modeling/entity-properties#maximum-length)
* Use [Migrações](xref:core/managing-schemas/migrations/index) para atualizar o esquema do banco de dados após alterar seu modelo
