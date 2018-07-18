---
title: Versões anteriores do Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
caps.latest.revision: 4
ms.openlocfilehash: 5be3632fd3a3f04e12e2d3aa67de6c1d9c7b56a2
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120102"
---
# <a name="past-releases-of-entity-framework"></a>Versões anteriores do Entity Framework

A primeira versão do Entity Framework foi lançada em 2008, como parte do .NET Framework 3.5 SP1 e Visual Studio 2008 SP1.

A partir da versão ef4.1 em diante, é enviado como o [pacote do NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) -atualmente um dos pacotes mais populares em NuGet.org.

Entre as versões 4.1 e 5.0, o pacote EntityFramework NuGet estendido as bibliotecas EF que é fornecida como parte do .NET Framework.   

Começando com a versão 6, o EF tornou-se um projeto de código-fonte aberto e também movido completamente fora do formulário de banda do .NET Framework.
Isso significa que, quando você adiciona o pacote do NuGet do EntityFramework versão 6 para um aplicativo, você está obtendo uma cópia completa da biblioteca do EF que não depende dos bits EF que são fornecidos como parte do .NET Framework.
Isso ajudou um pouco a acelerar o ritmo do desenvolvimento e a entrega de novos recursos.

Em junho de 2016, lançamos o EF Core 1.0. O EF Core baseia-se em uma nova base de código e foi projetado como uma versão mais leve e extensível do EF.
Atualmente, o EF Core é o foco principal de desenvolvimento para a equipe do Entity Framework na Microsoft.
Isso significa que não há novos recursos principais planejados para o EF6. No entanto o EF6 ainda é mantido como um projeto de código-fonte aberto e um produto da Microsoft com suporte.

Aqui está a lista de versões anteriores, em ordem cronológica inversa, com informações sobre os novos recursos que foram introduzidos em cada versão.

## <a name="ef-613"></a>EF 6.1.3
O EF 6.1.3 tempo de execução foi lançado para o NuGet em outubro de 2015.
Esta versão contém correções somente para defeitos de alta prioridade e regressões relatadas o 6.1.2 versão.
As correções incluem:

- Consulta: Regressão no EF 6.1.2: OUTER APPLY introduzido e consultas mais complexas para relações 1:1 e a cláusula "let"
- Problema TPT com ocultar a propriedade de classe herdada da classe base
- DbMigration.Sql falha quando a palavra 'go' está contida no texto
- Criar o sinalizador de compatibilidade para UnionAll e suporte a mesclagem de Intersect
- Consulta com várias inclusões não funciona em 6.1.2 (trabalhando no 6.1.1)
- "Você tem um erro na sintaxe SQL" exceção após a atualização do EF 6.1.1 para 6.1.2

## <a name="ef-612"></a>EF 6.1.2
O EF 6.1.2 tempo de execução foi lançado para o NuGet em dezembro de 2014.
Esta versão é principalmente sobre correções de bugs. Também aceitamos algumas alterações importantes de membros da comunidade:
- **Parâmetros de consulta de cache podem ser configurados no arquivo app/web.configuration**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **Métodos de SqlFile e SqlResource em DbMigration** permitem que você execute um SQL script armazenado como um arquivo ou recurso incorporado.

## <a name="ef-611"></a>EF 6.1.1
O EF 6.1.1 tempo de execução foi lançado para o NuGet em junho de 2014.
Esta versão contém correções para problemas que encontrou um número de pessoas. Entre outras:
- Designer: Erro ao abrir o EF5 edmx com precisão decimal no designer do EF6
- Lógica de detecção padrão instância de LocalDB não funciona com o SQL Server 2014

## <a name="ef-610"></a>EF 6.1.0
O EF 6.1.0 tempo de execução foi lançado para o NuGet em março de 2014.
Essa atualização secundária inclui um número significativo de novos recursos:

