---
title: Log de alterações de impacto no provedor-EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: b911a2da493e20c4e4ce6f1e25024bd0efd38b44
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656123"
---
# <a name="provider-impacting-changes"></a>Alterações que afetam o provedor

Esta página contém links para solicitações de pull feitas no repositório de EF Core que podem exigir que autores de outros provedores de banco de dados reajam. A intenção é fornecer um ponto de partida para autores de provedores de banco de dados de terceiros existentes ao atualizar seu provedor para uma nova versão.

Estamos iniciando esse log com as alterações de 2,1 a 2,2. Antes de 2,1, usamos os rótulos [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) em nossos problemas e solicitações pull.

## <a name="22-----30"></a>2,2---> 3,0

Observe que muitas das [alterações significativas no nível do aplicativo](../what-is-new/ef-core-3.0/breaking-changes.md) também afetarão os provedores.

* <https://github.com/aspnet/EntityFrameworkCore/pull/14022>
  * APIs obsoletas removidas e sobrecargas de parâmetros opcionais recolhidas
  * DatabaseColumn. GetUnderlyingStoreType () removido
* <https://github.com/aspnet/EntityFrameworkCore/pull/14589>
  * APIs obsoletas removidas
* <https://github.com/aspnet/EntityFrameworkCore/pull/15044>
  * As subclasses de CharTypeMapping podem ter sido interrompidas devido a alterações de comportamento necessárias para corrigir alguns bugs na implementação de base.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15090>
  * Adicionou uma classe base para IDatabaseModelFactory e a atualizou para usar um objeto de parâmetros para atenuar as quebras futuras.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15123>
  * Objetos de parâmetro usados no MigrationsSqlGenerator para atenuar as quebras futuras.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14972>
  * A configuração explícita dos níveis de log exigia algumas alterações nas APIs que os provedores podem estar usando. Especificamente, se os provedores estiverem usando a infraestrutura de registro em log diretamente, essa alteração poderá interromper esse uso. Além disso, os provedores que usam a infraestrutura (que será público) continuarão a derivar de `LoggingDefinitions` ou `RelationalLoggingDefinitions`. Consulte os provedores de SQL Server e na memória para obter exemplos.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15091>
  * As cadeias de caracteres de recurso de núcleo, relacional e abstrações agora são públicas.
  * `CoreLoggerExtensions` e `RelationalLoggerExtensions` agora são públicas. Os provedores devem usar essas APIs ao registrar eventos que são definidos no nível principal ou relacional. Não acesse recursos de log diretamente; Eles ainda são internos.
  * `IRawSqlCommandBuilder` mudou de um serviço singleton para um serviço com escopo
  * `IMigrationsSqlGenerator` mudou de um serviço singleton para um serviço com escopo
* <https://github.com/aspnet/EntityFrameworkCore/pull/14706>
  * A infraestrutura para criar comandos relacionais tornou-se pública para que possa ser usada com segurança pelos provedores e ligeiramente Refatorada.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14733>
  * `ILazyLoader` mudou de um serviço com escopo para um serviço transitório
* <https://github.com/aspnet/EntityFrameworkCore/pull/14610>
  * `IUpdateSqlGenerator` mudou de um serviço com escopo para um serviço singleton
  * Além disso, `ISingletonUpdateSqlGenerator` foi removido
* <https://github.com/aspnet/EntityFrameworkCore/pull/15067>
  * Muitos códigos internos que estavam sendo usados pelos provedores agora se tornaram públicos
  * Ele não deve mais ser necssary para fazer referência a `IndentedStringBuilder` porque foi fatorado dos lugares que o expunha
  * Os usos de `NonCapturingLazyInitializer` devem ser substituídos por `LazyInitializer` da BCL
* <https://github.com/aspnet/EntityFrameworkCore/pull/14608>
  * Essa alteração é totalmente abordada no documento de alterações significativas do aplicativo. Para provedores, isso pode ser mais impactante porque o teste do EF core pode, muitas vezes, resultar em atingir esse problema, portanto, a infraestrutura de teste mudou para tornar isso menos provável.
* <https://github.com/aspnet/EntityFrameworkCore/issues/13961>
  * `EntityMaterializerSource` foi simplificado
* <https://github.com/aspnet/EntityFrameworkCore/pull/14895>
  * A tradução StartsWith foi alterada de forma que os provedores talvez queiram/precisem reagir
* <https://github.com/aspnet/EntityFrameworkCore/pull/15168>
  * Os serviços do conjunto de convenções foram alterados. Os provedores agora devem herdar de "ProviderConventionSet" ou "RelationalConventionSet".
  * As personalizações podem ser adicionadas por meio de serviços `IConventionSetCustomizer`, mas isso se destina a ser usado por outras extensões, não pelos provedores.
  * As convenções usadas no tempo de execução devem ser resolvidas em `IConventionSetBuilder`.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15288>
  * A propagação de dados foi Refatorada em uma API pública para evitar a necessidade de usar tipos internos. Isso só deve afetar provedores não relacionais, pois a propagação é tratada pela classe relacional base para todos os provedores relacionais.

