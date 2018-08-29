---
title: Vários diagramas por modelo - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: f27afbd3b44ff3eb8044ab960f10b2856a603ee3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993772"
---
# <a name="multiple-diagrams-per-model"></a>Vários diagramas por modelo
> [!NOTE]
> **EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Esta página e o vídeo mostra como dividir um modelo em vários diagramas usando o Entity Framework Designer (Designer de EF). Talvez você queira usar esse recurso quando seu modelo se torna muito grande para exibir ou editar.

Em versões anteriores do EF Designer você pode ter apenas um diagrama pelo arquivo EDMX. Começando com o Visual Studio 2012, você pode usar o EF Designer para dividir o arquivo EDMX em vários diagramas.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como dividir um modelo em vários diagramas usando o Entity Framework Designer (Designer de EF). Talvez você queira usar esse recurso quando seu modelo se torna muito grande para exibir ou editar.

**Apresentado por**: Julia Kornich

**Vídeo**: [WMV](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Visão geral do EF Designer

Quando você cria um modelo usando Assistente do EF Designer de modelo de dados de entidade, um arquivo. edmx é criado e adicionado à sua solução. Esse arquivo define a forma de suas entidades e como eles são mapeados para o banco de dados.

O EF Designer consiste dos seguintes componentes:

-   Uma superfície de design visual para editar o modelo. Você pode criar, modificar ou excluir entidades e associações.
-   Um **Model Browser** janela que fornece modos de exibição de árvore do modelo.  As entidades e suas associações estão localizadas sob o *\[ModelName\]* pasta. As tabelas de banco de dados e restrições estão localizadas sob o  *\[ModelName\]*. Pasta de Store.
-   Um **Mapping Details** janela para exibir e editar os mapeamentos. Você pode mapear tipos de entidades ou associações para procedimentos armazenados, colunas e tabelas de banco de dados. 

A janela de superfície de design visual é aberta automaticamente quando o Assistente de modelo de dados de entidade é concluída. Se o navegador de modelo não estiver visível, clique com botão direito a principal superfície de design e selecione **Model Browser**.

Captura de tela a seguir mostra um arquivo. edmx aberto no Designer de EF. A captura de tela mostra a superfície de design visual (à esquerda) e o **Model Browser** janela (à direita).

![EFDesigner2](~/ef6/media/efdesigner2.png)

Para desfazer uma operação realizada no EF Designer, clique em Ctrl-Z.

## <a name="working-with-diagrams"></a>Trabalhando com diagramas

Por padrão, o EF Designer cria um diagrama chamado diagrama1. Se você tiver um diagrama com um grande número de entidades e associações, você mais gosta deseja dividi-los logicamente. Começando com o Visual Studio 2012, você pode exibir seu modelo conceitual em vários diagramas.   

Conforme você adiciona novos diagramas, eles aparecem na pasta de diagramas na janela do navegador de modelo. Para renomear um diagrama: selecione o diagrama na janela do navegador de modelo, clique uma vez no nome e digite o novo nome.  Você pode também o nome do diagrama com o botão direito e selecione **Renomear**.

O nome do diagrama é exibido ao lado do nome do arquivo. edmx, no editor do Visual Studio. Por exemplo, Model1.edmx\[diagrama1\].

![DiagramName](~/ef6/media/diagramname.png)

O conteúdo de diagramas (forma e cor de entidades e associações) é armazenado na. arquivo edmx.diagram. Para exibir esse arquivo, selecione o Gerenciador de soluções e desdobrar o arquivo. edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Você não deve editar o. edmx.diagram de arquivos manualmente, o conteúdo desse arquivo talvez substituídos pelo EF Designer.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Divisão de entidades e associações em um novo diagrama

Você pode selecionar as entidades no diagrama existente (mantenha a tecla Shift para selecionar várias entidades). Clique o botão direito do mouse e selecione **mover para o novo diagrama**. O novo diagrama é criado e as entidades selecionadas e suas associações são movidas para o diagrama.

Como alternativa, você pode clique na pasta de diagramas no modelo de navegador e selecionar **adicionar novo diagrama.** Você pode, então, arraste e solte entidades sob a pasta de tipos de entidade no modelo de navegador para a superfície de design.

Você também pode recortar ou copiar entidades (usando as teclas Ctrl-C ou Ctrl-X) de um diagrama e colar (usando a tecla Ctrl + V) no outro. Se o diagrama no qual você está colando uma entidade já contiver uma entidade com o mesmo nome, uma nova entidade será criada e adicionada ao modelo.  Por exemplo: Diagram2 contém a entidade de departamento. Em seguida, você deve colar outro departamento em Diagram2. A entidade Department1 é criada e adicionada ao modelo conceitual.   

Para incluir as entidades relacionadas em um diagrama, a entidade de rick-clique e selecione **relacionados incluem**. Isso fará uma cópia das entidades relacionadas e associações no diagrama especificado.

## <a name="changing-the-color-of-entities"></a>Alterar a cor de entidades

Além de divisão de um modelo em vários diagramas, você também pode alterar as cores das suas entidades.

Para alterar a cor, selecione uma entidade (ou várias entidades) na superfície de design. Em seguida, clique no botão direito do mouse e selecione **propriedades**. Na janela Propriedades, selecione a **cor de preenchimento** propriedade. Especifique a cor usando um nome de cor válidos (por exemplo, vermelho) ou um RGB válido (por exemplo, 255, 128, 128). 

![Cor](~/ef6/media/color.png)

## <a name="summary"></a>Resumo

Este tópico analisamos como dividir um modelo em vários diagramas e também como especificar uma cor diferente para uma entidade usando o Entity Framework Designer. 