- **Consolidação de ferramentas** fornece uma maneira consistente para criar um novo modelo do EF. Esse recurso [estende o Assistente de modelo de dados de entidade ADO.NET para dar suporte à criação de modelos de Code First](~/ef6/modeling/code-first/workflows/existing-database.md), incluindo engenharia reversa de um banco de dados existente. Esses recursos estavam disponíveis anteriormente na qualidade Beta no EF Power Tools.
- **[Tratamento de falhas de confirmação da transação](~/ef6/fundamentals/connection-resiliency/commit-failures.md)**  fornece o CommitFailureHandler que usa a capacidade de recentemente introduzida para interceptar operações de transação. O CommitFailureHandler permite a recuperação automática de falhas de conexão enquanto confirmar uma transação.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md)**  permite que os índices seja especificado, colocando um `[Index]` de atributo em uma propriedade (ou propriedades) em seu modelo Code First. Código pela primeira vez, em seguida, criará um índice correspondente no banco de dados.
- **A API pública do mapeamento** fornece acesso às informações de EF tem sobre como as propriedades e tipos são mapeados para colunas e tabelas no banco de dados. Em versões anteriores dessa API foi interna.
- **[Capacidade de configurar interceptores por meio do arquivo App/Web.config](~/ef6/fundamentals/configuring/config-file.md)**  permite que os interceptadores a ser adicionado sem recompilar o aplicativo.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**é um novo interceptador que torna mais fácil registrar todas as operações de banco de dados em um arquivo. Em combinação com o recurso anterior, isso permite que você facilmente [ative o log de operações de banco de dados para um aplicativo implantado](~/ef6/fundamentals/configuring/config-file.md), sem a necessidade de recompilar.
- **Detecção de alteração do modelo de migrações** foi aprimorado para que as migrações gerado por scaffolding sejam mais precisas; o desempenho do processo de detecção de alteração também foi aprimorado.
- **Melhorias de desempenho** incluindo operações de redução do banco de dados durante a inicialização, otimizações para comparação de igualdade nulo em consultas LINQ, mais rápido exibir geração (criação de modelo) em mais cenários e mais eficiente materialização de entidades controladas com várias associações.

## <a name="ef-602"></a>EF 6.0.2
O EF 6.0.2 tempo de execução foi lançado para o NuGet em dezembro de 2013.
Esta versão de patch é limitada a corrigir problemas que foram introduzidos na versão EF6 (regressões de desempenho/comportamento desde o EF5).

## <a name="ef-601"></a>EF 6.0.1
O EF 6.0.1 tempo de execução foi lançado para o NuGet em outubro de 2013 simultaneamente com o EF 6.0.0, pois o último foi inserido em uma versão do Visual Studio que tinha bloqueado poucos meses antes.
Esta versão de patch é limitada a corrigir problemas que foram introduzidos na versão EF6 (regressões de desempenho/comportamento desde o EF5).
As alterações mais importantes foram corrigir alguns problemas de desempenho durante aquecimento para modelos do EF.
Isso era importante à medida que o desempenho de aquecimento era uma área de foco no EF6 e esses problemas foram eliminando algumas das outros ganhos de desempenho feitos no EF6.

## <a name="ef-60"></a>EF 6.0
O EF 6.0.0 tempo de execução foi lançado para o NuGet em outubro de 2013.
Essa é a primeira versão um tempo de execução do EF completo está incluído na [pacote do NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) que não depende dos bits EF que fazem parte do .NET Framework.
Mover as partes restantes do tempo de execução para o pacote NuGet necessário um número de alteração para o código existente.
Consulte a seção sobre [atualizar para o Entity Framework 6](upgrading-to-ef6.md) para obter mais detalhes sobre as etapas manuais necessárias para atualizar.

Esta versão inclui diversos recursos novos.
Os recursos a seguir funcionam para modelos criados com o EF Designer ou o Code First:

