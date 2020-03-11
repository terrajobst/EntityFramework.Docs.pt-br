---
title: Obter Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419464"
---
# <a name="get-entity-framework"></a>Como obter o Entity Framework
Entity Framework é constituído das ferramentas do EF para Visual Studio e do tempo de execução do EF.

## <a name="ef-tools-for-visual-studio"></a>Ferramentas do EF para Visual Studio

O Entity Framework Tools para o Visual Studio inclui o designer do EF e o assistente de modelo do EF e são necessários primeiro ao banco de dados e ao modelo dos primeiros fluxos de trabalho. As ferramentas do EF estão incluídas em todas as versões recentes do Visual Studio. Se você executar uma instalação personalizada do Visual Studio, precisará garantir que o item "Entity Framework 6 ferramentas" seja selecionado escolhendo uma carga de trabalho que o inclua ou selecionando-o como um componente individual.

Para algumas versões anteriores do Visual Studio, as ferramentas do EF atualizadas estão disponíveis como um download. Consulte [versões do Visual Studio](~/ef6/what-is-new/visual-studio.md) para obter orientação sobre como obter a versão mais recente das ferramentas do EF disponíveis para sua versão do Visual Studio.

## <a name="ef-runtime"></a>Tempo de execução do EF

A versão mais recente do Entity Framework está disponível como o [pacote NuGet do EntityFramework](https://nuget.org/packages/EntityFramework/). Se você não estiver familiarizado com o Gerenciador de pacotes NuGet, recomendamos que leia a [visão geral do NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Instalando o pacote NuGet do EF

Você pode instalar o pacote do EntityFramework clicando com o botão direito do mouse na pasta **referências** do seu projeto e selecionando **gerenciar pacotes NuGet...**

![Gerenciar Pacotes NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Instalando do console do Gerenciador de pacotes

Como alternativa, você pode instalar o EntityFramework executando o comando a seguir no [console do Gerenciador de pacotes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Instalando uma versão específica do EF

Do EF 4,1 em diante, novas versões do tempo de execução do EF foram lançadas como o [pacote NuGet do EntityFramework](https://www.nuget.org/packages/EntityFramework/). Qualquer uma dessas versões pode ser adicionada a um projeto baseado em .NET Framework executando o seguinte comando no console do Gerenciador de [pacotes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)do Visual Studio:

``` powershell
Install-Package EntityFramework -Version <number>
```

Observe que `<number>` representa a versão específica do EF a ser instalada. Por exemplo, 6.2.0 é a versão do número para o EF 6,2.   

Os tempos de execução do EF anteriores a 4,1 faziam parte do .NET Framework e não podem ser instalados separadamente.

### <a name="installing-the-latest-preview"></a>Instalando a versão prévia mais recente

Os métodos acima fornecerão a versão mais recente com suporte total do Entity Framework. Geralmente, há versões de pré-lançamento do Entity Framework disponíveis que adoraríamos que você experimente e nos envie comentários.

Para instalar a versão prévia mais recente do EntityFramework, você pode selecionar **incluir pré-lançamento** na janela gerenciar pacotes NuGet. Se não houver versões de pré-lançamento disponíveis, você obterá automaticamente a versão mais recente com suporte total do Entity Framework.

![Incluir pré-lançamento](~/ef6/media/includeprerelease.png)

Como alternativa, você pode executar o comando a seguir no [console do Gerenciador de pacotes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