## <a name="21-----22"></a>2,1---> 2,2

### <a name="test-only-changes"></a>Alterações somente de teste

* <https://github.com/aspnet/EntityFrameworkCore/pull/12057>-permitir SQL delimitadores personalizável em testes
  * Alterações de teste que permitem comparações de ponto flutuante não estritas em BuiltInDataTypesTestBase
  * Alterações de teste que permitem que os testes de consulta sejam reutilizados com delimitadores SQL diferentes
* <https://github.com/aspnet/EntityFrameworkCore/pull/12072>-adicionar testes de DbFunction aos testes de especificação relacional
  * De modo que esses testes possam ser executados em todos os provedores de banco de dados
* <https://github.com/aspnet/EntityFrameworkCore/pull/12362>-limpeza de teste assíncrona
  * Remover chamadas `Wait`, Async não necessária e renomeado alguns métodos de teste
* <https://github.com/aspnet/EntityFrameworkCore/pull/12666>-unificar a infraestrutura de teste de log
  * Adição de `CreateListLoggerFactory` e remoção de alguma infraestrutura de log anterior, que exigirá que os provedores que usam esses testes reajam
* <https://github.com/aspnet/EntityFrameworkCore/pull/12500>-executar mais testes de consulta de forma síncrona e assíncrona
  * Os nomes e a fatoração do teste foram alterados, o que exigirá que os provedores que usam esses testes reajam
* <https://github.com/aspnet/EntityFrameworkCore/pull/12766>-renomeando navegações no modelo ComplexNavigations
  * Os provedores que usam esses testes podem precisar reagir
* <https://github.com/aspnet/EntityFrameworkCore/pull/12141>-retornar o contexto para o pool em vez de descartar em testes funcionais
  * Essa alteração inclui alguns refatoração de teste que podem exigir que os provedores reajam

### <a name="test-and-product-code-changes"></a>Alterações de código de teste e de produto

* <https://github.com/aspnet/EntityFrameworkCore/pull/12109>-consolidar métodos RelationalTypeMapping. Clone
  * As alterações em 2,1 para RelationalTypeMapping são permitidas para uma simplificação em classes derivadas. Não acreditamos que isso estivesse se dividindo aos provedores, mas os provedores podem aproveitar essa alteração em suas classes de mapeamento de tipo derivado.
* consultas marcadas ou <https://github.com/aspnet/EntityFrameworkCore/pull/12069>das
  * Adiciona a infraestrutura para marcar consultas LINQ e fazer com que essas marcas sejam exibidas como comentários no SQL. Isso pode exigir que os provedores reajam na geração de SQL.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13115>-suporte a dados espaciais via NTS
  * Permite que mapeamentos de tipo e conversores de membro sejam registrados fora do provedor
    * Os provedores devem chamar base. FindMapping () em sua implementação de ITypeMappingSource para que ele funcione
  * Siga este padrão para adicionar suporte espacial ao seu provedor que é consistente entre os provedores.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13199>-Adicionar depuração avançada para a criação do provedor de serviços
  * Permite que o DbContextOptionsExtensions implemente uma nova interface que possa ajudar as pessoas a entenderem por que o provedor de serviços interno está sendo recompilado
* <https://github.com/aspnet/EntityFrameworkCore/pull/13289>-adiciona a API canconnect para uso por verificações de integridade
  * Essa PR adiciona o conceito de `CanConnect` que será usado por ASP.NET Core verificações de integridade para determinar se o banco de dados está disponível. Por padrão, a implementação relacional apenas chama `Exist`, mas os provedores podem implementar algo diferente, se necessário. Os provedores não relacionais precisarão implementar a nova API para que a verificação de integridade seja utilizável.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13306>-atualizar RelationalTypeMapping base para não definir o tamanho de DbParameter
  * Pare de definir o tamanho por padrão, pois ele pode causar truncamento. Os provedores podem precisar adicionar sua própria lógica se o tamanho precisar ser definido.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13372>-RevEng: sempre especificar o tipo de coluna para colunas decimais
  * Sempre configure o tipo de coluna para colunas decimais no código com Scaffold em vez de configurar por convenção.
  * Os provedores não devem exigir nenhuma alteração na sua extremidade.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13469>-adiciona o Casey para gerar expressões de caso SQL
* <https://github.com/aspnet/EntityFrameworkCore/pull/13648>-adiciona a capacidade de especificar mapeamentos de tipo em sqlfunctionion para melhorar a inferência de tipo de repositório de argumentos e resultados.
