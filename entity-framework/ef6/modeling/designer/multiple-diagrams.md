---
title: Vários diagramas por modelo – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e78b927a147143629ac5b73e23edf439ba6d0264
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418290"
---
# <a name="multiple-diagrams-per-model"></a>Vários diagramas por modelo
> [!NOTE]
> **Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Este vídeo e página mostra como dividir um modelo em vários diagramas usando o Entity Framework Designer (EF designer). Talvez você queira usar esse recurso quando seu modelo se tornar muito grande para exibir ou editar.

Em versões anteriores do EF designer, você poderia ter apenas um diagrama de acordo com o arquivo EDMX. A partir do Visual Studio 2012, você pode usar o designer do EF para dividir o arquivo EDMX em vários diagramas.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como dividir um modelo em vários diagramas usando o Entity Framework Designer (Designer de EF). Talvez você queira usar esse recurso quando seu modelo se tornar muito grande para exibir ou editar.

**Apresentado por**: Julia Kornich

**Vídeo**: [wmv](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (zip)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Visão geral do designer do EF

Quando você cria um modelo usando o assistente de Modelo de Dados de Entidade do EF designer, um arquivo. edmx é criado e adicionado à sua solução. Esse arquivo define a forma de suas entidades e como elas são mapeadas para o banco de dados.

O designer do EF consiste nos seguintes componentes:

-   Uma superfície de Design Visual para editar o modelo. Você pode criar, modificar ou excluir entidades e associações.
-   Um **navegador de modelos** janela que fornece exibições de árvore do modelo.  As entidades e suas associações estão localizadas na pasta *\[modelname\]* . As tabelas e restrições do banco de dados estão localizadas sob o *\[modelname\]* . Pasta de armazenamento.
-   Uma janela de **detalhes de mapeamento** para exibir e editar mapeamentos. Você pode mapear tipos de entidade ou associações para tabelas, colunas e procedimentos armazenados de banco de dados. 

A janela superfície de Design Visual é aberta automaticamente quando o assistente de Modelo de Dados de Entidade é concluído. Se o navegador de modelos não estiver visível, clique com o botão direito do mouse na superfície de design principal e selecione **navegador de modelos**.

A captura de tela a seguir mostra um arquivo. edmx aberto no designer do EF. A captura de tela mostra a superfície de Design Visual (à esquerda) e o **navegador de modelo** janela (à direita).

![Designer de EF 2](~/ef6/media/efdesigner2.png)

Para desfazer uma operação feita no designer do EF, clique em CTRL-Z.

## <a name="working-with-diagrams"></a>Trabalhando com diagramas

Por padrão, o designer do EF cria um diagrama chamado Diagrama1. Se você tiver um diagrama com um grande número de entidades e associações, será mais interessante querer dividi-las logicamente. A partir do Visual Studio 2012, você pode exibir seu modelo conceitual em vários diagramas.   

À medida que você adiciona novos diagramas, eles aparecem na pasta diagramas na janela Navegador de modelos. Para renomear um diagrama: selecione o diagrama na janela Navegador de modelos, clique em uma vez no nome e digite o novo nome.  Você também pode clicar com o botão direito do mouse no nome do diagrama e selecionar **renomear**.

O nome do diagrama é exibido ao lado do nome do arquivo. edmx, no editor do Visual Studio. Por exemplo, Model1. edmx\[Diagrama1\].

![Diagramaname](~/ef6/media/diagramname.png)

O conteúdo dos diagramas (forma e cor de entidades e associações) é armazenado no arquivo. edmx. Diagram. Para exibir esse arquivo, selecione Gerenciador de Soluções e desdobrar o arquivo. edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Você não deve editar o arquivo. edmx. Diagram manualmente, o conteúdo desse arquivo pode ser substituído pelo designer do EF.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Dividindo entidades e associações em um novo diagrama

Você pode selecionar entidades no diagrama existente (Mantenha Shift para selecionar várias entidades). Clique com o botão direito do mouse e selecione **mover para novo diagrama**. O novo diagrama é criado e as entidades selecionadas e suas associações são movidas para o diagrama.

Como alternativa, você pode clicar com o botão direito do mouse na pasta Diagrams no navegador de modelos e selecionar **Adicionar novo diagrama.** Em seguida, você pode arrastar e soltar entidades na pasta tipos de entidade no navegador de modelos na superfície de design.

Você também pode recortar ou copiar entidades (usando as teclas CTRL-X ou CTRL-C) de um diagrama e colar (usando a tecla Ctrl-V) no outro. Se o diagrama no qual você está colando uma entidade já contiver uma entidade com o mesmo nome, uma nova entidade será criada e adicionada ao modelo.  Por exemplo: Diagram2 contém a entidade Department. Em seguida, Cole outro departamento em Diagram2. A entidade Department1 é criada e adicionada ao modelo conceitual.   

Para incluir entidades relacionadas em um diagrama, Rick-clique na entidade e selecione **incluir relacionados**. Isso fará uma cópia das entidades e associações relacionadas no diagrama especificado.

## <a name="changing-the-color-of-entities"></a>Alterando a cor das entidades

Além de dividir um modelo em vários diagramas, você também pode alterar as cores de suas entidades.

Para alterar a cor, selecione uma entidade (ou várias entidades) na superfície de design. Em seguida, clique com o botão direito do mouse e selecione **Propriedades**. Na janela Propriedades, selecione a propriedade **cor de preenchimento** . Especifique a cor usando um nome de cor válido (por exemplo, vermelho) ou um RGB válido (por exemplo, 255, 128, 128). 

![Color](~/ef6/media/color.png)

## <a name="summary"></a>Resumo

Neste tópico, examinamos como dividir um modelo em vários diagramas e também como especificar uma cor diferente para uma entidade usando a Entity Framework Designer. 
