---
title: Planejamento de liberação do EF Core
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417332"
---
# <a name="release-planning-process"></a>Processo de planejamento da versão

Frequentemente, recebemos perguntas sobre como escolhemos os recursos específicos de uma versão.
Este documento descreve o processo que usamos.
O processo está evoluindo continuamente à medida que encontramos melhores maneiras de planejar, mas as idéias gerais permanecem as mesmas.

## <a name="different-kinds-of-releases"></a>Diferentes tipos de lançamentos

Diferentes tipos de liberação contêm diferentes tipos de alterações.
Isso, por sua vez, significa que o planejamento de lançamento é diferente para diferentes tipos de lançamento.

### <a name="patch-releases"></a>Versões de patch

Os lançamentos de patch alteram apenas a parte "patch" da versão.
Por exemplo, EF Core 3.1. **1** é uma versão que corrige problemas encontrados no EF Core 3.1. **0**.

As versões de patch destinam-se a corrigir erros críticos do cliente.
Isso significa que as versões de patch não incluem novos recursos.
Alterações de API não são permitidas em versões de patch, exceto em circunstâncias especiais.

A barra para fazer uma mudança em uma versão de patch é muito alta.
Isso porque é fundamental que as versões de patch não introduzam novos bugs.
Portanto, o processo de decisão enfatiza alto valor e baixo risco.

É mais provável que corrigimos um problema se:
  * Está impactando vários clientes
  * É uma regressão de uma versão anterior
  * A falha causa corrupção de dados

É menos provável que corrigimos um problema se:
  * Existem alternativas razoáveis
  * A correção tem alto risco de quebrar outra coisa
  * O inseto está em um caso de canto

Esta barra sobe gradualmente durante a vida de uma liberação [de suporte de longo prazo (LTS).](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) Isso porque as versões LTS enfatizam a estabilidade.

A decisão final sobre a correção ou não de um problema é tomada pelos diretores da .NET na Microsoft.

### <a name="minor-releases"></a>Lançamentos menores

Lançamentos menores alteram apenas a parte "menor" da versão.
Por exemplo, EF Core 3. **1.0**é uma versão que melhora no EF Core 3. **0.0.**

Lançamentos menores:
* Destina-se a melhorar a qualidade e os recursos da versão anterior
* Normalmente contêm correções de bugs e novos recursos
* Não inclua mudanças intencionais de quebra
* Tenha algumas pré-visualizações de pré-lançamento empurradas para NuGet

### <a name="major-releases"></a>Principais lançamentos

Os principais lançamentos alteram o número da versão "principal" da EF.
Por exemplo, o EF Core **3**.0.0 é uma grande versão que dá um grande passo em frente sobre o EF Core 2.2.x.

Principais lançamentos:
* Destina-se a melhorar a qualidade e os recursos da versão anterior
* Normalmente contêm correções de bugs e novos recursos
  * Alguns dos novos recursos podem ser mudanças fundamentais na forma como o EF Core funciona
* Normalmente incluem mudanças intencionais de quebra
  * Mudanças de ruptura são necessárias parte da evolução do Núcleo EF à medida que aprendemos
  * No entanto, pensamos com muito cuidado em fazer qualquer mudança de quebra devido ao potencial impacto do cliente. Podemos ter sido muito agressivos com mudanças no passado. Daqui para frente, nos esforçaremos para minimizar as alterações que quebram aplicativos e reduzir as alterações que quebram provedores de banco de dados e extensões.
* Tenha muitas pré-visualizações de pré-lançamento empurradas para NuGet

## <a name="planning-for-majorminor-releases"></a>Planejamento para lançamentos maiores/menores

### <a name="github-issue-tracking"></a>Rastreamento de problemas do GitHub

GitHub[https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)( ) é a fonte da verdade para todo o planejamento do EF Core.

Os problemas no GitHub têm:

