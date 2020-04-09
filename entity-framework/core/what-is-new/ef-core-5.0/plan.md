---
title: Plano para o Núcleo-Quadro de Entidades 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136233"
---
# <a name="plan-for-entity-framework-core-50"></a>Plano para o Núcleo-Quadro de Entidades 5.0

Como descrito no [processo de planejamento,](../release-planning.md)reunimos contribuições das partes interessadas em um plano provisório para a versão do EF Core 5.0.

> [!IMPORTANT] 
> Este plano ainda é um trabalho em andamento. Nada aqui é um compromisso. Este plano é um ponto de partida que evoluirá à medida que aprendermos mais. Algumas coisas não planejadas para 5.0 podem ser puxadas para dentro. Algumas coisas atualmente planejadas para 5.0 podem ser pontuadas.

### <a name="version-number-and-release-date"></a>Número da versão e data de lançamento.

O EF Core 5.0 está atualmente programado para ser lançado [ao mesmo tempo que .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/). A versão "5.0" foi escolhida para alinhar com .NET 5.0.

### <a name="supported-platforms"></a>Plataformas compatíveis

O EF Core 5.0 está planejado para ser executado em qualquer plataforma .NET 5.0 com base na [convergência dessas plataformas para .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/). O que isso significa em termos de .NET Standard e o TFM real usado ainda é TBD.

O EF Core 5.0 não será executado no .NET Framework.

### <a name="breaking-changes"></a>Alterações de quebra

O EF Core 5.0 conterá algumas alterações de ruptura, mas estas serão muito menos graves do que foi o caso do EF Core 3.0. Nosso objetivo é permitir que a grande maioria dos aplicativos se atualize sem quebrar.

Espera-se que haja algumas mudanças de quebra para os provedores de banco de dados, especialmente em torno do suporte ao TPT. No entanto, esperamos que o trabalho para atualizar um provedor para 5.0 será menor do que o necessário para atualizar para 3.0.

## <a name="themes"></a>Temas

Extraímos algumas áreas ou temas importantes que formarão a base para os grandes investimentos no Núcleo 5.0 da EF.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Muitas a muitas propriedades de navegação (também conhecido como "skip navigations")

Desenvolvedores líderes: @smitpatel e@AndriySvyryd

