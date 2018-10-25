---
title: Considerações sobre desempenho para o EF4, EF5 e EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: c87c1412cb23abf232663d7e4f44eef5f7818ea2
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022383"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Considerações de desempenho do EF 4, 5 e 6
Por David Obando, Eric Dettinger e outros

Publicado em: Abril de 2012

Última atualização: maio de 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Introdução

Estruturas de mapeamento relacional de objeto são uma maneira conveniente para fornecer uma abstração para acesso a dados em um aplicativo orientado a objeto. Para aplicativos .NET, recomendada pela Microsoft que é o O/RM Entity Framework. Com qualquer abstração, desempenho pode se tornar uma preocupação.

Este white paper foi escrito para mostrar as considerações de desempenho ao desenvolver aplicativos usando o Entity Framework, para dar aos desenvolvedores uma ideia do que os algoritmos internos do Entity Framework que podem afetar o desempenho e para fornecer dicas para investigação e melhorando o desempenho de seus aplicativos que usam o Entity Framework. Há uma série de tópicos de bom desempenho já disponíveis na web e, tentamos apontando para esses recursos sempre que possível.

O desempenho é um assunto complicado. Este white paper destina-se como um recurso para ajudar a tornar o desempenho relacionados a decisões para seus aplicativos que usam o Entity Framework. Incluímos algumas métricas de teste para demonstrar o desempenho, mas essas métricas não se destinam como absolutos indicadores de desempenho, que você verá em seu aplicativo.

Para fins práticos, este documento considera o Entity Framework 4 é executado no .NET 4.0 e o Entity Framework 5 e 6 são executados no .NET 4.5. Muitas das melhorias de desempenho feitas para o Entity Framework 5 residem dentro de componentes principais que acompanham o .NET 4.5.

Entity Framework 6 é um fora da versão de banda e não depende dos componentes do Entity Framework que acompanham o .NET. Entity Framework 6 trabalhar no .NET 4.0 e .NET 4.5 e pode oferecer um benefício de desempenho grande para aqueles que ainda não tiver atualizado do .NET 4.0, mas deseja que os bits mais recentes do Entity Framework em seus aplicativos. Quando este documento menciona o Entity Framework 6, ele se refere a versão mais recente disponível no momento da redação deste artigo: versão 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Frios vs. Execução de consulta quente

Na primeira vez que qualquer consulta é feita em relação a um determinado modelo, o Entity Framework faz muito trabalho nos bastidores para carregar e validar o modelo. Com frequência chamamos essa primeira consulta como uma consulta "fria".  Mais consultas em um modelo carregado já são conhecidas como "passivos" consultas e são muito mais rápidas.

Vamos dar uma visão geral de onde o tempo é gasto ao executar uma consulta usando o Entity Framework e ver onde as coisas estão melhorando no Entity Framework 6.

**Primeira execução da consulta – consulta fria**

| Gravações de usuário do código                                                                                     | Ação                    | EF4 Impacto de desempenho                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 Impacto de desempenho                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 Impacto de desempenho                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Criação de contexto          | Médio                                                                                                                                                                                                                                                                                                                                                                                                                        | Médio                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Criação de expressão de consulta | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                           | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Execução de consulta LINQ      | -Carregamento de metadados: alta, mas em cache <br/> -Exibir geração: potencialmente muito alta, mas em cache <br/> -Avaliação a parâmetro: médio <br/> -Tradução de consulta: médio <br/> -Geração de materializer: médio, mas em cache <br/> -Banco de dados a execução da consulta: potencialmente alto <br/> + Connection <br/> + ExecuteReader <br/> + DataReader.Read <br/> Materialização de objeto: médio <br/> -Pesquisa identity: médio | -Carregamento de metadados: alta, mas em cache <br/> -Exibir geração: potencialmente muito alta, mas em cache <br/> -Avaliação a parâmetro: baixo <br/> -Tradução de consulta: médio, mas em cache <br/> -Geração de materializer: médio, mas em cache <br/> -Banco de dados a execução da consulta: potencialmente alto (melhor consultas em algumas situações) <br/> + Connection <br/> + ExecuteReader <br/> + DataReader.Read <br/> Materialização de objeto: médio <br/> -Pesquisa identity: médio | -Carregamento de metadados: alta, mas em cache <br/> -Exibir geração: médio, mas em cache <br/> -Avaliação a parâmetro: baixo <br/> -Tradução de consulta: médio, mas em cache <br/> -Geração de materializer: médio, mas em cache <br/> -Banco de dados a execução da consulta: potencialmente alto (melhor consultas em algumas situações) <br/> + Connection <br/> + ExecuteReader <br/> + DataReader.Read <br/> Materialização de objeto: médio (mais rápida que o EF5) <br/> -Pesquisa identity: médio |
| `}`                                                                                                  | Close          | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                           | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Segunda execução da consulta – consulta quente**

| Gravações de usuário do código                                                                                     | Ação                    | EF4 Impacto de desempenho                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 Impacto de desempenho                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 Impacto de desempenho                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Criação de contexto          | Médio                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Médio                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Criação de expressão de consulta | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Execução de consulta LINQ      | -Metadata ~~Carregando~~ pesquisa: ~~alta, mas em cache~~ baixo <br/> -Exibir ~~geração~~ pesquisa: ~~potencialmente muito alta, mas em cache~~ baixo <br/> -Avaliação a parâmetro: médio <br/> -Consultar ~~tradução~~ pesquisa: médio <br/> -Materializer ~~geração~~ pesquisa: ~~médio, mas em cache~~ baixo <br/> -Banco de dados a execução da consulta: potencialmente alto <br/> + Connection <br/> + ExecuteReader <br/> + DataReader.Read <br/> Materialização de objeto: médio <br/> -Pesquisa identity: médio | -Metadata ~~Carregando~~ pesquisa: ~~alta, mas em cache~~ baixo <br/> -Exibir ~~geração~~ pesquisa: ~~potencialmente muito alta, mas em cache~~ baixo <br/> -Avaliação a parâmetro: baixo <br/> -Consultar ~~tradução~~ pesquisa: ~~médio, mas em cache~~ baixo <br/> -Materializer ~~geração~~ pesquisa: ~~médio, mas em cache~~ baixo <br/> -Banco de dados a execução da consulta: potencialmente alto (melhor consultas em algumas situações) <br/> + Connection <br/> + ExecuteReader <br/> + DataReader.Read <br/> Materialização de objeto: médio <br/> -Pesquisa identity: médio | -Metadata ~~Carregando~~ pesquisa: ~~alta, mas em cache~~ baixo <br/> -Exibir ~~geração~~ pesquisa: ~~médio, mas em cache~~ baixo <br/> -Avaliação a parâmetro: baixo <br/> -Consultar ~~tradução~~ pesquisa: ~~médio, mas em cache~~ baixo <br/> -Materializer ~~geração~~ pesquisa: ~~médio, mas em cache~~ baixo <br/> -Banco de dados a execução da consulta: potencialmente alto (melhor consultas em algumas situações) <br/> + Connection <br/> + ExecuteReader <br/> + DataReader.Read <br/> Materialização de objeto: médio (mais rápida que o EF5) <br/> -Pesquisa identity: médio |
| `}`                                                                                                  | Close          | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Há várias maneiras para reduzir o custo de desempenho de consultas frios e quentes e vamos dar uma olhada na seção a seguir. Especificamente, vamos examinar reduzindo o custo de carregamento em consultas cold usando modos de exibição gerados previamente, que devem ajudar a minimizar problemas de desempenho durante a geração de exibição do modelo. Para consultas passivos, abordaremos o cache de plano de consulta, nenhuma consulta de acompanhamento e opções de execução de consulta diferentes.

### <a name="21-what-is-view-generation"></a>2.1 o que é a geração de exibição?

Para entender qual exibição geração é, precisamos primeiro compreender quais são os "Modos de exibição de mapeamento". Modos de exibição de mapeamento são representações executáveis das transformações especificadas no mapeamento para cada conjunto de entidades e a associação. Internamente, esses modos de exibição de mapeamento levam a forma de CQTs (árvores canônicas consulta). Há dois tipos de modos de exibição de mapeamento:

-   Modos de exibição de consulta: representam a transformação necessária para o esquema de banco de dados para o modelo conceitual.
-   Atualizar modos de exibição: representam a transformação necessária ir do modelo conceitual para o esquema de banco de dados.

Tenha em mente que o modelo conceitual pode ser diferente do esquema de banco de dados de várias maneiras. Por exemplo, uma única tabela pode ser usada para armazenar os dados para dois tipos de entidade diferentes. Herança e não triviais mapeamentos desempenham um papel na complexidade das exibições de mapeamento.

O processo de computação esses modos de exibição baseados na especificação do mapeamento é o que chamamos de geração de exibição. Geração de exibição ou podem ocorrer dinamicamente quando um modelo é carregado, ou em tempo de compilação, usando "exibições pré-geradas"; o último é serializado na forma de instruções SQL de entidade para a C\# ou arquivo do VB.

Quando os modos de exibição são gerados, eles também são validados. Do ponto de vista do desempenho, a grande maioria do custo de geração de exibição é, na verdade, a validação dos modos de exibição que garante que as conexões entre as entidades fazem sentido e tenham a cardinalidade correta para todas as operações com suporte.

Quando uma consulta ao longo de um conjunto de entidades é executada, a consulta é combinada com o modo de exibição de consulta correspondente, e o resultado dessa composição é executado por meio do compilador plano para criar a representação da consulta que o repositório de backup possa entender. Para o SQL Server, o resultado final desta compilação será uma instrução T-SQL SELECT. Na primeira vez em que uma atualização sobre um conjunto de entidades é executada, o modo de exibição de atualização é executado por meio de um processo semelhante para transformá-lo em instruções DML para o banco de dados de destino.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 fatores que afetam o desempenho de geração de exibição

O desempenho da etapa de geração de exibição depende não apenas o tamanho do seu modelo mas também interconectados como o modelo é. Se duas entidades são conectadas por meio de uma cadeia de herança ou uma associação, eles devem estar conectados. Da mesma forma, se duas tabelas estão conectadas por meio de uma chave estrangeira, eles estão conectados. À medida que aumenta o número de entidades de conectados e tabelas em seus esquemas, a geração de exibição custo aumenta.

O algoritmo que usamos para gerar e validar os modos de exibição é exponencial no pior caso, embora, podemos usar algumas otimizações para melhorar isso. Os principais fatores que parecem afetar negativamente o desempenho são:

-   Tamanho do modelo, referindo-se ao número de entidades e a quantidade de associações entre essas entidades.
-   Complexidade de modelos, especificamente a herança que envolvem um grande número de tipos.
-   Usando associações independente, em vez de associações de chave estrangeira.

