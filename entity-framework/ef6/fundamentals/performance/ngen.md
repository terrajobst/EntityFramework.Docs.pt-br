---
title: Melhorando o desempenho de inicialização com o NGen – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 324769ca6ee95e1576cf03d20e569f8b24778119
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998329"
---
# <a name="improving-startup-performance-with-ngen"></a>Melhorando o desempenho de inicialização com o NGen
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

O .NET Framework oferece suporte à geração de imagens nativas para aplicativos gerenciados e bibliotecas como uma maneira de ajudar a aplicativos mais veloz na inicialização e também em alguns casos usam menos memória. As imagens nativas são criadas através da conversão de assemblies de código gerenciado em arquivos que contêm instruções nativas da máquina antes do aplicativo é executado, liberando o compilador .NET JIT (Just-In-Time) de precisar gerar as instruções nativas em tempo de execução do aplicativo.  

Antes da versão 6, bibliotecas de núcleo do tempo de execução EF faziam parte do .NET Framework e as imagens nativas foram geradas automaticamente para eles. Começando com a versão 6 o tempo de execução do EF inteiro foi combinado no pacote do NuGet do EntityFramework. Imagens nativas têm Now ser gerado usando a ferramenta de linha de comando NGen.exe para obter resultados semelhantes.  

Observações empíricas mostram que as imagens nativas de assemblies de tempo de execução do EF podem cortar entre 1 e 3 segundos de tempo de inicialização do aplicativo.  

## <a name="how-to-use-ngenexe"></a>Como usar NGen.exe  

A função mais básica da ferramenta NGen.exe é "instalar" (ou seja, criar e manter no disco) imagens nativas para todas as suas dependências diretas e de um assembly. Aqui está como você pode fazer isso:  

1. Abra uma janela de Prompt de comando como administrador  
2. Altere o diretório de trabalho atual para o local dos assemblies que você deseja gerar imagens nativas para:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. Dependendo do seu sistema operacional e a configuração do aplicativo, você talvez precise gerar imagens nativas para arquitetura de 32 bits, arquitetura de 64 bits ou ambos.  

    Para 32 bits executados:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    Para 64 bits que execute:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Gerando imagens nativas para a arquitetura errada é um erro muito comum. Quando em caso de dúvida, você pode simplesmente gerar imagens nativas para todas as arquiteturas que se aplicam ao sistema operacional instalado no computador.  

NGen.exe também dá suporte a outras funções, como desinstalar e exibindo as imagens nativas instaladas, enfileiramento de mensagens a geração de várias imagens, etc. Para obter mais detalhes de uso, leia as [NGen.exe documentação](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Quando usar NGen.exe  

Quando se trata de decidir quais assemblies para gerar imagens nativas para em um aplicativo com base no EF versão 6 ou superior, você deve considerar as seguintes opções:  

- **O assembly principal do tempo de execução do EF, EntityFramework**: um aplicativo típico do EF com base em executa uma quantidade significativa de código desse assembly na inicialização ou no seu primeiro acesso ao banco de dados. Consequentemente, a criação de imagens nativas desse assembly produzirá os maiores ganhos no desempenho da inicialização.  
- **Qualquer assembly do provedor EF usado pelo seu aplicativo**: tempo de inicialização também pode beneficiar um pouco de geração de imagens nativas deles. Por exemplo, se o aplicativo usa o provedor do EF para o SQL Server você desejará gerar uma imagem nativa para EntityFramework.  
- **Assemblies do seu aplicativo e outras dependências**: O [NGen.exe documentação](https://msdn.microsoft.com/library/6t9t5wcf.aspx) aborda gerais critérios para escolher quais assemblies para gerar imagens nativas para e o impacto de imagens nativas em segurança, Opções avançadas como "associação forçada", cenários como o uso de imagens nativas na depuração e criação de perfil de cenários, etc.  

> [!TIP]
> Verifique se você medir o impacto de usar imagens nativas sobre o desempenho de inicialização e o desempenho geral do seu aplicativo com cuidado e compará-los com os requisitos reais. Embora as imagens nativas geralmente ajudará a melhorar a inicialização, o desempenho e reduzir o uso de memória em alguns casos, nem todos os cenários serão beneficiados igualmente. Por exemplo, na execução de estado estável (ou seja, depois que todos os métodos que estão sendo usados pelo aplicativo são chamados pelo menos uma vez) código gerado pelo compilador JIT pode na verdade produzir desempenho ligeiramente melhor do que as imagens nativas.  

## <a name="using-ngenexe-in-a-development-machine"></a>Usando NGen.exe em um computador de desenvolvimento  

Durante o desenvolvimento .NET JIT, o compilador oferecerá a melhor relação geral para o código que está sendo alterado com frequência. Gerando imagens nativas para dependências compiladas como os assemblies de tempo de execução do EF pode ajudar a acelerar o desenvolvimento e teste, recortar alguns segundos no início de cada execução.  

Um bom lugar para encontrar os assemblies de tempo de execução do EF é o local do pacote do NuGet para a solução. Por exemplo, para um aplicativo usando o EF 6.0.2 com o SQL Server e destinados ao .NET 4.5 ou mais você pode digitar o seguinte em uma janela de Prompt de comando (Lembre-se de abri-lo como um administrador):  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Isso tira proveito do fato de que a instalação de imagens nativas para o provedor do EF para SQL Server também por padrão instala as imagens nativas para o assembly principal de tempo de execução do EF. Isso funciona porque NGen.exe pode detectar que o EntityFramework é uma dependência direta do assembly EntityFramework localizado no mesmo diretório.  

## <a name="creating-native-images-during-setup"></a>Criando imagens nativas durante a instalação  

O WiX Toolkit oferece suporte ao enfileiramento a geração de imagens nativas para assemblies gerenciados durante a instalação, conforme explicado neste [guia de instruções](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Outra alternativa é criar uma tarefa de configuração personalizada que execute o comando NGen.exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Verificar que as imagens nativas estão sendo usadas para o EF  

Você pode verificar que um aplicativo específico é usando um assembly nativo procurando por assemblies carregados com a extensão ". ni.dll"ou". ni.exe". Por exemplo, uma imagem nativa para o assembly de tempo de execução principal do EF será chamada EntityFramework.ni.dll. Uma maneira fácil de inspecionar os assemblies .NET carregados de um processo é usar [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Outras coisas a serem consideradas  

**Criar uma imagem nativa de um assembly não deve ser confundido com o registro do assembly na [GAC (Cache de Assembly Global)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**. NGen.exe permite a criação de imagens de assemblies que não estão no GAC e, na verdade, vários aplicativos que usam uma versão específica do EF podem compartilhar a mesma imagem nativa. Enquanto o Windows 8 pode criar automaticamente as imagens nativas para assemblies no GAC, o tempo de execução do EF é otimizado para ser implantado junto com seu aplicativo e não recomendamos registrá-lo no GAC, pois isso tem um impacto negativo na resolução de assembly e seus aplicativos, entre outros aspectos de manutenção.  
