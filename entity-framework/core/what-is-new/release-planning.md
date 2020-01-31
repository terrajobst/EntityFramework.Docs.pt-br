---
title: Planejamento de versão EF Core
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888062"
---
# <a name="release-planning-process"></a>Processo de planejamento da versão

Frequentemente, recebemos perguntas sobre como escolhemos os recursos específicos de uma versão.
Este documento descreve o processo que usamos.
O processo está evoluindo continuamente, pois encontramos melhores maneiras de planejar, mas as ideias gerais permanecem as mesmas.

## <a name="different-kinds-of-releases"></a>Diferentes tipos de versões

Tipos diferentes de versão contêm tipos diferentes de alterações.
Isso, por sua vez, significa que o planejamento da versão é diferente para diferentes tipos de versão.

### <a name="patch-releases"></a>Versões de patch

As versões de patch alteram apenas a parte "patch" da versão.
Por exemplo, EF Core 3,1. **1** é uma versão que corrige os problemas encontrados no EF Core 3,1. **0**.

As versões de patch são destinadas a corrigir bugs críticos do cliente.
Isso significa que as versões de patch não incluem novos recursos.
As alterações de API não são permitidas em versões de patch, exceto em circunstâncias especiais.

A barra para fazer uma alteração em uma versão de patch é muito alta.
Isso ocorre porque é essencial que as versões de patch não introduzam novos bugs.
Portanto, o processo de decisão enfatiza alto valor e baixo risco.

É mais provável que você corrija um problema se:
  * Ele está afetando vários clientes
  * É uma regressão de uma versão anterior
  * A falha causa corrupção de dados

É menos provável que você corrija um problema se:
  * Há soluções alternativas razoáveis
  * A correção tem alto risco de quebrar algo mais
  * O bug está em um caso de canto

Essa barra aumenta gradualmente durante o tempo de vida de uma versão de [LTS (suporte a longo prazo)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) . Isso ocorre porque as versões do LTS enfatizam a estabilidade.

A decisão final sobre se é ou não corrigir um problema é feita pelos directors do .NET na Microsoft.

### <a name="minor-releases"></a>Versões secundárias

As versões secundárias alteram apenas a parte "secundária" da versão.
Por exemplo, EF Core 3. **1**0 é uma versão que melhora o EF Core 3. **0**. 0.

Versões secundárias:
* Destina-se a melhorar a qualidade e os recursos da versão anterior
* Normalmente contêm correções de bugs e novos recursos
* Não incluir alterações de quebra intencional
* Ter algumas visualizações de pré-lançamento enviadas por push para o NuGet

### <a name="major-releases"></a>Versões principais

As versões principais alteram o número de versão "principal" do EF.
Por exemplo, EF Core **3**. 0,0 é uma versão principal que faz um grande avanço sobre EF Core 2.2. x.

Principais versões:
* Destina-se a melhorar a qualidade e os recursos da versão anterior
* Normalmente contêm correções de bugs e novos recursos
  * Alguns dos novos recursos podem ser alterações fundamentais na maneira como EF Core funciona
* Normalmente incluem alterações de quebra intencional
  * Alterações significativas são parte necessária do desenvolvimento de EF Core à medida que aprendemos
  * No entanto, acreditamos muito cuidadosamente sobre como fazer qualquer alteração significativa devido ao possível impacto do cliente. Podemos ser muito agressivos com alterações significativas no passado. No futuro, nos esforçaremos para minimizar as alterações que interrompem os aplicativos e para reduzir as alterações que interrompem os provedores de banco de dados e as extensões.
* Ter muitas visualizações de pré-lançamento enviadas por push para o NuGet

## <a name="planning-for-majorminor-releases"></a>Planejando versões principais/secundárias

### <a name="github-issue-tracking"></a>Acompanhamento de problemas do GitHub

O GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) é a fonte de verdade para todo o planejamento de EF Core.

Os problemas no GitHub têm:

* Um estado
  * Problemas em [aberto](https://github.com/dotnet/efcore/issues) não foram resolvidos.
  * Problemas [fechados](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) foram resolvidos.
    * Todos os problemas que foram corrigidos são [marcados com fixo fechado](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed). Um problema marcado com fixo fechado é fixo e mesclado, mas pode não ter sido liberado.
    * Outros rótulos de `closed-` indicam outros motivos para fechar um problema. Por exemplo, as duplicatas são marcadas com cópia fechada.
* Um tipo
  * [Bugs](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) representam bugs.
  * Os [aprimoramentos](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) representam novos recursos ou uma funcionalidade melhor nos recursos existentes.
* Uma etapa
  * [Problemas sem nenhuma etapa](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) estão sendo considerados pela equipe. A decisão sobre o que fazer com o problema ainda não foi feita ou uma alteração na decisão está sendo considerada.
  * Os [problemas na etapa da lista de pendências](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) são itens que a equipe do EF considerará trabalhando em uma versão futura
    * Os problemas na lista de pendências podem ser [marcados com o considere-for-next-release](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) , indicando que esse item de trabalho está no nível superior para a próxima versão.
  * Os problemas em aberto em uma etapa de versão são itens nos quais a equipe planeja trabalhar nessa versão. Por exemplo, [esses são os problemas que pretendemos trabalhar para EF Core 5,0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * Problemas fechados em um marco com versão são problemas que são concluídos para essa versão. Observe que a versão pode ainda não ter sido liberada. Por exemplo, [estes são os problemas concluídos para o EF Core 3,0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* Votos!
  * A votação é a melhor maneira de indicar que um problema é importante para você.
  * Para votar, basta adicionar um 👍 "thumbs" ao problema. Por exemplo, [esses são os problemas mais votados](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * Além disso, comente sobre os motivos específicos que você precisa do recurso se sentir que isso agrega valor. Comentar "+ 1" ou semelhante não adiciona valor.

### <a name="the-planning-process"></a>O processo de planejamento

O processo de planejamento é mais envolvido do que apenas pegar os recursos solicitados mais importantes da lista de pendências.
Isso ocorre porque coletamos comentários de vários participantes de várias maneiras.
Em seguida, moldamos uma versão com base em:

* Entrada de clientes
* Entrada de outros participantes
* Direção estratégica
* Recursos disponíveis
* Agenda

Algumas das perguntas que pedimos são:

1. **Quantos desenvolvedores achamos que usarão o recurso? Como ele vai aprimorar os aplicativos ou as experiência deles?** Para responder a essa pergunta, podemos coletar comentários de várias fontes – comentários e votos em problemas é uma dessas fontes. Contratos específicos com clientes importantes é outro.

2. **Quais são as soluções alternativas que podem ser usadas caso não implementemos este recurso?** Por exemplo, diversos desenvolvedores podem mapear uma tabela de junção para solucionar a falta de suporte nativo de muitos para muitos. Obviamente, nem todos os desenvolvedores desejam fazê-lo, mas muitos podem, e isso conta como um fator em nossa decisão.

3. **Implementar este recurso desenvolve a arquitetura do EF Core de forma que nos aproxime da implementação de outros recursos?** Favorecemos os recursos que atuam como blocos de construção para outros recursos. Por exemplo, entidades de recipiente da propriedades podem nos ajudar a mover na direção do suporte de muitos-para-muitos, e construtores de entidade habilitaram o nosso suporte a carregamento lento.

4. **O recurso é um ponto de extensibilidade?** Favorecemos os pontos de extensibilidade em relação aos recursos normais, pois eles permitem aos desenvolvedores conectar seus próprios comportamentos e compensar qualquer funcionalidade ausente.

5. **O que é a sinergia do recurso quando usado em combinação com outros produtos?** Favorecemos recursos que habilitam ou melhoram significativamente a experiência de usar o EF Core com outros produtos, como .NET Core, a versão mais recente do Visual Studio, Microsoft Azure e assim por diante.

6. **Quais são as habilidades das pessoas disponíveis para trabalhar em um recurso e como podemos melhor utilizar esses recursos?** Cada membro da equipe do EF e os colaboradores da comunidade têm diferentes níveis de experiência em áreas distintas, portanto, temos que planejar adequadamente. Mesmo se quiséssemos que todos se empenhassem em trabalhar em um recurso específico, como as traduções do GroupBy ou muitos para muitos, isso não seria prático.