Para modelos pequenos e simples, o custo pode ser pequeno o suficiente para não perder tempo com isso usando as exibições pré-geradas. À medida que aumentam de tamanho do modelo e a complexidade, há várias opções disponíveis para reduzir o custo de geração de exibição e a validação.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2.3 usando Pre-Generated modos de exibição para diminuir o modelo de tempo de carregamento

Para obter informações detalhadas sobre como usar exibições pré-gerados no Entity Framework 6, visite [Pre-Generated modos de exibição de mapeamento](~/ef6/fundamentals/performance/pre-generated-views.md)

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>2.3.1 exibições pré-geradas usando o Entity Framework Power Tools Community Edition

Você pode usar o [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) para gerar exibições dos modelos EDMX e o Code First clicando duas vezes o arquivo de classe de modelo e usando o menu do Entity Framework para selecionar "Gerar exibições". O Entity Framework Power Tools Community Edition funcionam somente em contextos derivada de DbContext.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 como usar exibições pré-geradas com um modelo criado pelo EDMGen

EDMGen é um utilitário que é fornecido com o .NET e funciona com o Entity Framework 4 e 5, mas não com o Entity Framework 6. EDMGen permite que você gere um arquivo de modelo, a camada de objeto e os modos de exibição da linha de comando. Uma das saídas será um arquivo de modos de exibição em seu idioma de preferência, VB ou C\#. Este é um arquivo de código que contém trechos de código do Entity SQL para cada conjunto de entidades. Para habilitar exibições pré-geradas, basta incluir o arquivo em seu projeto.

Se você fizer edições manualmente os arquivos de esquema para o modelo, será preciso gerar novamente o arquivo de modos de exibição. Você pode fazer isso executando EDMGen com o **/mode:ViewGeneration** sinalizador.

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 como usar exibições Pre-Generated com um arquivo EDMX

Você também pode usar o EDMGen para gerar os modos de exibição para um arquivo EDMX - tópico do MSDN mencionado anteriormente descreve como adicionar um evento de pré-compilação para fazer isso, mas isso é complicado e há alguns casos em que não é possível. É geralmente mais fácil usar um modelo T4 para gerar os modos de exibição quando o modelo estiver em um arquivo edmx.

Blog da equipe do ADO.NET tem uma postagem que descreve como usar um modelo T4 para geração de exibição ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Esta postagem inclui um modelo que pode ser baixado e adicionado ao seu projeto. O modelo foi gravado para a primeira versão do Entity Framework, portanto, eles não são garantidos para trabalhar com as versões mais recentes do Entity Framework. No entanto, você pode baixar um conjunto de modelos de geração de exibição mais atualizado para o Entity Framework 4 e 5from Galeria do Visual Studio:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Se você estiver usando o Entity Framework 6 você pode obter a exibição de modelos de geração de T4 da Galeria do Visual Studio no \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 reduzindo o custo de geração de exibição

Usar exibições pré-geradas move o custo de geração de exibição do modelo de carregamento (tempo de execução) para tempo de design. Enquanto isso melhora o desempenho de inicialização em tempo de execução, você ainda terá a dificuldade de geração de exibição enquanto você está desenvolvendo. Há várias outras dicas e truques que podem ajudar a reduzir o custo de geração de exibição, em tempo de compilação e tempo de execução.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 usando as associações de chave estrangeira para reduzir o custo de geração de exibição

Temos visto um número de casos em que a troca as associações no modelo de associações independente para as associações de chave estrangeira drasticamente aprimorado o tempo gasto na geração de exibição.

Para demonstrar essa melhoria, geramos duas versões do modelo Navision usando EDMGen. *Observação: seeappendix Cfor uma descrição do modelo Navision.* O modelo Navision é interessante para este exercício devido a sua quantidade muito grande de entidades e relações entre eles.

Uma versão desse modelo muito grandes foi gerada com associações de chaves estrangeiras e outro foi gerado com associações independentes. Podemos então atingiu o tempo quanto tempo levou para gerar os modos de exibição para cada modelo. Teste de Framework5 de entidade usado o método GenerateViews() da classe EntityViewGenerator para gerar os modos de exibição, enquanto o teste do Entity Framework 6 usou o método GenerateViews() da classe StorageMappingItemCollection. Isso devido a reestruturação do código que ocorreram na Base de código do Entity Framework 6.

Usando o Entity Framework 5, geração de exibição para o modelo com chaves estrangeiras levou 65 minutos em um computador de laboratório. Sabe quanto seria necessário para gerar os modos de exibição para o modelo que usado associações independentes. Deixamos que o teste em execução por mais de um mês antes do computador foi reinicializado em nosso laboratório para instalar atualizações mensais.

Usando o Entity Framework 6, geração de exibição para o modelo com chaves estrangeiras levou 28 segundos no mesmo computador do laboratório. Geração de exibição para o modelo que usa associações independentes levou 58 segundos. As melhorias feitas para o Entity Framework 6 em seu código de geração de exibição significam que muitos projetos não será necessário pré-gerado modos de exibição para obter tempos de inicialização mais rápidos.

