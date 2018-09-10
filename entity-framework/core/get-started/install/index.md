---
title: Instalar o Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250316"
---
# <a name="installing-entity-framework-core"></a>Instalar o Entity Framework Core

## <a name="prerequisites"></a>Pré-requisitos

* Para desenvolver aplicativos destinados ao .NET Core 2.1, instale o [SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core). O SDK precisa ser instalado, mesmo se você tiver a versão mais recente do Visual Studio 2017.

* Para usar o Visual Studio para desenvolvimento de aplicativos destinados ao .NET Core 2.1, instale o Visual Studio 2017 versão 15.7 ou posterior.

* Para usar o Entity Framework 2.1 em aplicativos do ASP.NET Core, use o ASP.NET Core 2.1. Aplicativos que usam versões anteriores do ASP.NET Core precisam ser atualizados para o 2.1.

* É possível usar o Visual Studio 2015 para aplicativos destinados ao .NET Framework 4.6.1 ou posterior. No entanto, é necessário ter uma versão do NuGet que reconheça .NET Standard 2.0 e suas estruturas compatíveis. Para fazer isso no Visual Studio 2015, [atualize o cliente do NuGet para a versão 3.6.0](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Obter o tempo de execução do Entity Framework Core

Para adicionar as bibliotecas de tempo de execução do EF Core a um aplicativo, instale o pacote do NuGet para o provedor de banco de dados que você deseja usar. Para ver uma lista de provedores compatíveis e seus nomes de pacote do NuGet, confira [Provedores de banco de dados](../../providers/index.md).

Para instalar ou atualizar pacotes do NuGet, use a CLI do .NET Core, a caixa de diálogo do gerenciador de pacotes do Visual Studio ou o console do gerenciador de pacotes do Visual Studio.

Para aplicativos do ASP.NET Core 2.1, os provedores SQL Server e na memória são incluídos automaticamente, portanto, não é necessário instalá-los separadamente.

> [!TIP]  
> Se você precisar atualizar um aplicativo que esteja usando um provedor de banco de dados de terceiros, sempre verifique se há uma atualização do provedor compatível com a versão do EF Core que você deseja usar. Por exemplo, provedores de banco de dados para versões anteriores não são compatíveis com a versão 2.1 do tempo de execução do EF Core.  

### <a name="net-core-cli"></a>CLI do .NET Core

O seguinte comando da CLI do .NET Core instala ou atualiza o provedor do SQL Server:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

É possível indicar uma versão específica no comando `dotnet add package`, usando o modificador `-v`. Por exemplo, para instalar pacotes do EF Core 2.1.0, acrescente `-v 2.1.0` ao comando.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Caixa de diálogo do gerenciador de pacotes do NuGet do Visual Studio

* No menu, selecione **Projeto > Gerenciar Pacotes do NuGet**

* Clique na guia **Procurar** ou **Atualizações**

* Para instalar ou atualizar o provedor do SQL Server, selecione o pacote `Microsoft.EntityFrameworkCore.SqlServer` e confirme.

Para saber mais, confira [Caixa de Diálogo do Gerenciador de Pacotes do NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Console do gerenciador de pacotes do NuGet do Visual Studio

* No menu, selecione **Ferramentas > Gerenciador de Pacotes do NuGet > Console do Gerenciador de Pacotes**

* Para instalar o provedor do SQL Server, execute o seguinte comando no Console do Gerenciador de Pacotes:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Para atualizar o provedor, use o comando `Update-Package`.

* Para especificar uma versão, use o modificador `-Version`. Por exemplo, para instalar pacotes do EF Core 2.1.0, acrescente `-Version 2.1.0` aos comandos

Para saber mais, confira [Console do gerenciador de pacotes](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Obter ferramentas do Entity Framework Core

Além das bibliotecas de tempo de execução, é possível instalar as ferramentas que podem executar algumas tarefas relacionadas ao EF Core no projeto em tempo de design. Por exemplo, é possível criar migrações, aplicá-las e criar um modelo com base em um banco de dados existente.

Dois conjuntos de ferramentas estão disponíveis:
* As [ferramentas da CLI (interface de linha de comando)](../../miscellaneous/cli/dotnet.md) do .NET Core podem ser usadas no Windows, Linux ou macOS. Os comandos começam com `dotnet ef`. 
* As [ferramentas do Console do Gerenciador de Pacotes](../../miscellaneous/cli/powershell.md) são executadas no Visual Studio 2017 no Windows. Esses comandos começam com um verbo, por exemplo `Add-Migration`, `Update-Database`.

Embora você possa usar os comandos `dotnet ef` no Console do Gerenciador de Pacotes, é mais conveniente usar as ferramentas dele quando estiver usando o Visual Studio:
* Elas funcionam automaticamente com o projeto atual selecionado no Console do Gerenciador de Pacotes sem necessidade de trocar manualmente os diretórios.  
* Eles abrem automaticamente os arquivos gerados pelos comandos no Visual Studio depois que o comando for concluído.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>Obter as ferramentas da CLI

Os comandos `dotnet ef` estão incluídos no SDK do .NET Core, mas para habilitá-los é necessário instalar o pacote `Microsoft.EntityFrameworkCore.Design`:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Nos aplicativos do ASP.NET Core 2.1, esse pacote é incluído automaticamente.

Conforme explicado anteriormente em [Pré-requisitos](#prerequisites), também será necessário instalar o SDK do .NET Core 2.1.

> [!IMPORTANT]      
> Sempre use a versão do pacote de ferramentas que corresponda à versão principal dos pacotes em tempo de execução.

### <a name="get-the-package-manager-console-tools"></a>Obter as ferramentas do Console do Gerenciador de Pacotes

Para obter as ferramentas do Console do Gerenciador de Pacotes para EF Core, instale o pacote `Microsoft.EntityFrameworkCore.Tools`:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

Nos aplicativos do ASP.NET Core 2.1, esse pacote é incluído automaticamente.

## <a name="upgrading-to-ef-core-21"></a>Atualizar para o EF Core 2.1

Se você estiver atualizando um aplicativo existente para o EF Core 2.1, talvez seja necessário remover manualmente algumas referências aos pacotes mais antigos do EF Core:

* Pacotes de tempo de design do provedor do banco de dados, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, não são mais necessários nem compatíveis com o EF Core 2.1, mas não são automaticamente removidos ao atualizar os outros pacotes.

* As ferramentas da CLI do .NET agora estão incluídas no SDK do .NET, portanto, a referência a esse pacote pode ser removida do arquivo *.csproj*:

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

Quanto aos aplicativos destinados ao .NET Framework e criados em versões mais antigas do Visual Studio, é necessário verificar se são compatíveis com as bibliotecas do .NET Standard 2.0:

  * Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Para projetos de teste, verifique também se a seguinte entrada está presente:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
