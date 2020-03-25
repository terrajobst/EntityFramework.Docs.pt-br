---
title: Planejar para o Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136233"
---
# <a name="plan-for-entity-framework-core-50"></a>Planejar para o Entity Framework Core 5,0

Conforme descrito no [processo de planejamento](../release-planning.md), reunimos a entrada dos participantes em um plano provisório para a versão EF Core 5,0.

> [!IMPORTANT] 
> Esse plano ainda é um trabalho em andamento. Nada aqui é um compromisso. Este plano é um ponto de partida que evoluirá à medida que aprendemos mais. Algumas coisas que não estão atualmente planejadas para 5,0 podem ser retiradas. Algumas coisas atualmente planejadas para 5,0 podem ser desativadas.

### <a name="version-number-and-release-date"></a>Número de versão e data de lançamento.

O EF Core 5,0 está atualmente agendado para lançamento ao [mesmo tempo que o .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/). A versão "5,0" foi escolhida para se alinhar com o .NET 5,0.

### <a name="supported-platforms"></a>Plataformas compatíveis

EF Core 5,0 está planejado para ser executado em qualquer plataforma .NET 5,0 com base na [convergência dessas plataformas para o .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/). O que isso significa em termos de .NET Standard e o TFM real usado ainda é TBD.

EF Core 5,0 não será executado em .NET Framework.

### <a name="breaking-changes"></a>Alterações de quebra

EF Core 5,0 conterá algumas alterações significativas, mas elas serão muito menos graves do que o caso para EF Core 3,0. Nossa meta é permitir que a grande maioria dos aplicativos seja atualizada sem interrupções.

Espera-se que haja algumas alterações significativas para provedores de banco de dados, especialmente em relação ao suporte a TPT. No entanto, esperamos que o trabalho para atualizar um provedor para 5,0 seja menor que o necessário para atualizar para 3,0.

## <a name="themes"></a>Temas

Extraímos algumas áreas ou temas principais que formam a base dos grandes investimentos em EF Core 5,0.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Propriedades de navegação muitos para muitos (a. k. a "ignorar navegações")

Desenvolvedores potenciais: @smitpatel e @AndriySvyryd