- **[Consulta assíncrona e salvamento](~/ef6/fundamentals/async.md)**  adiciona suporte para os padrões assíncronos baseados em tarefas que foram introduzidas no .NET 4.5.
- **[Resiliência de Conexão](~/ef6/fundamentals/connection-resiliency/retry-logic.md)**  permite a recuperação automática de falhas transitórias de conexão.
- **[Configuração baseada em código](~/ef6/fundamentals/configuring/code-based.md)**  lhe dá a opção de executar a configuração – que tradicionalmente foi realizada em um arquivo de configuração no código.
- **[Resolução de dependência](~/ef6/fundamentals/configuring/dependency-resolution.md)**  introduz o suporte para o localizador de serviço padrão e estamos já fatorado algumas partes da funcionalidade que podem ser substituídos por implementações personalizadas.
- **[Registro em log do SQL/interceptação](~/ef6/fundamentals/logging-and-interception.md)**  fornece os blocos de construção de baixo nível para interceptação de operações do EF com o log de SQL simple em cima.
- **Aprimoramentos de capacidade de teste** torná-lo mais fácil de criar duplicatas de teste para o DbContext e DbSet quando [usando uma estrutura de simulação](~/ef6/fundamentals/testing/mocking.md) ou [das duplicatas de teste de escrever seu próprio](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[DbContext agora pode ser criado com uma DbConnection que já está aberta](~/ef6/fundamentals/connection-management.md)**  que permite cenários em que seria útil se a conexão pode ser aberto durante a criação de contexto (como o compartilhamento de uma conexão entre os componentes onde Você não pode garantir o estado da conexão).
- **[Melhor suporte a transações](~/ef6/saving/transactions.md)**  fornece suporte para uma transação externa para o framework, bem como maneiras avançadas de criação de uma transação dentro da estrutura.
- **Enums, espacial e um melhor desempenho no .NET 4.0** – movendo os componentes principais que costumavam ser, no .NET Framework no pacote do NuGet do EF, agora estamos prontos oferecer suporte a enum, tipos de dados espaciais e os aprimoramentos de desempenho do EF5 no .NET 4.0.
- **Melhor desempenho de Enumerable em consultas LINQ**.
- **(Geração de exibição) de tempo de aquecimento aprimorado**, especialmente para grandes modelos.
- **Pluralização conectável &amp; Singularização serviço**.
- **As implementações personalizadas de é igual a ou GetHashCode** na entidade classes agora têm suporte.
- **DbSet.AddRange/RemoveRange** fornece uma forma otimizada para adicionar ou remover várias entidades de um conjunto.
- **DbChangeTracker.HasChanges** fornece uma maneira fácil e eficiente para ver se há alterações pendentes sejam salvos no banco de dados.
- **Com SqlCeFunctions** fornece um SQL Compact equivalente para o SqlFunctions.

Os recursos a seguir se aplicam somente ao Code First:

- **[Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md)**  permitir escrever suas próprias convenções para ajudar a evitar a configuração repetitiva. Nós fornecemos uma API simples para convenções leves, bem como alguns blocos de construção mais complexos para permitir a criação de convenções mais complicadas.
- **[Código de mapeamento primeiro para os procedimentos armazenados Insert/Update/Delete](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)**  agora tem suporte.
- **[Scripts de migrações idempotentes](~/ef6/modeling/code-first/migrations/index.md)**  permitem que você gerar um script SQL que pode atualizar um banco de dados em qualquer versão até a versão mais recente.
- **[Tabela de histórico de migrações configurável](~/ef6/modeling/code-first/migrations/history-customization.md)**  permite que você personalize a definição da tabela de histórico de migrações. Isso é particularmente útil para provedores de banco de dados que exigem tipos de dados apropriados etc. deve ser especificado para a tabela de histórico de migrações funcionar corretamente.
- **Vários contextos por banco de dados** remove a limitação anterior de um modelo de Code First por banco de dados ao usar migrações ou quando o Code First criou automaticamente o banco de dados para você.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)**  é uma nova API do Code First que permite que o esquema de banco de dados padrão para um modelo Code First ser configurado em um só lugar. O esquema padrão Code First era codificados para &quot;dbo&quot; e a única maneira de configurar o esquema ao qual pertencia uma tabela foi por meio da API ToTable.
- **Método DbModelBuilder.Configurations.AddFromAssembly** permite que você adicione facilmente todas as classes de configuração definidas em um assembly quando você estiver usando classes de configuração com a API Fluent do Code First.
- **[Operações de migrações personalizadas](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)**  habilitado para adicionar operações adicionais para ser usado em suas migrações baseadas em código.
- **Nível de isolamento de transação padrão é alterado para READ_COMMITTED_SNAPSHOT** bancos de dados criados usando o Code First, permitindo mais escalabilidade e menos deadlocks.
- **Entidade e tipos complexos podem agora ser classes nestedinside**. |

## <a name="ef-50"></a>EF 5.0
O EF 5.0.0 tempo de execução foi lançado para o NuGet em agosto de 2012.
Esta versão apresenta alguns novos recursos, incluindo suporte a enum, funções com valor de tabela, tipos de dados espaciais e várias melhorias de desempenho.

