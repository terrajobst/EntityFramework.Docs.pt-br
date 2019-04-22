---
title: Log de alterações que afetam o provedor – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 9ef87a737111053df0359f3b2d7a4f82d25c578a
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58867938"
---
# <a name="provider-impacting-changes"></a>Alterações que afetam o provedor

Esta página contém links para efetuar pull de solicitações feitas no repositório do EF Core que pode exigir que os autores de outros provedores de banco de dados para reagir. A intenção é fornecer um ponto de partida para autores de provedores de banco de dados de terceiros existente ao atualizar o seu provedor para uma nova versão.

Esse log estamos começando com alterações da 2.1 para 2.2. Antes de 2.1 usamos a [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) rótulos em nossos problemas e solicitações de pull.

## <a name="22-----30"></a>2.2 ---> 3.0

Observe que muitos do [alterações significativas de nível de aplicativo](../what-is-new/ef-core-3.0/breaking-changes.md) também terá impacto sobre provedores.

* https://github.com/aspnet/EntityFrameworkCore/pull/14022
  * APIs obsoletas removidas e sobrecargas de parâmetros opcionais recolhido
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* https://github.com/aspnet/EntityFrameworkCore/pull/14589
  * Removidas as APIs obsoletas
* https://github.com/aspnet/EntityFrameworkCore/pull/15044
  * As subclasses de CharTypeMapping podem ter sido interrompidas devido a alterações de comportamento necessárias para corrigir alguns bugs na implementação de base.
* https://github.com/aspnet/EntityFrameworkCore/pull/15090
  * Adicionada uma classe base para IDatabaseModelFactory e atualizá-lo para usar um objeto de parâmetro inválido para atenuar as quebras de futuras.
* https://github.com/aspnet/EntityFrameworkCore/pull/15123
  * Usado objetos de parâmetro no MigrationsSqlGenerator para atenuar as quebras de futuras.
* https://github.com/aspnet/EntityFrameworkCore/pull/14972
  * Configuração explícita de níveis de log necessário algumas alterações às APIs que podem estar usando provedores. Especificamente, se provedores estiver usando a infraestrutura de registro em log diretamente, essa alteração pode interromper que usam. Além disso, os provedores que usam a infraestrutura (que será pública) no futuro será necessário derivar `LoggingDefinitions` ou `RelationalLoggingDefinitions`. Consulte os provedores SQL Server e na memória para obter exemplos.
* https://github.com/aspnet/EntityFrameworkCore/pull/15091
  * Agora o Core, relacional e abstrações de cadeias de caracteres de recurso são públicas.
  * `CoreLoggerExtensions` e `RelationalLoggerExtensions` agora são públicos. Provedores devem usar essas APIs quando o log de eventos que são definidos no nível relacional ou core. Não acessam recursos de registro em log diretamente; Esses são ainda internos.
  * `IRawSqlCommandBuilder` foi alterado de um serviço singleton para um serviço com escopo
  * `IMigrationsSqlGenerator` foi alterado de um serviço singleton para um serviço com escopo
* https://github.com/aspnet/EntityFrameworkCore/pull/14706
  * A infraestrutura para a criação de comandos relacionais se tornou pública para que possa ser com segurança usada por provedores e refatorado um pouco.
* https://github.com/aspnet/EntityFrameworkCore/pull/14733
  * `ILazyLoader` foi alterado de um serviço com escopo para um serviço transitório
* https://github.com/aspnet/EntityFrameworkCore/pull/14610
  * `IUpdateSqlGenerator` foi alterado de um serviço com escopo para um serviço singleton
  * Além disso, `ISingletonUpdateSqlGenerator` foi removido
* https://github.com/aspnet/EntityFrameworkCore/pull/15067
  * Muito código interno que estava sendo usado por provedores agora foi publicado
  * Ele não deve mais ser necssary para fazer referência a `IndentedStringBuilder` , pois ele foi decomposto fora de locais que são expostos a ele
  * Usos `NonCapturingLazyInitializer` deve ser substituído pelo `LazyInitializer` da BCL
* https://github.com/aspnet/EntityFrameworkCore/pull/14608
  * Essa alteração está totalmente coberta no aplicativo quebra o documento de alterações. Para provedores, isso pode ser mais afetando porque o teste do EF core pode resultar em tendo esse problema, portanto, a infraestrutura de teste foi alterado para certificar que menos provável.
* https://github.com/aspnet/EntityFrameworkCore/issues/13961
  * `EntityMaterializerSource` foi simplificado
* https://github.com/aspnet/EntityFrameworkCore/pull/14895
  * StartsWith tradução foi alterado de forma que provedores podem desejar/precisar react
* https://github.com/aspnet/EntityFrameworkCore/pull/15168
  * Serviços de conjunto de convenção foram alterados. Agora, os provedores devem herdar de "ProviderConventionSet" ou "RelationalConventionSet".
  * As personalizações podem ser adicionadas por meio de `IConventionSetCustomizer` serviços, mas isso se destina a ser usado por outras extensões, não os provedores.
  * As convenções usadas no tempo de execução devem ser resolvidas a partir `IConventionSetBuilder`.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Alterações de teste