Rastreado por [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

Tamanho da camiseta: L

Status: Em andamento

Muitos para muitos é o [recurso mais solicitado](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votos) no backlog do GitHub.

O apoio a muitos relacionamentos em sua totalidade é acompanhado como [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508). Isso pode ser dividido em três grandes áreas:

* Pular propriedades de navegação. Estes permitem que o modelo seja usado para consultas, etc. sem referência à entidade de tabela de adesão subjacente. [(#19003)](https://github.com/aspnet/EntityFrameworkCore/issues/19003)
* Tipos de entidade sacolas de propriedade. Estes permitem que um tipo padrão `Dictionary`de CLR (por exemplo) seja usado para instâncias de entidade, de tal forma que um tipo de CLR explícito não seja necessário para cada tipo de entidade. (Alongamento para 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)
* Açúcar para fácil configuração de muitos a muitos relacionamentos. (Estique para 5.0.)

Acreditamos que o bloqueador mais significativo para aqueles que querem apoio de muitos a muitos é não poder usar as relações "naturais", sem se referir à mesa de adesão, na lógica empresarial, como consultas. O tipo de entidade de mesa de adesão ainda pode existir, mas não deve atrapalhar a lógica dos negócios. É por isso que optamos por enfrentar as propriedades de navegação skip para 5.0.

Neste momento, as outras partes de muitos a muitos estão sendo perseguidas como uma meta de estiramento para o EF Core 5.0. Isso significa que eles não estão no plano para 5.0, mas se as coisas vão bem, esperamos puxá-los para dentro.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Mapeamento de herança tabela por tipo (TPT)

Desenvolvedor líder:@AndriySvyryd

Rastreado por [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

Tamanho da camiseta: XL

Status: Em andamento

Estamos fazendo TPT porque é um recurso altamente solicitado (~254 votos; 3º no geral) e porque requer algumas mudanças de baixo nível que achamos apropriadas para a natureza fundamental do plano global .NET 5. Esperamos que isso resulte em mudanças para os provedores de banco de dados, embora estas devam ser muito menos severas do que as mudanças necessárias para o 3.0.

## <a name="filtered-include"></a>Inclusão filtrada

Desenvolvedor líder:@maumar

Rastreado por [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

Tamanho da camiseta: M

Status: Em andamento

Filtered Include é um recurso altamente solicitado (~317 votos; 2º no geral) que não é uma enorme quantidade de trabalho, e que acreditamos que irá desbloquear ou facilitar muitos cenários que atualmente exigem filtros de nível de modelo ou consultas mais complexas.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Racionalize ToTable, ToQuery, ToView, FromSql, etc.

Desenvolvedores líderes: @maumar e@smitpatel

Rastreado por [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

Tamanho da camiseta: L

Status: Em andamento

Fizemos progressos em versões anteriores para apoiar sql bruto, tipos sem chave e áreas relacionadas. No entanto, há lacunas e inconsistências na forma como tudo funciona em conjunto como um todo. O objetivo do 5.0 é corrigi-los e criar uma boa experiência para definir, migrar e usar diferentes tipos de entidades e suas consultas associadas e artefatos de banco de dados. Isso também pode envolver atualizações na API de consulta compilada.

Observe que este item pode resultar em algumas mudanças no nível do aplicativo, uma vez que algumas das funcionalidades que temos atualmente são muito permissivas de modo que pode rapidamente levar as pessoas a poços de falha. Provavelmente acabaremos bloqueando algumas dessas funcionalidades juntamente com orientações sobre o que fazer em vez disso.

## <a name="general-query-enhancements"></a>Aprimoramentos de consulta geral

Desenvolvedores líderes: @smitpatel e@maumar

Rastreado por [questões `area-query` rotuladas com o marco 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

Tamanho da camiseta: XL

Status: Em andamento

O código de tradução de consulta foi extensivamente reescrito para o EF Core 3.0. O código de consulta é geralmente em um estado muito mais robusto por causa disso. Para o 5.0 não estamos planejando fazer grandes alterações de consulta, fora aquelas necessárias para suportar o TPT e pular propriedades de navegação. No entanto, ainda há um trabalho significativo necessário para corrigir alguma dívida técnica que sobrou da revisão 3.0. Também planejamos corrigir muitos bugs e implementar pequenos aprimoramentos para melhorar ainda mais a experiência geral de consulta.

## <a name="migrations-and-deployment-experience"></a>Migrações e experiência de implantação

Desenvolvedores líderes:@bricelam

Rastreado por [#19587](https://github.com/dotnet/efcore/issues/19587)

Tamanho da camiseta: L

Status: Em andamento

Atualmente, muitos desenvolvedores migram seus bancos de dados na hora da inicialização do aplicativo. Isso é fácil, mas não é recomendado porque:

* Vários threads/processos/servidores podem tentar migrar o banco de dados simultaneamente
* Os aplicativos podem tentar acessar estado inconsistente enquanto isso está acontecendo
* Normalmente, as permissões do banco de dados para modificar o esquema não devem ser concedidas para execução de aplicativos
* É difícil voltar a um estado limpo se algo der errado.

Queremos oferecer uma experiência melhor aqui que permita uma maneira fácil de migrar o banco de dados no momento da implantação. Isso deve:

* Trabalhar em Linux, Mac e Windows
* Seja uma boa experiência na linha de comando
* Cenários de suporte com contêineres
* Trabalhe com ferramentas/fluxos de implantação do mundo real comumente usados
* Integre-se ao menos no Visual Studio

O resultado provavelmente será muitas pequenas melhorias no EF Core (por exemplo, melhores migrações no SQLite), juntamente com orientação e colaborações de longo prazo com outras equipes para melhorar experiências de ponta a ponta que vão além apenas da EF.

## <a name="ef-core-platforms-experience"></a>Experiência em plataformas EF Core 

Desenvolvedores líderes: @roji e@bricelam

Rastreado por [#19588](https://github.com/dotnet/efcore/issues/19588)

Tamanho da camiseta: L

Status: Não iniciado

Temos uma boa orientação para o uso do EF Core em aplicações web tradicionais semelhantes a MVC. A orientação para outras plataformas e modelos de aplicativos está ausente ou desatualizada. Para o EF Core 5.0, planejamos investigar, melhorar e documentar a experiência de usar o EF Core com:

* Blazor
* Xamarin, inclusive usando a história AOT/linker
* WinForms/WPF/WinUI e possivelmente outros U.I. estruturas

Isso provavelmente será muitas pequenas melhorias no EF Core, juntamente com orientação e colaborações de longo prazo com outras equipes para melhorar experiências de ponta a ponta que vão além apenas da EF.

Áreas específicas que planejamos analisar são:

* Implantação, incluindo a experiência para usar ferramentas EF, como para migrações
* Modelos de aplicação, incluindo Xamarin e Blazor, e provavelmente outros
* Experiências SQLite, incluindo a experiência espacial e reconstruções de mesa
* AOT e experiências de vinculação
* Integração de diagnósticos, incluindo contadores perf

## <a name="performance"></a>Desempenho

Desenvolvedor líder:@roji

Rastreado por [questões `area-perf` rotuladas com o marco 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

Tamanho da camiseta: L

Status: Em andamento

Para a EF Core, planejamos melhorar nosso conjunto de benchmarks de desempenho e fazer melhorias de desempenho direcionadas para o tempo de execução. Além disso, planejamos concluir a nova ADO.NET API de loteamento que foi protótipo durante o ciclo de lançamento 3.0. Também na camada ADO.NET, planejamos melhorias adicionais de desempenho para o provedor Npgsql.

Como parte deste trabalho, também planejamos adicionar ADO.NET/EF contadores de desempenho Core e outros diagnósticos conforme apropriado.

## <a name="architecturalcontributor-documentation"></a>Documentação arquitetônica/contribuinte

Documentador líder:@ajcvickers

Rastreado por [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)

Tamanho da camiseta: L

Status: Em andamento

A ideia aqui é facilitar a compreensão do que está acontecendo nos internos da EF Core. Isso pode ser útil para qualquer pessoa que use o EF Core, mas a principal motivação é tornar mais fácil para as pessoas externas:

* Contribua para o código EF Core
* Criar provedores de banco de dados
* Construa outras extensões

## <a name="microsoftdatasqlite-documentation"></a>Documentação do Microsoft.Data.Sqlite

Documentador líder:@bricelam

Rastreado por [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)

Tamanho da camiseta: M

Status: Concluído. A nova documentação está [ao vivo no site de documentos da Microsoft.](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli)

A Equipe EF também possui o provedor de ADO.NET Microsoft.Data.Sqlite. Planejamos documentar totalmente este provedor como parte da versão 5.0.

## <a name="general-documentation"></a>Documentação geral

Documentador líder:@ajcvickers

Rastreado por [problemas no repo docs na marca 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

Tamanho da camiseta: L

Status: Em andamento

Já estamos em processo de atualização da documentação para as versões 3.0 e 3.1. Também estamos trabalhando em:
  * Uma revisão dos docs iniciais para torná-los mais acessíveis/mais fáceis de seguir
  * Reorganização de docs para tornar as coisas mais fáceis de encontrar e adicionar referências cruzadas
  * Adicionando mais detalhes e esclarecimentos aos docs existentes
  * Atualizando as amostras e adicionando mais exemplos

## <a name="fixing-bugs"></a>Corrigindo bugs

Rastreado por [questões `type-bug` rotuladas com o marco 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

Desenvolvedores: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , ,@ajcvickers

Tamanho da camiseta: L

Status: Em andamento

No momento da escrita, temos 135 bugs triados para serem corrigidos na versão 5.0 (com 62 já _corrigidos),_ mas há uma sobreposição significativa com a seção de aprimoramentos de consulta geral acima.

A taxa de entrada (questões que acabam como trabalho em um marco) foi de cerca de 23 questões por mês ao longo da versão 3.0. Nem tudo isso precisará ser corrigido em 5.0. Como uma estimativa aproximada, planejamos corrigir 150 problemas adicionais no período de tempo 5.0.

## <a name="small-enhancements"></a>Pequenos aprimoramentos

Rastreado por [questões `type-enhancement` rotuladas com o marco 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

Desenvolvedores: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , ,@ajcvickers

Tamanho da camiseta: L

Status: Em andamento

Além das características maiores descritas acima, também temos muitas melhorias menores programadas para o 5.0 para corrigir "cortes de papel". Note que muitos desses aprimoramentos também são cobertos pelos temas mais gerais descritos acima.

## <a name="below-the-line"></a>Abaixo da linha

Rastreado por [questões `consider-for-next-release` rotuladas com](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

Estas são correções de bugs e melhorias que **não** estão programadas para a versão 5.0 no momento, mas vamos olhar como metas de alongamento dependendo do progresso feito no trabalho acima.

Além disso, sempre consideramos as [questões mais votadas](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) no planejamento. Cortar qualquer uma dessas questões de uma liberação é sempre doloroso, mas precisamos de um plano realista para os recursos que temos.

## <a name="feedback"></a>Comentários

Seus comentários sobre o planejamento são importantes. A melhor maneira de indicar a importância de um problema é votar (polegar para cima) nesse problema no GitHub. Esses dados serão então alimentados no [processo de planejamento](../release-planning.md) para a próxima versão.
