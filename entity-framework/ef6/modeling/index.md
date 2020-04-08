---
title: Criando um modelo – EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: bd9843a93121f53518a307c9d2d43b68ae03369c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413461"
---
# <a name="creating-a-model"></a>Criar um Modelo

Um modelo do EF armazena os detalhes sobre como as propriedades e classes de aplicativo são mapeadas para colunas e tabelas de banco de dados. Existem duas maneiras principais de criar um modelo do EF:

- **Usando o Code First**: o desenvolvedor escreve o código para especificar o modelo. O EF gera os modelos e os mapeamentos em runtime com base em classes de entidade e configurações de modelo adicionais fornecidas pelo desenvolvedor.

- **Usando o EF Designer**: o desenvolvedor desenha caixas e linhas para especificar o modelo usando o EF Designer. O modelo resultante é armazenado como XML em um arquivo com a extensão EDMX. Os objetos de domínio do aplicativo normalmente são gerados de forma automática com base no modelo conceitual.

## <a name="ef-workflows"></a>Fluxos de trabalho do EF

Essas duas abordagens podem ser usadas para visar um banco de dados existente ou criar um novo banco de dados, resultando em quatro fluxos de trabalho diferentes.
Saiba sobre qual é melhor para você:  

|                                           | Eu quero apenas escrever o código...                                                                                                                   | Eu quero usar um designer...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Estou criando um novo banco de dados**          | [Use o **Code First** para definir o modelo no código e, em seguida, gerar um banco de dados.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Use o **Model First** para definir o modelo usando caixas e linhas e, em seguida, gerar um banco de dados.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Preciso acessar um banco de dados existente** | [Use o **Code First** para criar um modelo baseado em código que seja mapeado para um banco de dados existente.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Use o **Database First** para criar um modelo de caixas e linhas que é mapeado para um banco de dados existente.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Assista ao vídeo: Qual fluxo de trabalho do EF devo usar?

Este breve vídeo explica as diferenças e como encontrar o que é ideal para você.

**Apresentado por**: [Rowan Miller](https://romiller.com/)

![Which Workflow Thumb](../media/whichworkflow-thumb.png) [WMV](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Se depois de assistir ao vídeo você ainda não se sentir confortável para decidir se deseja usar o EF Designer ou o Code First, aprenda os dois.

## <a name="a-look-under-the-hood"></a>Uma olhada nos bastidores

Independentemente de você usar o Code First ou o EF Designer, um modelo do EF sempre tem vários componentes:

- Os objetos de domínio do aplicativo ou os próprios tipos de entidade. Com frequência chamamos isso de camada de objeto

- Um modelo conceitual, consistindo em tipos de entidade específicos de domínio e relacionamentos, descritos usando o [Modelo de Dados de Entidade](~/ef6/resources/glossary.md#entity-data-model). Essa camada normalmente é referida com a letra “C”, para _conceitual_.

- Um modelo de armazenamento que representam tabelas, colunas e relações, conforme definido no banco de dados. Essa camada normalmente é referida com a letra “s”, para _storage (armazenamento)_ .  

- Um mapeamento entre o modelo conceitual e o esquema de banco de dados. Esse mapeamento é conhecido como mapeamento de "C-S".

O mecanismo de mapeamento do EF utiliza o mapeamento de “C-S” para transformar as operações em entidades, como criar, ler, atualizar e excluir, em operações equivalentes em tabelas no banco de dados.

O mapeamento entre o modelo conceitual e os objetos do aplicativo é conhecido como mapeamento de "O-C". Em comparação com o mapeamento de "C-S", o mapeamento de "O-C" é implícito e de um para um: entidades, propriedades e relações definidas no modelo conceitual devem corresponder às formas e aos tipos dos objetos .NET. Do EF4 e adiante, a camada de objetos pode ser composta por objetos simples com propriedades sem nenhuma dependência no EF. Eles são geralmente conhecidos como objetos POCO (Objetos de CLR Antigos Planos) e o mapeamento de tipos e propriedades é realizado com base nas convenções de correspondência de nomes. Anteriormente, no EF 3.5 havia restrições específicas para a camada de objetos, como entidades precisando derivar da classe EntityObject e precisando carregar atributos do EF para implementar o mapeamento de “O-C”.
