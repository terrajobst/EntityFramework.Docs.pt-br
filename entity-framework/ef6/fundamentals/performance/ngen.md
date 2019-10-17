---
title: Melhorando o desempenho de inicialização com o NGen-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 841aec645abdb2a56076d0b70bfb2614b0acafb4
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446000"
---
# <a name="improving-startup-performance-with-ngen"></a>Melhorando o desempenho de inicialização com o NGen
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

O .NET Framework dá suporte à geração de imagens nativas para bibliotecas e aplicativos gerenciados como uma maneira de ajudar os aplicativos a serem iniciados mais rapidamente e também em alguns casos usam menos memória. As imagens nativas são criadas pela tradução de assemblies de código gerenciado em arquivos que contêm instruções de máquina nativa antes de o aplicativo ser executado, aliviando o compilador JIT .NET (just-in-time) de ter que gerar as instruções nativas em tempo de execução do aplicativo.  

Antes da versão 6, as bibliotecas principais do tempo de execução do EF faziam parte do .NET Framework e as imagens nativas foram geradas automaticamente para eles. A partir da versão 6, o tempo de execução do EF inteiro foi combinado no pacote NuGet do EntityFramework. As imagens nativas devem agora ser geradas usando a ferramenta de linha de comando NGen. exe para obter resultados semelhantes.  

Empírica observações mostram que as imagens nativas dos assemblies de tempo de execução do EF podem ser cortadas entre 1 e 3 segundos de tempo de inicialização do aplicativo.  

## <a name="how-to-use-ngenexe"></a>Como usar NGen. exe  

A função mais básica da ferramenta NGen. exe é "instalar" (ou seja, criar e persistir em disco) imagens nativas para um assembly e todas as suas dependências diretas. Veja como você pode conseguir isso:  

1. Abra uma janela de prompt de comando como administrador.
2. Altere o diretório de trabalho atual para o local dos assemblies dos quais você deseja gerar imagens nativas:

   ``` console
   cd <*Assemblies location*>  
   ```

3. Dependendo do seu sistema operacional e da configuração do aplicativo, talvez seja necessário gerar imagens nativas para a arquitetura de 32 bits, a arquitetura de 64 bits ou para ambos.

   Para execução de 32 bits:

   ``` console
   %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
   ```

   Para execução de 64 bits:
  
   ``` console
   %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
   ```

> [!TIP]
> A geração de imagens nativas para a arquitetura errada é um erro muito comum. Quando em dúvida, você pode simplesmente gerar imagens nativas para todas as arquiteturas que se aplicam ao sistema operacional instalado no computador.  

O NGen. exe também dá suporte a outras funções, como desinstalar e exibir as imagens nativas instaladas, enfileirar a geração de várias imagens, etc. Para obter mais detalhes sobre o uso, leia a [documentação do NGen. exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Quando usar NGen. exe  

Quando se trata de decidir quais assemblies gerar imagens nativas para em um aplicativo baseado no EF versão 6 ou superior, você deve considerar as seguintes opções:  

- **O assembly de tempo de execução do EF principal, EntityFramework. dll**: um aplicativo típico baseado em EF executa uma quantidade significativa de código desse assembly na inicialização ou no seu primeiro acesso ao banco de dados. Consequentemente, a criação de imagens nativas desse assembly produzirá os maiores ganhos no desempenho de inicialização.  
- **Qualquer assembly de provedor de EF usado pelo seu aplicativo: o**tempo de inicialização também pode se beneficiar ligeiramente da geração de imagens nativas desses. Por exemplo, se o aplicativo usar o provedor do EF para SQL Server, você desejará gerar uma imagem nativa para o EntityFramework. SqlServer. dll.  
- **Os assemblies de seu aplicativo e outras dependências**: a [documentação NGen. exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx) aborda os critérios gerais para escolher quais assemblies gerar imagens nativas e o impacto de imagens nativas sobre segurança, opções avançadas como "Hard Associação ", cenários como usar imagens nativas em cenários de depuração e criação de perfil, etc.  

> [!TIP]
> Certifique-se de medir cuidadosamente o impacto de usar imagens nativas no desempenho de inicialização e no desempenho geral do seu aplicativo e compará-los com os requisitos reais. Embora as imagens nativas geralmente ajudem a melhorar o desempenho de inicialização e, em alguns casos, reduzir o uso de memória, nem todos os cenários se beneficiarão igualmente. Por exemplo, na execução de estado estável (ou seja, quando todos os métodos que estão sendo usados pelo aplicativo tiverem sido invocados pelo menos uma vez), o código gerado pelo compilador JIT pode, na verdade, gerar um desempenho um pouco melhor do que as imagens nativas.  

## <a name="using-ngenexe-in-a-development-machine"></a>Usando NGen. exe em um computador de desenvolvimento  

Durante o desenvolvimento, o compilador JIT do .NET oferecerá a melhor compensação geral para o código que é alterado com frequência. A geração de imagens nativas para dependências compiladas, como os assemblies de tempo de execução do EF, pode ajudar a acelerar o desenvolvimento e os testes, recortando alguns segundos no início de cada execução.  

Um bom local para localizar os assemblies do tempo de execução do EF é o local do pacote NuGet para a solução. Por exemplo, para um aplicativo que usa o EF 6.0.2 com SQL Server e visando o .NET 4,5 ou superior, você pode digitar o seguinte em uma janela de prompt de comando (Lembre-se de abri-lo como administrador):  

```console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Isso aproveita o fato de que a instalação das imagens nativas para o provedor do EF para SQL Server também instalará por padrão as imagens nativas para o assembly de tempo de execução do EF principal. Isso funciona porque o NGen. exe pode detectar que o EntityFramework. dll é uma dependência direta do assembly do EntityFramework. SqlServer. dll localizado no mesmo diretório.  

## <a name="creating-native-images-during-setup"></a>Criando imagens nativas durante a instalação  

O kit de ferramentas do WiX dá suporte à colocação da geração de imagens nativas para assemblies gerenciados durante a instalação, conforme explicado neste [Guia de instruções](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Outra alternativa é criar uma tarefa de instalação personalizada que execute o comando NGen. exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Verificando se as imagens nativas estão sendo usadas para o EF  

Você pode verificar se um aplicativo específico está usando um assembly nativo procurando assemblies carregados que tenham a extensão ". ni. dll" ou ". ni. exe". Por exemplo, uma imagem nativa para o assembly de tempo de execução principal do EF será chamada de EntityFramework. ni. dll. Uma maneira fácil de inspecionar os assemblies .NET carregados de um processo é usar o [Gerenciador de processos](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Outras coisas que você deve conhecer  

A **criação de uma imagem nativa de um assembly não deve ser confundida com o registro do assembly no [GAC (cache de assembly global)](https://msdn.microsoft.com/library/yf1d93sz.aspx)** . O NGen. exe permite a criação de imagens de assemblies que não estão no GAC e, na verdade, vários aplicativos que usam uma versão específica do EF podem compartilhar a mesma imagem nativa. Embora o Windows 8 possa criar automaticamente imagens nativas para assemblies colocados no GAC, o tempo de execução do EF é otimizado para ser implantado junto com seu aplicativo e não é recomendável registrá-lo no GAC, pois isso tem um impacto negativo na resolução do assembly e manutenção de seus aplicativos entre outros aspectos.  
