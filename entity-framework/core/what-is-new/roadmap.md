---
title: Roteiro do Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 9064b323c11282418f2bedf70f874d45c18bb78a
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325334"
---
# <a name="entity-framework-core-roadmap"></a>Roteiro do Entity Framework Core

> [!IMPORTANT]
> Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.

### <a name="ef-core-30"></a>EF Core 3.0

Com o lançamento do EF Core 2.2, agora nosso foco principal é o EF Core 3.0, que será alinhado com o .NET Core 3.0 e o ASP.NET 3.0.

Ainda não concluímos novos recursos, portanto, os [pacotes do EF Core 3.0 Preview 1 publicados na Galeria do NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1) em dezembro de 2018 contêm apenas [correções de bugs, pequenas melhorias e alterações feitas na preparação para o trabalho no 3.0](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed).

Na verdade, ainda precisamos refinar nosso [planejamento de lançamento](#release-planning-process) do 3.0 para garantir que temos o conjunto certo de recursos que possam ser concluídos no tempo alocado.
Compartilharemos mais informações à medida que houver mais clareza, mas aqui estão alguns recursos e temas de alto nível nos quais pretendemos trabalhar:

- **Melhorias no LINQ ([#12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))**: O LINQ permite que você escreva consultas de banco de dados sem sair da linguagem de sua escolha, aproveitando as informações de tipo avançado para obter IntelliSense e a verificação do tipo em tempo de compilação.
  Mas o LINQ também permite que você escreva um número ilimitado de consultas complicadas, o que sempre foi um enorme desafio para provedores do LINQ.
  Nas primeiras versões do EF Core, solucionamos parte disso ao descobrir quais partes de uma consulta podem ser convertidas para o SQL e depois permitindo que o restante da consulta seja executada na memória no cliente.
  Essa execução do lado do cliente pode ser desejável em algumas situações, mas, em muitos outros casos, pode resultar em consultas ineficientes que podem não ser identificadas até que um aplicativo seja implantado na produção.
  No EF Core 3.0, planejamos fazer alterações profundas no funcionamento da implementação do LINQ e em como faremos os testes.
  Os objetivos são torná-lo mais robusto (por exemplo, para evitar a interrupção de consultas em lançamento de patch), conseguir converter corretamente mais expressões em SQL, gerar consultas eficientes em mais casos e impedir que as consultas ineficientes não sejam detectadas.

- **Suporte do Cosmos DB ([#8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))**: Estamos trabalhando em um provedor do Cosmos DB para o EF Core para permitir que desenvolvedores familiarizados com o modelo de programação EF conduzam facilmente o Azure Cosmos DB como um banco de dados de aplicativo.
  O objetivo é tornar algumas das vantagens do Cosmos DB ainda mais acessíveis para os desenvolvedores de .NET, tais como a distribuição global, disponibilidade "always on", escalabilidade elástica e baixa latência.
  O provedor ativará a maioria dos recursos do EF Core, como controle automático de alterações, LINQ e conversões de valores, em relação à API do SQL no Cosmos DB. Iniciamos este trabalho antes do EF Core 2.2 e [fizemos algumas versões prévias do provedor disponível](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  O novo plano é continuar desenvolvendo o provedor junto com o EF Core 3.0.   

- **Suporte do C# 8.0 ([#12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))**: Queremos que nossos clientes aproveitem alguns dos [novos recursos que virão no C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), como os fluxos assíncronos (incluindo await para cada um) e os tipos de referência nula durante o uso do EF Core.

- **Engenharia reversa dos modos de exibição de banco de dados em tipos de consulta ([#1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))**: No EF Core 2.1, adicionamos suporte para tipos de consulta que podem representar dados que podem ser lidos no banco de dados, mas que talvez não estejam atualizados.
  Os tipos de consultas são uma excelente opção para os modos de exibição do banco de dados de mapeamento. Assim, no EF Core 3.0, queremos automatizar a criação de tipos de consulta para os modos de exibição de banco de dados.

- **Entidades de recipiente de propriedade ([#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) e [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))**: Este recurso trata da habilitação das entidades que armazenam dados em propriedades indexadas em vez de propriedades regulares e também da capacidade de usar as instâncias da mesma classe do .NET (potencialmente algo tão simples quanto um `Dictionary<string, object>`) para representar tipos de entidades diferentes no mesmo modelo do EF Core.
  Este recurso é uma etapa fundamental para proporcionar suporte a relacionamentos de muitos para muitos sem uma entidade de junção, que é um dos aprimoramentos mais solicitados no EF Core.

- **EF 6.3 no .NET Core ([EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271))**: Entendemos que muitos aplicativos existentes usam versões anteriores do EF e que a portabilidade deles para o EF Core somente para aproveitar o .NET Core pode exigir um esforço significativo.
  Por esse motivo, adaptaremos a próxima versão do EF 6 para execução no .NET Core 3.0.
  Faremos isso para facilitar a portabilidade de aplicativos existentes com alterações mínimas.
  Haverá algumas limitações (por exemplo, ele exigirá novos provedores, o suporte espacial com o SQL Server não será habilitado) e não haverá novos recursos planejados no EF 6.

Enquanto isso, use [esta consulta no nosso rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) para ver os itens de trabalho atribuídos provisoriamente à versão 3.0.

## <a name="schedule"></a>Agendamento

O agendamento do EF Core está sincronizado com o do [.NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) e o do [ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

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