Acompanhado por [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

Tamanho de camiseta: L

Status: em andamento

Muitos-para-muitos é o [recurso mais solicitado](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (aproximadamente 407 votos) na pendência do github.

O suporte para relações muitos para muitos em sua totalidade é acompanhado como [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508). Isso pode ser dividido em três áreas principais:

* Ignorar Propriedades de navegação. Eles permitem que o modelo seja usado para consultas, etc. sem referência à entidade de tabela de junção subjacente. ([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))
* Tipos de entidade de conjunto de propriedades. Eles permitem que um tipo CLR padrão (por exemplo, `Dictionary`) seja usado para instâncias de entidade, de modo que um tipo CLR explícito não seja necessário para cada tipo de entidade. (Stretch para 5,0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)
* Para facilitar a configuração de relações muitos para muitos. (Stretch para 5,0.)

Acreditamos que o bloqueador mais significativo para aqueles que desejam suporte muitos para muitos não é capaz de usar as relações "naturais", sem referir-se à tabela de junção, em lógica de negócios, como consultas. O tipo de entidade da tabela de junção ainda pode existir, mas não deve ficar na forma de lógica de negócios. É por isso que optamos por lidar com as propriedades de navegação Skip para 5,0.

Neste momento, as outras partes de muitos para muitos estão sendo buscadas como uma meta de ampliação para o EF Core 5,0. Isso significa que eles não estão atualmente no plano de 5,0, mas se as coisas estiverem bem, esperamos poder extraí-los.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Mapeamento de herança de tabela por tipo (TPT)

Desenvolvedor líder: @AndriySvyryd

Acompanhado por [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

Tamanho de camiseta: XL

Status: em andamento

Estamos fazendo TPT porque ele é um recurso altamente solicitado (aproximadamente 254 votos; terceiro geral) e porque requer algumas alterações de baixo nível que sentem que são apropriadas para a natureza fundamental do plano geral do .NET 5. Esperamos que isso resulte em alterações significativas para provedores de banco de dados, embora eles devam ser muito menos graves do que as alterações necessárias para 3,0.

## <a name="filtered-include"></a>Inclusão filtrada

Desenvolvedor líder: @maumar

Acompanhado por [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

Tamanho de camiseta: M

Status: em andamento

A inclusão filtrada é um recurso altamente solicitado (aproximadamente 317 votos; 2º geral) que não é uma grande quantidade de trabalho, e que acreditamos que desbloqueiaremos ou facilitaremos muitos cenários que atualmente exigem filtros no nível de modelo ou consultas mais complexas.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Racionalizar totabela, toconsulta, toView, das, etc.

Desenvolvedores potenciais: @maumar e @smitpatel

Acompanhado por [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

Tamanho de camiseta: L

Status: em andamento

Fizemos o progresso nas versões anteriores para oferecer suporte a SQL bruto, tipos sem formatação e áreas relacionadas. No entanto, há lacunas e inconsistências na maneira como tudo funciona em conjunto como um todo. O objetivo do 5,0 é corrigi-los e criar uma boa experiência para definir, migrar e usar diferentes tipos de entidades e suas consultas associadas e artefatos de banco de dados. Isso também pode envolver atualizações na API de consulta compilada.

Observe que esse item pode resultar em algumas alterações significativas no nível do aplicativo, pois algumas das funcionalidades que temos atualmente são muito permissivas, de modo que isso pode levar rapidamente as pessoas a Pits de falha. Provavelmente, acabaremos bloqueando algumas dessas funcionalidades com diretrizes sobre o que fazer.

## <a name="general-query-enhancements"></a>Aprimoramentos de consulta geral

Desenvolvedores potenciais: @smitpatel e @maumar

Acompanhado por [problemas rotulados com `area-query` no marco 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

Tamanho de camiseta: XL

Status: em andamento

O código de conversão de consulta foi reescrito extensivamente para EF Core 3,0. O código de consulta geralmente está em um estado muito mais robusto por causa disso. Para 5,0, não estamos planejando fazer alterações de consulta principais, fora das necessárias para dar suporte a TPT e ignorar Propriedades de navegação. No entanto, ainda há um trabalho significativo necessário para corrigir alguma dívida técnica à esquerda da revisão 3,0. Também planejamos corrigir muitos bugs e implementar pequenos aprimoramentos para melhorar ainda mais a experiência de consulta geral.

## <a name="migrations-and-deployment-experience"></a>Migração e experiência de implantação

Desenvolvedores potenciais: @bricelam

Acompanhado por [#19587](https://github.com/dotnet/efcore/issues/19587)

Tamanho de camiseta: L

Status: em andamento

Atualmente, muitos desenvolvedores migram seus bancos de dados no momento da inicialização do aplicativo. Isso é fácil, mas não é recomendado porque:

* Vários threads/processos/servidores podem tentar migrar o banco de dados simultaneamente
* Os aplicativos podem tentar acessar o estado inconsistente enquanto isso acontece
* Geralmente, as permissões de banco de dados para modificar o esquema não devem ser concedidas para a execução do aplicativo
* É difícil reverter para um estado limpo se algo der errado

Queremos fornecer uma melhor experiência aqui que permite uma maneira fácil de migrar o banco de dados no momento da implantação. Isso deve:

* Trabalhar em Linux, Mac e Windows
* Seja uma boa experiência na linha de comando
* Cenários de suporte com contêineres
* Trabalhe com fluxos/ferramentas de implantação do mundo real usados com frequência
* Integre em pelo menos o Visual Studio

É provável que o resultado seja muitas pequenas melhorias no EF Core (por exemplo, melhores migrações no SQLite), juntamente com orientações e colaborações de longo prazo com outras equipes para melhorar experiências de ponta a ponta que vão além do EF.

## <a name="ef-core-platforms-experience"></a>Experiência em plataformas EF Core 

Desenvolvedores potenciais: @roji e @bricelam

Acompanhado por [#19588](https://github.com/dotnet/efcore/issues/19588)

Tamanho de camiseta: L

Status: não iniciado

Temos uma boa orientação para usar EF Core em aplicativos Web tradicionais como o MVC. A orientação para outras plataformas e modelos de aplicativos está ausente ou desatualizado. Para EF Core 5,0, planejamos investigar, melhorar e documentar a experiência de usar o EF Core com:

* Blazor
* Xamarin, incluindo o uso da história de AOT/vinculador
* WinForms/WPF/WinUI e possivelmente outros U.I. estruturas

É provável que isso seja muitas pequenas melhorias no EF Core, juntamente com orientações e colaborações de longo prazo com outras equipes para melhorar experiências de ponta a ponta que vão além do EF.

As áreas específicas que planejamos examinar são:

* Implantação, incluindo a experiência de usar ferramentas do EF, como para migrações
* Modelos de aplicativos, incluindo Xamarin e mais ou mais, e provavelmente outros
* Experiências do SQLite, incluindo a experiência espacial e recompilações de tabela
* Experiências de AOT e de vinculação
* Integração de diagnóstico, incluindo contadores de desempenho

## <a name="performance"></a>Desempenho

Desenvolvedor líder: @roji

Acompanhado por [problemas rotulados com `area-perf` no marco 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

Tamanho de camiseta: L

Status: em andamento

Por EF Core, planejamos aprimorar nosso conjunto de benchmarks de desempenho e fazer melhorias de desempenho direcionadas para o tempo de execução. Além disso, planejamos concluir a nova API de envio em lote ADO.NET que foi protótipo durante o ciclo de lançamento 3,0. Além disso, na camada ADO.NET, planejamos aprimoramentos de desempenho adicionais para o provedor Npgsql.

Como parte desse trabalho, também planejamos adicionar contadores de desempenho do ADO.NET/EF Core e outros diagnósticos, conforme apropriado.

## <a name="architecturalcontributor-documentation"></a>Documentação de arquitetura/colaborador

Documentador de leads: @ajcvickers

Acompanhado por [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)

Tamanho de camiseta: L

Status: em andamento

A ideia aqui é tornar mais fácil entender o que está acontecendo nos elementos internos de EF Core. Isso pode ser útil para qualquer pessoa que esteja usando EF Core, mas a principal motivação é tornar mais fácil para as pessoas externas:

* Contribuir com o código de EF Core
* Criar provedores de banco de dados
* Criar outras extensões

## <a name="microsoftdatasqlite-documentation"></a>Documentação do Microsoft. Data. sqlite

Documentador de leads: @bricelam

Acompanhado por [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)

Tamanho de camiseta: M

Status: concluído. A nova documentação está ativa [no site do Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).

A equipe do EF também possui o provedor de ADO.NET Microsoft. Data. sqlite. Planejamos documentar completamente esse provedor como parte da versão 5,0.

## <a name="general-documentation"></a>Documentação geral

Documentador de leads: @ajcvickers

Acompanhado por [problemas no repositório de documentos no marco 5,0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

Tamanho de camiseta: L

Status: em andamento

Já estamos no processo de atualização de documentação para as versões 3,0 e 3,1. Também estamos trabalhando em:
  * Uma revisão dos documentos de introdução para torná-los mais abordáveis/fáceis de acompanhar
  * Reorganização de docs para tornar as coisas mais fáceis de localizar e adicionar referências cruzadas
  * Adicionando mais detalhes e esclarecimentos aos documentos existentes
  * Atualizando os exemplos e adicionando mais exemplos

## <a name="fixing-bugs"></a>Corrigindo bugs

Acompanhado por [problemas rotulados com `type-bug` no marco 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

Desenvolvedores: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

Tamanho de camiseta: L

Status: em andamento

No momento da elaboração do artigo, temos 135 bugs desclassificados para serem corrigidos na versão 5,0 (com 62 já corrigido), mas há uma sobreposição significativa com a seção de _aprimoramentos gerais de consulta_ acima.

A taxa de entrada (problemas que terminam como trabalho em uma etapa) foi de cerca de 23 problemas por mês ao longo da versão 3,0. Nem todos eles precisarão ser corrigidos em 5,0. Como uma estimativa aproximada, planejamos corrigir um adicional 150 problemas no período de 5,0.

## <a name="small-enhancements"></a>Pequenos aprimoramentos

Acompanhado por [problemas rotulados com `type-enhancement` no marco 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

Desenvolvedores: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

Tamanho de camiseta: L

Status: em andamento

Além dos maiores recursos descritos acima, também temos muitas melhorias menores agendadas para 5,0 para corrigir "cortes de papel". Observe que muitos desses aprimoramentos também são cobertos pelos temas mais gerais descritos acima.

## <a name="below-the-line"></a>Abaixo-a-linha

Acompanhado por [problemas rotulados com `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

Essas são correções de bugs e aprimoramentos que **não** estão atualmente agendados para a versão 5,0, mas veremos como metas de ampliação, dependendo do progresso feito no trabalho acima.

Além disso, sempre consideramos os [problemas mais votados](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) durante o planejamento. A redução de qualquer um desses problemas de uma versão é sempre difícil, mas precisamos de um plano realista para os recursos que temos.

## <a name="feedback"></a>Comentários

Seus comentários sobre o planejamento são importantes. A melhor maneira de indicar a importância de um problema é votar (polegar para cima) nesse problema no GitHub. Esses dados serão então alimentados no [processo de planejamento](../release-planning.md) para a próxima versão.
