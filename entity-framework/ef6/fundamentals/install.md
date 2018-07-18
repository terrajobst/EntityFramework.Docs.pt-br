---
title: Obter o Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
caps.latest.revision: 4
ms.openlocfilehash: 400bf1428e6754a88dbc1264c346bb66282725a0
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120091"
---
# <a name="get-entity-framework"></a>Obter o Entity Framework
Entity Framework é composto das ferramentas do EF para Visual Studio e o tempo de execução do EF.

## <a name="ef-tools-for-visual-studio"></a>Ferramentas do EF para Visual Studio

O Entity Framework Tools para Visual Studio incluem o EF Designer e o Assistente de modelo do EF e são necessário para o banco de dados pela primeira vez e modelar fluxos de trabalho primeiro. Ferramentas do EF estão incluídas em todas as versões recentes do Visual Studio. Se você executar uma instalação personalizada do Visual Studio, você precisará garantir que o item "Ferramentas do Entity Framework 6" é selecionada escolhendo uma carga de trabalho que inclui-lo ou selecionando-o como um componente individual.

Para algumas versões anteriores do Visual Studio, ferramentas atualizadas do EF estão disponíveis para download. Ver [versões do Visual Studio](~/ef6/what-is-new/visual-studio.md) para obter orientação sobre como obter a versão mais recente das ferramentas do EF disponíveis para sua versão do Visual Studio.

## <a name="ef-runtime"></a>Tempo de execução do EF

A versão mais recente do Entity Framework está disponível como a [pacote EntityFramework NuGet](http://nuget.org/packages/EntityFramework/). Se você não estiver familiarizado com o Gerenciador de pacotes do NuGet, recomendamos que você leia as [visão geral do NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Instalando o pacote do NuGet do EF

Você pode instalar o pacote EntityFramework clicando com o **referências** pasta do seu projeto e selecionando **gerenciar pacotes NuGet...**

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Instalação do Console do Gerenciador de pacotes

Como alternativa, você pode instalar o EntityFramework, executando o seguinte comando na [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Instalar uma versão específica do EF

Do EF 4.1 em diante, as novas versões do tempo de execução do EF foi lançadas como o [pacote do NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/). Qualquer uma dessas versões podem ser adicionados a um projeto com base no .NET Framework, executando o seguinte comando no Visual Studio [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Observe que `<number>` representa a versão específica do EF para instalar. Por exemplo, 6.2.0 é a versão de número para o EF 6.2.   

Tempos de execução do EF antes 4.1 faziam parte do .NET Framework e não podem ser instalados separadamente.

### <a name="installing-the-latest-preview"></a>Instalar a visualização mais recente

Os métodos acima lhe dará a versão mais recente com suporte total a versão do Entity Framework. Geralmente há versões de pré-lançamento do Entity Framework disponível que Adoraríamos que você experimente e nos fornecer comentários sobre.

Para instalar a visualização mais recente do EntityFramework, você pode selecionar **Include Prerelease** na janela Gerenciar pacotes NuGet. Se não há versões de pré-lançamento estão disponível automaticamente obterá a versão mais recente versão com suporte total do Entity Framework.

![IncludePreRelease](~/ef6/media/includeprerelease.png)

Como alternativa, você pode executar o comando a seguir [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
