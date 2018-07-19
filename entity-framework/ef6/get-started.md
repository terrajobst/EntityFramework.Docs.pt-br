---
title: Introdução ao Entity Framework 6 – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
caps.latest.revision: 3
ms.openlocfilehash: 36857650bc546acd769e629a1e92948a63bfb786
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39119632"
---
# <a name="get-started-with-entity-framework-6"></a>Introdução ao Entity Framework 6

Este guia contém uma coleção de links para artigos de documentação selecionada, instruções passo a passo e vídeos que podem ajudar você a começar a usar rapidamente:

## <a name="get-entity-frameworkef6fundamentalsinstallmd"></a>[Como obter o Entity Framework](~/ef6/fundamentals/install.md)
Aqui você aprenderá a adicionar o Entity Framework aos seus aplicativos. Se desejar usar o EF Designer, o instale no Visual Studio.

## <a name="creating-a-model-code-first-the-ef-designer-and-the-ef-workflowsef6modelingindexmd"></a>[Criação de um modelo: o Code First, o EF Designer e os Fluxos de trabalho do EF](~/ef6/modeling/index.md)
Você prefere especificar seu modelo do EF escrevendo código ou desenhando caixas e linhas?
Você usará o EF para mapear seus objetos para um banco de dados existente ou gostaria que o EF criasse um banco de dados personalizado para seus objetos?
Aqui você conhecerá duas abordagens diferentes para usar o EF6: o EF Designer e o Code First.
Siga a discussão e assista ao vídeo sobre as diferenças.

## <a name="working-with-dbcontextef6fundamentalsworking-with-dbcontextmd"></a>[Como trabalhar com DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
O DbContext é o primeiro e mais importante tipo de EF que você precisa aprender como usar. Ele funciona como a barra inicial para consultas de banco de dados e controla alterações feitas nos objetos para que elas possam ser persistidas novamente no banco de dados.

## <a name="ask-a-questionef6resourcesget-helpmd"></a>[Fazer uma pergunta](~/ef6/resources/get-help.md)
Descubra como obter ajuda de especialistas e contribuir com suas próprias respostas à comunidade.

## <a name="contributehttpgithubcomaspnetentityframework6"></a>[Contribuir](http://github.com/aspnet/EntityFramework6/)
O Entity Framework 6 usa um modelo de desenvolvimento aberto. Descubra como é possível ajudar a aprimorar ainda mais o EF acessando nosso repositório do GitHub.

## <a name="index-of-walkthroughs"></a>Índices de orientações passo a passo

- Code First
  - [Code First para um fluxo de trabalho existente do banco de dados](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [Code First para um novo fluxo de trabalho do banco de dados](~/ef6/modeling/code-first/workflows/new-database.md)
  - [Mapeamento de enumerações usando o Code First](~/ef6/modeling/code-first/data-types/enums.md)
  - [Mapeamento de tipos espaciais usando o Code First](~/ef6/modeling/code-first/data-types/spatial.md)
  - [Como escrever convenções personalizadas do Code First](~/ef6/modeling/code-first/conventions/custom.md)
  - [Uso da configuração fluente do Code First com o Visual Basic](~/ef6/modeling/code-first/fluent/vb.md)
  - [Migrações do Code First](~/ef6/modeling/code-first/migrations/index.md)
  - [Migrações do Code First em ambientes de equipe](~/ef6/modeling/code-first/migrations/teams.md)
  - [Migrações automáticas do Code First](~/ef6/modeling/code-first/migrations/automatic.md) (não é mais recomendado)

- EF Designer
  - [Fluxo de trabalho do Database First](~/ef6/modeling/designer/workflows/database-first.md)
  - [Fluxo de trabalho do Model First](~/ef6/modeling/designer/workflows/model-first.md)
  - [Mapeamento de enumerações](~/ef6/modeling/designer/data-types/enums.md)
  - [Mapeamento de tipos espaciais](~/ef6/modeling/designer/data-types/spatial.md)
  - [Mapeamento de herança de tabela por hierarquia](~/ef6/modeling/designer/inheritance/tph.md)
  - [Mapeamento de herança de tabela por tipo](~/ef6/modeling/designer/inheritance/tpt.md)
  - [Mapeamento de procedimento armazenado para atualizações](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [Mapeamento de procedimento armazenado para consulta](~/ef6/modeling/designer/stored-procedures/query.md)
  - [Divisão de entidade](~/ef6/modeling/designer/entity-splitting.md)
  - [Divisão de tabela](~/ef6/modeling/designer/table-splitting.md)
  - [Definição de uma consulta](~/ef6/modeling/designer/advanced/defining-query.md) (Avançado)
  - [Funções com valor de tabela](~/ef6/modeling/designer/advanced/tvfs.md) (Avançado)

- Princípios básicos
  - [Consulta assíncrona e salvamento](~/ef6/fundamentals/async.md)
  - [Associação de dados com WinForms](~/ef6/fundamentals/databinding/winforms.md)
  - [Associação de dados com WPF](~/ef6/fundamentals/databinding/wpf.md)
  - [Cenários desconectados com entidades de rastreamento automático](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (não mais recomendado)