* [https://github.com/aspnet/EntityFrameworkCore/pull/12057](https://github.com/aspnet/EntityFrameworkCore/pull/12057) -Permitir delimitadores personalizáveis do SQL nos testes
  * Testar as alterações que permitem que comparações de ponto flutuante não restrito em BuiltInDataTypesTestBase
  * Alterações de teste que permitem testes de consulta ser reutilizado com diferentes dos delimitadores SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/12072](https://github.com/aspnet/EntityFrameworkCore/pull/12072) -Adicionar testes de DbFunction para os testes de especificação relacional
  * De modo que esses testes podem ser executados em relação a todos os provedores de banco de dados
* [https://github.com/aspnet/EntityFrameworkCore/pull/12362](https://github.com/aspnet/EntityFrameworkCore/pull/12362) -Limpeza de teste Async
  * Remover `Wait` chamadas, desnecessários async e renomeado alguns métodos de teste
* [https://github.com/aspnet/EntityFrameworkCore/pull/12666](https://github.com/aspnet/EntityFrameworkCore/pull/12666) -Unificar a infraestrutura de teste do log
  * Adicionado `CreateListLoggerFactory` e removidos alguns infraestrutura de log anterior, o que exigirá provedores que usam esses testes para reagir
* [https://github.com/aspnet/EntityFrameworkCore/pull/12500](https://github.com/aspnet/EntityFrameworkCore/pull/12500) -Executar mais testes de consulta de forma síncrona e assíncrona
  * Nomes de teste e a fatoração foi alterado, que exigirá provedores que usam esses testes para reagir
* [https://github.com/aspnet/EntityFrameworkCore/pull/12766](https://github.com/aspnet/EntityFrameworkCore/pull/12766) -Renomeação navegações no modelo ComplexNavigations
  * Talvez seja necessário reagir provedores que usam esses testes
* [https://github.com/aspnet/EntityFrameworkCore/pull/12141](https://github.com/aspnet/EntityFrameworkCore/pull/12141) -Retornar o contexto para o pool em vez de descartar em testes funcionais
  * Essa alteração inclui certa refatoração do teste que podem exigir provedores reagir


### <a name="test-and-product-code-changes"></a>Alterações de código do produto e de teste

* [https://github.com/aspnet/EntityFrameworkCore/pull/12109](https://github.com/aspnet/EntityFrameworkCore/pull/12109) -Consolidar RelationalTypeMapping.Clone métodos
  * As alterações no 2.1 para a RelationalTypeMapping permitidas para uma simplificação em classes derivadas. Não acreditamos que isso estava interrompendo aos provedores, mas provedores podem tirar proveito dessa alteração no seu tipo derivado classes de mapeamento.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12069](https://github.com/aspnet/EntityFrameworkCore/pull/12069) -Consultas nomeadas ou marcadas
  * Adiciona a infraestrutura para marcação de consultas LINQ e ter essas marcas que aparecem como comentários no SQL. Isso pode exigir provedores reagir na geração de SQL.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13115](https://github.com/aspnet/EntityFrameworkCore/pull/13115) -Suporte a dados espaciais via NTS
  * Permite que os mapeamentos de tipo e membro tradutores a serem registrados fora do provedor
    * Provedores devem chamar base. FindMapping() em sua implementação ITypeMappingSource para funcionar
  * Siga esse padrão para adicionar suporte espacial ao seu provedor que é consistente com provedores.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13199](https://github.com/aspnet/EntityFrameworkCore/pull/13199) -Adicionar depuração aprimorada para criação de provedor de serviços
  * Permite que DbContextOptionsExtensions implementar uma nova interface que pode ajudar as pessoas a entender por que está sendo compilado novamente o provedor de serviço interno
* [https://github.com/aspnet/EntityFrameworkCore/pull/13289](https://github.com/aspnet/EntityFrameworkCore/pull/13289) -Adiciona CanConnect API para uso por verificações de integridade
  * Essa solicitação de pull adiciona o conceito de `CanConnect` que será usada pela integridade do ASP.NET Core verifica para determinar se o banco de dados está disponível. Por padrão, a implementação relacional apenas chama `Exist`, mas os provedores podem implementar algo diferente se necessário. Provedores não relacionais precisará implementar a nova API para que a verificação de integridade ser usado.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13306](https://github.com/aspnet/EntityFrameworkCore/pull/13306) -Atualizar RelationalTypeMapping base para definir o tamanho de DbParameter
  * Pare definindo o tamanho por padrão, pois isso pode causar truncamento. Provedores talvez seja necessário adicionar sua própria lógica, se o tamanho deve ser definida.
* (https://github.com/aspnet/EntityFrameworkCore/pull/13372) -RevEng: Sempre especifique o tipo de coluna para colunas decimais
  * Sempre configure tipo de coluna para colunas decimais no código gerado por scaffolding em vez de configurar por convenção.
  * Provedores não devem exigir qualquer alteração em seu lado.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13469](https://github.com/aspnet/EntityFrameworkCore/pull/13469) -Adiciona CaseExpression para gerar as expressões de caso de SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/13648](https://github.com/aspnet/EntityFrameworkCore/pull/13648) – Adiciona a capacidade de especificar mapeamentos de tipo em SqlFunctionExpression para melhorar a inferência de tipo de repositório de argumentos e os resultados.
