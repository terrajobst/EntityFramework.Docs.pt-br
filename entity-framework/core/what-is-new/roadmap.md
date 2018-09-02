---
title: Roteiro do Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: cd4b7ddaafe9501c4bb9f2496e87f619d239ab62
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995254"
---
# <a name="entity-framework-core-roadmap"></a>Roteiro do Entity Framework Core

> [!IMPORTANT]
> Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.

A versão estável do EF Core 2.1 foi lançada em 30 de maio de 2018. Mais informações sobre essa versão podem ser encontradas em [Novidades no EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

Ainda não concluímos o [processo de planejamento de lançamento](#release-planning-process) para a versão subsequente à versão 2.1.

## <a name="schedule"></a>Agendamento

O agendamento do EF Core está sincronizado com o do [.NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) e o do [ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Lista de pendências

Usamos o [Marco da Lista de Pendências](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) no nosso rastreador de problemas para manter uma lista detalhada de problemas e recursos. Os clientes podem comentar e votar neles.

Costumamos deixar abertos os problemas que esperamos resolver em algum momento ou que algum participante da comunidade possa solucionar. Porém, isso não significa que tentaremos resolvê-los em um período de tempo específico até atribuí-los a um determinado marco como parte do [processo de planejamento de versão](#release-planning-process).

Caso não tenhamos planos de implementar um recurso, provavelmente fecharemos o problema. Um problema fechado pode ser reconsiderado posteriormente se obtivermos novas informações sobre ele.

Considerando todos esses fatores, não temos informações suficientes sobre o futuro para dizer que o recurso X será solucionado na hora/versão Y. Como em todos os projetos de software, as prioridades, os agendamentos de versão e os recursos disponíveis podem mudar a qualquer momento.

## <a name="release-planning-process"></a>Processo de planejamento da versão

Frequentemente, recebemos perguntas sobre como escolhemos os recursos específicos de uma versão. É certo que nossa lista de pendências não se traduz automaticamente em planos de versão. A presença de um recurso no EF6 também não significa que esse recurso será automaticamente implementado no EF Core.

É difícil detalhar todo o processo seguido para planejar uma versão por dois motivos: grande parte do processo consiste na discussão sobre recursos específicos, oportunidades e prioridades; e porque o processo em si geralmente evolui com cada versão. No entanto, é relativamente fácil resumir as perguntas comuns que tentamos responder ao decidir qual será a próxima tarefa:

1. **Quantos desenvolvedores utilizarão o recurso e como ele melhorará os aplicativos/a experiência?** Agregamos os comentários de várias fontes — Comentários e votos sobre essas questões são uma dessas fontes.

2. **Quais são as soluções alternativas que podem ser usadas caso não implementemos este recurso?** Por exemplo, diversos desenvolvedores conseguem mapear uma tabela de junção para solucionar a falta de suporte nativo muitos para muitos. Obviamente, nem todos os desenvolvedores podem fazer isso, mas muitos podem e esse é um fator que conta.

3. **Implementar este recurso desenvolve a arquitetura do EF Core de forma que nos aproxime da implementação de outros recursos?** Tendemos favorecer recursos que atuam como blocos de construção para outros recursos — por exemplo, a divisão de tabelas feita para tipos proprietários ajuda a mover na direção do suporte TPT.

4. **O recurso é um ponto de extensibilidade?** Tendemos a favorecer pontos de extensibilidade porque eles permitem que desenvolvedores conectem seus comportamentos e obtenham parte da funcionalidade ausente dessa maneira. Nosso plano é fazer um pouco disso como início do trabalho de carregamento lento.

5. **O que é a sinergia do recurso quando usado em combinação com outros produtos?** Tendemos a favorecer recursos que permitem que o EF Core seja usado com outros produtos ou melhore significativamente a experiência de utilizar outros produtos como o .NET Core, a última versão do Visual Studio, do Microsoft Azure etc.

6. **Quais são as habilidades das pessoas disponíveis para trabalhar em um recurso e como melhor aproveitá-los?** Cada membro da equipe do EF e até mesmo os colaboradores da comunidade têm diferentes níveis de experiência em áreas distintas. Por isso, é necessário planejar adequadamente. Mesmo se quiséssemos que todos se empenhassem em trabalhar em um recurso específico, como as traduções do GroupBy ou muitos para muitos, isso não seria prático.

Como mencionado anteriormente, esse processo evolui a cada versão. No futuro, gostaríamos de dar mais oportunidades a membros da comunidade de desenvolvedores de contribuir com os planos de versão, facilitando a revisão dos rascunhos propostos dos recursos e do próprio plano de versão.