É importante para o comentário de gerar previamente exibições no Entity Framework 4 e 5 pode ser feito com o Entity Framework Power Tools ou o EDMGen. Para o modo de exibição do Entity Framework 6 geração pode ser feita por meio do Entity Framework Power Tools ou programaticamente, conforme descrito em [modos de exibição de mapeamento Pre-Generated](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 como usar chaves estrangeiras em vez de associações independentes

Ao usar o EDMGen ou Entity Designer no Visual Studio, você obtém FKs por padrão, e demora apenas uma única sinalização de caixa de seleção ou a linha de comando para alternar entre FKs e IAs.

Se você tiver um grande modelo Code First, usando associações independentes terá o mesmo efeito sobre a geração de exibição. Você pode evitar esse impacto, incluindo propriedades de chave estrangeira nas classes para seus objetos dependentes, embora alguns desenvolvedores considerará como ser poluir seu modelo de objeto. Você pode encontrar mais informações sobre esse assunto \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Ao usar      | Faça isto                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity Designer | Depois de adicionar uma associação entre duas entidades, verifique se que você tem uma restrição referencial. Restrições referenciais informar ao Entity Framework para usar as chaves estrangeiras em vez de associações independentes. Para obter mais detalhes, visite \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Ao usar o EDMGen para gerar os arquivos do banco de dados, suas chaves estrangeiras serão respeitadas e adicionados como tal no modelo. Para obter mais informações sobre as diferentes opções expostas pelo EDMGen visite [ http://msdn.microsoft.com/library/bb387165.aspx ](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Consulte a seção "Relação convenção" a [convenções de Code First](~/ef6/modeling/code-first/conventions/built-in.md) tópico para obter informações sobre como incluir propriedades de chave estrangeira em objetos dependentes ao usar o Code First.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 movendo seu modelo para um assembly separado

Quando seu modelo está incluído diretamente no projeto do seu aplicativo e você gera exibições por meio de um evento de pré-Build ou de um modelo T4, geração de exibição e a validação ocorrerá sempre que o projeto for recriado, mesmo se o modelo não foi alterado. Se você move o modelo para um assembly separado e referenciá-lo no projeto do seu aplicativo, você pode fazer outras alterações ao seu aplicativo sem a necessidade de recompilar o projeto que contém o modelo.

*Observação:*  ao mover seu modelo para separar os assemblies Lembre-se de copiar as cadeias de caracteres de conexão para o modelo para o arquivo de configuração de aplicativo do projeto do cliente.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 desabilitar a validação de um modelo baseado em edmx

Modelos EDMX são validados em tempo de compilação, mesmo se o modelo permanece inalterado. Se já tiver sido validado seu modelo, você pode suprimir a validação em tempo de compilação, definindo a propriedade "Validar na compilação" como false na janela Propriedades. Quando você alterar o mapeamento ou modelo, você pode temporariamente reativar validação para verificar suas alterações.

Observe que foram feitas melhorias de desempenho para o Entity Framework Designer para o Entity Framework 6 e o custo do "validar na compilação" é muito menor do que nas versões anteriores do designer.

## <a name="3-caching-in-the-entity-framework"></a>Cache de 3 no Entity Framework

O Entity Framework tem os seguintes formulários de cache interno:

1.  Cache de objeto – ObjectStateManager incorporado a uma instância de ObjectContext mantém o controle na memória dos objetos que foram recuperados usando essa instância. Isso também é conhecido como cache de primeiro nível.
2.  Cache de plano de consulta - reutilizando o comando de armazenamento gerado quando uma consulta é executada mais de uma vez.
3.  Metadados de cache – compartilhando os metadados para um modelo entre diferentes conexões com o mesmo modelo.

Além de caches de que o EF oferece fora da caixa, um tipo especial de provedor de dados ADO.NET conhecido como um provedor de encapsulamento também pode ser usado para estender o Entity Framework com um cache para resultados recuperados do banco de dados, também conhecido como cache de segundo nível.

### <a name="31-object-caching"></a>3.1 do objeto de cache

Por padrão quando uma entidade for retornada nos resultados de uma consulta, antes do EF materializa, ObjectContext verificará se uma entidade com a mesma chave já foi carregada em seu ObjectStateManager. Se já houver uma entidade com as mesmas chaves EF incluirá nos resultados da consulta. Embora o EF ainda emitir a consulta no banco de dados, esse comportamento pode ignorar grande parte do custo de materializar a entidade várias vezes.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 Obtendo entidades do cache de objetos usando DbContext localizar

Ao contrário de uma consulta regular, o método Find no DbSet (APIs incluídas pela primeira vez no EF 4.1) executará uma pesquisa na memória antes de emitir até mesmo a consulta no banco de dados. É importante observar que duas instâncias diferentes do ObjectContext terá duas instâncias diferentes de ObjectStateManager, significando que elas têm os caches do objeto separado.

Localizar usa o valor de chave primária para tentar encontrar uma entidade acompanhada pelo contexto. Se a entidade não está no contexto, em seguida, uma consulta será executada e avaliada no banco de dados e null será retornado se a entidade não for encontrada no contexto ou no banco de dados. Observe que localizar também retorna as entidades que foram adicionadas ao contexto, mas ainda não foi salvo no banco de dados.

Há uma consideração de desempenho a ser tomada quando o uso de Find. Chamadas para esse método por padrão irá disparar uma validação de cache de objetos para detectar alterações ainda aguardando confirmação no banco de dados. Esse processo pode ser muito caro, se houver um número muito grande de objetos no cache de objetos ou em um gráfico de objeto grande, que está sendo adicionado ao cache de objeto, mas também pode ser desabilitado. Em alguns casos, você pode perceber sobre uma ordem de magnitude de diferença na chamada de localizar o método quando você desabilita automaticamente detectar alterações. Ainda, uma segunda ordem de magnitude é percebida quando, na verdade, o objeto está no cache versus quando o objeto tem a ser recuperado do banco de dados. Aqui está um exemplo de grafo com medidas calculadas usando alguns dos nossos microbenchmarks, expressada em milissegundos, com uma carga de 5000 entidades:

![Escala logarítmica do .NET 4.5](~/ef6/media/net45logscale.png ".NET 4.5 – escala logarítmica")

Exemplo de localização com alterações de detecção automática desabilitada:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

O que você deve considerar ao usar o método Find é:

1.  Se o objeto não está em cache os benefícios de localização são negados, mas a sintaxe é ainda mais simples do que uma consulta pela chave.
2.  Se as alterações de detecção automática está habilitada o custo do método Find pode aumentar em uma ordem de magnitude ou ainda mais, dependendo da complexidade do seu modelo e a quantidade de entidades em seu cache de objeto.

Além disso, tenha em mente que encontrar apenas retorna a entidade que você está procurando e ele faz automaticamente carrega suas entidades associadas se elas não ainda estiverem no cache de objetos. Se você precisar recuperar entidades associadas, você pode usar uma consulta por chave com o carregamento adiantado. Para obter mais informações, consulte **8.1 vs o carregamento lento. O carregamento adiantado**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 problemas de desempenho de quando o cache de objetos tem muitas entidades

O cache de objetos ajuda a aumentar a capacidade de resposta geral do Entity Framework. No entanto, quando o cache de objetos tem uma grande quantidade de entidades carregadas pode afetar determinadas operações, como adicionar, remover, localizar, entrada, SaveChanges e muito mais. Em particular, operações que disparam uma chamada para DetectChanges serão afetadas negativamente por caches do objeto muito grande. DetectChanges sincroniza o grafo de objeto com o Gerenciador de estado do objeto e sua vontade de desempenho determinado diretamente pelo tamanho do grafo do objeto. Para obter mais informações sobre DetectChanges, consulte [Controlando alterações em entidades POCO](https://msdn.microsoft.com/library/dd456848.aspx).

Ao usar o Entity Framework 6, os desenvolvedores são capazes de chamar AddRange e RemoveRange diretamente em um DbSet, em vez de iterar em uma coleção e chamar a adicionar uma vez por instância. A vantagem de usar os métodos range é que o custo de DetectChanges é pago apenas uma vez para todo o conjunto de entidades em vez de uma vez por cada entidade adicionada.

### <a name="32-query-plan-caching"></a>3.2 cache de plano de consulta de

Na primeira vez que uma consulta é executada, ele passa pelo compilador plano interno para traduzir a consulta conceitual para o comando de armazenamento (por exemplo, o T-SQL que é executado quando executado no SQL Server).  Se o cache de plano de consulta estiver habilitado, na próxima vez em que a consulta é executada o repositório de comando é recuperado diretamente do cache de plano de consulta para execução, ignorando o compilador de plano.

O cache do plano de consulta é compartilhado entre instâncias de ObjectContext dentro do mesmo AppDomain. Você não precisa manter uma instância de ObjectContext para se beneficiar do cache do plano de consulta.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 algumas observações sobre o cache de plano de consulta

-   O cache do plano de consulta é compartilhado para todos os tipos de consulta: Entity SQL, LINQ to Entities e objetos CompiledQuery.
-   Por padrão, o cache do plano de consulta está habilitado para consultas de Entity SQL, se executados por meio de um EntityCommand ou um ObjectQuery. Ele também é habilitado por padrão para LINQ para consultas de entidades no Entity Framework no .NET 4.5 e no Entity Framework 6
    -   Cache de plano de consulta pode ser desabilitado definindo a propriedade EnablePlanCaching (em EntityCommand ou ObjectQuery) como false. Por exemplo:
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   Para consultas parametrizadas, alterar o valor do parâmetro serão atingidos ainda a consulta em cache. Mas a alteração de facetas de um parâmetro (por exemplo, tamanho, precisão ou escala) atingirá uma entrada diferente no cache.
-   Ao usar o Entity SQL, a cadeia de caracteres de consulta é parte da chave. Alterar a consulta em todos os resultará em entradas de cache diferentes, mesmo se as consultas são funcionalmente equivalentes. Isso inclui alterações de maiusculas e minúsculas ou espaço em branco.
-   Ao usar o LINQ, a consulta é processada para gerar uma parte da chave. Alterando a expressão LINQ, portanto, gerará uma chave diferente.
-   Outras limitações técnicas podem ser aplicadas; Consulte Autocompiled consultas para obter mais detalhes.

#### <a name="322-cache-eviction-algorithm"></a>3.2.2 algoritmo de remoção do cache

Noções básicas sobre como funciona o algoritmo interno irá ajudá-lo a descobrir quando para habilitar ou desabilitar o cache de plano de consulta. O algoritmo de limpeza é da seguinte maneira:

1.  Depois que o cache contém um número definido de entradas (800), podemos iniciar um timer que periodicamente (uma vez por minuto) varre o cache.
2.  Durante limpezas de cache, as entradas são removidas do cache em um LFRU (menos com frequência – usados recentemente) Base. Esse algoritmo leva contagem de ocorrências e a idade em conta ao decidir quais entradas são desconsideradas.
3.  No final de cada varredura de cache, o cache novamente contém 800 entradas.

Todas as entradas de cache são tratadas igualmente ao determinar quais entradas para remover. Isso significa que o comando de armazenamento para um CompiledQuery tem a mesma chance de remoção que o comando de armazenamento para uma consulta Entity SQL.

Observe que o timer de remoção do cache é iniciado quando houver 800 entidades no cache, mas o cache é apenas varridas para 60 segundos depois que esse temporizador é iniciado. Isso significa que para até 60 segundos o cache pode atingir a ser muito grandes.

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 métricas demonstrando o desempenho do cache de plano de consulta de teste

Para demonstrar o efeito de plano de consulta em cache no desempenho do seu aplicativo, realizamos um teste no qual executamos um número de consultas do Entity SQL com base no modelo Navision. Consulte o Apêndice para obter uma descrição do modelo Navision e os tipos de consultas que foram executadas. Este teste, primeiro iterar na lista de consultas e executar cada um de uma vez para adicioná-los no cache (se o cache é habilitado). Esta etapa é untimed. Em seguida, podemos suspender o thread principal para mais de 60 segundos permitir que o cache de varredura para assumir o lugar; Por fim, iteramos por meio de tempo a lista um 2º para executar as consultas armazenadas em cache. Além disso, ele cache de plano do SQL Server é liberado antes de cada conjunto de consultas é executado para que os horários que obtemos com precisão refletem o benefício dado pelo cache de plano de consulta.

##### <a name="3231-test-results"></a>3.2.3.1 resultados de teste

| Teste                                                                   | EF5 sem cache | EF5 armazenados em cache | EF6 sem cache | EF6 armazenados em cache |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Enumerando todas as 18723 consultas                                          | 124          | 125.4      | 124.3        | 125.3      |
| Evitando varredura (somente as primeiro 800 consultas, independentemente da complexidade)  | 41.7         | 5.5        | 40.5         | 5.4        |
| Somente as consultas AggregatingSubtotals (178 total - que evita a varredura) | 39.5         | 4.5        | 38.1         | 4.6        |

*Todo o tempo em segundos.*

Moral - quando a execução muito de consultas distintas (por exemplo, criado dinamicamente consultas), armazenamento em cache não ajuda e a liberação resultante do cache pode manter as consultas que pode se beneficiar ao máximo do cache de plano de realmente usá-lo.

As consultas de AggregatingSubtotals são as consultas que testamos com mais complexos. Conforme o esperado, quanto mais complexa a consulta é, mais benefícios terá do cache do plano de consulta.

Como um CompiledQuery realmente é uma consulta LINQ com seu plano armazenado em cache, a comparação de um CompiledQuery versus a consulta Entity SQL equivalente deve ter resultados semelhantes. Na verdade, se um aplicativo tiver muitas consultas de Entity SQL dinâmicas, preenchimento do cache com consultas também efetivamente causará CompiledQueries a "descompilação" quando eles são liberados do cache. Nesse cenário, o desempenho pode ser melhorado ao desabilitar o cache nas consultas dinâmicas para priorizar o CompiledQueries. Melhor ainda, é claro, seria reescrever o aplicativo para usar consultas parametrizadas, em vez de consultas dinâmicas.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 usando CompiledQuery para melhorar o desempenho com consultas LINQ

Nossos testes indicarem que usar CompiledQuery pode trazer uma vantagem de 7% autocompiled consultas LINQ; Isso significa que você gastará 7% menos tempo de execução de código da pilha do Entity Framework; Isso não significa que seu aplicativo será 7% mais rápida. Em termos gerais, o custo de escrever e manter objetos CompiledQuery no EF 5.0 pode não ser que vale a pena o trabalho em comparação com os benefícios. Sua milhagem pode variar, exercício então, essa opção se seu projeto requer o envio por push adicional. Observe que CompiledQueries só são compatíveis com modelos derivada de ObjectContext e não é compatível com modelos derivada de DbContext.

Para obter mais informações sobre como criar e invocar um CompiledQuery, consulte [consultas compiladas (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Há duas considerações que você precisa tomar ao usar um CompiledQuery, ou seja, o requisito para usar instâncias estáticas e os problemas que eles têm com capacidade de composição. Apresentamos a seguir uma explicação detalhada sobre essas duas considerações.

#### <a name="331-use-static-compiledquery-instances"></a>3.3.1 use instâncias estáticas de CompiledQuery

Como compilar uma consulta LINQ é um processo demorado, não queremos fazer isso sempre que precisamos para buscar dados do banco de dados. As instâncias de CompiledQuery permitem que você compile uma vez e executado várias vezes, mas você precisa ter cuidado e adquiridos para usar a mesma instância de CompiledQuery novamente cada vez, em vez de compilá-lo repetidamente. O uso de membros estáticos para armazenar as instâncias de CompiledQuery se torna necessário; Caso contrário, você não verá nenhum benefício.

Por exemplo, suponha que sua página tem o seguinte corpo de método para lidar com exibindo os produtos para a categoria selecionada:

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

Nesse caso, você criará uma nova instância de CompiledQuery dinamicamente sempre que o método é chamado. Em vez de ver os benefícios de desempenho, recuperando o comando de armazenamento de cache do plano de consulta, o CompiledQuery passará pelo compilador plano sempre que uma nova instância é criada. Na verdade, você irá ser poluir seu cache do plano de consulta com uma nova entrada CompiledQuery sempre que o método é chamado.

Em vez disso, você deseja criar uma instância estática da consulta compilada, portanto, você está invocando a mesma consulta compilada sempre que o método é chamado. Uma maneira para que isso seja adicionando-se a instância de CompiledQuery como um membro de seu contexto de objeto.  Em seguida, você pode tornar as coisas um pouco cartucho de limpeza, acessando o CompiledQuery através de um método auxiliar:

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

Esse método auxiliar seria invocado da seguinte maneira:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a>3.3.2 compondo ao longo de um CompiledQuery

A capacidade de composição ao longo de qualquer consulta LINQ é extremamente útil; Para fazer isso, você simplesmente invoca um método após o IQueryable como *Skip* ou *Count ()*. No entanto, fazer, então, essencialmente, retorna um novo objeto IQueryable. Embora não haja nada tecnicamente de composição ao longo de um CompiledQuery, isso fará com que a geração de um novo objeto IQueryable que exige passar por meio do compilador plano novamente.

Alguns componentes usarão IQueryable composto objetos para habilitar a funcionalidade avançada. Por exemplo, o ASP. GridView da rede pode ser associada a dados para um objeto IQueryable por meio da propriedade SelectMethod. O GridView será, em seguida, compor sobre esse objeto IQueryable para permitir a classificação e paginação em relação ao modelo de dados. Como você pode ver, usando um CompiledQuery do GridView não seria atingidos a consulta compilada, mas geraria uma nova consulta autocompiled.

A equipe consultiva para clientes discute isso na sua postagem do blog "Potencial desempenho problemas com compilado LINQ consulta recompilações": <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.

Um lugar onde você pode enfrentar isso é ao adicionar filtros progressivos para uma consulta. Por exemplo, suponha que você tivesse uma página de clientes com várias listas suspensas de filtros opcionais (por exemplo, país e OrdersCount). Você pode compor esses filtros nos resultados IQueryable de um CompiledQuery, mas fazer isso resultará na nova consulta vai por meio do compilador plano sempre que você executá-lo.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Para evitar essa recompilação, você pode reescrever o CompiledQuery para levar os possíveis filtros em consideração:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Qual seria invocado na interface do usuário, como:

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Uma desvantagem aqui é o comando de repositório gerada sempre terá os filtros que as verificações de null, mas eles devem ser bastante simples para o servidor de banco de dados otimizar o:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 cache de metadados

O Entity Framework também oferece suporte a cache de metadados. Isso essencialmente é armazenar em cache de informações de tipo e informações de mapeamento de tipo para banco de dados em conexões diferentes para o mesmo modelo. O cache de metadados é exclusivo por AppDomain.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 algoritmo de cache de metadados

1.  Informações de metadados para um modelo são armazenadas em um ItemCollection para cada EntityConnection.
    -   Como uma observação adicional, há objetos ItemCollection diferentes para diferentes partes do modelo. Por exemplo, StoreItemCollections contém as informações sobre o modelo de banco de dados. ObjectItemCollection contém informações sobre o modelo de dados. EdmItemCollection contém informações sobre o modelo conceitual.

2.  Se duas conexões usam a mesma cadeia de conexão, eles compartilharão a mesma instância ItemCollection.
3.  Cadeias de caracteres de conexão funcionalmente equivalentes, mas textualmente diferentes podem resultar em caches de metadados diferentes. Podemos criar tokens de cadeias de caracteres de conexão, simplesmente alterando a ordem dos tokens deve resultar em metadados compartilhados. Mas, duas cadeias de caracteres de conexão que parecem funcionalmente o mesmo não podem ser avaliadas como idênticos após a geração de tokens.
4.  A ItemCollection periodicamente é verificada para uso. Se for determinado que um espaço de trabalho não tiver sido acessado recentemente, ele será marcado para a varredura de cache próxima limpeza.
5.  Apenas a criação de uma EntityConnection fará com que um cache de metadados a ser criado (embora as coleções de itens em que ele não serão inicializadas até que a conexão é aberta). Este espaço de trabalho permanece na memória até que o algoritmo do cache determina que ele não é "em uso".

A equipe consultiva para clientes escreveu uma postagem de blog que descreve o que contém uma referência a um ItemCollection para evitar "substituição" ao usar modelos grandes: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 a relação entre o cache de metadados e o cache de plano de consulta

A instância de cache do plano de consulta reside em ItemCollection do MetadataWorkspace de tipos de armazenamento. Isso significa que os comandos de armazenamento em cache serão usados para consultas em qualquer contexto instanciada usando um determinado MetadataWorkspace. Isso também significa que se você tiver duas cadeias de caracteres de conexões que são um pouco diferentes e não coincidem depois de criar tokens, você terá consulta diferentes instâncias do cache de plano.

### <a name="35-results-caching"></a>3.5 resultados de de cache

Com os resultados de cache (também conhecido como "cache de segundo nível"), você pode manter os resultados de consultas em um cache local. Ao emitir uma consulta, você primeiro ver se os resultados estão disponíveis localmente antes de você consulta em relação ao armazenamento. Enquanto o cache de resultados não são suportados diretamente pelo Entity Framework, é possível adicionar um cache de segundo nível, usando um provedor de encapsulamento. Um provedor de encapsulamento de exemplo com um cache de segundo nível é do Alachisoft [Cache de segundo nível Entity Framework com base em NCache](http://www.alachisoft.com/ncache/entity-framework.html).

Essa implementação do cache de segundo nível é uma funcionalidade injetada que usa o local depois que tiver sido avaliada com a expressão LINQ (e funcletized) e o plano de execução de consulta é computado ou recuperado do cache de primeiro nível. O cache de segundo nível, em seguida, armazenará apenas os resultados de banco de dados brutos, portanto, o pipeline de materialização ainda executa posteriormente.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 referências adicionais do para armazenamento em cache com o provedor de encapsulamento de resultados

-   Julie Lerman escreveu um artigo do MSDN "Segundo nível de cache no Entity Framework e Windows Azure" que inclui como atualizar o provedor de encapsulamento de exemplo para usar o Windows Server AppFabric caching: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Se você estiver trabalhando com o Entity Framework 5, o blog da equipe tem uma postagem que descreve como executar tarefas em execução com o provedor de cache para o Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Ele também inclui um modelo T4 para ajudar a automatizar a adição de cache de 2º nível ao seu projeto.

## <a name="4-autocompiled-queries"></a>Consultas de Autocompiled 4

Quando uma consulta é feita em um banco de dados usando o Entity Framework, precisa passar por uma série de etapas antes de realmente materializar resultados; um essa etapa é a compilação da consulta. Consultas Entity SQL eram conhecidas por ter bom desempenho conforme eles são automaticamente armazenadas em cache, portanto, a segunda ou terceira vez que você executar a mesma consulta pode ignorar o compilador de plano e usar o plano armazenado em cache em vez disso.

O Entity Framework 5 introduziu o armazenamento em cache automático para LINQ para consultas de entidades, bem. Em edições anteriores do Entity Framework, criando um CompiledQuery para acelerar o desempenho foi uma prática comum, pois isso tornaria o LINQ para consultar entidades armazenáveis em cache. Uma vez que o cache agora é feito automaticamente sem o uso de um CompiledQuery, chamamos esse recurso "autocompiled consultas". Para obter mais informações sobre o cache do plano de consulta e sua mecânica, consulte o cachê do plano de consulta.

Entity Framework detecta quando uma consulta precisa ser recompilado, e faz isso quando a consulta é invocada, mesmo se ele tivesse sido compilado antes. Condições comuns que fazem com que a consulta seja recompilada são:

-   Alterando o MergeOption associado à sua consulta. A consulta em cache não será usada, em vez disso, o compilador de plano será executado novamente e o plano recém-criado é armazenado em cache.
-   Alterar o valor de ContextOptions.UseCSharpNullComparisonBehavior. Você obtém o mesmo efeito de alterar o MergeOption.

Outras condições podem impedir que a consulta usando o cache. Exemplos comuns são:

-   Usando o IEnumerable&lt;T&gt;. Contém&lt;&gt;(valor de T).
-   Usando funções que geram consultas com constantes.
-   Usando as propriedades de um objeto não mapeados.
-   Vinculando a consulta a outra consulta que requer ser recompilado.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 usando IEnumerable&lt;T&gt;. Contém&lt;T&gt;(valor de T)

Entity Framework não armazena em cache consultas que invocam IEnumerable&lt;T&gt;. Contém&lt;T&gt;(valor de T) em relação a uma coleção em memória, como os valores da coleção são considerados voláteis. A consulta de exemplo a seguir não será armazenada, portanto, ele sempre será processado pelo compilador plano:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Observe que o tamanho de IEnumerable em relação a quais Contains é executado determina o quão rápido ou lento como sua consulta é compilada. Desempenho poderá ser afetado de forma significativa ao uso de coleções grandes, como mostrado no exemplo acima.

Entity Framework 6 contém otimizações na forma IEnumerable&lt;T&gt;. Contém&lt;T&gt;(valor de T) funciona quando as consultas são executadas. O código SQL gerado é muito mais rápido para produzir e mais legível e, na maioria dos casos ele também executa com mais rapidez no servidor.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 usando funções que geram consultas com constantes

Os operadores Skip, Take (,) Contains () e LINQ DefautIfEmpty() gera consultas SQL com parâmetros, mas em vez disso, coloque os valores passados a eles como constantes. Por isso, consultas que poderiam ser idêntica acaba poluir a consulta planejar o cache, na pilha de EF e o servidor de banco de dados e não obter reutilizadas, a menos que o mesmas constantes são usadas em uma execução de consulta subsequentes. Por exemplo:

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

Neste exemplo, cada vez que essa consulta é executada com um valor diferente para a id da consulta será compilado em um novo plano.

Em particular preste atenção o uso de Skip e Take ao fazer a paginação. No EF6, esses métodos têm uma sobrecarga de lambda que torna efetivamente o plano de consulta em cache reutilizáveis porque o EF pode capturar variáveis passadas para esses métodos e convertê-las em SQLparameters. Isso também ajuda a manter o cache de limpeza, pois, caso contrário, cada consulta com uma constante diferente de Skip e Take teria sua própria entrada de cache do plano de consulta.

Considere o código a seguir, que está abaixo do ideal, mas serve para exemplificar essa classe de consultas:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Uma versão mais rápida desse mesmo código envolveria chamando Skip com um lambda:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

O segundo trecho pode executar até 11% mais rápido porque o mesmo plano de consulta é usado sempre que a consulta é executada, o que economiza tempo de CPU e evita poluam o cache de consulta. Além disso, porque o parâmetro para ignorar está em um fechamento o código também essa aparência agora:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 usando as propriedades de um objeto não mapeados

Quando uma consulta usa as propriedades de um tipo de objeto não mapeados como um parâmetro, a consulta será não ficar armazenada em cache. Por exemplo:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

Neste exemplo, suponha que a classe NonMappedType não é parte do modelo de entidade. Essa consulta pode ser alterada facilmente para não usar um tipo não mapeados e em vez disso, use uma variável local como o parâmetro para a consulta:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

Nesse caso, a consulta poderá ficar armazenada em cache e se beneficiará do cache do plano de consulta.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 vinculando a consultas que requerem a recompilação

Seguindo o mesmo exemplo acima, se você tiver uma segunda consulta que se baseia em uma consulta que precisa ser recompilado, toda segunda consulta também será recompilada. Aqui está um exemplo para ilustrar esse cenário:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

O exemplo é genérico, mas ilustra como a vinculação a firstQuery está causando secondQuery não possa ficar armazenada em cache. Se firstQuery não tivesse sido uma consulta que requer a recompilação, em seguida, secondQuery seria tiverem sido armazenados em cache.

## <a name="5-notracking-queries"></a>5 consultas NoTracking

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 desabilitando o controle de alterações para reduzir a sobrecarga de gerenciamento de estado

Se você estiver em um cenário de somente leitura e deseja evitar a sobrecarga de carregar os objetos no ObjectStateManager, você pode emitir consultas de "Sem o controle".  Controle de alterações podem ser desabilitada no nível da consulta.

No entanto, observe que ao desabilitar a você o controle de alterações são efetivamente desativar o cache de objetos. Quando você consulta para uma entidade, podemos não é possível ignorar materialização puxando os resultados da consulta materializado anteriormente do ObjectStateManager. Se você estiver consultando repetidamente as mesmas entidades no mesmo contexto, você pode realmente ver um desempenho se beneficiar da habilitação do controle de alterações.

Ao consultar o uso de ObjectContext, instâncias ObjectQuery e ObjectSet irão se lembrar um MergeOption depois que ele é definido e consultas que são compostos por neles herdará o MergeOption efetivação da consulta pai. Ao usar o DbContext, o rastreamento pode ser desabilitado chamando o modificador AsNoTracking() no DbSet.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 desabilitando o controle de alterações para uma consulta ao usar o DbContext

Você pode alternar o modo de uma consulta para NoTracking por meio do encadeamento uma chamada ao método AsNoTracking() na consulta. Ao contrário de ObjectQuery, as classes de DbSet e DbQuery na API do DbContext não tem uma propriedade mutável para o MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 desabilitando o controle de alterações no nível da consulta usando ObjectContext

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 desabilitando o controle de alterações para uma entidade inteira definida usando ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 métricas de teste demonstrando o benefício de desempenho de consultas NoTracking

Nesse teste, vamos às custas de preencher o ObjectStateManager comparando o acompanhamento para consultas NoTracking para o modelo Navision. Consulte o Apêndice para obter uma descrição do modelo Navision e os tipos de consultas que foram executadas. Nesse teste, podemos percorrer a lista de consultas e executar uma vez, cada um deles. Executamos duas variações do teste, uma vez com consultas NoTracking e uma vez com a opção de mesclagem padrão de "AppendOnly". Executamos cada variação de 3 vezes e levar o valor médio das execuções. Entre os testes, limpar o cache de consulta no SQL Server e reduzir o tempdb, executando os comandos a seguir:

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Teste os resultados, mediano executa mais de 3:

|                        | NENHUM CONTROLE – CONJUNTO DE TRABALHO | NENHUM RASTREAMENTO – HORA | ACRESCENTAR APENAS – CONJUNTO DE TRABALHO | TEMPO – SOMENTE DE ACRÉSCIMO |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **O Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

O Entity Framework 5 terão um volume de memória menor no final da execução que o Entity Framework 6. A memória adicional consumida pelo Entity Framework 6 é o resultado de estruturas de memória adicional e o código que habilitam novos recursos e melhorar o desempenho.

Também há uma diferença clara na superfície de memória ao usar o ObjectStateManager. O Entity Framework 5 aumentou seu impacto em 30% ao manter o controle de todas as entidades que podemos materializados do banco de dados. Entity Framework 6 aumenta sua pegada, 28% ao fazer isso.

Em termos de tempo, o Entity Framework 6 supera o Entity Framework 5 neste teste por uma grande margem. Entity Framework 6 concluiu o teste em aproximadamente 16% do tempo consumido pelo Entity Framework 5. Além disso, o Entity Framework 5 leva 9% mais tempo para ser concluído quando o ObjectStateManager está sendo usado. Em comparação, o Entity Framework 6 está usando mais tempo ao usar o ObjectStateManager de % 3.

## <a name="6-query-execution-options"></a>Opções de execução de consulta 6

Entity Framework oferece várias maneiras diferentes de consulta. Vamos examinar as opções a seguir, comparar os prós e contras de cada e examinar suas características de desempenho:

-   LINQ to Entities.
-   Nenhum rastreamento LINQ to Entities.
-   Entity SQL em um ObjectQuery.
-   Entity SQL ao longo de um EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-linq-to-entities-queries"></a>6.1 consultas LINQ to Entities

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Profissionais**

-   Adequado para operações CUD.
-   Objetos totalmente materializados.
-   Mais simples de escrever com sintaxe integrada à linguagem de programação.
-   Bom desempenho.

**Contras**

-   Determinadas restrições técnicas, tais como:
    -   Padrões para consultas de junção externa. usando DefaultIfEmpty resultam em consultas mais complexas que instruções simples OUTER JOIN em Entity SQL.
    -   Você ainda não é possível usar LIKE com a correspondência de padrões gerais.

### <a name="62-no-tracking-linq-to-entities-queries"></a>6.2 nenhum acompanhamento LINQ para consultas de entidades

Quando o contexto deriva ObjectContext:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Quando o contexto deriva de DbContext:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Profissionais**

-   Desempenho aprimorado em consultas LINQ regulares.
-   Objetos totalmente materializados.
-   Mais simples de escrever com sintaxe integrada à linguagem de programação.

**Contras**

-   Não é adequado para operações CUD.
-   Determinadas restrições técnicas, tais como:
    -   Padrões para consultas de junção externa. usando DefaultIfEmpty resultam em consultas mais complexas que instruções simples OUTER JOIN em Entity SQL.
    -   Você ainda não é possível usar LIKE com a correspondência de padrões gerais.

Observe que consultas que propriedades escalares do projeto não são controladas, mesmo se o NoTracking não for especificado. Por exemplo:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Essa consulta específica não especifica explicitamente que está sendo NoTracking, mas uma vez que não a materialização um tipo que se sabe que o Gerenciador de estado do objeto, em seguida, o resultado materializado não é controlado.

### <a name="63-entity-sql-over-an-objectquery"></a>6.3 entity SQL sobre um ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Profissionais**

-   Adequado para operações CUD.
-   Objetos totalmente materializados.
-   Dá suporte a cache de plano de consulta.

**Contras**

-   Envolve cadeias de caracteres de consulta textual que são mais propensas a erro de usuário que construções de consulta integradas à linguagem.

### <a name="64-entity-sql-over-an-entity-command"></a>6.4 entity SQL sobre um comando de entidade

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**Profissionais**

-   Dá suporte à consulta o cache de plano no .NET 4.0 (cache de plano é suportado por todos os outros tipos de consulta no .NET 4.5).

**Contras**

-   Envolve cadeias de caracteres de consulta textual que são mais propensas a erro de usuário que construções de consulta integradas à linguagem.
-   Não é adequado para operações CUD.
-   Os resultados não são materializados automaticamente e devem ser lido do leitor de dados.

### <a name="65-sqlquery-and-executestorequery"></a>6.5 SqlQuery e ExecuteStoreQuery

SqlQuery no banco de dados:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery no DbSet:

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**Profissionais**

-   Geralmente, desempenho mais rápido, pois o compilador de plano é ignorado.
-   Objetos totalmente materializados.
-   Adequado para operações de CUD quando usados a partir o DbSet.

**Contras**

-   Consulta é textual e sujeito a erros.
-   Consulta está ligada a um back-end específico usando a semântica do repositório em vez de semântica conceitual.
-   Quando a herança está presente, consulta artesanal precisa levar em conta condições de mapeamento para o tipo solicitado.

### <a name="66-compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Profissionais**

-   Fornece até uma melhoria de desempenho de % 7 em consultas LINQ regulares.
-   Objetos totalmente materializados.
-   Adequado para operações CUD.

**Contras**

-   Maior complexidade e sobrecarga de programação.
-   A melhoria de desempenho é perdida quando a composição na parte superior de uma consulta compilada.
-   Algumas consultas LINQ não podem ser gravadas como um CompiledQuery - por exemplo, as projeções de tipos anônimos.

### <a name="67-performance-comparison-of-different-query-options"></a>6.7 comparação de desempenho de das opções de consulta diferentes

Microbenchmarks simples em que a criação de contexto não tenha sido programada foram inseridos para o teste. Medimos consultando 5000 vezes para um conjunto de entidades não armazenado em cache em um ambiente controlado. Esses números devem ser criados com um aviso: eles não refletem os números reais produzidos por um aplicativo, mas em vez disso, eles são uma medida bastante precisa da quantidade de uma diferença de desempenho que não há quando as opções de consulta diferentes são comparadas maçãs-com-maçãs, excluindo o custo de criação de um novo contexto.

| EF  | Teste                                 | Tempo (ms) | Memória   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | Consulta de Linq ObjectContext             | 2692      | 38277120 |
| EF5 | Nenhum rastreamento de consulta do Linq de DbContext     | 2818      | 41840640 |
| EF5 | Consulta de Linq DbContext                 | 2930      | 41771008 |
| EF5 | Nenhum rastreamento de consulta do Linq de ObjectContext | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | Consulta de Linq ObjectContext             | 3074      | 45248512 |
| EF6 | Nenhum rastreamento de consulta do Linq de DbContext     | 3125      | 47575040 |
| EF6 | Consulta de Linq DbContext                 | 3420      | 47652864 |
| EF6 | Nenhum rastreamento de consulta do Linq de ObjectContext | 3593      | 45260800 |

![EF5 microbenchmarks, 5000 iterações quentes](~/ef6/media/ef5micro5000warm.png)

![EF6 microbenchmarks, 5000 iterações quentes](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks são muito sensíveis a pequenas alterações no código. Nesse caso, a diferença entre os custos do Entity Framework 5 e o Entity Framework 6 são devido à adição de [interceptação](~/ef6/fundamentals/logging-and-interception.md) e [melhorias transacionais](~/ef6/saving/transactions.md). Esses números microbenchmarks, no entanto, são uma visão elevada em um fragmento muito pequena do que o Entity Framework faz. Cenários do mundo real de consultas passivos não deverá ver uma regressão de desempenho ao atualizar do Entity Framework 5 para o Entity Framework 6.

Para comparar o desempenho das opções de consulta diferentes no mundo real, nós criamos 5 variações de teste separado, em que podemos usar uma opção de consulta diferente para selecionar todos os produtos cujo nome de categoria é "Bebidas". Cada iteração inclui o custo da criação do contexto e o custo de materializar entidades retornadas tudo. 10 iterações são executadas untimed antes de fazer a soma de atingiu o tempo limitado de 1000 iterações. Os resultados mostrados são a execução mediana tirada 5 execuções de cada teste. Para obter mais informações, consulte o Apêndice B, que inclui o código de teste.

| EF  | Teste                                        | Tempo (ms) | Memória   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | Comando de entidade de ObjectContext                | 621       | 39350272 |
| EF5 | Consulta Sql de DbContext no banco de dados             | 825       | 37519360 |
| EF5 | Consulta de Store de ObjectContext                   | 878       | 39460864 |
| EF5 | Nenhum rastreamento de consulta do Linq de ObjectContext        | 969       | 38293504 |
| EF5 | ObjectContext Entity Sql usando o objeto de consulta | 1089      | 38981632 |
| EF5 | Consulta compilada do ObjectContext                | 1099      | 38682624 |
| EF5 | Consulta de Linq ObjectContext                    | 1152      | 38178816 |
| EF5 | Nenhum rastreamento de consulta do Linq de DbContext            | 1208      | 41803776 |
| EF5 | Consulta Sql de DbContext no DbSet                | 1414      | 37982208 |
| EF5 | Consulta de Linq DbContext                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | Comando de entidade de ObjectContext                | 480       | 47247360 |
| EF6 | Consulta de Store de ObjectContext                   | 493       | 46739456 |
| EF6 | Consulta Sql de DbContext no banco de dados             | 614       | 41607168 |
| EF6 | Nenhum rastreamento de consulta do Linq de ObjectContext        | 684       | 46333952 |
| EF6 | ObjectContext Entity Sql usando o objeto de consulta | 767       | 48865280 |
| EF6 | Consulta compilada do ObjectContext                | 788       | 48467968 |
| EF6 | Nenhum rastreamento de consulta do Linq de DbContext            | 878       | 47554560 |
| EF6 | Consulta de Linq ObjectContext                    | 953       | 47632384 |
| EF6 | Consulta Sql de DbContext no DbSet                | 1023      | 41992192 |
| EF6 | Consulta de Linq DbContext                        | 1290      | 47529984 |


![Iterações do EF5 passiva consulta 1000](~/ef6/media/ef5warmquery1000.png)

![Iterações do EF6 passiva consulta 1000](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Para fins de integridade, incluímos uma variação onde executamos uma consulta Entity SQL em um EntityCommand. No entanto, porque os resultados não são materializados para tais consultas, a comparação não é necessariamente igual para igual. O teste inclui uma aproximação para materializar tentar fazer a comparação obter.

Nesse caso de ponta a ponta, o Entity Framework 6 supera o Entity Framework 5 devido aos aprimoramentos de desempenho feitos em várias partes da pilha, incluindo uma inicialização muito mais leve do DbContext e mais rápido MetadataCollection&lt;T&gt; pesquisas.

## <a name="7-design-time-performance-considerations"></a>Considerações de desempenho de tempo de design 7

### <a name="71-inheritance-strategies"></a>7.1 estratégias de herança

Outra consideração de desempenho ao usar o Entity Framework é a estratégia de herança que você usar. Entity Framework dá suporte a 3 tipos básicos de herança e suas combinações:

-   Tabela por hierarquia (TPH) – em que cada herança definida mapas em uma tabela com uma coluna discriminatória para indicar qual tipo específico na hierarquia está sendo representada na linha.
-   Tabela por tipo (TPT) – em que cada tipo tem sua própria tabela no banco de dados; as tabelas filho apenas definem as colunas que não contém a tabela pai.
-   Tabela por classe (TPC) – em que cada tipo tem sua própria tabela completa no banco de dados; as tabelas filho definem todos os campos, incluindo aqueles definidos em tipos pai.

Se seu modelo usa herança TPT, as consultas que são geradas será mais complexas do que aquelas que são gerados com as outras estratégias de herança, que podem resultar em tempos de execução no repositório.  Geralmente, levará mais tempo para gerar consultas em um modelo de TPT e materializar os objetos resultantes.

Consulte as "Considerações de desempenho ao usar herança TPT (tabela por tipo) no Entity Framework" postagem de blog do MSDN: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 evitar TPT em aplicativos Model First ou Code First

Quando você cria um modelo em um banco de dados existente que tenha um esquema TPT, você não tem muitas opções. Mas, ao criar um aplicativo usando Model First ou Code First, você deve evitar herança TPT para questões de desempenho.

Quando você usa o Model First no Entity Designer de assistente, você obterá TPT para qualquer herança em seu modelo. Se você quiser mudar para uma estratégia de herança TPH com Model First, você pode usar o "Entity Designer de banco de dados Generation Power Pack" disponíveis na Galeria do Visual Studio ( \< http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Ao usar o Code First para configurar o mapeamento de um modelo com a herança, o EF usará TPH por padrão, portanto todas as entidades na hierarquia de herança serão mapeadas para a mesma tabela. Consulte a seção "Mapeamento com a API Fluent" do artigo "Código primeiro na entidade Framework4.1" na MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) para obter mais detalhes.

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 para atualizar do EF4 para melhorar a geração de modelo de tempo

Uma melhoria específica do SQL Server para o algoritmo que gera a camada de armazenamento (SSDL) do modelo está disponível no Entity Framework 5 e 6 e como uma atualização para o Entity Framework 4 ao Visual Studio 2010 SP1 está instalado. Os resultados de teste a seguir demonstram a melhoria ao gerar um modelo muito grande, neste caso, o modelo Navision. Consulte o Apêndice C para obter mais detalhes sobre ele.

O modelo contém conjuntos de entidade 1005 e 4227 associações.

| Configuração                              | Divisão de tempo consumido                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | Geração de SSDL: 2 hr 27 min <br/> Geração de mapeamento: 1 segundo <br/> Geração de CSDL: 1 segundo <br/> Geração de ObjectLayer: 1 segundo <br/> A geração de exibição: 2 h 14 min |
| Visual Studio 2010 SP1, Entity Framework 4 | Geração de SSDL: 1 segundo <br/> Geração de mapeamento: 1 segundo <br/> Geração de CSDL: 1 segundo <br/> Geração de ObjectLayer: 1 segundo <br/> A geração de exibição: 1 hr 53 min   |
| Visual Studio 2013, o Entity Framework 5     | Geração de SSDL: 1 segundo <br/> Geração de mapeamento: 1 segundo <br/> Geração de CSDL: 1 segundo <br/> Geração de ObjectLayer: 1 segundo <br/> Geração de exibição: minutos de 65    |
| Visual Studio 2013, Entity Framework 6     | Geração de SSDL: 1 segundo <br/> Geração de mapeamento: 1 segundo <br/> Geração de CSDL: 1 segundo <br/> Geração de ObjectLayer: 1 segundo <br/> A geração de exibição: 28 segundos.   |


Vale a pena observar que ao gerar o SSDL, a carga é quase que totalmente gasto no SQL Server, enquanto a máquina de desenvolvimento do cliente está aguardando ocioso para resultados antes de voltar do servidor. Os DBAs particularmente devem apreciar essa melhoria. Também vale a pena observar que, essencialmente, todo o custo de geração de modelo ocorre na geração de exibição agora.

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a>7.3 dividir grandes modelos com o banco de dados pela primeira vez e Model First

À medida que aumenta de tamanho do modelo, a superfície do designer fica confuso e difícil de usar. Normalmente, vamos considerar um modelo com mais de 300 entidades a serem muito grande para ser efetivamente usar o designer. A seguinte postagem de blog descreve as várias opções para a divisão de grandes modelos: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

A postagem foi escrita para a primeira versão do Entity Framework, mas as etapas ainda se aplicam.

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a>7.4 Considerações sobre desempenho de com o controle de fonte de dados de entidade

Já vimos casos em testes de estresse e desempenho com multithread em que o desempenho de um aplicativo web usando o controle EntityDataSource cai significativamente. A causa subjacente é que o EntityDataSource chama repetidamente MetadataWorkspace.LoadFromAssembly em assemblies referenciados pelo aplicativo Web para descobrir os tipos a serem usados como entidades.

A solução é definir o ContextTypeName do EntityDataSource como o nome do tipo de sua classe derivada de ObjectContext. Isso desativa o mecanismo que examina todos os assemblies referenciados para os tipos de entidade.

Definir o campo ContextTypeName também impede que um problema funcional em que o EntityDataSource no .NET 4.0 gera uma ReflectionTypeLoadException quando ele não é possível carregar um tipo de um assembly por meio de reflexão. Esse problema foi corrigido no .NET 4.5.

### <a name="75-poco-entities-and-change-tracking-proxies"></a>7.5 entidades POCO e proxies de controle de alterações

Entity Framework permite que você use classes de dados personalizados junto com seu modelo de dados sem fazer nenhuma modificação às classes de dados em si. Isso significa que você pode usar "simples" objetos CLR (POCO), como objetos de domínio existente, com seu modelo de dados. Essas classes de dados POCO (também conhecido como com ignorância de persistência de objetos), que são mapeados para entidades que são definidas em um modelo de dados, dão suporte à maioria da mesma consulta, inserir, atualizarem e excluir comportamentos como tipos de entidade que são gerados pelas ferramentas do modelo de dados de entidade.

Entity Framework também pode criar classes proxy derivadas de seus tipos de POCO, que são usados quando você deseja habilitar os recursos, como carregamento lento e automática controle de alterações em entidades POCO. Suas classes POCO devem atender a certos requisitos para permitir que o Entity Framework para poderem usar proxies, conforme descrito aqui: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).

Proxies de controle de oportunidade notificará o Gerenciador de estado do objeto sempre que qualquer uma das propriedades das entidades tem seu valor alterado, para que o Entity Framework saiba o estado real das suas entidades o tempo todo. Isso é feito adicionando eventos de notificação para o corpo dos métodos setter de suas propriedades e ter o Gerenciador de estado do objeto tais eventos de processamento. Observe que a criação de um proxy entidade será normalmente ser mais caro do que criar uma entidade POCO não proxy devido ao conjunto de eventos criados pelo Entity Framework adicionado.

Quando uma entidade POCO não tem um proxy de controle de alterações, as alterações são encontradas, comparando o conteúdo das suas entidades em uma cópia de um estado salvo anterior. Essa comparação profunda se tornará um processo demorado quando você tem muitas entidades em seu contexto, ou quando suas entidades têm uma quantidade muito grande de propriedades, mesmo que nenhuma delas alterado desde a última comparação ocorreu.

Em Resumo: você pagará um impacto ao criar o proxy de controle de alterações no desempenho, mas o controle de alterações o ajudará a acelerar o processo de detecção de alteração quando as entidades têm muitas propriedades ou quando você tem muitas entidades em seu modelo. Para entidades com um pequeno número de propriedades em que a quantidade de entidades não aumentar muito, ter proxies de controle de alteração não pode não ser muito benéfico.

## <a name="8-loading-related-entities"></a>Entidades relacionadas ao carregamento de 8

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 vs de carregamento lentas. Carregamento adiantado

Entity Framework oferece várias maneiras diferentes para carregar as entidades relacionadas à sua entidade de destino. Por exemplo, quando você consulta para os produtos, há diferentes maneiras que os pedidos relacionados serão carregados no Gerenciador de estado do objeto. Do ponto de vista do desempenho, a pergunta maiores serem consideradas ao carregar entidades relacionadas será se deve usar o carregamento lento ou o carregamento adiantado.

Ao usar o carregamento adiantado, as entidades relacionadas são carregadas, juntamente com seu conjunto de entidades de destino. Você pode usar uma instrução Include em sua consulta para indicar quais entidades que você deseja colocar relacionadas.

Ao usar o carregamento lento, sua consulta inicial traz apenas o conjunto de entidades de destino. Mas sempre que você acessar uma propriedade de navegação, outra consulta é emitida para o armazenamento para carregar a entidade relacionada.

Depois que uma entidade tiver sido carregada, quaisquer consultas adicionais para a entidade carregará diretamente do Gerenciador de estado de objeto, se você estiver usando o carregamento lento ou o carregamento adiantado.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 como escolher entre o carregamento lento e o carregamento adiantado

O importante é que você compreendeu a diferença entre o carregamento lento e o carregamento adiantado para que você possa fazer a escolha correta para seu aplicativo. Isso ajudará você a avaliar a compensação entre várias solicitações no banco de dados em comparação com uma única solicitação que pode conter uma extensa carga útil. Ele pode ser apropriado usar o carregamento adiantado em algumas partes do seu aplicativo e o carregamento lento em outras partes.

Um exemplo do que está acontecendo nos bastidores, suponha que você deseja consultar os clientes que moram no Reino Unido e sua contagem de ordem.

**Usando o carregamento adiantado**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Usar o carregamento lento**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

Ao usar o carregamento adiantado, você vai emitir uma única consulta que retorna todos os clientes e todos os pedidos. O comando de armazenamento se parece com:

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

Ao usar o carregamento lento, você vai emitir a consulta a seguir inicialmente:

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

E cada vez que você acessa a propriedade de navegação de pedidos de um cliente outra consulta semelhante à seguinte é emitida para o repositório:

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

Para obter mais informações, consulte o [Carregando objetos relacionados](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 carregamento lento em comparação com o carregamento adiantado de folha de consulta

Não há nenhum algo como uma única para escolher o carregamento adiantado em comparação com o carregamento lento. Tente primeiro entender as diferenças entre as duas estratégias para que você possa fazer uma decisão bem informada; Além disso, considere se seu código se ajusta a qualquer um dos seguintes cenários:

| Cenário                                                                    | Nosso sugestão                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Você precisa acessar muitas propriedades de navegação de entidades buscadas? | **Não** -provavelmente fará as duas opções. No entanto, se a carga que está trazendo a sua consulta não é muito grande, que você pode enfrentar os benefícios de desempenho usando o carregamento adiantado, como ele vai exigir menos rede idas e vindas para materializar seus objetos. <br/> <br/> **Sim** -se você precisar acessar muitas propriedades de navegação de entidades, você faria se por meio de várias incluem instruções em sua consulta com o carregamento adiantado. As entidades mais incluir, maior será a carga de sua consulta retornará. Depois que você inclui três ou mais entidades em sua consulta, considere alternar para lento ao carregar. |
| Você sabe exatamente quais dados serão necessários no tempo de execução?                   | **Não** -carregamento lento será melhor para você. Caso contrário, você pode acabar consultando dados que não será necessário. <br/> <br/> **Sim** - adiantado carregamento provavelmente é sua melhor aposta; ele o ajudará a carregar conjuntos de inteiros com mais rapidez. Se sua consulta requer a buscar uma grande quantidade de dados, e isso se torna muito lento, em seguida, tente Lazy carregando em vez disso.                                                                                                                                                                                                                                                       |
| O código em execução longe de seu banco de dados? (maior latência na rede)  | **Não** – quando a latência de rede não é um problema, usar o carregamento lento pode simplificar o seu código. Lembre-se de que a topologia do seu aplicativo pode mudar, tome a proximidade de banco de dados para concedidas. <br/> <br/> **Sim** – quando a rede é um problema, somente você pode decidir o que melhor se adapta para seu cenário. Normalmente o carregamento adiantado será melhor porque exige menos viagens de ida e volta.                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a>8.2.2 as preocupações de desempenho de with inclui vários

Quando ouvimos questões de desempenho que envolvem problemas de tempo de resposta do servidor, a origem do problema é com frequência consultas com várias instruções Include. Enquanto incluindo entidades relacionadas em uma consulta seja eficiente, é importante entender o que está acontecendo nos bastidores.

Demora um tempo relativamente longo para uma consulta com várias instruções Include nele passar pelo compilador nosso plano interno para produzir o comando de armazenamento. A maior parte desse tempo é gasto tentando otimizar a consulta resultante. O comando de armazenamento gerado conterá um Outer Join ou Union para cada inclusão, dependendo do seu mapeamento. Consultas como este trará grandes gráficos conectados do banco de dados em uma única carga, que será acerbate quaisquer problemas de largura de banda, especialmente quando há muita de redundância na carga (por exemplo, quando vários níveis de inclusão são usados para percorrer associações a direção de um-para-muitos).

Você pode verificar para casos em que suas consultas estão retornando excessivamente grandes cargas, acessando o TSQL subjacente para a consulta usando o ToTraceString e executando o comando de armazenamento no SQL Server Management Studio para ver o tamanho da carga. Nesses casos, que você pode tentar reduzir o número de instruções Include na consulta para trazer os dados que necessários. Ou, você poderá dividir a consulta em uma sequência menor de subconsultas, por exemplo:

**Antes de interromper a consulta:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**Depois de interromper a consulta:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

Isso funcionará apenas em consultas rastreadas, como estamos fazendo uso da capacidade de contexto deve executar a correção de associação e a resolução de identidade automaticamente.

Assim como acontece com carregamento lento, a desvantagem será mais consultas para cargas menores. Você também pode usar projeções de propriedades individuais explicitamente selecionar apenas os dados que você precisa de cada entidade, mas você será não ser Carregando entidades nesse caso, e atualizações não terão suporte.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 solução alternativa de para que o carregamento lento de propriedades

Entity Framework atualmente não dá suporte a carregamento lento de propriedades escalares ou complexos. No entanto, em casos em que você tem uma tabela que inclui um objeto grande, como um BLOB, você pode usar a divisão de tabela para separar as propriedades grandes em uma entidade separada. Por exemplo, suponha que você tenha uma tabela Product que inclui uma coluna de foto varbinary. Se você não precisar com frequência acessar essa propriedade em suas consultas, você pode usar a divisão para trazer apenas as partes da entidade que você normalmente precisa de tabela. A entidade que representa a foto do produto será carregada apenas quando explicitamente necessário.

Um bom recurso que mostra como habilitar a divisão de tabela é a postagem no blog do Gil Fink "Tabela dividindo no Entity Framework": \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 outras considerações

### <a name="91-server-garbage-collection"></a>9.1 coleta de lixo de servidor

Alguns usuários podem enfrentar a contenção de recursos que limita o paralelismo esperado quando o coletor de lixo não está configurado corretamente. Sempre que o EF é usado em um cenário multithreaded, ou em qualquer aplicativo que se parece com um sistema de servidor, certifique-se de habilitar a coleta de lixo do servidor. Isso é feito através de uma configuração simple no arquivo de configuração de aplicativo:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Isso deve reduzir a contenção de thread e aumentar a taxa de transferência por até 30% em cenários de CPU saturado. Em termos gerais, você deve sempre testar como seu aplicativo se comporta usando a coleta de lixo clássico (que é melhor ajustados para cenários de lado do cliente e da interface do usuário), bem como a coleta de lixo do servidor.

### <a name="92-autodetectchanges"></a>9.2 AutoDetectChanges

Como mencionado anteriormente, o Entity Framework pode mostrar a problemas de desempenho quando o cache de objetos tem muitas entidades. Determinadas operações, como adicionar, remover, localizar, entrada e SaveChanges, disparam chamadas a DetectChanges que podem consumir uma grande quantidade de CPU com base em quão grande tornou o cache de objetos. A razão para isso é que o cache de objetos e a tentativa de Gerenciador de estado do objeto permanecer como sincronizados o máximo possível em cada operação executada em um contexto para que os dados produzidos é obrigatoriamente correto em uma ampla gama de cenários.

Ele geralmente é uma boa prática para deixar a detecção de alterações automático do Entity Framework habilitada para toda a vida do seu aplicativo. Se seu cenário está sendo afetado negativamente pelo alto uso da CPU e os perfis de indicam que o culpado é a chamada para DetectChanges, considere desativar temporariamente AutoDetectChanges na parte confidencial do seu código:

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

Antes de desligar AutoDetectChanges, é bom entender o que isso pode causar o Entity Framework para perder a capacidade de rastrear certas informações sobre as alterações que estão ocorrendo nas entidades. Se tratadas de forma incorreta, isso pode causar inconsistência de dados em seu aplicativo. Para obter mais informações sobre como desativar AutoDetectChanges, leia \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93-context-per-request"></a>9.3 contexto por solicitação

Contextos do Entity Framework destinam-se a ser usado como a experiência de instâncias de curta duração para fornecer o desempenho ideal. Contextos devem ser curtos vivia e descartadas e como tal, foram implementadas para ser muito leve e reutilize metadados sempre que possível. Em cenários de web, é importante ter isso em mente e não tem um contexto para obter mais informações que a duração de uma única solicitação. Da mesma forma, em cenários de não web contexto deve ser descartado com base na sua compreensão dos diferentes níveis de armazenamento em cache no Entity Framework. Em termos gerais, um Evite ter uma instância de contexto durante a vida útil do aplicativo, bem como contextos por thread e contextos estáticos.

### <a name="94-database-null-semantics"></a>9.4 a semântica nula do banco de dados

Entity Framework por padrão irá gerar o código SQL que tem C\# semântica de comparação de null. Considere a seguinte consulta de exemplo:

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

Neste exemplo, estamos comparando um número de variáveis que permitem valor nulos em relação a propriedades que permitem valor nulas na entidade, como SupplierID e UnitPrice. O SQL gerado para esta consulta perguntará se o valor do parâmetro é o mesmo que o valor da coluna, ou se o parâmetro e os valores de coluna forem nulos. Isso ocultará a forma como o servidor de banco de dados trata valores nulos e fornecerá um C consistente\# null experiência entre os fornecedores de banco de dados diferente. Por outro lado, o código gerado é um pouco intrincado e talvez não sejam executados bem quando a quantidade de comparações em where cresce na instrução da consulta para um grande número.

Uma maneira de lidar com essa situação é usando a semântica nula do banco de dados. Observe que isso pode se comportar de maneira diferente para o C\# null semântica desde agora o Entity Framework irá gerar SQL mais simples que expõe a forma como o mecanismo de banco de dados trata valores nulos. A semântica nula do banco de dados pode ser ativado por contexto com uma linha única de configuração com a configuração de contexto:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Pequeno a médio porte consultas não exibirá uma melhoria de desempenho perceptível ao usar semântica nula do banco de dados, mas a diferença se tornará mais perceptível em consultas com um grande número de possíveis comparações nulas.

Na consulta de exemplo acima, a diferença de desempenho era inferior a % 2 em um microbenchmark em execução em um ambiente controlado.

### <a name="95-async"></a>9.5 Async

Entity Framework 6 introduziu suporte das operações assíncronas quando executado no .NET 4.5 ou posterior. Na maior parte, os aplicativos que têm e/s relacionadas contenção irá se beneficiar mais usando a consulta assíncrona e salvar operações. Se seu aplicativo não sofre de contenção de e/s, o uso de async irão, nos melhores casos, executado de forma síncrona e retornar o resultado na mesma quantidade de tempo como uma chamada síncrona ou no pior caso, simplesmente adiar a execução para uma tarefa assíncrona e adicione tim extra e para a conclusão do seu cenário.

Para obter informações sobre o trabalho como assíncrono de programação que ajudarão você a decidir se async melhorará o desempenho do seu aplicativo visite [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx). Para obter mais informações sobre o uso de operações assíncronas no Entity Framework, consulte [consulta assíncrona e salvar](~/ef6/fundamentals/async.md
).

### <a name="96-ngen"></a>9.6 NGEN

Entity Framework 6 não vem na instalação padrão do .NET framework. Dessa forma, os assemblies do Entity Framework não são que NGen seria por padrão, que significa que todo o código do Entity Framework está sujeito a custos JIT'ing mesmos como qualquer outro assembly MSIL. Isso pode prejudicar a experiência de F5 ao desenvolver e também a inicialização a frio do seu aplicativo em ambientes de produção. Para reduzir os custos de CPU e memória de JIT'ing é aconselhável NGEN o Entity Framework imagens conforme apropriado. Para obter mais informações sobre como melhorar o desempenho de inicialização do Entity Framework 6 com o NGEN, consulte [melhorando o desempenho de inicialização com o NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97-code-first-versus-edmx"></a>9.7 code First versus EDMX

Motivos do Entity Framework sobre o problema de incompatibilidade de impedância entre a programação orientada a objeto e bancos de dados relacionais, fazendo com que uma representação na memória de modelo conceitual (os objetos), o esquema de armazenamento (o banco de dados) e um mapeamento entre o dois. Esses metadados é chamado um modelo de dados de entidade, ou EDM, de forma abreviada. Partir esse EDM, Entity Framework será derivar as exibições para dados de ida e volta de objetos na memória no banco de dados e de volta.

Quando Entity Framework é usado com um arquivo EDMX formalmente que especifica o modelo conceitual, o esquema de armazenamento e o mapeamento, em seguida, o estágio de carregamento de modelo tem apenas validar que o EDM está correto (por exemplo, verifique se que não há mapeamentos estão faltando), em seguida, gerar os modos de exibição, valide os modos de exibição e ter esses metadados prontos para uso. Só pode, em seguida, uma consulta ser executada ou novos dados a ser salvo para o armazenamento de dados.

Em seu cerne, a abordagem de Code First é um gerador de modelo de dados de entidade sofisticado. O Entity Framework tem que produzir um EDM a partir do código fornecido. Isso é feito analisando as classes envolvidas no modelo, aplicar as convenções e definir o modelo por meio da API Fluent. Depois que o EDM é compilado, o Entity Framework essencialmente se comporta da mesma maneira que faria tivesse um arquivo EDMX foi presente no projeto. Portanto, a criação do modelo do Code First adiciona complexidade extra que se traduz em um tempo de inicialização mais lento para o Entity Framework em comparação com a necessidade de um EDMX. O custo depende completamente o tamanho e a complexidade do modelo que está sendo criado.

Ao escolher usar EDMX versus Code First, é importante saber que a flexibilidade introduzida pelo Code First aumenta o custo da criação do modelo pela primeira vez. Se seu aplicativo pode suportar o custo dessa carga pela primeira vez, em seguida, normalmente Code First será a maneira preferencial para ir.

## <a name="10-investigating-performance"></a>Desempenho de investigação de 10

### <a name="101-using-the-visual-studio-profiler"></a>10.1 usando o Visual Studio Profiler

Se você estiver tendo problemas de desempenho com o Entity Framework, você pode usar um criador de perfil, como no Visual Studio para ver onde seu aplicativo está gastando tempo. Esta é a ferramenta são usadas para gerar os gráficos de pizza na postagem do blog "Explorando o desempenho do ADO.NET Entity Framework - parte 1" ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) que mostram onde o Entity Framework gasta seu tempo durante consultas frios e quentes.

A postagem do blog "Criação de perfil Entity Framework usando o Profiler de 2010 Visual Studio" gravada pelos dados e modelagem equipe consultiva para clientes mostra um exemplo do mundo real de como eles usaram o criador de perfil para investigar um problema de desempenho.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Esta postagem foi escrita para um aplicativo do windows. Se você precisar criar o perfil de um aplicativo web do Windows Performance Recorder (WPR) e o analisador de desempenho do Windows (WPA) das ferramentas podem funcionar melhor do que o trabalho do Visual Studio. WPR e WPA fazem parte do Toolkit de desempenho do Windows que é incluído com o Windows Assessment and Deployment Kit ( [ http://www.microsoft.com/download/details.aspx?id=39982 ](https://www.microsoft.com/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 aplicativo/banco de dados de criação de perfil

Ferramentas como o criador de perfil incorporado ao Visual Studio lhe dizer onde seu aplicativo está gastando tempo.  Outro tipo de criador de perfil está disponível que executa a análise dinâmica de seu aplicativo em execução, na produção ou pré-produção dependendo das necessidades e procura por antipadrões de acesso de banco de dados e armadilhas comuns.

Dois criadores de perfil disponíveis no mercado são o Entity Framework Profiler ( \<http://efprof.com>) e ORMProfiler ( \<http://ormprofiler.com>).

Se seu aplicativo for um aplicativo MVC usando o Code First, você pode usar o MiniProfiler da StackExchange. Scott Hanselman descreve esta ferramenta em seu blog em: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Para obter mais informações sobre a criação de perfil de atividade de banco de dados do seu aplicativo, consulte artigo do MSDN Magazine de Julie [atividade de banco de dados de criação de perfil no Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>10.3 agente de log do banco de dados

Se você estiver usando o Entity Framework 6 também considere usar a funcionalidade de registro em log interno. A propriedade de banco de dados do contexto pode ser instruída a fazer sua atividade por meio de uma configuração de uma linha simples:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

Neste exemplo, a atividade de banco de dados será registrada no console, mas a propriedade de Log pode ser configurada para chamar qualquer ação&lt;cadeia de caracteres&gt; delegar.

Se você quiser habilitar o registro em log do banco de dados sem recompilar e você estiver usando o Entity Framework 6.1 ou posterior, você pode fazer isso adicionando um interceptor no arquivo Web. config ou App. config do seu aplicativo.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Para obter mais informações sobre como adicionar o registro em log sem recompilar acesse \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>Apêndice 11

### <a name="111-a-test-environment"></a>11.1 ambiente de teste de r.

Esse ambiente usa uma configuração de computador 2 com o banco de dados em um computador separado do aplicativo cliente. Computadores estão no mesmo rack, para que a latência de rede é relativamente baixo, mas mais realista que o ambiente de um único computador.

#### <a name="1111-app-server"></a>11.1.1 servidor de aplicativo

##### <a name="11111-software-environment"></a>11.1.1.1 ambiente de software

-   Ambiente de Software do Entity Framework 4
    -   Nome do sistema operacional: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 – Ultimate.
    -   Visual Studio 2010 SP1 (apenas para algumas comparações).
-   Ambiente de Software do Entity Framework 5 e 6
    -   Nome do sistema operacional: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate.

##### <a name="11112-hardware-environment"></a>11.1.1.2 ambiente de hardware

-   Processador duplo: Intel (r) Xeon (r) CPU L5520 W3530 2,27 GHz, 2261 Mhz8 GHz, 4 núcleos, 84 processadores lógicos.
-   RamRAM 2412 GB.
-   136 GB SCSI250GB SATA 7200 rpm 3GB/s unidade dividido em partições de 4.

#### <a name="1112-db-server"></a>11.1.2 servidor de banco de dados

##### <a name="11121-software-environment"></a>11.1.2.1 ambiente de software

-   Nome do sistema operacional: Windows Server 2008 R28.1 Enterprise SP1.
-   R22012 do SQL Server 2008.

##### <a name="11122-hardware-environment"></a>11.1.2.2 ambiente de hardware

-   Processador único: Intel (r) Xeon (r) CPU L5520 2,27 GHz, 2261 MhzES-1620 0 @ 3,60 GHz, 4 núcleos, 8 processadores lógicos.
-   RamRAM 824 GB.
-   465 GB ATA500GB SATA 7200 rpm 6GB/s unidade dividido em partições de 4.

### <a name="112-b-query-performance-comparison-tests"></a>11.2 testes de comparação de desempenho de consulta B.

O modelo do Northwind foi usado para executar esses testes. Ele foi gerado do banco de dados usando o Entity Framework designer. Em seguida, o código a seguir foi usado para comparar o desempenho das opções de execução de consulta:

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>C. 11.3 Navision modelo

O banco de dados Navision é um grande banco de dados usado para a demonstração do Microsoft Dynamics – painel de navegação. O modelo conceitual gerado contém conjuntos de entidade 1005 e 4227 associações. O modelo usado no teste é "simples" – nenhuma herança foi adicionada a ele.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 consultas de usadas para testes de Navision

Na lista de consultas usada com o modelo Navision contém 3 categorias de consultas do Entity SQL:

##### <a name="11311-lookup"></a>11.3.1.1 pesquisa de

Uma consulta de pesquisa simples com nenhuma agregação

-   Contagem: 16232
-   Exemplo:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a>11.3.1.2 SingleAggregating

Uma consulta normal de BI com várias agregações, mas nenhum subtotais (consulta única)

-   Contagem: 2313
-   Exemplo:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Onde MDF\_SessionLogin\_tempo\_Max () é definido no modelo como:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Uma consulta de BI com agregações e subtotais (por meio de unir tudo)

-   Contagem: 178
-   Exemplo:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
