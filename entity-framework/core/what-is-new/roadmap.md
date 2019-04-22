---
title: Roteiro do Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: e9d1a707c643036fa15b1e0e46730eadefd65518
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562462"
---
# <a name="entity-framework-core-roadmap"></a>Roteiro do Entity Framework Core

> [!IMPORTANT]
> Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.

## <a name="ef-core-30"></a>EF Core 3.0

Com o lançamento do EF Core 2.2, nosso foco principal agora é o EF Core 3.0.
Veja [O que há de novo no EF Core 3.0](xref:core/what-is-new/ef-core-3.0/index) para obter informações sobre os [novos recursos](xref:core/what-is-new/ef-core-3.0/features) planejado e as [alterações significativas](xref:core/what-is-new/ef-core-3.0/breaking-changes) intencionais incluídas na versão.

## <a name="schedule"></a>Agendamento

A agenda de lançamento para o EF Core está em sincronia com a [agenda de lançamento do .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

## <a name="backlog"></a>Lista de pendências

O [marco da lista de pendências](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) em nosso rastreador de problemas contém problemas que esperamos resolver um dia ou que acreditamos que alguém da comunidade possa solucionar.
Incentivamos os clientes a enviar comentários e votos sobre esses problemas.
Encorajamos os colaboradores que procuram trabalhar em qualquer um desses problemas a começar uma discussão sobre como abordá-los.

Não há uma garantia de que vamos trabalhar em um determinado recurso em uma versão específica do EF Core.
Como em todos os projetos, prioridades, cronogramas da versão e recursos disponíveis do software podem mudar a qualquer momento.
Mas, se pretendemos resolver um problema em um período de tempo específico, atribuiremos este problema a um marco de lançamento em vez de um marco da lista de pendências.
Podemos mover rotineiramente os problemas entre os marcos de lançamento e a lista de pendências como parte do nosso [processo de planejamento da versão](#release-planning-process).

Provavelmente vamos fechar um problema se não estivermos planejando abordá-lo.
Mas podemos reconsiderar um problema fechado anteriormente se houver novas informações sobre ele.

## <a name="release-planning-process"></a>Processo de planejamento da versão

Frequentemente, recebemos perguntas sobre como escolhemos os recursos específicos de uma versão.
É certo que nossa lista de pendências não se traduz automaticamente em planos de versão.
A presença de um recurso no EF6 também não significa que esse recurso será automaticamente implementado no EF Core.

É difícil detalhar todo o processo que seguimos para planejar uma versão.
Grande parte dele envolve a discussão de recursos específicos, oportunidades e prioridades, e o processo em si também evolui a cada versão.
No entanto, podemos resumir as perguntas comuns que tentamos responder ao decidir qual será a próxima tarefa:

1. **Quantos desenvolvedores utilizarão o recurso e como ele melhorará os aplicativos/a experiência?** Para responder a isso, coletamos os comentários de várias fontes. Comentários e votos sobre essas questões são uma dessas fontes.

2. **Quais são as soluções alternativas que podem ser usadas caso não implementemos este recurso?** Por exemplo, diversos desenvolvedores podem mapear uma tabela de junção para solucionar a falta de suporte nativo de muitos para muitos. Obviamente, nem todos os desenvolvedores desejam fazê-lo, mas muitos podem, e isso conta como um fator em nossa decisão.

3. **Implementar este recurso desenvolve a arquitetura do EF Core de forma que nos aproxime da implementação de outros recursos?** Favorecemos os recursos que atuam como blocos de construção para outros recursos. Por exemplo, entidades de recipiente da propriedade podem nos ajudar a oferecer suporte de muitos-para-muitos e os construtores de entidade permitiram o nosso suporte ao carregamento lento.

4. **O recurso é um ponto de extensibilidade?** Favorecemos os pontos de extensibilidade em relação aos recursos normais, pois eles permitem aos desenvolvedores conectar seus próprios comportamentos e compensar qualquer funcionalidade ausente.

5. **O que é a sinergia do recurso quando usado em combinação com outros produtos?** Favorecemos recursos que permitem ou melhore significativamente a experiência de utilizar EF Core com outros produtos como o .NET Core, a última versão do Visual Studio, do Microsoft Azure etc.

6. **Quais são as habilidades das pessoas disponíveis para trabalhar em um recurso e como melhor aproveitá-los?** Cada membro da equipe do EF e os colaboradores da comunidade têm diferentes níveis de experiência em áreas distintas, portanto, temos que planejar adequadamente. Mesmo se quiséssemos que todos se empenhassem em trabalhar em um recurso específico, como as traduções do GroupBy ou muitos para muitos, isso não seria prático.

Como mencionado anteriormente, o processo evolui a cada versão.
No futuro, gostaríamos de dar mais oportunidades a membros da comunidade de desenvolvedores de contribuir com os planos de versão.
Por exemplo, facilitando a revisão dos rascunhos propostos dos recursos e do próprio plano de versão.
