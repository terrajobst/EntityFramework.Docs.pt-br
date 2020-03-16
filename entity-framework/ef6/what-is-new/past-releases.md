---
title: Versões anteriores do Entity Framework-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
uid: ef6/what-is-new/past-releases
ms.openlocfilehash: b7181334cd125c5cbf296d5b3674c0b5f087f438
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402169"
---
# <a name="past-releases-of-entity-framework"></a>Versões anteriores do Entity Framework

A primeira versão do Entity Framework foi lançada em 2008, como parte do .NET Framework 3,5 SP1 e do Visual Studio 2008 SP1.

A partir da versão EF 4.1, ele foi enviado como o [pacote NuGet do EntityFramework](https://www.nuget.org/packages/EntityFramework/) , atualmente um dos pacotes mais populares em NuGet.org.

Entre as versões 4,1 e 5,0, o pacote NuGet do EntityFramework estendeu as bibliotecas do EF fornecidas como parte do .NET Framework.

A partir da versão 6, o EF tornou-se um projeto de software livre e também se moveu completamente fora da banda do .NET Framework.
Isso significa que, quando você adiciona o pacote NuGet versão 6 do EntityFramework a um aplicativo, você está obtendo uma cópia completa da biblioteca do EF que não depende dos bits do EF que são fornecidos como parte do .NET Framework.
Isso ajudou um pouco a acelerar o ritmo de desenvolvimento e entrega de novos recursos.

Em junho de 2016, lançamos EF Core 1,0. O EF Core é baseado em uma nova base de código e é projetado como uma versão mais simples e extensível do EF.
Atualmente EF Core é o foco principal do desenvolvimento da equipe de Entity Framework da Microsoft.
Isso significa que não há novos recursos principais planejados para EF6. No entanto, o EF6 ainda é mantido como um projeto de software livre e um produto da Microsoft com suporte.

Aqui está a lista de versões anteriores, em ordem cronológica inversa, com informações sobre os novos recursos que foram introduzidos em cada versão.

## <a name="ef-tools-update-in-visual-studio-2017-157"></a>Atualização de ferramentas do EF no Visual Studio 2017 15.7
Em maio de 2018, lançamos uma versão atualizada das ferramentas do EF como parte do Visual Studio 2017 15.7.
Ela inclui melhorias para alguns pontos de problemas comuns:

- Correções para vários bugs de acessibilidade da interface do usuário
- Solução alternativa para a regressão de desempenho do SQL Server ao gerar modelos de bancos de dados existentes [#4](https://github.com/aspnet/entityframework6/issues/4)
- Suporte para atualizar modelos maiores no SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Outra melhoria nessa nova versão das ferramentas do EF é a instalação do runtime do EF 6.2 quando um modelo é criado em um novo projeto. Com versões mais antigas do Visual Studio, é possível usar o runtime do EF 6.2 (bem como qualquer versão anterior do EF) instalando a versão correspondente do pacote NuGet.

## <a name="ef-620"></a>6\.2.0 EF
O runtime do EF 6.2 foi lançado para o NuGet em outubro de 2017.
Graças, em grande parte, aos esforços de nossa comunidade de colaboradores de software livre, o EF 6.2 inclui várias [correções de bugs](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) e [aprimoramentos de produtos](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Aqui está uma lista resumida das alterações mais importantes que afetam o runtime do EF 6.2:

- Redução do tempo de inicialização carregando modelos do Code First concluídos de um cache persistente [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- API fluente para definir índices [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions.Like() para habilitar a gravação de consultas LINQ que são traduzidas como LIKE no SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Agora Migrate.exe é compatível com a opção -script [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- Agora o EF6 pode trabalhar com valores de chave gerados por uma sequência no SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Atualização da lista de erros transitórios para a estratégia de execução do SQL Azure [n#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Bug: a repetição de consultas ou comandos SQL falha com "O SqlParameter já está contido em outro SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Bug: a avaliação de DbQuery.ToString() frequentemente atinge o tempo limite no depurador [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="ef-613"></a>6\.1.3 EF
O tempo de execução de 6.1.3 do EF foi liberado para o NuGet em outubro de 2015.
Esta versão contém apenas correções para defeitos de alta prioridade e regressões relatadas na versão do 6.1.2.
As correções incluem:

- Consulta: regressão no EF 6.1.2: aplicação externa introduzida e consultas mais complexas para relações 1:1 e cláusula "Let"
- Problema de TPT com a propriedade de classe base oculta na classe herdada
- Falha de DbMigration. SQL quando a palavra ' Go ' está contida no texto
- Criar sinalizador de compatibilidade para UnionAll e interceptar suporte de mesclagem
- A consulta com vários inclusões não funciona em 6.1.2 (funcionando em 6.1.1)
- Exceção "você tem um erro na sintaxe do SQL" após a atualização do EF 6.1.1 para 6.1.2

## <a name="ef-612"></a>6\.1.2 EF
O tempo de execução de 6.1.2 do EF foi liberado para o NuGet em dezembro de 2014.
Essa versão é principalmente sobre correções de bugs. Também aceitamos algumas alterações notáveis dos membros da Comunidade:
- **Os parâmetros do cache de consulta podem ser configurados no arquivo app/Web. Configuration**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- Os **métodos sqlfile e sqlresource em DbMigration** permitem que você execute um script SQL armazenado como um arquivo ou recurso inserido.

## <a name="ef-611"></a>FE 6.1.1
O tempo de execução do EF 6.1.1 foi liberado para o NuGet em junho de 2014.
Esta versão contém correções para problemas que várias pessoas encontraram. Entre outros:
- Designer: erro ao abrir o EF5 edmx com precisão decimal no designer de EF6
- A lógica de detecção de instância padrão para LocalDB não funciona com SQL Server 2014

## <a name="ef-610"></a>6\.1.0 EF
O tempo de execução do 6.1.0 do EF foi liberado para o NuGet em março de 2014.
Essa atualização secundária inclui um número significativo de novos recursos:

- A **consolidação de ferramentas** fornece uma maneira consistente de criar um novo modelo do EF. Esse recurso [estende o assistente de modelo de dados de entidade de ADO.net para dar suporte à criação de modelos de Code First](~/ef6/modeling/code-first/workflows/existing-database.md), incluindo a engenharia reversa de um banco de dados existente. Esses recursos estavam disponíveis anteriormente em qualidade beta no EF Power Tools.
- A **[manipulação de falhas de confirmação de transação](~/ef6/fundamentals/connection-resiliency/commit-failures.md)** fornece o CommitFailureHandler que utiliza a capacidade introduzida recentemente para interceptar operações de transação. O CommitFailureHandler permite a recuperação automática de falhas de conexão durante a confirmação de uma transação.
- **[Indexattribute](~/ef6/modeling/code-first/data-annotations.md)** permite que os índices sejam especificados colocando um atributo `[Index]` em uma propriedade (ou Propriedades) em seu modelo de Code First. Code First criará um índice correspondente no banco de dados.
- **A API de mapeamento público** fornece acesso às informações que o EF tem sobre como as propriedades e os tipos são mapeados para colunas e tabelas no banco de dados. Em versões anteriores, essa API era interna.
- A **[capacidade de configurar os interceptores por meio do arquivo app/Web. config](~/ef6/fundamentals/configuring/config-file.md)** permite que os interceptores sejam adicionados sem recompilar o aplicativo.
- **System. Data. Entity. Infrastructure. interceptoion. DatabaseLogger**é um novo interceptador que facilita o log de todas as operações de banco de dados em um arquivo. Em combinação com o recurso anterior, isso permite que você alterne facilmente o [log de operações de banco de dados para um aplicativo implantado](~/ef6/fundamentals/configuring/config-file.md), sem a necessidade de recompilar.
- A **detecção de alteração do modelo de migrações** foi aprimorada para que as migrações de com Scaffold sejam mais precisas; o desempenho do processo de detecção de alteração também foi aprimorado.
- **Melhorias de desempenho** , incluindo operações de banco de dados reduzidas durante a inicialização, otimizações para comparação de igualdade nula em consultas LINQ, geração de exibição mais rápida (criação de modelo) em mais cenários e materialização mais eficiente de entidades rastreadas com várias associações.

## <a name="ef-602"></a>6\.0.2 EF
O tempo de execução de 6.0.2 do EF foi liberado para o NuGet em dezembro de 2013.
Esta versão de patch é limitada à correção de problemas introduzidos na versão EF6 (regressões no desempenho/comportamento desde EF5).

## <a name="ef-601"></a>EF 6.0.1
O tempo de execução do EF 6.0.1 foi lançado para o NuGet em outubro de 2013 simultaneamente com o EF 6.0.0, pois o último foi inserido em uma versão do Visual Studio que havia bloqueado alguns meses antes.
Esta versão de patch é limitada à correção de problemas introduzidos na versão EF6 (regressões no desempenho/comportamento desde EF5).
As alterações mais notáveis foram corrigir alguns problemas de desempenho durante o aquecimento para modelos do EF.
Isso era importante, pois o desempenho de aquecimento era uma área de foco no EF6 e esses problemas estavam negando alguns dos outros ganhos de desempenho feitos no EF6.

## <a name="ef-60"></a>EF 6,0
O tempo de execução de 6.0.0 do EF foi liberado para o NuGet em outubro de 2013.
Esta é a primeira versão na qual um tempo de execução completo do EF é incluído no [pacote NuGet do EntityFramework](https://www.nuget.org/packages/EntityFramework/) , que não depende dos bits do EF que fazem parte do .NET Framework.
Mover as partes restantes do tempo de execução para o pacote NuGet exigiu uma série de alterações significativas para o código existente.
Consulte a seção [Atualizando para o Entity Framework 6](upgrading-to-ef6.md) para obter mais detalhes sobre as etapas manuais necessárias para a atualização.

Esta versão inclui vários recursos novos.
Os recursos a seguir funcionam para modelos criados com Code First ou o designer do EF:

- A **[consulta assíncrona e a salvar](~/ef6/fundamentals/async.md)** adicionam suporte para os padrões assíncronos baseados em tarefas que foram introduzidos no .NET 4,5.
- A **[resiliência de conexão](~/ef6/fundamentals/connection-resiliency/retry-logic.md)** permite a recuperação automática de falhas de conexão transitórias.
- A **[Configuração baseada em código](~/ef6/fundamentals/configuring/code-based.md)** oferece a opção de executar a configuração – que foi tradicionalmente executada em um arquivo de configuração, em código.
- A **[resolução de dependências](~/ef6/fundamentals/configuring/dependency-resolution.md)** apresenta suporte para o padrão de localizador de serviço e apresentamos algumas partes de funcionalidade que podem ser substituídas por implementações personalizadas.
- O **[log de interceptações/SQL](~/ef6/fundamentals/logging-and-interception.md)** fornece blocos de construção de baixo nível para interceptação de operações do EF com log de SQL simples criado no topo.
- **Aprimoramentos de capacidade de teste** facilitam a criação de duplicatas de teste para DbContext e DbSet ao [usar uma estrutura fictícia](~/ef6/fundamentals/testing/mocking.md) ou [escrever suas próprias duplicatas de teste](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[Agora, o DbContext pode ser criado com uma DbConnection que já está aberta](~/ef6/fundamentals/connection-management.md)** , o que permite cenários onde seria útil se a conexão fosse aberta ao criar o contexto (por exemplo, compartilhar uma conexão entre componentes, em que você não pode garantir o estado da conexão).
- O **[suporte a transações aprimoradas](~/ef6/saving/transactions.md)** fornece suporte para uma transação externa à estrutura, bem como maneiras aprimoradas de criar uma transação dentro da estrutura.
- **Enums, espacial e melhor desempenho no .net 4,0** – ao mover os componentes principais que costumavam estar no .NET Framework no pacote NUGET do EF, agora podemos oferecer suporte a enum, tipos de dados espaciais e melhorias de desempenho do EF5 no .NET 4,0.
- **Desempenho aprimorado de Enumerable. Contains em consultas LINQ**.
- **Tempo de aquecimento aprimorado (geração de exibição)** , especialmente para modelos grandes.
- **Pluralização conectável &amp; serviço de singularização**.
- Agora há suporte para **implementações personalizadas de Equals ou GetHashCode** em classes de entidade.
- **DbSet. AddRange/RemoveRange** fornece uma maneira otimizada para adicionar ou remover várias entidades de um conjunto.
- **DbChangeTracker. HasChanges** fornece uma maneira fácil e eficiente de ver se há alterações pendentes a serem salvas no banco de dados.
- **SqlCeFunctions** fornece um equivalente do SQL Compact para o SqlFunctions.

Os recursos a seguir se aplicam somente a Code First:

- As **[convenções de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md)** permitem escrever suas próprias convenções para ajudar a evitar a configuração repetitiva. Fornecemos uma API simples para convenções leves, bem como alguns blocos de construção mais complexos para permitir que você crie convenções mais complicadas.
- Agora há suporte **[para o mapeamento de Code First para inserir/atualizar/excluir procedimentos armazenados](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)** .
- Os **[scripts de migrações idempotentes](~/ef6/modeling/code-first/migrations/index.md)** permitem gerar um script SQL que pode atualizar um banco de dados em qualquer versão até a versão mais recente.
- A **[tabela de histórico de migrações configuráveis](~/ef6/modeling/code-first/migrations/history-customization.md)** permite que você personalize a definição da tabela de histórico de migrações. Isso é particularmente útil para provedores de banco de dados que exigem os tipos de dado apropriados, etc. para serem especificados para que a tabela de histórico de migrações funcione corretamente.
- **Vários contextos por banco de dados** remove a limitação anterior de um modelo de Code First por banco de dados ao usar migrações ou quando Code First criou automaticamente o banco de dados para você.
- **[DbModelBuilder. HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)** é uma nova API Code First que permite que o esquema de banco de dados padrão de um modelo de Code First seja configurado em um único local. Anteriormente, o esquema padrão Code First era embutido em código para &quot;dbo&quot; e a única maneira de configurar o esquema ao qual uma tabela pertencia era por meio da API ToTable.
- O **método DbModelBuilder. Configurations. AddFromAssembly** permite que você adicione facilmente todas as classes de configuração definidas em um assembly quando você está usando classes de configuração com o Code First API fluente.
- **[As operações de migração personalizadas](https://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)** permitiram que você adicione operações adicionais a serem usadas em suas migrações baseadas em código.
- O **nível de isolamento da transação padrão é alterado para READ_COMMITTED_SNAPSHOT** para bancos de dados criados usando Code First, permitindo mais escalabilidade e menos deadlocks.
- Os **tipos de entidade e complexos agora podem ser classes nestedinside**.

## <a name="ef-50"></a>EF 5,0
O tempo de execução de 5.0.0 do EF foi liberado para o NuGet em agosto de 2012.
Esta versão apresenta alguns recursos novos, incluindo suporte a enum, funções com valor de tabela, tipos de dados espaciais e várias melhorias de desempenho.

O Entity Framework Designer no Visual Studio 2012 também apresenta suporte para vários diagramas por modelo, cor das formas na superfície de design e na importação em lote de procedimentos armazenados.

Aqui está uma lista de conteúdo que reunimos especificamente para a versão do EF 5:

-   [Postagem de versão do EF 5](https://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Novos recursos no EF5
    -   [Suporte a enum no Code First](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Suporte a enum no designer do EF](~/ef6/modeling/designer/data-types/enums.md)
    -   [Tipos de dados espaciais no Code First](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Tipos de dados espaciais no designer do EF](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Funções com valor de tabela](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Vários diagramas por modelo](~/ef6/modeling/designer/multiple-diagrams.md)
-   Configurando seu modelo
    -   [Criação de um modelo](~/ef6/modeling/index.md)
    -   [Conexões e modelos](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Considerações sobre desempenho](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Trabalhando com o Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Configurações do arquivo de configuração](~/ef6/fundamentals/configuring/config-file.md)
    -   [Glossário](~/ef6/resources/glossary.md)
    -   Code First
        -   [Code First a um novo banco de dados (passo a passos e vídeo)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First a um banco de dados existente (passo a passos e vídeo)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Convenções](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Anotações de dados](~/ef6/modeling/code-first/data-annotations.md)
        -   [API fluente – configuração/mapeamento de propriedades & tipos](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [API fluente – Configurando relações](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [API fluente com VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migrações do Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Migrações do Code First automática](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrar. exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Definindo DbSets](~/ef6/modeling/code-first/dbsets.md)
    -   EF Designer
        -   [Model First (Walkthrough e video)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (Walkthrough e video)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Tipos complexos](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Associações/relações](~/ef6/modeling/designer/relationships.md)
        -   [Padrão de herança TPT](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Padrão de herança TPH](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Consulta com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Procedimentos armazenados com vários conjuntos de resultados](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Inserir, atualizar & excluir com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Mapear uma entidade para várias tabelas (divisão de entidade)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Mapear várias entidades para uma tabela (divisão de tabela)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definindo consultas](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Modelos de geração de código](~/ef6/modeling/designer/codegen/index.md)
        -   [Revertendo para ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Usando seu modelo
    -   [Como trabalhar com DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Consultando/localizando entidades](~/ef6/querying/index.md)
    -   [Trabalhando com relações](~/ef6/fundamentals/relationships.md)
    -   [Carregando entidades relacionadas](~/ef6/querying/related-data.md)
    -   [Trabalhando com dados locais](~/ef6/querying/local-data.md)
    -   [Aplicativos de N camadas](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Consultas SQL Brutas](~/ef6/querying/raw-sql.md)
    -   [Padrões de simultaneidade otimista](~/ef6/saving/concurrency.md)
    -   [Trabalhando com proxies](~/ef6/fundamentals/proxies.md)
    -   [Detectar alterações automaticamente](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Consultas sem controle](~/ef6/querying/no-tracking.md)
    -   [O método Load](~/ef6/querying/load-method.md)
    -   [Adicionar/anexar e Estados de entidade](~/ef6/saving/change-tracking/entity-state.md)
    -   [Trabalhando com valores de propriedade](~/ef6/saving/change-tracking/property-values.md)
    -   [Associação de dados com o WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Associação de dados com WinForms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
O tempo de execução do EF 4.3.1 foi liberado para o NuGet em fevereiro de 2012 logo após o EF 4.3.0.
Esta versão de patch incluiu algumas correções de bug para a versão do EF 4,3 e introduziu melhor suporte do LocalDB para clientes que usam o EF 4,3 com o Visual Studio 2012.

Aqui está uma lista de conteúdo que reunimos especificamente para a versão do EF 4.3.1, a maior parte do conteúdo fornecido para o EF 4,1 ainda se aplica ao EF 4,3 também:

-   [Postagem do blog da versão do EF 4.3.1](https://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4,3
O tempo de execução do EF 4.3.0 foi lançado para o NuGet em fevereiro de 2012.
Esta versão incluiu o novo recurso Migrações do Code First que permite que um banco de dados criado por Code First seja alterado incrementalmente à medida que seu modelo de Code First evolui.

Aqui está uma lista de conteúdo que reunimos especificamente para a versão do EF 4,3, a maior parte do conteúdo fornecido para o EF 4,1 ainda se aplica ao EF 4,3 também:
-   [Postagem de versão do EF 4,3](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Passo a passos de migrações baseadas em código do EF 4,3](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Passo a passos de migrações automáticas do EF 4,3](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4,2
O tempo de execução de 4.2.0 do EF foi lançado para o NuGet em novembro de 2011.
Esta versão inclui correções de bugs para a versão do EF 4.1.1.
Como esta versão inclui apenas correções de bug, ele poderia ter sido a versão de patch do EF 4.1.2, mas optamos por mudar para 4,2 para nos permitirmos sair dos números de versão de patch com base na data que usamos nas versões 4.1. x e adotar o padrão de [controle de versão semântico](https://semver.org) para controle de versão semântico.

Aqui está uma lista de conteúdo que reunimos especificamente para a versão do EF 4,2, o conteúdo fornecido para o EF 4,1 ainda se aplica ao EF 4,2 também:

-   [Postagem de versão do EF 4,2](https://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Instruções Code First](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Instruções de & de modelo Database First](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
O tempo de execução de 4.1.10715 do EF foi lançado para o NuGet em julho de 2011.
Além das correções de bugs, essa versão de patch introduziu alguns componentes para facilitar a criação de ferramentas de tempo de design com um modelo de Code First.
Esses componentes são usados pelo Migrações do Code First (incluído no EF 4,3) e no EF Power Tools.

Você observará que o número de versão estranho 4.1.10715 do pacote.
Usamos versões de patch com base em data antes de decidirmos adotar o [controle de versão semântico](https://semver.org).
Considere esta versão como o patch 1 do EF 4,1 (ou o EF 4.1.1).

Aqui está uma lista de conteúdo que reunimos para a versão 4.1.1:

-   [Post da versão do EF 4.1.1](https://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4,1
O tempo de execução de 4.1.10331 do EF foi o primeiro a ser publicado no NuGet, em abril de 2011.
Esta versão incluiu a API DbContext simplificada e o fluxo de trabalho Code First.

Você notará o número de versão estranho, 4.1.10331, que deve realmente ser 4,1. Além disso, há uma versão 4.1.10311 que deveria ter sido 4.1.0-RC (o ' RC ' significa ' Release Candidate ').
Usamos versões de patch com base em data antes de decidirmos adotar o [controle de versão semântico](https://semver.org).

Aqui está uma lista de conteúdo que reunimos para a versão 4,1. Grande parte dele ainda se aplica às versões mais recentes do Entity Framework:

-   [Postagem de versão do EF 4,1](https://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Instruções Code First](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Instruções de & de modelo Database First](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure federações e o Entity Framework](https://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4,0
Esta versão foi incluída no .NET Framework 4 e no Visual Studio 2010, em abril de 2010.
Os novos recursos importantes desta versão incluíam suporte POCO, mapeamento de chave estrangeira, carregamento lento, melhorias de capacidade de teste, geração de código personalizável e o fluxo de trabalho de Model First.

Embora fosse a segunda versão do Entity Framework, ele foi chamado de EF 4 para se alinhar com a versão de .NET Framework fornecida com o.
Depois desta versão, começamos a disponibilizar Entity Framework no NuGet e adotou o controle de versão semântico, já que não estávamos mais vinculados à versão .NET Framework.

Observe que algumas versões subsequentes do .NET Framework foram enviadas com atualizações significativas para os bits do EF incluídos.
Na verdade, muitos dos novos recursos do EF 5,0 foram implementados como melhorias nesses bits.
No entanto, para racionalizar a história do controle de versão para o EF, continuamos a consultar os bits do EF que fazem parte do .NET Framework como o tempo de execução do EF 4,0, enquanto todas as versões mais recentes consistem no [pacote NuGet do EntityFramework](https://www.nuget.org/packages/EntityFramework/).

## <a name="ef-35"></a>EF 3,5
A versão inicial do Entity Framework foi incluída no .NET 3,5 Service Pack 1 e no Visual Studio 2008 SP1, lançado em agosto de 2008.
Esta versão forneceu suporte básico O/RM usando o fluxo de trabalho Database First.
