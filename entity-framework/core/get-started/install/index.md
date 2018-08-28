---
title: Instalar o EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 30ca81a0ede65506a6684d2322d31332115b1ed3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996922"
---
# <a name="installing-ef-core"></a>Instalar o EF Core

## <a name="prerequisites"></a>Pré-requisitos

Para desenvolver aplicativos .NET Core 2.1 (incluindo aplicativos ASP.NET Core 2.1 que têm como destino o .NET Core), será necessário baixar e instalar uma versão do [SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core) apropriado para a sua plataforma. **Isso vale mesmo que você tenha instalado o Visual Studio 2017 versão 15.7.**

Para usar o EF Core 2.1 ou qualquer outra biblioteca .NET Standard 2.0 padrão com uma plataforma .NET além do .NET Core 2.1 (por exemplo, com o .NET Framework 4.6.1 ou superior), você precisará de uma versão do NuGet que reconheça o padrão .NET 2.0 e suas estruturas compatíveis. Aqui estão algumas maneiras de obter isso:

* Instalar o Visual Studio 2017 versão 15.7
* Se você estiver usando o Visual Studio 2015, [baixe e atualize o cliente do NuGet para a versão 3.6.0](https://www.nuget.org/downloads)

Projetos criados com versões anteriores do Visual Studio e que tenham .NET Framework como destino podem precisar de modificações adicionais para serem compatíveis com bibliotecas .NET 2.0 Standard:

* Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Para projetos de teste, verifique também se a seguinte entrada está presente:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Obter os bits
A maneira recomendada de adicionar bibliotecas de tempo de execução do EF Core a um aplicativo é instalar um provedor de banco de dados do EF Core do NuGet.

Além das bibliotecas de tempo de execução, você pode instalar as ferramentas que facilitam executar várias tarefas relacionadas ao EF Core em seu projeto no tempo de design, como criar e aplicar migrações e criar um modelo com base em um banco de dados existente.

> [!TIP]  
> Se você precisar atualizar um aplicativo que esteja usando um provedor de banco de dados de terceiros, sempre verifique se há uma atualização do provedor compatível com a versão do EF Core que você deseja usar. Por exemplo, provedores de banco de dados para versões anteriores não são compatíveis com a versão 2.1 do tempo de execução do EF Core.  

> [!TIP]  
> Aplicativos que têm como destino o ASP.NET Core 2.1 podem usar o EF Core 2.1 sem dependências adicionais além de provedores de banco de dados de terceiros. Aplicativos que têm como destino versões anteriores do ASP.NET Core precisam fazer upgrade para o ASP.NET Core 2.1 para usar o EF Core 2.1.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Desenvolvimento de multiplaforma usando a CLI (Interface de Linha de Comando) do .NET Core

Para desenvolver aplicativos que têm como destino o [.NET Core](https://www.microsoft.com/net/download/core), você pode optar por usar [`dotnet` comandos da CLI](https://docs.microsoft.com/dotnet/core/tools/) combinados ao seu editor de texto favorito ou um IDE (Ambiente de Desenvolvimento Integrado), como o Visual Studio, o Visual Studio para Mac ou o Visual Studio Code.

> [!IMPORTANT]  
> Aplicativos que têm como destino o .NET Core requerem versões específicas do Visual Studio. Por exemplo, o desenvolvimento do .NET Core 1.x requer o Visual Studio 2017, já o desenvolvimento do .NET Core 2.1 requer a versão 15.7 do Visual Studio 2017.

Para instalar ou fazer o upgrade para o provedor do SQL Server em um aplicativo .NET Core de multiplaforma, alterne para o diretório do aplicativo e execute o seguinte em uma linha de comando:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Você pode indicar a instalação de uma versão específica no comando `dotnet add package` usando o modificador `-v`. Por exemplo, para instalar pacotes do EF Core 2.1, acrescente `-v 2.1.0` ao comando.

O EF Core inclui um conjunto de [comandos adicionais à `dotnet` CLI](../../miscellaneous/cli/dotnet.md), começando com `dotnet ef`. As ferramentas de CLI do .NET Core para o EF Core exigem um pacote chamado `Microsoft.EntityFrameworkCore.Design`. Você pode adicioná-lo ao projeto usando:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> Sempre use a versão do pacote de ferramentas que corresponda à versão principal dos pacotes em tempo de execução.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Desenvolvimento do Visual Studio

Você pode desenvolver vários tipos diferentes de aplicativos que tenham como destino .NET Core, .NET Framework ou outras plataformas compatíveis no EF Core usando o Visual Studio.

Há duas maneiras de instalar um provedor de banco de dados do EF Core em seu aplicativo usando o Visual Studio:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Usando a [Interface do Usuário do Gerenciador de Pacotes](https://docs.microsoft.com/nuget/tools/package-manager-ui) do NuGet

* Selecione no menu **Projeto > Gerenciar Pacotes NuGet**

* Clique na guia **Procurar** ou **Atualizações**

* Selecione o pacote `Microsoft.EntityFrameworkCore.SqlServer` e a versão desejada e confirme

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Usando o [PMC (Console do Gerenciador de Pacotes)](https://docs.microsoft.com/nuget/tools/package-manager-console) do NuGet

* Selecione no menu **Ferramentas > Gerenciador de Pacotes do NuGet > Console do Gerenciador de Pacotes**

* Digite e execute o seguinte comando no PMC:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Você pode usar o comando `Update-Package` em vez de atualizar um pacote que já esteja instalado para uma versão mais recente

* Para especificar uma determinada versão, use o modificador `-Version`. Por exemplo, para instalar pacotes do EF Core 2.1, acrescente `-Version 2.1.0` aos comandos

#### <a name="tools"></a>Ferramentas

Também há uma versão do PowerShell dos comandos do [EF Core que é executada dentro do PMC](../../miscellaneous/cli/powershell.md) no Visual Studio, com recursos semelhantes aos dos comandos do `dotnet ef`. 

> [!TIP]  
> Embora seja possível usar os comandos do `dotnet ef` do PMC no Visual Studio, é muito mais conveniente usar a versão do PowerShell:
> * Eles funcionam automaticamente com o projeto atual selecionado no PMC sem necessidade de trocar manualmente os diretórios.  
> * Eles abrem automaticamente os arquivos gerados pelos comandos no Visual Studio depois que o comando for concluído.

> [!IMPORTANT]  
> **Pacotes preterido no EF Core 2.1:** se você estiver fazendo o upgrade de um aplicativo existente para o EF Core 2.1, talvez seja necessário remover manualmente algumas referências aos pacotes mais antigos do EF Core:
> * Pacotes de tempo de design do provedor do banco de dados, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, não são mais necessários nem compatíveis com o EF Core 2.1, mas não serão automaticamente removidos ao atualizar os outros pacotes.
> * As ferramentas da CLI do .NET agora estão incluídas no SDK do .NET, portanto, a referência a esse pacote pode ser removida do arquivo *.csproj*:
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
