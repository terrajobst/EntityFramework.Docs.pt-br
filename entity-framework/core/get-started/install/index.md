---
title: Instalar o Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: db1b83a9222e00a5e226a134085b18247b31f29a
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306483"
---
# <a name="installing-entity-framework-core"></a>Instalar o Entity Framework Core

## <a name="prerequisites"></a>Pré-requisitos

* O EF Core é uma biblioteca do [.NET Standard 2.0](/dotnet/standard/net-standard). Dessa maneira, para o EF Core ser executado, é necessária uma implementação do .NET compatível com o .NET Standard 2.0. O EF Core também pode ser referenciado por outras bibliotecas do .NET Standard 2.0. 

* Por exemplo, é possível usar o EF Core para desenvolver aplicativos para o .NET Core. Para criar aplicativos do .NET Core, é necessária a [SDK do .NET Core](https://dotnet.microsoft.com/download). Como opção, também é possível usar um ambiente de desenvolvimento como o Visual Studio, o Visual Studio para Mac ou o Visual Studio Code. Para saber mais, confira a [Introdução ao .NET Core](/dotnet/core/get-started).

* É possível usar o EF Core para desenvolver aplicativos para o .NET Framework 4.6.1 ou posterior no Windows usando o Visual Studio. É recomendável usar a versão mais recente do Visual Studio. Se você quiser usar uma versão mais antiga, como o Visual Studio 2015, [atualize o cliente NuGet para a versão 3.6.0](https://www.nuget.org/downloads). Assim, a versão anterior funcionará com as bibliotecas do .NET Standard 2.0.

* O EF Core pode ser executado em outras implementações do .NET, como Xamarin e .NET Native. No entanto, na prática, essas implementações têm limitações de tempo de execução que podem afetar o funcionamento do EF Core no aplicativo. Para saber mais, confira [Implementações do .NET compatíveis com o EF Core](xref:core/platforms/index).

* Por fim, provedores de banco de dados diferentes podem exigir versões específicas de mecanismo de banco de dados, implementações do .NET ou sistemas operacionais. Verifique se há um [provedor de banco de dados do EF Core](xref:core/providers/index) disponível que seja compatível com o ambiente correto para o aplicativo.

## <a name="get-the-entity-framework-core-runtime"></a>Obter o tempo de execução do Entity Framework Core

Para adicionar o EF Core a um aplicativo, instale o pacote do NuGet do provedor de banco de dados que você quer usar.

Se você estiver criando um aplicativo do ASP.NET Core, não precisará instalar os provedores in-memory e do SQL Server. Esses provedores estão incluídos nas versões atuais do ASP.NET Core, juntamente com o tempo de execução do EF Core.  

Para instalar ou atualizar os pacotes NuGet, use a CLI (interface de linha de comando) do .NET Core, a Caixa de Diálogo do Gerenciador de Pacotes do Visual Studio ou o Console do Gerenciador de Pacotes do Visual Studio.

### <a name="net-core-cli"></a>CLI do .NET Core

* Use o seguinte comando da CLI do .NET Core na linha de comando do sistema operacional para instalar ou atualizar o provedor de SQL Server do EF Core:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* É possível indicar uma versão específica no comando `dotnet add package`, usando o modificador `-v`. Por exemplo, para instalar pacotes do EF Core 2.2.0, acrescente `-v 2.2.0` ao comando.

Para saber mais, confira [Ferramentas da CLI (interface de linha de comando) do .NET](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Caixa de diálogo do gerenciador de pacotes do NuGet do Visual Studio

* No menu do Visual Studio, selecione **Projeto > Gerenciar Pacotes do NuGet**

* Clique na guia **Procurar** ou **Atualizações**

* Para instalar ou atualizar o provedor do SQL Server, selecione o pacote `Microsoft.EntityFrameworkCore.SqlServer` e confirme.

Para saber mais, confira [Caixa de Diálogo do Gerenciador de Pacotes do NuGet](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Console do gerenciador de pacotes do NuGet do Visual Studio

* No menu do Visual Studio, selecione **Ferramentas > Gerenciador de Pacotes do NuGet > Console do Gerenciador de Pacotes**

* Para instalar o provedor do SQL Server, execute o seguinte comando no Console do Gerenciador de Pacotes:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Para atualizar o provedor, use o comando `Update-Package`.

* Para especificar uma versão, use o modificador `-Version`. Por exemplo, para instalar pacotes do EF Core 2.2.0, acrescente `-Version 2.2.0` aos comandos

Para saber mais, confira [Console do gerenciador de pacotes](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Obtenha as ferramentas do Entity Framework Core

É possível instalar ferramentas para realizar tarefas relacionadas com o EF Core no projeto, como criar e aplicar migrações de banco de dados ou criar um modelo do EF Core baseado em um banco de dados existente.

Dois conjuntos de ferramentas estão disponíveis:

* As [ferramentas da CLI (interface de linha de comando) do .NET Core](xref:core/miscellaneous/cli/dotnet) podem ser usadas no Windows, no Linux ou no macOS. Os comandos começam com `dotnet ef`. 

* As [ferramentas do PMC (Console do Gerenciador de Pacotes)](xref:core/miscellaneous/cli/powershell) são executadas no Visual Studio no Windows. Esses comandos começam com um verbo, por exemplo `Add-Migration`, `Update-Database`.

Embora você possa usar os comandos `dotnet ef` no Console do Gerenciador de Pacotes, é recomendável usar as ferramentas dele quando estiver usando o Visual Studio:

* Elas funcionam automaticamente com o projeto atual selecionado no PMC no Visual Studio, sem necessidade de trocar manualmente os diretórios.  

* Eles abrem automaticamente os arquivos gerados pelos comandos no Visual Studio depois que o comando for concluído.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Obtenha as ferramentas da CLI do .NET Core

As ferramentas da CLI do .NET Core precisam do SDK do .NET Core mencionado anteriormente nos [Pré-requisitos](#prerequisites).

Os comandos `dotnet ef` estão incluídos nas versões atuais do SDK do .NET Core, mas para habilitá-los em um projeto específico, é necessário instalar o pacote `Microsoft.EntityFrameworkCore.Design`:

``` Console 
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Nos aplicativos do ASP.NET Core, esse pacote está incluído automaticamente.

> [!IMPORTANT]      
> Sempre use a versão do pacote de ferramentas que corresponda à versão principal dos pacotes em tempo de execução.

### <a name="get-the-package-manager-console-tools"></a>Obter as ferramentas do Console do Gerenciador de Pacotes

Para obter as ferramentas do Console do Gerenciador de Pacotes para EF Core, instale o pacote `Microsoft.EntityFrameworkCore.Tools`. Por exemplo, no Visual Studio:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

Nos aplicativos do ASP.NET Core, esse pacote está incluído automaticamente.

## <a name="upgrading-to-the-latest-ef-core"></a>Atualizar para o EF Core mais recente

* Sempre que lançamos uma nova versão do EF Core, também lançamos uma nova versão dos provedores que fazem parte do projeto do EF Core, como Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.InMemory. Basta atualizar para a nova versão do provedor para obter todas as melhorias. 

* O EF Core, junto com o SQL Server e os provedores in-memory, estão incluídos nas versões atuais do ASP.NET Core. Para atualizar um aplicativo ASP.NET Core existente para uma nova versão do EF Core, sempre atualize a versão do ASP.NET Core.

* Se você precisar atualizar um aplicativo que esteja usando um provedor de banco de dados de terceiros, sempre verifique se há uma atualização do provedor compatível com a versão do EF Core que você deseja usar. Por exemplo, provedores de banco de dados para versões anteriores não são compatíveis com a versão 2.0 do tempo de execução do EF Core.

* Normalmente, os provedores de terceiros do EF Core não lançam versões de patch junto com o tempo de execução do EF Core. Para atualizar um aplicativo que usa um provedor de terceiros para uma versão de patch do EF Core, talvez seja necessário adicionar uma referência direta aos componentes de tempo de execução do EF Core, como Microsoft.EntityFrameworkCore e Microsoft.EntityFrameworkCore.Relational.

* Se você estiver atualizando um aplicativo existente para a versão mais recente do EF Core, talvez seja necessário remover manualmente algumas referências aos pacotes mais antigos do EF Core:

  * Os pacotes de tempo de design do provedor do banco de dados, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, não são mais necessários nem compatíveis com o EF Core 2.0, mas não são automaticamente removidos ao atualizar os outros pacotes.

  * As ferramentas da CLI do .NET estão incluídas no SDK do .NET desde a versão 2.1, portanto, a referência a esse pacote pode ser removida do arquivo de projeto:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* Os aplicativos para .NET Framework podem precisar de alterações para funcionar com as bibliotecas do .NET Standard 2.0:

  * Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Para projetos de teste, verifique também se a seguinte entrada está presente:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