O Entity Framework Designer no Visual Studio 2012 também introduz o suporte para diagramas de vários por modelo, coloração de formas em que a importação de lote e de superfície de design de procedimentos armazenados.

Aqui está uma lista de conteúdo que reunimos especificamente para a versão do EF 5.

-   [Postagem de versão 5 do EF](http://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Novos recursos no EF5
    -   [Suporte a enum no código pela primeira vez](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Suporte a enum no EF Designer](~/ef6/modeling/designer/data-types/enums.md)
    -   [Tipos de dados espaciais no código pela primeira vez](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Tipos de dados espaciais no EF Designer](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Funções com valor de tabela](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Vários diagramas por modelo](~/ef6/modeling/designer/multiple-diagrams.md)
-   Como configurar seu modelo
    -   [Criar um Modelo](~/ef6/modeling/index.md)
    -   [Conexões e modelos](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Considerações sobre desempenho](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Trabalhando com o Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Arquivo de configuração](~/ef6/fundamentals/configuring/config-file.md)
    -   [Glossário](~/ef6/resources/glossary.md)
    -   Code First
        -   [Code First para um novo banco de dados (passo a passo e vídeo)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First para um banco de dados (passo a passo e vídeo)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Convenções](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Anotações de dados](~/ef6/modeling/code-first/data-annotations.md)
        -   [API Fluent - Configurando/mapeamento & tipos de propriedades](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [API Fluent - configurando relações](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [API Fluent com VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migrações do Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Migrações automáticas do Code First](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Definindo DbSets](~/ef6/modeling/code-first/dbsets.md)
    -   Designer EF
        -   [Primeiro modelo (passo a passo e vídeo)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (passo a passo e vídeo)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Tipos complexos](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Associações/relações](~/ef6/modeling/designer/relationships.md)
        -   [Padrão de herança TPT](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Padrão de herança TPH](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Consulta com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Procedimentos armazenados com vários conjuntos de resultados](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Inserir, atualizar e excluir com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Mapear uma entidade para várias tabelas (separação da entidade)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Mapear várias entidades para uma tabela (divisão de tabela)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definindo as consultas](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Modelos de geração de código](~/ef6/modeling/designer/codegen/index.md)
        -   [Revertendo a ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Usando seu modelo
    -   [Como trabalhar com DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Localizando/consultando entidades](~/ef6/querying/index.md)
    -   [Trabalhando com relações](~/ef6/fundamentals/relationships.md)
    -   [Carregando entidades relacionadas](~/ef6/querying/related-data.md)
    -   [Trabalhando com dados locais](~/ef6/querying/local-data.md)
    -   [Aplicativos de N camadas](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Consultas SQL Brutas](~/ef6/querying/raw-sql.md)
    -   [Padrões de simultaneidade otimista](~/ef6/saving/concurrency.md)
    -   [Trabalhar com Proxies](~/ef6/fundamentals/proxies.md)
    -   [Automático detectar alterações](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Consultas sem controle](~/ef6/querying/no-tracking.md)
    -   [O método Load](~/ef6/querying/load-method.md)
    -   [Adicionar/anexar e estados da entidade](~/ef6/saving/change-tracking/entity-state.md)
    -   [Trabalhando com valores de propriedade](~/ef6/saving/change-tracking/property-values.md)
    -   [Associação de dados com o WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Associação de dados com o WinForms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
O EF 4.3.1 tempo de execução foi lançado para o NuGet em fevereiro de 2012 logo após EF 4.3.0.
Esta versão do patch incluído algumas correções de bug para a versão 4.3 do EF e introduziu o melhor suporte de LocalDB para clientes que usam o EF 4.3 com o Visual Studio 2012.

Aqui está uma lista de conteúdo que reunimos especificamente para o EF 4.3.1 de versão, a maioria do conteúdo fornecido para o EF 4.1 ainda se aplica ao EF 4.3 também.

-   [Postagem no Blog de versão EF 4.3.1](http://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
O EF 4.3.0 tempo de execução foi lançado para o NuGet em fevereiro de 2012.
Esta versão incluído o novo recurso de migrações do Code First que permite que um banco de dados criado pelo Code First incrementalmente seja alterado à medida que seu modelo Code First evolui.

Aqui está uma lista de conteúdo que reunimos especificamente para a versão do EF 4.3, a maioria do conteúdo fornecido para o EF 4.1 ainda se aplica ao EF 4.3 bem:
-   [Postagem de versão 4.3 do EF](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Passo a passo do EF 4.3 migrações baseadas em código](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Passo a passo do EF 4.3 migrações automáticas](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4.2
O EF 4.2.0 em tempo de execução foi lançado para o NuGet em novembro de 2011.
Esta versão inclui correções de bugs para o EF 4.1.1 de versão.
Porque esta versão é incluído somente versão de patch de correções de bug que poderia ter sido o EF 4.1.2, mas podemos optar por mover para 4.2 para permitir a mudança a data com base em números de versão de patch, usamos o 4.1.x libera e adotarem o [Versionsing semântica](https://semver.org) padrão para controle de versão semântico.

Aqui está uma lista de conteúdo que reunimos especificamente para a versão 4.2 do EF, o conteúdo fornecido para o EF 4.1 ainda se aplica ao EF 4.2 também.

-   [Postagem de versão 4.2 do EF](http://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Primeiro passo a passo de código](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Modelo de & banco de dados primeiro passo a passo](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
O EF 4.1.10715 tempo de execução foi lançado para o NuGet em julho de 2011.
Além das correções de bugs nesta versão do patch introduziu alguns componentes para tornar mais fácil para tempo de design de ferramentas para trabalhar com um modelo Code First.
Esses componentes são usados por migrações do Code First (incluído no EF 4.3) e o EF Power Tools.

Você observará que a versão estranha número 4.1.10715 do pacote.
Usamos a usar versões de patch de data com base antes, decidimos adotar [controle de versão semântico](https://semver.org).
Considere esta versão como o EF 4.1 patch 1 (ou EF 4.1.1).

Aqui está uma lista de conteúdo que reunimos para o 4.1.1 versão:

-   [Postagem de versão do EF 4.1.1](http://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4.1
O EF 4.1.10331 tempo de execução foi a primeira a ser publicado no NuGet, em abril de 2011.
Esta versão incluído a API DbContext simplificada e o fluxo de trabalho do Code First.

Você observará que o número de versão estranho 4.1.10331, o que realmente deveria ter sido 4.1. Além disso, há um 4.1.10311 versão que deveria ter sido 4.1.0-rc ('rc' significa 'rc').
Usamos a usar versões de patch de data com base antes, decidimos adotar [controle de versão semântico](https://semver.org).

Aqui está uma lista de conteúdo que reunimos para a versão 4.1. Muitas delas ainda se aplica a versões mais recentes do Entity Framework:

-   [Postagem de versão 4.1 do EF](http://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Primeiro passo a passo de código](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Modelo de & banco de dados primeiro passo a passo](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure federações e o Entity Framework](http://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
Esta versão foi incluída no .NET Framework 4 e Visual Studio 2010, em abril de 2010.
Novos recursos importantes nesta versão incluíam POCO suporte, mapeamento de chaves estrangeiro, carregamento lento, aprimoramentos de capacidade de teste, geração de código personalizável e o fluxo de trabalho Model First.

Embora tenha sido a segunda versão do Entity Framework, ele se chamava EF 4 para se alinhar com a versão do .NET Framework que ele foi enviado com.
Depois desta versão, começamos disponibilizando o Entity Framework no NuGet e adotou o controle de versão semântico, pois estamos não eram vinculados à versão do .NET Framework.

Observe que algumas versões subsequentes do .NET Framework fornecidos com atualizações significativas para os bits EF incluídos.
Na verdade, muitos dos novos recursos do EF 5.0 foram implementados como melhorias nesses bits.
No entanto, para racionalizar a história do controle de versão para o EF, continuamos a fazer referência aos bits de EF que fazem parte do .NET Framework como o tempo de execução do EF 4.0, enquanto que consistem em todas as versões mais recentes do [pacote do NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).         

## <a name="ef-35"></a>EF 3.5
A versão inicial do Entity Framework foi incluída no .NET 3.5 Service Pack 1 e o Visual Studio 2008 SP1, lançado em agosto de 2008.
Esta versão fornecido suporte básico do / RM usando o fluxo de trabalho de banco de dados primeiro.