* Um estado
  * [Questões abertas](https://github.com/dotnet/efcore/issues) não foram abordadas.
  * [Questões fechadas](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) foram abordadas.
    * Todos os problemas corrigidos são [marcados com fixação fechada](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed). Um problema marcado com fixo fechado é corrigido e mesclado, mas pode não ter sido liberado.
    * Outras `closed-` etiquetas indicam outras razões para fechar um problema. Por exemplo, as duplicatas são marcadas com duplicata fechada.
* Um tipo
  * [Bugs](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) representam insetos.
  * [Os aprimoramentos](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) representam novos recursos ou melhor funcionalidade nos recursos existentes.
* Um marco
  * [Problemas sem marco](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) estão sendo considerados pela equipe. A decisão sobre o que fazer com o assunto ainda não foi tomada ou uma mudança na decisão está sendo considerada.
  * [Problemas no marco do Backlog](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) são itens que a equipe da EF considerará trabalhar em um lançamento futuro
    * Problemas no backlog podem ser [marcados com a próxima versão](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) indicando que este item de trabalho está no topo da lista para a próxima versão.
  * Problemas abertos em um marco em versão são itens que a equipe planeja trabalhar nessa versão. Por exemplo, [estes são os problemas que planejamos trabalhar para o EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * Problemas fechados em um marco em versão são questões que são concluídas para essa versão. Note que a versão pode ainda não ter sido lançada. Por exemplo, [estes são os problemas concluídos para o EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* Votos!
  * Votar é a melhor maneira de indicar que um assunto é importante para você.
  * Para votar, basta adicionar um "polegar para cima" 👍 à questão. Por exemplo, [essas são as questões mais votadas](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * Por favor, comente também descrevendo razões específicas que você precisa do recurso se você sentir que isso agrega valor. Comentar "+1" ou similar não agrega valor.

### <a name="the-planning-process"></a>O processo de planejamento

O processo de planejamento está mais envolvido do que apenas tirar os recursos mais solicitados do backlog.
Isso porque reunimos feedback de várias partes interessadas de várias maneiras.
Em seguida, moldamos uma versão baseada em:

* Entrada de clientes
* Entrada de outras partes interessadas
* Direção estratégica
* Recursos disponíveis
* Agenda

Algumas das perguntas que fazemos são:

1. **Quantos desenvolvedores achamos que usarão o recurso? Como ele vai aprimorar os aplicativos ou as experiência deles?** Para responder a essa pergunta, podemos coletar comentários de várias fontes – comentários e votos em problemas é uma dessas fontes. Compromissos específicos com clientes importantes é outra.

2. **Quais são as soluçãos que as pessoas podem usar se ainda não implementarmos esse recurso?** Por exemplo, diversos desenvolvedores podem mapear uma tabela de junção para solucionar a falta de suporte nativo de muitos para muitos. Obviamente, nem todos os desenvolvedores desejam fazê-lo, mas muitos podem, e isso conta como um fator em nossa decisão.

3. **Implementar este recurso desenvolve a arquitetura do EF Core de forma que nos aproxime da implementação de outros recursos?** Favorecemos os recursos que atuam como blocos de construção para outros recursos. Por exemplo, entidades de recipiente da propriedades podem nos ajudar a mover na direção do suporte de muitos-para-muitos, e construtores de entidade habilitaram o nosso suporte a carregamento lento.

4. **O recurso é um ponto de extensibilidade?** Favorecemos os pontos de extensibilidade em relação aos recursos normais, pois eles permitem aos desenvolvedores conectar seus próprios comportamentos e compensar qualquer funcionalidade ausente.

5. **O que é a sinergia do recurso quando usado em combinação com outros produtos?** Favorecemos recursos que habilitam ou melhoram significativamente a experiência de usar o EF Core com outros produtos, como .NET Core, a versão mais recente do Visual Studio, Microsoft Azure e assim por diante.

6. **Quais são as habilidades das pessoas disponíveis para trabalhar em um recurso, e como podemos aproveitar melhor esses recursos?** Cada membro da equipe do EF e os colaboradores da comunidade têm diferentes níveis de experiência em áreas distintas, portanto, temos que planejar adequadamente. Mesmo se quiséssemos que todos se empenhassem em trabalhar em um recurso específico, como as traduções do GroupBy ou muitos para muitos, isso não seria prático.
