---
title: Considerações sobre desempenho para EF4, EF5 e EF6-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: 07eb605f0d39f0c1bcfe781540525180f0dd0b22
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181664"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Considerações sobre desempenho para o EF 4, 5 e 6
Por David Obando, Eric e outros

Publicado em: abril de 2012

Última atualização: maio de 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Introdução

As estruturas de mapeamento relacional de objeto são uma maneira conveniente de fornecer uma abstração para acesso a dados em um aplicativo orientado a objeto. Para aplicativos .NET, O Microsoft/RM recomendado é Entity Framework. No entanto, com qualquer abstração, o desempenho pode se tornar uma preocupação.

Este White Paper foi escrito para mostrar as considerações de desempenho ao desenvolver aplicativos usando Entity Framework, para dar aos desenvolvedores uma ideia da Entity Framework algoritmos internos que podem afetar o desempenho e fornecer dicas para investigação e melhorando o desempenho em seus aplicativos que usam Entity Framework. Há vários bons tópicos sobre o desempenho já disponíveis na Web, e também tentamos apontar para esses recursos sempre que possível.

O desempenho é um tópico complicado. Este White Paper é destinado a um recurso para ajudá-lo a tomar decisões relacionadas ao desempenho para seus aplicativos que usam Entity Framework. Incluímos algumas métricas de teste para demonstrar o desempenho, mas essas métricas não se destinam a indicadores absolutos do desempenho que você verá em seu aplicativo.

Para fins práticos, este documento pressupõe que Entity Framework 4 seja executado em .NET 4,0 e Entity Framework 5 e 6 são executados no .NET 4,5. Muitos dos aprimoramentos de desempenho feitos para o Entity Framework 5 residem nos componentes principais que acompanham o .NET 4,5.

O Entity Framework 6 é uma versão fora da banda e não depende dos componentes Entity Framework fornecidos com o .NET. O Entity Framework 6 trabalha no .NET 4,0 e no .NET 4,5 e pode oferecer um grande benefício de desempenho para aqueles que não fizeram a atualização do .NET 4,0, mas que desejam os mais recentes Entity Framework bits em seus aplicativos. Quando este documento mencionar Entity Framework 6, ele se refere à versão mais recente disponível no momento da redação deste artigo: versão 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. execução de consulta fria versus passiva

Na primeira vez em que qualquer consulta é feita em um determinado modelo, a Entity Framework faz muito trabalho em segundo plano para carregar e validar o modelo. Frequentemente, nos referimos a essa primeira consulta como uma consulta "Cold".  Outras consultas em um modelo já carregado são conhecidas como consultas "quentes" e são muito mais rápidas.

Vamos dar uma visão de alto nível de onde o tempo é gasto ao executar uma consulta usando Entity Framework e ver onde as coisas estão melhorando Entity Framework 6.

**Primeira execução de consulta – consulta a frio**

| Código de gravações do usuário                                                                                     | Ação                    | Impacto no desempenho do EF4                                                                                                                                                                                                                                                                                                                                                                                                        | Impacto no desempenho do EF5                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Impacto no desempenho do EF6                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Criação de contexto          | Média                                                                                                                                                                                                                                                                                                                                                                                                                        | Média                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Criação de expressão de consulta | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                           | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Execução de consulta LINQ      | -Carregamento de metadados: alto, mas em cache <br/> -Exibição de geração: potencialmente muito alta, mas em cache <br/> -Avaliação de parâmetro: média <br/> -Conversão de consulta: média <br/> -Geração de materializador: média, mas em cache <br/> -Execução de consulta de banco de dados: potencialmente alta <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materialização de objeto: média <br/> -Pesquisa de identidade: média | -Carregamento de metadados: alto, mas em cache <br/> -Exibição de geração: potencialmente muito alta, mas em cache <br/> -Avaliação de parâmetro: baixa <br/> -Conversão de consulta: média, mas em cache <br/> -Geração de materializador: média, mas em cache <br/> -Execução de consulta de banco de dados: potencialmente alta (consultas melhores em algumas situações) <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materialização de objeto: média <br/> -Pesquisa de identidade: média | -Carregamento de metadados: alto, mas em cache <br/> -Exibição de geração: média, mas em cache <br/> -Avaliação de parâmetro: baixa <br/> -Conversão de consulta: média, mas em cache <br/> -Geração de materializador: média, mas em cache <br/> -Execução de consulta de banco de dados: potencialmente alta (consultas melhores em algumas situações) <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materialização de objeto: média (mais rápido que EF5) <br/> -Pesquisa de identidade: média |
| `}`                                                                                                  | Conexão. fechar          | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                           | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Segunda execução de consulta – consulta a quente**

| Código de gravações do usuário                                                                                     | Ação                    | Impacto no desempenho do EF4                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Impacto no desempenho do EF5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Impacto no desempenho do EF6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Criação de contexto          | Média                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Média                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Criação de expressão de consulta | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Execução de consulta LINQ      | -Pesquisa de ~~carregamento~~ de metadados: ~~alta, mas em cache~~ baixa <br/> -Exibir pesquisa de ~~geração~~ : ~~potencialmente muito alto, mas em cache~~ baixo <br/> -Avaliação de parâmetro: média <br/> -Pesquisa de ~~conversão~~ de consulta: média <br/> -Pesquisa de ~~geração~~ de materializador: ~~média, mas em cache~~ baixo <br/> -Execução de consulta de banco de dados: potencialmente alta <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materialização de objeto: média <br/> -Pesquisa de identidade: média | -Pesquisa de ~~carregamento~~ de metadados: ~~alta, mas em cache~~ baixa <br/> -Exibir pesquisa de ~~geração~~ : ~~potencialmente muito alto, mas em cache~~ baixo <br/> -Avaliação de parâmetro: baixa <br/> -Pesquisa de ~~conversão~~ de consulta: ~~média, mas em cache~~ baixo <br/> -Pesquisa de ~~geração~~ de materializador: ~~média, mas em cache~~ baixo <br/> -Execução de consulta de banco de dados: potencialmente alta (consultas melhores em algumas situações) <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materialização de objeto: média <br/> -Pesquisa de identidade: média | -Pesquisa de ~~carregamento~~ de metadados: ~~alta, mas em cache~~ baixa <br/> -Exibir pesquisa de ~~geração~~ : ~~média, mas em cache~~ baixo <br/> -Avaliação de parâmetro: baixa <br/> -Pesquisa de ~~conversão~~ de consulta: ~~média, mas em cache~~ baixo <br/> -Pesquisa de ~~geração~~ de materializador: ~~média, mas em cache~~ baixo <br/> -Execução de consulta de banco de dados: potencialmente alta (consultas melhores em algumas situações) <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materialização de objeto: média (mais rápido que EF5) <br/> -Pesquisa de identidade: média |
| `}`                                                                                                  | Conexão. fechar          | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Baixo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Há várias maneiras de reduzir o custo de desempenho de consultas frias e quentes, e vamos dar uma olhada nelas na seção a seguir. Especificamente, veremos como reduzir o custo do carregamento do modelo em consultas frias usando exibições geradas previamente, o que deve ajudar a aliviar os problemas de desempenho enfrentados durante a geração da exibição. Para consultas quentes, abordaremos o cache do plano de consulta, nenhuma consulta de acompanhamento e opções de execução de consulta diferentes.

### <a name="21-what-is-view-generation"></a>2,1 o que é a geração de exibição?

Para entender o que é a geração de exibição, devemos primeiro entender o que são "exibições de mapeamento". As exibições de mapeamento são representações executáveis das transformações especificadas no mapeamento para cada conjunto de entidades e associação. Internamente, essas exibições de mapeamento assumem a forma de CQTs (árvores de consulta canônicas). Há dois tipos de exibições de mapeamento:

-   Exibições de consulta: representam a transformação necessária para ir do esquema de banco de dados para o modelo conceitual.
-   Atualizar exibições: elas representam a transformação necessária para passar do modelo conceitual para o esquema de banco de dados.

Tenha em mente que o modelo conceitual pode ser diferente do esquema de banco de dados de várias maneiras. Por exemplo, uma única tabela pode ser usada para armazenar os dados de dois tipos de entidade diferentes. A herança e os mapeamentos não triviais desempenham uma função na complexidade das exibições de mapeamento.

O processo de computação dessas exibições com base na especificação do mapeamento é o que chamamos de geração de exibição. A geração de exibição pode ocorrer dinamicamente quando um modelo é carregado, ou no momento da compilação, usando "exibições previamente geradas"; os últimos são serializados na forma de instruções Entity SQL para um arquivo C\# ou VB.

Quando as exibições são geradas, elas também são validadas. Do ponto de vista do desempenho, a grande maioria do custo da geração de exibição é, na verdade, a validação das exibições, o que garante que as conexões entre as entidades façam sentido e tenham a cardinalidade correta para todas as operações com suporte.

Quando uma consulta em um conjunto de entidades é executada, a consulta é combinada com a exibição de consulta correspondente e o resultado dessa composição é executado por meio do compilador de plano para criar a representação da consulta que o repositório de backup pode entender. Por SQL Server, o resultado final dessa compilação será uma instrução T-SQL SELECT. Na primeira vez que uma atualização em um conjunto de entidades é executada, a exibição de atualização é executada por meio de um processo semelhante para transformá-la em instruções DML para o banco de dados de destino.

### <a name="22-factors-that-affect-view-generation-performance"></a>2,2 fatores que afetam o desempenho da geração de exibição

O desempenho da etapa de geração de exibição não só depende do tamanho do seu modelo, mas também da interconexão do modelo. Se duas entidades estiverem conectadas por uma cadeia de herança ou uma associação, elas serão consideradas conectadas. Da mesma forma, se duas tabelas estiverem conectadas por meio de uma chave estrangeira, elas serão conectadas. Como o número de entidades e tabelas conectadas em seus esquemas aumentam, o custo da geração de exibição aumenta.

O algoritmo que usamos para gerar e validar exibições é exponencial no pior caso, embora possamos usar algumas otimizações para melhorar isso. Os maiores fatores que parecem afetar negativamente o desempenho são:

-   Tamanho do modelo, referindo-se ao número de entidades e à quantidade de associações entre essas entidades.
-   Complexidade do modelo, especificamente herança envolvendo um grande número de tipos.
-   Usando associações independentes, em vez de associações de chave estrangeira.

Para modelos pequenos e simples, o custo pode ser pequeno o suficiente para não se preocupar usando exibições geradas previamente. À medida que o tamanho do modelo e a complexidade aumentam, há várias opções disponíveis para reduzir o custo de geração e validação de exibição.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2,3 usando exibições geradas previamente para diminuir o tempo de carregamento do modelo

Para obter informações detalhadas sobre como usar exibições geradas previamente no Entity Framework 6, visite [exibições de mapeamento geradas previamente](~/ef6/fundamentals/performance/pre-generated-views.md)

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>2.3.1 exibições geradas previamente usando o Entity Framework Power Tools Community Edition

Você pode usar a [edição da Comunidade do Entity Framework 6 Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) para gerar exibições dos modelos EDMX e Code First clicando com o botão direito do mouse no arquivo de classe de modelo e usando o menu Entity Framework para selecionar "gerar exibições". O Entity Framework Power Tools Community Edition funciona apenas em contextos derivados de DbContext.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 como usar exibições geradas previamente com um modelo criado pelo EDMGen

O EDMGen é um utilitário fornecido com o .NET e funciona com o Entity Framework 4 e 5, mas não com Entity Framework 6. O EDMGen permite que você gere um arquivo de modelo, a camada de objeto e as exibições da linha de comando. Uma das saídas será um arquivo de exibições no idioma de sua escolha, VB ou C\#. Este é um arquivo de código que contém trechos de Entity SQL para cada conjunto de entidades. Para habilitar exibições geradas previamente, você simplesmente inclui o arquivo em seu projeto.

Se você fizer edições manualmente nos arquivos de esquema para o modelo, será necessário gerar novamente o arquivo de exibições. Você pode fazer isso executando EDMGen com o sinalizador **/Mode: ViewGeneration** .

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 como usar exibições geradas previamente com um arquivo EDMX

Você também pode usar EDMGen para gerar exibições para um arquivo EDMX – o tópico do MSDN mencionado anteriormente descreve como adicionar um evento de pré-compilação para fazer isso, mas isso é complicado e há alguns casos em que não é possível. Geralmente, é mais fácil usar um modelo T4 para gerar as exibições quando seu modelo estiver em um arquivo edmx.

Blog da equipe do ADO.NET tem uma postagem que descreve como usar um modelo T4 para geração de exibição ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Essa postagem inclui um modelo que pode ser baixado e adicionado ao seu projeto. O modelo foi escrito para a primeira versão do Entity Framework, portanto, não há garantia de trabalhar com as versões mais recentes do Entity Framework. No entanto, você pode baixar um conjunto mais atualizado de modelos de geração de exibição para Entity Framework 4 e 5from a galeria do Visual Studio:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Se você estiver usando o Entity Framework 6 você pode obter a exibição de modelos de geração de T4 da Galeria do Visual Studio no \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2,4 reduzindo o custo da geração de exibição

O uso de exibições geradas previamente move o custo da geração de exibição do carregamento do modelo (tempo de execução) para o tempo de design. Embora isso melhore o desempenho de inicialização em tempo de execução, você ainda enfrentará o problema de geração de exibição durante o desenvolvimento. Há vários truques adicionais que podem ajudar a reduzir o custo da geração de exibição, tanto no tempo de compilação quanto no tempo de execução.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 usando associações de chave estrangeira para reduzir o custo de geração de exibição

Vimos vários casos em que mudar as associações no modelo de associações independentes para associações de chave estrangeira melhorou drasticamente o tempo gasto na geração da exibição.

Para demonstrar essa melhoria, geramos duas versões do modelo do Navision usando EDMGen. *Observação: consulte o Apêndice C para obter uma descrição do modelo do Navision.* O modelo do Navision é interessante para este exercício devido à quantidade muito grande de entidades e relações entre elas.

Uma versão desse modelo muito grande foi gerada com associações de chaves estrangeiras e a outra foi gerada com associações independentes. Em seguida, cronometramos o tempo necessário para gerar as exibições para cada modelo. O Entity Framework 5 Test usou o método GenerateViews () da classe EntityViewGenerator para gerar as exibições, enquanto o teste Entity Framework 6 usou o método GenerateViews () da classe StorageMappingItemCollection. Isso devido à reestruturação de código que ocorreu na codebase do Entity Framework 6.

Usando Entity Framework 5, a geração de exibição para o modelo com chaves estrangeiras levou 65 minutos em um computador de laboratório. É desconhecido quanto tempo teria levado para gerar as exibições para o modelo que usavam associações independentes. Deixamos o teste em execução por mais de um mês antes de o computador ser reinicializado em nosso laboratório para instalar atualizações mensais.

Usando Entity Framework 6, a geração de exibição para o modelo com chaves estrangeiras levou 28 segundos na mesma máquina de laboratório. A geração de exibição para o modelo que usa associações independentes demorou 58 segundos. Os aprimoramentos feitos para Entity Framework 6 em seu código de geração de exibição significam que muitos projetos não precisarão de exibições geradas previamente para obter tempos de inicialização mais rápidos.

É importante marcar que as exibições de pré-compilação no Entity Framework 4 e 5 podem ser feitas com o EDMGen ou o Entity Framework Power Tools. Para a geração de exibição Entity Framework 6 pode ser feita por meio do Entity Framework Power Tools ou programaticamente, conforme descrito em [exibições de mapeamento geradas previamente](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 como usar chaves estrangeiras em vez de associações independentes

Ao usar o EDMGen ou o Entity Designer no Visual Studio, você obtém FKs por padrão e apenas uma única caixa de seleção ou sinalizador de linha de comando para alternar entre FKs e IAs.

Se você tiver um modelo de Code First grande, usar associações independentes terá o mesmo efeito na geração de exibição. Você pode evitar esse impacto, incluindo propriedades de chave estrangeira nas classes de seus objetos dependentes, embora alguns desenvolvedores considerem isso para poluir seu modelo de objeto. Você pode encontrar mais informações sobre esse assunto \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Ao usar o      | Faça isto                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity Designer | Depois de adicionar uma associação entre duas entidades, verifique se você tem uma restrição referencial. Restrições referenciais dizem Entity Framework usar chaves estrangeiras em vez de associações independentes. Para obter mais detalhes, visite \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Ao usar o EDMGen para gerar os arquivos do banco de dados, as chaves estrangeiras serão respeitadas e adicionadas ao modelo como tal. Para obter mais informações sobre as diferentes opções expostas por EDMGen, visite [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Consulte a seção "Convenção de relação" do tópico [convenções de Code First](~/ef6/modeling/code-first/conventions/built-in.md) para obter informações sobre como incluir propriedades de chave estrangeira em objetos dependentes ao usar Code First.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 mover seu modelo para um assembly separado

Quando o modelo é incluído diretamente no projeto do aplicativo e você gera exibições por meio de um evento de pré-compilação ou um modelo T4, a geração e a validação da exibição ocorrerão sempre que o projeto for recriado, mesmo que o modelo não tenha sido alterado. Se você mover o modelo para um assembly separado e referenciá-lo do projeto do seu aplicativo, poderá fazer outras alterações em seu aplicativo sem a necessidade de recriar o projeto que contém o modelo.

*Observação:*  ao mover seu modelo para assemblies separados, lembre-se de copiar as cadeias de conexão para o modelo no arquivo de configuração do aplicativo do projeto do cliente.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 desabilitar a validação de um modelo baseado em edmx

Os modelos EDMX são validados no momento da compilação, mesmo se o modelo não for alterado. Se seu modelo já foi validado, você pode suprimir a validação em tempo de compilação definindo a propriedade "validar no Build" como false na janela Propriedades. Ao alterar o mapeamento ou o modelo, você pode reabilitar a validação temporariamente para verificar as alterações.

Observe que melhorias de desempenho foram feitas na Entity Framework Designer para Entity Framework 6, e o custo da "validação na compilação" é muito menor do que nas versões anteriores do designer.

## <a name="3-caching-in-the-entity-framework"></a>3 cache no Entity Framework

Entity Framework tem as seguintes formas de cache interno:

1.  Cache de objetos – o ObjectStateManager interno de uma instância de ObjectContext mantém o controle na memória dos objetos que foram recuperados usando essa instância. Isso também é conhecido como cache de primeiro nível.
2.  Cache do plano de consulta – reutilizando o comando de armazenamento gerado quando uma consulta é executada mais de uma vez.
3.  Cache de metadados – compartilhando os metadados para um modelo entre diferentes conexões com o mesmo modelo.

Além dos caches que o EF fornece pronto, um tipo especial de provedor de dados ADO.NET conhecido como provedor de encapsulamento também pode ser usado para estender Entity Framework com um cache para os resultados recuperados do banco de dados, também conhecido como cache de segundo nível.

### <a name="31-object-caching"></a>Cache de objetos 3,1

Por padrão, quando uma entidade é retornada nos resultados de uma consulta, logo antes do EF materializar, o ObjectContext verificará se uma entidade com a mesma chave já foi carregada em seu ObjectStateManager. Se uma entidade com as mesmas chaves já estiver presente, o EF a incluirá nos resultados da consulta. Embora o EF ainda emita a consulta no banco de dados, esse comportamento pode ignorar grande parte do custo de materializar a entidade várias vezes.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 obtendo entidades do cache de objetos usando DbContext Find

Ao contrário de uma consulta regular, o método Find em DbSet (APIs incluídas pela primeira vez no EF 4,1) executará uma pesquisa na memória antes mesmo de emitir a consulta no banco de dados. É importante observar que duas instâncias de ObjectContext diferentes terão duas instâncias de ObjectStateManager diferentes, o que significa que elas têm caches de objetos separados.

Find usa o valor de chave primária para tentar localizar uma entidade rastreada pelo contexto. Se a entidade não estiver no contexto, uma consulta será executada e avaliada em relação ao banco de dados, e NULL será retornado se a entidade não for encontrada no contexto ou no banco de dados. Observe que Find também retorna entidades que foram adicionadas ao contexto, mas que ainda não foram salvas no banco de dados.

Há uma consideração de desempenho a ser tomada ao usar Find. Por padrão, invocações para esse método dispararão uma validação do cache de objetos para detectar alterações que ainda estão com a confirmação pendente no banco de dados. Esse processo pode ser muito caro se houver um número muito grande de objetos no cache de objetos ou em um grafo de objeto grande sendo adicionado ao cache de objetos, mas ele também pode ser desabilitado. Em determinados casos, você pode perceber uma ordem de magnitude da diferença na chamada do método Find ao desabilitar as alterações de detecção automática. Ainda assim, uma segunda ordem de magnitude é percebida quando o objeto realmente está no cache versus quando o objeto precisa ser recuperado do banco de dados. Aqui está um grafo de exemplo com medições feitas usando alguns dos nossos microbenchmarks, expressos em milissegundos, com uma carga de 5000 entidades:

![Escala logarítmica do .net 4,5](~/ef6/media/net45logscale.png ".NET 4,5-escala logarítmica")

Exemplo de localizar com alterações de detecção automática desabilitado:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

O que você precisa considerar ao usar o método Find é:

1.  Se o objeto não estiver no cache, os benefícios da localização serão negados, mas a sintaxe ainda será mais simples do que uma consulta por chave.
2.  Se a detecção automática de alterações estiver habilitada, o custo do método Find poderá aumentar em uma ordem de magnitude, ou ainda mais, dependendo da complexidade do modelo e da quantidade de entidades no cache de objetos.

Além disso, tenha em mente que a localização só retorna a entidade que você está procurando e não carrega automaticamente suas entidades associadas se elas ainda não estiverem no cache de objetos. Se você precisar recuperar entidades associadas, poderá usar uma consulta por chave com carregamento adiantado. Para obter mais informações, consulte **carregamento lento de 8,1 versus carregamento adiantado**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 problemas de desempenho quando o cache de objetos tem muitas entidades

O cache de objetos ajuda a aumentar a capacidade de resposta geral do Entity Framework. No entanto, quando o cache de objeto tem uma quantidade muito grande de entidades carregadas, isso pode afetar determinadas operações, como adicionar, remover, localizar, entrada, SaveChanges e muito mais. Em particular, as operações que disparam uma chamada para DetectChanges serão afetadas negativamente por caches de objetos muito grandes. DetectChanges sincroniza o grafo de objeto com o Gerenciador de estado de objeto e seu desempenho será determinado diretamente pelo tamanho do grafo do objeto. Para obter mais informações sobre DetectChanges, consulte [controlar alterações em entidades POCO](https://msdn.microsoft.com/library/dd456848.aspx).

Ao usar o Entity Framework 6, os desenvolvedores podem chamar AddRange e RemoveRange diretamente em um DbSet, em vez de iterar em uma coleção e chamar adicionar uma vez por instância. A vantagem de usar os métodos Range é que o custo de DetectChanges é pago apenas uma vez para todo o conjunto de entidades, em vez de uma vez por cada entidade adicionada.

### <a name="32-query-plan-caching"></a>Cache de plano de consulta 3,2

Na primeira vez que uma consulta é executada, ela passa pelo compilador do plano interno para converter a consulta conceitual no comando Store (por exemplo, o T-SQL que é executado quando executado em SQL Server).  Se o cache do plano de consulta estiver habilitado, na próxima vez que a consulta for executada, o comando Store será recuperado diretamente do cache do plano de consulta para execução, ignorando o compilador do plano.

O cache do plano de consulta é compartilhado entre instâncias de ObjectContext dentro do mesmo AppDomain. Você não precisa manter uma instância de ObjectContext para se beneficiar do cache do plano de consulta.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 algumas observações sobre o cache do plano de consulta

-   O cache do plano de consulta é compartilhado para todos os tipos de consulta: objetos Entity SQL, LINQ to Entities e CompiledQuery.
-   Por padrão, o cache do plano de consulta é habilitado para consultas Entity SQL, seja executado por meio de um EntityCommand ou por meio de um ObjectQuery. Ele também é habilitado por padrão para consultas de LINQ to Entities no Entity Framework no .NET 4,5 e no Entity Framework 6
    -   O cache do plano de consulta pode ser desabilitado definindo a propriedade EnablePlanCaching (em EntityCommand ou ObjectQuery) como false. Por exemplo:
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
-   Para consultas parametrizadas, alterar o valor do parâmetro ainda atingirá a consulta armazenada em cache. Mas alterar as facetas de um parâmetro (por exemplo, tamanho, precisão ou escala) atingirá uma entrada diferente no cache.
-   Ao usar Entity SQL, a cadeia de caracteres de consulta faz parte da chave. Alterar a consulta resultará em entradas de cache diferentes, mesmo que as consultas sejam funcionalmente equivalentes. Isso inclui alterações em maiúsculas ou espaços em branco.
-   Ao usar o LINQ, a consulta é processada para gerar uma parte da chave. A alteração da expressão LINQ, portanto, gerará uma chave diferente.
-   Outras limitações técnicas podem ser aplicadas; consulte consultas autocompiladas para obter mais detalhes.

#### <a name="322-cache-eviction-algorithm"></a>algoritmo de remoção do cache 3.2.2

Entender como o algoritmo interno funciona ajudará você a descobrir quando habilitar ou desabilitar o cache do plano de consulta. O algoritmo de limpeza é o seguinte:

1.  Depois que o cache contiver um número definido de entradas (800), iniciaremos um temporizador que periodicamente (uma vez por minuto) varre o cache.
2.  Durante as varreduras de cache, as entradas são removidas do cache em uma base LFRU (com menos frequência – usado recentemente). Esse algoritmo usa a contagem de acesso e a idade em conta ao decidir quais entradas são ejetadas.
3.  No final de cada varredura de cache, o cache novamente contém 800 entradas.

Todas as entradas de cache são tratadas igualmente ao determinar quais entradas devem ser removidas. Isso significa que o comando Store para um CompiledQuery tem a mesma chance de remoção que o comando Store para uma consulta Entity SQL.

Observe que o timer de remoção do cache é acionado quando há 800 entidades no cache, mas o cache é apenas removido 60 segundos depois que o temporizador é iniciado. Isso significa que, por até 60 segundos, o cache pode aumentar para ser muito grande.

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a>Métricas de teste do 3.2.3 que demonstram o desempenho do cache do plano de consulta

Para demonstrar o efeito do cache do plano de consulta no desempenho do seu aplicativo, executamos um teste no qual executamos várias consultas de Entity SQL no modelo do Navision. Consulte o apêndice para obter uma descrição do modelo do Navision e os tipos de consultas que foram executadas. Neste teste, vamos iterar pela primeira vez na lista de consultas e executar cada uma delas para adicioná-las ao cache (se o Caching estiver habilitado). Esta etapa não está em tempo hábil. Em seguida, suspendemos o thread principal por mais de 60 segundos para permitir que a limpeza do cache ocorra; Por fim, iteramos pela lista uma segunda vez para executar as consultas armazenadas em cache. Além disso, o cache do plano de SQL Server é liberado antes de cada conjunto de consultas ser executado para que os tempos que obtemos com precisão reflitam o benefício fornecido pelo cache do plano de consulta.

##### <a name="3231-test-results"></a>Resultados de Teste 3.2.3.1

| Teste                                                                   | EF5 nenhum cache | EF5 em cache | EF6 nenhum cache | EF6 em cache |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Enumerando todas as consultas de 18723                                          | 124          | 125,4      | 124,3        | 125,3      |
| Evitando a varredura (apenas as primeiras 800 consultas, independentemente da complexidade)  | 41,7         | 5.5        | 40,5         | 5.4        |
| Apenas as consultas de AggregatingSubtotals (total de 178-que evita a varredura) | 39,5         | 4.5        | 38,1         | 4.6        |

*Todos os tempos em segundos.*

Moral – ao executar muitas consultas distintas (por exemplo, consultas criadas dinamicamente), o Caching não ajuda e a liberação resultante do cache pode manter as consultas que mais se beneficiarão mais do que o plano de cache de realmente usá-lo.

As consultas AggregatingSubtotals são as mais complexas das consultas com as quais testamos. Como esperado, quanto mais complexa for a consulta, mais benefício você verá do cache do plano de consulta.

Como um CompiledQuery é realmente uma consulta LINQ com seu plano armazenado em cache, a comparação de um CompiledQuery versus a consulta Entity SQL equivalente deve ter resultados semelhantes. Na verdade, se um aplicativo tiver muitas consultas de Entity SQL dinâmicas, o preenchimento do cache com consultas também fará com que o CompiledQueries seja "descompilado" quando for liberado do cache. Nesse cenário, o desempenho pode ser melhorado desabilitando o cache nas consultas dinâmicas para priorizar o CompiledQueries. Melhor ainda, é claro, seria reescrever o aplicativo para usar consultas parametrizadas em vez de consultas dinâmicas.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3,3 usando CompiledQuery para melhorar o desempenho com consultas LINQ

Nossos testes indicam que o uso do CompiledQuery pode trazer um benefício de 7% sobre consultas LINQ compiladas. Isso significa que você gastará 7% menos tempo executando código da pilha de Entity Framework; Isso não significa que seu aplicativo será 7% mais rápido. Em termos gerais, o custo de escrever e manter objetos CompiledQuery no EF 5,0 pode não valer o problema quando comparado aos benefícios. Sua quilometragem pode variar, portanto, exerça essa opção se seu projeto exigir o push extra. Observe que CompiledQueries são compatíveis apenas com modelos derivados de ObjectContext e não são compatíveis com modelos derivados de DbContext.

Para obter mais informações sobre como criar e invocar um CompiledQuery, consulte [consultas compiladas (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Há duas considerações que você precisa tomar ao usar um CompiledQuery, ou seja, a necessidade de usar instâncias estáticas e os problemas que eles têm com a capacidade de composição. Segue uma explicação detalhada dessas duas considerações.

#### <a name="331-use-static-compiledquery-instances"></a>3.3.1 usar instâncias CompiledQuery estáticas

Como a compilação de uma consulta LINQ é um processo demorado, não queremos fazer isso toda vez que precisar buscar dados do banco de dado. As instâncias de CompiledQuery permitem que você compile uma vez e execute várias vezes, mas você precisa ter cuidado e adquirir para reutilizar a mesma instância de CompiledQuery a cada vez, em vez de compilá-la repetidamente. O uso de membros estáticos para armazenar as instâncias de CompiledQuery se torna necessário; caso contrário, você não verá nenhum benefício.

Por exemplo, suponha que sua página tenha o seguinte corpo de método para lidar com a exibição dos produtos para a categoria selecionada:

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

Nesse caso, você criará uma nova instância do CompiledQuery instantaneamente sempre que o método for chamado. Em vez de ver os benefícios de desempenho recuperando o comando Store do cache de plano de consulta, o CompiledQuery passará pelo compilador do plano sempre que uma nova instância for criada. Na verdade, você estará poluindo o cache do plano de consulta com uma nova entrada CompiledQuery toda vez que o método for chamado.

Em vez disso, você deseja criar uma instância estática da consulta compilada, portanto, você está invocando a mesma consulta compilada sempre que o método é chamado. Uma maneira de fazer isso é adicionando a instância CompiledQuery como um membro do seu contexto de objeto.  Em seguida, você pode tornar as coisas um pouco mais claras acessando o CompiledQuery por meio de um método auxiliar:

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

#### <a name="332-composing-over-a-compiledquery"></a>3.3.2 composição em um CompiledQuery

A capacidade de compor qualquer consulta LINQ é extremamente útil; para fazer isso, basta invocar um método após IQueryable, como *Skip ()* ou *Count ()* . No entanto, isso essencialmente retorna um novo objeto IQueryable. Embora não haja nada para interrompê-lo tecnicamente da composição de um CompiledQuery, fazer isso fará com que a geração de um novo objeto IQueryable requeira passar pelo compilador de plano novamente.

Alguns componentes farão uso de objetos IQueryable compostos para habilitar a funcionalidade avançada. Por exemplo, ASP. O GridView da rede pode ser associado a dados a um objeto IQueryable por meio da propriedade SelectMethod. Em seguida, o GridView comporá esse objeto IQueryable para permitir a classificação e a paginação sobre o modelo de dados. Como você pode ver, o uso de um CompiledQuery para GridView não atingiria a consulta compilada, mas geraria uma nova consulta autocompilada.

Um lugar onde você pode encontrar isso é ao adicionar filtros progressivos a uma consulta. Por exemplo, suponha que você tenha uma página clientes com várias listas suspensas para filtros opcionais (por exemplo, Country e OrdersCount). Você pode compor esses filtros nos resultados IQueryable de um CompiledQuery, mas isso fará com que a nova consulta passe pelo compilador de plano sempre que você executá-lo.

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

 Para evitar essa recompilação, você pode reescrever o CompiledQuery para levar em conta os possíveis filtros:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Que seria invocado na interface do usuário como:

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

 Uma desvantagem aqui é que o comando Store gerado sempre terá os filtros com as verificações nulas, mas eles devem ser bem simples para que o servidor de banco de dados Otimize:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>cache de metadados 3,4

O Entity Framework também dá suporte ao cache de metadados. Isso é basicamente o cache de informações de tipo e informações de mapeamento de tipo para banco de dados em diferentes conexões com o mesmo modelo. O cache de metadados é exclusivo por AppDomain.

#### <a name="341-metadata-caching-algorithm"></a>algoritmo de cache de metadados 3.4.1

1.  As informações de metadados para um modelo são armazenadas em uma ItemCollection para cada EntityConnection.
    -   Como uma observação adicional, há diferentes objetos ItemCollection para diferentes partes do modelo. Por exemplo, StoreItemCollections contém as informações sobre o modelo de banco de dados; ObjectItemCollection contém informações sobre o modelo de dados; EdmItemCollection contém informações sobre o modelo conceitual.

2.  Se duas conexões usarem a mesma cadeia de conexão, elas irão compartilhar a mesma instância de ItemCollection.
3.  Funcionalmente equivalente, mas cadeias de conexão textualmente diferentes podem resultar em caches de metadados diferentes. Fazemos o token de cadeias de conexão, portanto, simplesmente alterar a ordem dos tokens deve resultar em metadados compartilhados. Mas duas cadeias de conexão que parecem funcionar da mesma forma podem não ser avaliadas como idênticas após a geração de tokens.
4.  A ItemCollection é verificada periodicamente para uso. Se for determinado que um espaço de trabalho não foi acessado recentemente, ele será marcado para limpeza na próxima limpeza de cache.
5.  Simplesmente criar um EntityConnection fará com que um cache de metadados seja criado (embora as coleções de itens nela não sejam inicializadas até que a conexão seja aberta). Esse espaço de trabalho permanecerá na memória até que o algoritmo de cache determine que ele não está "em uso".

A equipe consultiva para clientes escreveu uma postagem de blog que descreve o que contém uma referência a um ItemCollection para evitar "substituição" ao usar modelos grandes: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 a relação entre cache de metadados e cache de plano de consulta

A instância de cache do plano de consulta reside na ItemCollection de tipos de armazenamento do MetadataWorkspace. Isso significa que os comandos de armazenamento armazenados em cache serão usados para consultas em qualquer contexto instanciado usando um determinado MetadataWorkspace. Isso também significa que, se você tiver duas cadeias de conexão que são ligeiramente diferentes e não correspondem após a geração de tokens, você terá instâncias de cache de plano de consulta diferentes.

### <a name="35-results-caching"></a>3,5 cache de resultados

Com o cache de resultados (também conhecido como "cache de segundo nível"), você mantém os resultados das consultas em um cache local. Ao emitir uma consulta, primeiro você verá se os resultados estão disponíveis localmente antes de consultar o repositório. Embora o cache de resultados não tenha suporte direto pelo Entity Framework, é possível adicionar um cache de segundo nível usando um provedor de encapsulamento. Um provedor de encapsulamento de exemplo com um cache de segundo nível é de Alachisoft [Entity Framework cache de segundo nível com base no NCache](https://www.alachisoft.com/ncache/entity-framework.html).

Essa implementação de cache de segundo nível é uma funcionalidade injetada que ocorre depois que a expressão LINQ é avaliada (e funcletized) e o plano de execução de consulta é computado ou recuperado do cache de primeiro nível. O cache de segundo nível armazenará apenas os resultados brutos do banco de dados, de modo que o pipeline de materialização ainda será executado posteriormente.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 referências adicionais para o cache de resultados com o provedor de encapsulamento

-   Julie Lerman escreveu um artigo do "cache de segundo nível no Entity Framework e no Windows Azure", que inclui como atualizar o provedor de encapsulamento de exemplo para usar o Windows Server AppFabric Caching: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Se você estiver trabalhando com o Entity Framework 5, o blog da equipe tem uma postagem que descreve como executar tarefas em execução com o provedor de cache para o Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Ele também inclui um modelo T4 para ajudar a automatizar a adição do cache de segundo nível ao seu projeto.

## <a name="4-autocompiled-queries"></a>4 consultas autocompiladas

Quando uma consulta é emitida em um banco de dados usando Entity Framework, ela deve passar por uma série de etapas antes de realmente materializar os resultados; uma dessas etapas é a compilação da consulta. Entity SQL consultas eram conhecidas por ter um bom desempenho, pois elas são armazenadas em cache automaticamente, de modo que a segunda ou a terceira vez que você executa a mesma consulta, ela pode ignorar o compilador de plano e usar o plano em cache.

Entity Framework 5 introduziu o cache automático para consultas de LINQ to Entities também. Nas edições anteriores do Entity Framework a criação de um CompiledQuery para acelerar o desempenho era uma prática comum, pois isso tornaria a consulta LINQ to Entities armazenável em cache. Como o Caching agora é feito automaticamente sem o uso de um CompiledQuery, chamamos esse recurso de "consultas autocompiladas". Para obter mais informações sobre o cache do plano de consulta e sua mecânica, consulte cache do plano de consulta.

Entity Framework detecta quando uma consulta requer a recompilação e faz isso quando a consulta é invocada mesmo que tenha sido compilada antes. As condições comuns que fazem com que a consulta seja recompilada são:

-   Alterando o MergeOption associado à sua consulta. A consulta armazenada em cache não será usada, em vez disso, o compilador do plano será executado novamente e o plano recém-criado é armazenado em cache.
-   Alterando o valor de ContextOptions. UseCSharpNullComparisonBehavior. Você Obtém o mesmo efeito que alterar o MergeOption.

Outras condições podem impedir que a consulta use o cache. Os exemplos comuns são:

-   Usando IEnumerable&lt;T&gt;. Contém&lt;&gt;(T valor).
-   Usando funções que produzem consultas com constantes.
-   Usando as propriedades de um objeto não mapeado.
-   Vinculando sua consulta a outra consulta que requer a recompilação.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4,1 usando IEnumerable&lt;T&gt;. Contém&lt;T&gt;(valor T)

Entity Framework não armazena em cache consultas que invoquem IEnumerable&lt;T&gt;. Contém&lt;T&gt;(valor T) em uma coleção na memória, já que os valores da coleção são considerados voláteis. A consulta de exemplo a seguir não será armazenada em cache, portanto, ela sempre será processada pelo compilador do plano:

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

Observe que o tamanho da IEnumerable em que Contains contém é executado determina a velocidade ou a lentidão da compilação da consulta. O desempenho pode ser significativamente afetado ao usar coleções grandes, como a mostrada no exemplo acima.

O Entity Framework 6 contém otimizações para o modo como IEnumerable&lt;T&gt;. Contém&lt;T&gt;(T value) funciona quando as consultas são executadas. O código SQL gerado é muito mais rápido para produzir e mais legível e, na maioria dos casos, ele também é executado mais rapidamente no servidor.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4,2 usando funções que produzem consultas com constantes

Os operadores de ignorar (), Take (), Contains () e DefautIfEmpty () do LINQ não produzem consultas SQL com parâmetros, mas colocam os valores passados para eles como constantes. Por isso, as consultas que, de outra forma, poderiam ser idênticas acabam com o cache do plano de consulta, tanto na pilha do EF quanto no servidor de banco de dados, e não são reutilizadas, a menos que as mesmas constantes sejam usadas em uma execução de consulta subsequente. Por exemplo:

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

Neste exemplo, cada vez que essa consulta é executada com um valor diferente para ID, a consulta será compilada em um novo plano.

Em particular, preste atenção ao uso de Skip e Take ao fazer paginação. No EF6, esses métodos têm uma sobrecarga lambda que efetivamente torna o plano de consulta em cache reutilizável porque o EF pode capturar variáveis passadas para esses métodos e traduzi-las para Parameters. Isso também ajuda a manter a limpeza do cache, pois, de outra forma, cada consulta com uma constante diferente para Skip e Take obteria sua própria entrada de cache do plano de consulta.

Considere o código a seguir, que é inferior, mas é apenas para exemplificar esta classe de consultas:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Uma versão mais rápida desse mesmo código envolveria chamar Skip com um lambda:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

O segundo trecho pode executar até 11% mais rápido porque o mesmo plano de consulta é usado toda vez que a consulta é executada, o que economiza tempo de CPU e evita poluir o cache de consulta. Além disso, como o parâmetro a ser ignorado está em um fechamento, o código também pode ser assim:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4,3 usando as propriedades de um objeto não mapeado

Quando uma consulta usa as propriedades de um tipo de objeto não mapeado como um parâmetro, a consulta não será armazenada em cache. Por exemplo:

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

Neste exemplo, suponha que a classe não Mapeadatype não faça parte do modelo de entidade. Essa consulta pode ser facilmente alterada para não usar um tipo não mapeado e, em vez disso, usar uma variável local como o parâmetro para a consulta:

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

Nesse caso, a consulta será capaz de ser armazenada em cache e se beneficiará do cache do plano de consulta.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4,4 vinculando a consultas que exigem recompilação

Seguindo o mesmo exemplo acima, se você tiver uma segunda consulta que dependa de uma consulta que precisa ser recompilada, toda a segunda consulta também será recompilada. Aqui está um exemplo para ilustrar esse cenário:

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

O exemplo é genérico, mas ilustra como vincular a firstQuery está fazendo com que secondQuery não possa ser armazenado em cache. Se firstQuery não tivesse sido uma consulta que requer recompilação, então secondQuery teria sido armazenado em cache.

## <a name="5-notracking-queries"></a>5 consultas de registro

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5,1 desabilitando o controle de alterações para reduzir a sobrecarga de gerenciamento de estado

Se você estiver em um cenário somente leitura e quiser evitar a sobrecarga de carregar os objetos no ObjectStateManager, poderá emitir consultas "sem rastreamento".  O controle de alterações pode ser desabilitado no nível de consulta.

Observe que, ao desabilitar o controle de alterações, você está efetivamente desativando o cache de objetos. Quando você consulta uma entidade, não podemos ignorar a materialização, puxando os resultados da consulta materializada anteriormente do ObjectStateManager. Se você estiver consultando repetidamente as mesmas entidades no mesmo contexto, poderá, na verdade, ver um benefício de desempenho ao habilitar o controle de alterações.

Ao consultar usando ObjectContext, as instâncias ObjectQuery e ObjectSet lembrarão um MergeOption assim que ele for definido, e as consultas que são compostas a elas herdarão o MergeOption efetivo da consulta pai. Ao usar DbContext, o controle pode ser desabilitado chamando o modificador AsNoTracking () no DbSet.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 desabilitando o controle de alterações para uma consulta ao usar DbContext

Você pode alternar o modo de uma consulta para NoTracking encadeando uma chamada para o método AsNoTracking () na consulta. Ao contrário de ObjectQuery, as classes DbSet e DbQuery na API DbContext não têm uma propriedade mutável para o MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 desabilitando o controle de alterações no nível de consulta usando ObjectContext

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 desabilitando o controle de alterações para um conjunto de entidades inteiro usando ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5,2 métricas de teste que demonstram o benefício de desempenho de consultas NoTracking

Neste teste, examinamos o custo de preencher o ObjectStateManager, comparando o rastreamento com consultas NoTracking para o modelo do Navision. Consulte o apêndice para obter uma descrição do modelo do Navision e os tipos de consultas que foram executadas. Neste teste, iteramos na lista de consultas e executamos cada uma delas. Executamos duas variações do teste, uma vez com consultas NoTracking e uma vez com a opção de mesclagem padrão de "AppendOnly". Executamos cada variação três vezes e pegamos o valor médio das execuções. Entre os testes, limpamos o cache de consulta no SQL Server e reduzimos o tempdb executando os seguintes comandos:

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Resultados de Teste, mediana em 3 execuções:

|                        | SEM CONTROLE – CONJUNTO DE TRABALHO | SEM RASTREAMENTO – TEMPO | SOMENTE ACRÉSCIMO – CONJUNTO DE TRABALHO | SOMENTE ACRÉSCIMO – TEMPO |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 MS         |
| **Entity Framework 6** | 647127040                 | 190228 MS          | 832798720                 | 195521 MS          |

O Entity Framework 5 terá uma superfície de memória menor no final da execução do que a Entity Framework 6. A memória adicional consumida pelo Entity Framework 6 é o resultado de estruturas de memória e código adicionais que permitem novos recursos e melhor desempenho.

Há também uma diferença clara na superfície da memória ao usar o ObjectStateManager. Entity Framework 5 aumentou seu espaço em 30% ao manter o controle de todas as entidades que materializamos do banco de dados. Entity Framework 6 aumentou seu espaço em 28% ao fazer isso.

Em termos de tempo, Entity Framework 6 desempenho Entity Framework 5 nesse teste por uma grande margem. Entity Framework 6 concluiu o teste em aproximadamente 16% do tempo consumido por Entity Framework 5. Além disso, Entity Framework 5 leva 9% mais tempo para ser concluído quando o ObjectStateManager está sendo usado. Em comparação, Entity Framework 6 está usando 3% mais tempo ao usar o ObjectStateManager.

## <a name="6-query-execution-options"></a>6 opções de execução de consulta

O Entity Framework oferece várias maneiras diferentes de consultar. Vamos dar uma olhada nas seguintes opções, comparar os prós e contras de cada um deles e examinar suas características de desempenho:

-   LINQ to Entities.
-   Nenhum LINQ to Entities de rastreamento.
-   Entity SQL sobre um ObjectQuery.
-   Entity SQL em um EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-linq-to-entities-queries"></a>6,1 LINQ to Entities consultas

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Prós**

-   Adequado para operações CUD.
-   Objetos totalmente materializados.
-   Mais simples de escrever com a sintaxe incorporada à linguagem de programação.
-   Bom desempenho.

**Contras**

-   Determinadas restrições técnicas, como:
    -   Padrões que usam DefaultIfEmpty para consultas de junção externa resultam em consultas mais complexas do que instruções de junção externa simples em Entity SQL.
    -   Você ainda não pode usar como com correspondência de padrão geral.

### <a name="62-no-tracking-linq-to-entities-queries"></a>6,2 sem acompanhamento LINQ to Entities consultas

Quando o contexto deriva ObjectContext:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Quando o contexto deriva DbContext:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Prós**

-   Desempenho aprimorado em consultas comuns do LINQ.
-   Objetos totalmente materializados.
-   Mais simples de escrever com a sintaxe incorporada à linguagem de programação.

**Contras**

-   Não é adequado para operações CUD.
-   Determinadas restrições técnicas, como:
    -   Padrões que usam DefaultIfEmpty para consultas de junção externa resultam em consultas mais complexas do que instruções de junção externa simples em Entity SQL.
    -   Você ainda não pode usar como com correspondência de padrão geral.

Observe que as consultas que as propriedades escalares do projeto não são controladas, mesmo que não esteja especificado. Por exemplo:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Essa consulta específica não especifica explicitamente o registro de alterações, mas como não está materializando um tipo conhecido pelo Gerenciador de estado de objeto, o resultado materializado não é acompanhado.

### <a name="63-entity-sql-over-an-objectquery"></a>6,3 Entity SQL em um ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Prós**

-   Adequado para operações CUD.
-   Objetos totalmente materializados.
-   Dá suporte ao cache do plano de consulta.

**Contras**

-   Envolve cadeias de caracteres de consulta textuais que são mais propensas a erros do usuário do que construções de consulta incorporadas à linguagem.

### <a name="64-entity-sql-over-an-entity-command"></a>6,4 Entity SQL sobre um comando de entidade

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

**Prós**

-   Dá suporte ao cache de plano de consulta no .NET 4,0 (o Caching de plano tem suporte de todos os outros tipos de consulta no .NET 4,5).

**Contras**

-   Envolve cadeias de caracteres de consulta textuais que são mais propensas a erros do usuário do que construções de consulta incorporadas à linguagem.
-   Não é adequado para operações CUD.
-   Os resultados não são materializados automaticamente e devem ser lidos no leitor de dados.

### <a name="65-sqlquery-and-executestorequery"></a>6,5 SQLQuery e ExecuteStoreQuery

SqlQuery no banco de dados:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery em DbSet:

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

**Prós**

-   Geralmente, o desempenho mais rápido, pois o compilador do plano é ignorado.
-   Objetos totalmente materializados.
-   Adequado para operações CUD quando usado no DbSet.

**Contras**

-   A consulta é textual e sujeita a erros.
-   A consulta está vinculada a um back-end específico usando a semântica de armazenamento em vez da semântica conceitual.
-   Quando a herança está presente, a consulta programada precisa considerar as condições de mapeamento para o tipo solicitado.

### <a name="66-compiledquery"></a>6,6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Prós**

-   Fornece uma melhoria de até 7% no desempenho em relação a consultas comuns do LINQ.
-   Objetos totalmente materializados.
-   Adequado para operações CUD.

**Contras**

-   Maior complexidade e sobrecarga de programação.
-   A melhoria do desempenho é perdida ao compor sobre uma consulta compilada.
-   Algumas consultas LINQ não podem ser escritas como um CompiledQuery, por exemplo, projeções de tipos anônimos.

### <a name="67-performance-comparison-of-different-query-options"></a>Comparação de desempenho de 6,7 de diferentes opções de consulta

Microbenchmarks simples em que a criação de contexto não foi atingido foram colocados no teste. Medimos a consulta de 5000 vezes para um conjunto de entidades não armazenadas em cache em um ambiente controlado. Esses números devem ser feitos com um aviso: eles não refletem os números reais produzidos por um aplicativo, mas, em vez disso, são uma medida muito precisa de quanto de uma diferença de desempenho há quando diferentes opções de consulta são comparadas maçãs-to-maçãs, excluindo o custo da criação de um novo contexto.

| EF  | Teste                                 | Tempo (MS) | Memória   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | Consulta LINQ do ObjectContext             | 2692      | 38277120 |
| EF5 | Consulta do LINQ do DbContext sem rastreamento     | 2818      | 41840640 |
| EF5 | Consulta do LINQ do DbContext                 | 2930      | 41771008 |
| EF5 | Consulta LINQ do ObjectContext sem rastreamento | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | Consulta LINQ do ObjectContext             | 3074      | 45248512 |
| EF6 | Consulta do LINQ do DbContext sem rastreamento     | 3125      | 47575040 |
| EF6 | Consulta do LINQ do DbContext                 | 3420      | 47652864 |
| EF6 | Consulta LINQ do ObjectContext sem rastreamento | 3593      | 45260800 |

![Benchmarks EF5 micro, 5000 iterações quentes](~/ef6/media/ef5micro5000warm.png)

![Benchmarks EF6 micro, 5000 iterações quentes](~/ef6/media/ef6micro5000warm.png)

Microparâmetros de comparação são muito sensíveis a pequenas alterações no código. Nesse caso, a diferença entre os custos de Entity Framework 5 e Entity Framework 6 se deve à adição de aprimoramentos de [interceptação](~/ef6/fundamentals/logging-and-interception.md) e [transacionais](~/ef6/saving/transactions.md). Esses números de MicroBenchMark, no entanto, são uma visão amplificada em um fragmento muito pequeno do que Entity Framework faz. Cenários do mundo real de consultas quentes não devem ver uma regressão de desempenho ao atualizar do Entity Framework 5 para Entity Framework 6.

Para comparar o desempenho do mundo real das diferentes opções de consulta, criamos 5 variações de teste separadas, nas quais usamos uma opção de consulta diferente para selecionar todos os produtos cujo nome de categoria é "bebidas". Cada iteração inclui o custo de criar o contexto e o custo de materializar todas as entidades retornadas. 10 iterações são executadas com tempo inoportuno antes de levar a soma de 1000 iterações com tempo. Os resultados mostrados são a execução mediana de 5 execuções de cada teste. Para obter mais informações, consulte o apêndice B, que inclui o código para o teste.

| EF  | Teste                                        | Tempo (MS) | Memória   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | Comando de entidade ObjectContext                | 621       | 39350272 |
| EF5 | Consulta SQL DbContext no banco de dados             | 825       | 37519360 |
| EF5 | Consulta de repositório de ObjectContext                   | 878       | 39460864 |
| EF5 | Consulta LINQ do ObjectContext sem rastreamento        | 969       | 38293504 |
| EF5 | Entidade ObjectContext SQL usando consulta de objeto | 1089      | 38981632 |
| EF5 | Consulta compilada de ObjectContext                | 1099      | 38682624 |
| EF5 | Consulta LINQ do ObjectContext                    | 1152      | 38178816 |
| EF5 | Consulta do LINQ do DbContext sem rastreamento            | 1208      | 41803776 |
| EF5 | Consulta SQL DbContext em DbSet                | 1414      | 37982208 |
| EF5 | Consulta do LINQ do DbContext                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | Comando de entidade ObjectContext                | 480       | 47247360 |
| EF6 | Consulta de repositório de ObjectContext                   | 493       | 46739456 |
| EF6 | Consulta SQL DbContext no banco de dados             | 614       | 41607168 |
| EF6 | Consulta LINQ do ObjectContext sem rastreamento        | 684       | 46333952 |
| EF6 | Entidade ObjectContext SQL usando consulta de objeto | 767       | 48865280 |
| EF6 | Consulta compilada de ObjectContext                | 788       | 48467968 |
| EF6 | Consulta do LINQ do DbContext sem rastreamento            | 878       | 47554560 |
| EF6 | Consulta LINQ do ObjectContext                    | 953       | 47632384 |
| EF6 | Consulta SQL DbContext em DbSet                | 1023      | 41992192 |
| EF6 | Consulta do LINQ do DbContext                        | 1290      | 47529984 |


![Iterações de 1000 de consulta passiva de EF5](~/ef6/media/ef5warmquery1000.png)

![Iterações de 1000 de consulta passiva de EF6](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Para fins de integridade, incluímos uma variação na qual executamos uma consulta Entity SQL em um EntityCommand. No entanto, como os resultados não são materializados para essas consultas, a comparação não é necessariamente igual a maçãs. O teste inclui uma aproximação de fechamento para materializar para tentar tornar a comparação mais justa.

Neste caso de ponta a ponta, Entity Framework 6 realizações Entity Framework 5 devido a melhorias de desempenho feitas em várias partes da pilha, incluindo uma inicialização DbContext muito mais clara e um metadataid mais rápido&lt;T&gt; pesquisas.

## <a name="7-design-time-performance-considerations"></a>7 considerações sobre o desempenho do tempo de design

### <a name="71-inheritance-strategies"></a>Estratégias de herança 7,1

Outra consideração de desempenho ao usar Entity Framework é a estratégia de herança que você usa. O Entity Framework dá suporte a três tipos básicos de herança e suas combinações:

-   Tabela por hierarquia (TPH) – em que cada conjunto de heranças é mapeado para uma tabela com uma coluna discriminadora para indicar qual tipo específico na hierarquia está sendo representado na linha.
-   Tabela por tipo (TPT) – onde cada tipo tem sua própria tabela no banco de dados; As tabelas filhas definem apenas as colunas que a tabela pai não contém.
-   Tabela por classe (TPC) – onde cada tipo tem sua própria tabela completa no banco de dados; As tabelas filhas definem todos os seus campos, incluindo aqueles definidos em tipos pai.

Se o modelo usar a herança TPT, as consultas geradas serão mais complexas do que aquelas geradas com as outras estratégias de herança, o que pode resultar em tempos de execução mais longos na loja.  Geralmente, levará mais tempo para gerar consultas em um modelo TPT e para materializar os objetos resultantes.

Consulte as "Considerações de desempenho ao usar herança TPT (tabela por tipo) no Entity Framework" postagem de blog do MSDN: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 evitando TPT em aplicativos Model First ou Code First

Quando você cria um modelo em um banco de dados existente que tem um esquema TPT, você não tem muitas opções. Mas ao criar um aplicativo usando Model First ou Code First, você deve evitar a herança TPT para preocupações com o desempenho.

Ao usar Model First no assistente de Entity Designer, você receberá TPT para qualquer herança em seu modelo. Se você quiser mudar para uma estratégia de herança TPH com Model First, você pode usar o "Entity Designer de banco de dados Generation Power Pack" disponíveis na Galeria do Visual Studio ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Ao usar Code First para configurar o mapeamento de um modelo com herança, o EF usará TPH por padrão, portanto, todas as entidades na hierarquia de herança serão mapeadas para a mesma tabela. Consulte a seção "Mapeamento com a API Fluent" do artigo "Código primeiro na entidade Framework4.1" na MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) para obter mais detalhes.

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a>7,2 Atualizando do EF4 para melhorar o tempo de geração do modelo

Uma melhoria específica de SQL Server para o algoritmo que gera a camada de repositório (SSDL) do modelo está disponível no Entity Framework 5 e 6 e como uma atualização para o Entity Framework 4 quando o Visual Studio 2010 SP1 é instalado. Os resultados de teste a seguir demonstram a melhoria ao gerar um modelo muito grande, nesse caso, o modelo do Navision. Consulte o Apêndice C para obter mais detalhes sobre ele.

O modelo contém conjuntos de entidades de 1005 e 4227 conjuntos de associação.

| Configuração                              | Divisão de tempo consumida                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | Geração de SSDL: 2 hr 27 min <br/> Geração de mapeamento: 1 segundo <br/> Geração de CSDL: 1 segundo <br/> Geração de objectlayer: 1 segundo <br/> Geração de exibição: 2 h 14 min |
| Visual Studio 2010 SP1, Entity Framework 4 | Geração de SSDL: 1 segundo <br/> Geração de mapeamento: 1 segundo <br/> Geração de CSDL: 1 segundo <br/> Geração de objectlayer: 1 segundo <br/> Geração de exibição: 1 hr 53 min   |
| Visual Studio 2013, Entity Framework 5     | Geração de SSDL: 1 segundo <br/> Geração de mapeamento: 1 segundo <br/> Geração de CSDL: 1 segundo <br/> Geração de objectlayer: 1 segundo <br/> Geração de exibição: 65 minutos    |
| Visual Studio 2013, Entity Framework 6     | Geração de SSDL: 1 segundo <br/> Geração de mapeamento: 1 segundo <br/> Geração de CSDL: 1 segundo <br/> Geração de objectlayer: 1 segundo <br/> Geração de exibição: 28 segundos.   |


Vale a pena observar que, ao gerar o SSDL, a carga é quase totalmente gasta no SQL Server, enquanto o computador de desenvolvimento do cliente está aguardando ociosidade para que os resultados voltem do servidor. Os DBAs devem especialmente apreciar essa melhoria. Também vale a pena observar que, essencialmente, o custo total da geração de modelos ocorre na geração de exibição agora.

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a>7,3 dividindo modelos grandes com Database First e Model First

À medida que o tamanho do modelo aumenta, a superfície do designer fica congestionada e difícil de usar. Normalmente, consideramos que um modelo com mais de 300 entidades seja muito grande para usar efetivamente o designer. A seguinte postagem de blog descreve as várias opções para a divisão de grandes modelos: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

A postagem foi escrita para a primeira versão do Entity Framework, mas as etapas ainda se aplicam.

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a>7,4 considerações de desempenho com o controle da fonte de dados de entidade

Vimos casos em testes de estresse e desempenho multithread em que o desempenho de um aplicativo Web que usa o controle de EntityDataSource deteriora significativamente. A causa subjacente é que a EntityDataSource chama repetidamente MetadataWorkspace. LoadFromAssembly nos assemblies referenciados pelo aplicativo Web para descobrir os tipos a serem usados como entidades.

A solução é definir o ContextTypeName da EntityDataSource para o nome do tipo de sua classe ObjectContext derivada. Isso desativa o mecanismo que examina todos os assemblies referenciados para tipos de entidade.

Definir o campo ContextTypeName também impede um problema funcional em que a EntityDataSource no .NET 4,0 gera um ReflectionTypeLoadException quando não é possível carregar um tipo de um assembly por meio de reflexão. Esse problema foi corrigido no .NET 4,5.

### <a name="75-poco-entities-and-change-tracking-proxies"></a>7,5 entidades POCO e proxies de controle de alterações

Entity Framework permite que você use classes de dados personalizadas junto com seu modelo de dados sem fazer modificações nas próprias classes de dados. Isso significa que você pode usar os objetos CLR "Plain-Old" (POCO), como objetos de domínio existentes, com seu modelo de dados. Essas classes de dados POCO (também conhecidas como objetos Persistence-que desconhecem), que são mapeadas para entidades que são definidas em um modelo de dados, dão suporte à maioria dos comportamentos de consulta, inserção, atualização e exclusão como tipos de entidade que são gerados pelas ferramentas de Modelo de Dados de Entidade.

Entity Framework também pode criar classes de proxy derivadas de seus tipos POCO, que são usados quando você deseja habilitar recursos como carregamento lento e controle de alterações automático em entidades POCO. Suas classes POCO devem atender a certos requisitos para permitir que o Entity Framework para poderem usar proxies, conforme descrito aqui: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).

Os proxies de controle de chance notificarão o Gerenciador de estado do objeto sempre que qualquer uma das propriedades de suas entidades tiver seu valor alterado, portanto Entity Framework sabe o estado real de suas entidades o tempo todo. Isso é feito adicionando eventos de notificação ao corpo dos métodos setter de suas propriedades e fazendo com que o Gerenciador de estado do objeto processe tais eventos. Observe que a criação de uma entidade proxy normalmente será mais cara do que a criação de uma entidade POCO não proxy devido ao conjunto adicionado de eventos criado por Entity Framework.

Quando uma entidade POCO não tem um proxy de controle de alterações, as alterações são encontradas comparando o conteúdo de suas entidades com uma cópia de um estado salvo anterior. Essa comparação profunda se tornará um processo longo quando você tiver muitas entidades em seu contexto ou quando suas entidades tiverem uma quantidade muito grande de propriedades, mesmo que nenhuma delas seja alterada desde que a última comparação tenha ocorrido.

Em Resumo: você pagará um impacto no desempenho ao criar o proxy de controle de alterações, mas o controle de alterações o ajudará a acelerar o processo de detecção de alterações quando suas entidades tiverem muitas propriedades ou quando você tiver muitas entidades em seu modelo. Para entidades com um pequeno número de propriedades em que a quantidade de entidades não cresce muito, ter proxies de controle de alterações pode não ser muito vantajoso.

## <a name="8-loading-related-entities"></a>8 carregando entidades relacionadas

### <a name="81-lazy-loading-vs-eager-loading"></a>8,1 carregamento lento versus carregamento adiantado

O Entity Framework oferece várias maneiras diferentes de carregar as entidades relacionadas à sua entidade de destino. Por exemplo, quando você consulta produtos, há diferentes maneiras pelas quais os pedidos relacionados serão carregados no Gerenciador de estado de objeto. Do ponto de vista do desempenho, a maior pergunta a ser considerada ao carregar entidades relacionadas será usar o carregamento lento ou o carregamento adiantado.

Ao usar o carregamento adiantado, as entidades relacionadas são carregadas junto com o conjunto de entidades de destino. Você usa uma instrução include em sua consulta para indicar quais entidades relacionadas deseja trazer.

Ao usar o carregamento lento, a consulta inicial só traz a entidade de destino definida. Mas sempre que você acessa uma propriedade de navegação, outra consulta é emitida no repositório para carregar a entidade relacionada.

Depois que uma entidade for carregada, qualquer consulta adicional para a entidade a carregará diretamente do Gerenciador de estado de objeto, independentemente de você estar usando carregamento lento ou carregamento adiantado.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8,2 como escolher entre carregamento lento e carregamento adiantado

O importante é que você compreenda a diferença entre o carregamento lento e o carregamento adiantado para que você possa fazer a escolha correta para seu aplicativo. Isso o ajudará a avaliar a compensação entre várias solicitações no banco de dados em comparação com uma única solicitação que pode conter uma carga grande. Pode ser apropriado usar o carregamento adiantado em algumas partes do seu aplicativo e o carregamento lento em outras partes.

Como um exemplo do que está acontecendo nos bastidores, suponha que você queira consultar os clientes que vivem no Reino Unido e sua contagem de pedidos.

**Usando o carregamento adiantado**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Usando o carregamento lento**

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

Ao usar o carregamento adiantado, você emitirá uma única consulta que retorna todos os clientes e todos os pedidos. O comando Store é semelhante a:

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

Ao usar o carregamento lento, você emitirá a seguinte consulta inicialmente:

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

E sempre que você acessa a propriedade de navegação Orders de um cliente, outra consulta como a seguinte é emitida em relação à loja:

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

Para obter mais informações, consulte [carregando objetos relacionados](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>roteiro de carregamento lento do 8.2.1 versus carregamento adiantado

Não há tal coisa como um único ajuste para escolher o carregamento adiantado versus o carregamento lento. Tente primeiro entender as diferenças entre ambas as estratégias para que você possa fazer uma decisão bem informada; Além disso, considere se o seu código se ajusta a qualquer um dos seguintes cenários:

| Cenário                                                                    | Nossa sugestão                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Você precisa acessar muitas propriedades de navegação das entidades buscadas? | **Nenhuma** das duas opções provavelmente fará. No entanto, se a carga que sua consulta está trazendo não for muito grande, você poderá experimentar benefícios de desempenho usando o carregamento adiantado, pois ela exigirá menos viagens de ida e volta da rede para materializar seus objetos. <br/> <br/> **Sim** – se você precisar acessar muitas propriedades de navegação das entidades, faça isso usando várias instruções include em sua consulta com carregamento adiantado. Quanto mais entidades você incluir, maior será a carga que a consulta retornará. Depois de incluir três ou mais entidades em sua consulta, considere alternar para o carregamento lento. |
| Você sabe exatamente quais dados serão necessários em tempo de execução?                   | O carregamento **não** lento será melhor para você. Caso contrário, você pode acabar consultando os dados que não serão necessários. <br/> <br/> **Sim** – o carregamento adiantado é provavelmente sua melhor aposta; Ele ajudará a carregar conjuntos inteiros mais rapidamente. Se sua consulta exigir a busca de uma quantidade muito grande de dados, isso se tornará muito lento e, em vez disso, experimente o carregamento lento.                                                                                                                                                                                                                                                       |
| Seu código está sendo executado longe do seu banco de dados? (maior latência de rede)  | **Não** , quando a latência de rede não é um problema, usar o carregamento lento pode simplificar seu código. Lembre-se de que a topologia do seu aplicativo pode mudar, portanto, não tome a proximidade do banco de dados. <br/> <br/> **Sim** – quando a rede é um problema, apenas você pode decidir o que se adapta melhor ao seu cenário. Normalmente, o carregamento adiantado será melhor porque requer menos viagens de ida e volta.                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a>8.2.2 preocupações com o desempenho com vários inclusões

Quando ouvimos perguntas de desempenho que envolvem problemas de tempo de resposta do servidor, a origem do problema é consultada frequentemente com várias instruções include. Embora a inclusão de entidades relacionadas em uma consulta seja eficiente, é importante entender o que está acontecendo nos bastidores.

Leva um tempo relativamente longo para uma consulta com várias instruções include em ti para percorrer nosso compilador de plano interno para produzir o comando Store. A maior parte desse tempo é gasta tentando otimizar a consulta resultante. O comando Store gerado conterá uma junção externa ou Union para cada include, dependendo do seu mapeamento. Consultas como essa irão trazer gráficos grandes conectados do seu banco de dados em uma única carga, o que causará problemas de largura de banda, especialmente quando houver muita redundância na carga (por exemplo, quando vários níveis de inclusão forem usados para atravessar associações na direção um-para-muitos).

Você pode verificar se há casos em que suas consultas estão retornando cargas excessivamente grandes acessando o TSQL subjacente para a consulta usando ToTraceString e executando o comando Store em SQL Server Management Studio para ver o tamanho da carga. Nesses casos, você pode tentar reduzir o número de instruções include em sua consulta para apenas trazer os dados de que precisa. Ou talvez você consiga dividir a consulta em uma sequência menor de subconsultas, por exemplo:

**Antes de dividir a consulta:**

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

Isso só funcionará em consultas rastreadas, pois estamos usando a capacidade de o contexto ter que executar a resolução de identidade e a correção de associação automaticamente.

Assim como ocorre com o carregamento lento, a compensação será mais consultas para cargas menores. Você também pode usar projeções de propriedades individuais para selecionar explicitamente apenas os dados necessários de cada entidade, mas você não carregará entidades nesse caso, e não haverá suporte para atualizações.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>solução alternativa de 8.2.3 para obter o carregamento lento de propriedades

Entity Framework atualmente não dá suporte ao carregamento lento de propriedades escalares ou complexas. No entanto, nos casos em que você tem uma tabela que inclui um objeto grande, como um BLOB, você pode usar a divisão de tabela para separar as propriedades grandes em uma entidade separada. Por exemplo, suponha que você tenha uma tabela Product que inclui uma coluna de foto varbinary. Se você não precisa acessar com frequência essa propriedade em suas consultas, você pode usar a divisão de tabela para trazer apenas as partes da entidade que você normalmente precisa. A entidade que representa a foto do produto só será carregada quando você precisar explicitamente dela.

Um bom recurso que mostra como habilitar a divisão de tabela é a postagem no blog do Gil Fink "Tabela dividindo no Entity Framework": \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 outras considerações

### <a name="91-server-garbage-collection"></a>Coleta de lixo do servidor 9,1

Alguns usuários podem experimentar a contenção de recursos que limita o paralelismo esperado quando o coletor de lixo não está configurado corretamente. Sempre que o EF é usado em um cenário multi-threaded ou em qualquer aplicativo que se assemelha a um sistema do lado do servidor, certifique-se de habilitar a coleta de lixo do servidor. Isso é feito por meio de uma configuração simples no arquivo de configuração do aplicativo:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Isso deve diminuir a contenção de thread e aumentar sua taxa de transferência em até 30% em cenários saturados da CPU. Em termos gerais, você deve sempre testar como seu aplicativo se comporta usando a coleta de lixo clássica (que é mais bem ajustada para cenários do lado do cliente e da interface do usuário), bem como a coleta de lixo do servidor.

### <a name="92-autodetectchanges"></a>9,2 AutoDetectChanges

Como mencionado anteriormente, Entity Framework pode mostrar problemas de desempenho quando o cache de objetos tem muitas entidades. Determinadas operações, como adicionar, remover, localizar, entrada e SaveChanges, disparam chamadas para DetectChanges que podem consumir uma grande quantidade de CPU com base em quão grande o cache de objetos se tornou. O motivo disso é que o cache de objeto e o Gerenciador de estado de objeto tentam permanecer o mais sincronizado possível em cada operação executada em um contexto para que os dados produzidos tenham a garantia de estar corretos em uma ampla gama de cenários.

Geralmente, é uma boa prática deixar a detecção de alteração automática de Entity Framework habilitada para toda a vida do seu aplicativo. Se seu cenário estiver sendo afetado negativamente pelo alto uso da CPU e seus perfis indicarem que o culpado é a chamada para DetectChanges, considere desativar temporariamente o AutoDetectChanges na parte confidencial do seu código:

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

Antes de desligar o AutoDetectChanges, é bom entender que isso pode fazer com que Entity Framework perca sua capacidade de controlar determinadas informações sobre as alterações que estão ocorrendo nas entidades. Se for manipulado incorretamente, isso poderá causar inconsistência de dados em seu aplicativo. Para obter mais informações sobre como desativar AutoDetectChanges, leia \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93-context-per-request"></a>contexto de 9,3 por solicitação

Os contextos de Entity Framework devem ser usados como instâncias de curta duração para fornecer a melhor experiência de desempenho. Os contextos devem ser de curta duração e descartados e, como tal, foram implementados para serem muito leves e reutilizar os metadados sempre que possível. Em cenários da Web, é importante ter isso em mente e não ter um contexto por mais do que a duração de uma única solicitação. Da mesma forma, em cenários que não são da Web, o contexto deve ser Descartado com base no seu entendimento dos diferentes níveis de cache no Entity Framework. Em termos gerais, um deve evitar ter uma instância de contexto durante toda a vida útil do aplicativo, bem como contextos por thread e contextos estáticos.

### <a name="94-database-null-semantics"></a>9,4 semântica nula do banco de dados

Entity Framework, por padrão, gerará o código SQL que tem a semântica de comparação C\# NULL. Considere o exemplo de consulta seguinte:

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

Neste exemplo, estamos comparando várias variáveis anuláveis em relação às propriedades anuláveis na entidade, como CódigoDoFornecedor e UnitPrice. O SQL gerado para essa consulta perguntará se o valor do parâmetro é igual ao valor da coluna ou se os valores do parâmetro e da coluna são nulos. Isso ocultará a maneira como o servidor de banco de dados trata os valores nulos e fornecerá uma experiência de C\# nula consistente entre diferentes fornecedores de banco de dados. Por outro lado, o código gerado é um pouco complicadas e pode não funcionar bem quando a quantidade de comparações na instrução WHERE da consulta cresce para um grande número.

Uma maneira de lidar com essa situação é usando a semântica nula do banco de dados. Observe que isso pode potencialmente se comportar de modo diferente com a semântica C\# NULL, já que agora Entity Framework gerará um SQL mais simples que expõe a maneira como o mecanismo de banco de dados lida com valores nulos. A semântica nula do banco de dados pode ser ativada por contexto com uma única linha de configuração em relação à configuração de contexto:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Consultas de pequeno a médio porte não exibirão uma melhoria de desempenho perceptível ao usar a semântica nula do banco de dados, mas a diferença será mais perceptível em consultas com um grande número de comparações nulas possíveis.

Na consulta de exemplo acima, a diferença de desempenho era menor que 2% em um MicroBenchMark em execução em um ambiente controlado.

### <a name="95-async"></a>9,5 assíncrono

O Entity Framework 6 introduziu o suporte de operações assíncronas ao executar o .NET 4,5 ou posterior. Para a maior parte, os aplicativos que têm a contenção de e/s que se beneficiarão mais do uso de consultas assíncronas e operações de salvamento. Se seu aplicativo não sofre de contenção de e/s, o uso de Async será, nos melhores casos, executado de forma síncrona e retorna o resultado na mesma quantidade de tempo que uma chamada síncrona, ou na pior das hipóteses, basta adiar a execução para uma tarefa assíncrona e adicionar um Tim extra e para a conclusão do seu cenário.

Para obter informações sobre o trabalho como assíncrono de programação que ajudarão você a decidir se async melhorará o desempenho do seu aplicativo visite [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx). Para obter mais informações sobre o uso de operações assíncronas em Entity Framework, consulte [Async Query e Save](~/ef6/fundamentals/async.md
).

### <a name="96-ngen"></a>NGEN 9,6

O Entity Framework 6 não é fornecido na instalação padrão do .NET Framework. Assim, os assemblies de Entity Framework não são NGEN por padrão, o que significa que todo o código Entity Framework está sujeito aos mesmos custos de JIT'ing como qualquer outro assembly MSIL. Isso pode prejudicar a experiência de F5 enquanto desenvolve e também a inicialização a frio de seu aplicativo nos ambientes de produção. Para reduzir os custos de CPU e memória do JIT'ing, é aconselhável que as imagens de Entity Framework do NGEN sejam as mesmas, conforme apropriado. Para obter mais informações sobre como melhorar o desempenho de inicialização do Entity Framework 6 com o NGEN, consulte [melhorando o desempenho de inicialização com o NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97-code-first-versus-edmx"></a>9,7 Code First versus EDMX

Entity Framework motivos sobre o problema de incompatibilidade de impedância entre a programação orientada a objeto e bancos de dados relacionais por ter uma representação na memória do modelo conceitual (os objetos), o esquema de armazenamento (o banco de dados) e um mapeamento entre o duas. Esses metadados são chamados de Modelo de Dados de Entidade ou EDM para curto. Neste EDM, Entity Framework derivará as exibições para dados de ida e volta dos objetos na memória para o banco de dado e vice-versa.

Quando Entity Framework é usado com um arquivo EDMX que especifica formalmente o modelo conceitual, o esquema de armazenamento e o mapeamento, o estágio de carregamento de modelo precisa apenas validar que o EDM está correto (por exemplo, verifique se não há mapeamentos ausentes) e, em seguida, gere os modos de exibição e, em seguida, valide as exibições e tenha esses metadados prontos para uso. Somente então uma consulta pode ser executada ou novos dados são salvos no armazenamento de dados.

A abordagem Code First é, em seu coração, um gerador de Modelo de Dados de Entidade sofisticado. O Entity Framework tem de produzir um EDM a partir do código fornecido; Ele faz isso analisando as classes envolvidas no modelo, aplicando convenções e Configurando o modelo por meio da API fluente. Depois que o EDM é criado, o Entity Framework basicamente se comporta da mesma forma que teria um arquivo EDMX presente no projeto. Assim, a criação do modelo a partir de Code First adiciona complexidade extra que se traduz em um tempo de inicialização mais lento para o Entity Framework quando comparado a ter um EDMX. O custo é completamente dependente do tamanho e da complexidade do modelo que está sendo compilado.

Ao optar por usar o EDMX versus Code First, é importante saber que a flexibilidade introduzida pelo Code First aumenta o custo da criação do modelo pela primeira vez. Se o seu aplicativo puder resistir ao custo dessa primeira carga, normalmente Code First será a melhor opção.

## <a name="10-investigating-performance"></a>10 investigando o desempenho

### <a name="101-using-the-visual-studio-profiler"></a>10,1 usando o criador de perfil do Visual Studio

Se você estiver tendo problemas de desempenho com o Entity Framework, poderá usar um criador de perfil como aquele interno do Visual Studio para ver onde seu aplicativo está gastando seu tempo. Esta é a ferramenta são usadas para gerar os gráficos de pizza na postagem do blog "Explorando o desempenho do ADO.NET Entity Framework - parte 1" ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) que mostram onde o Entity Framework gasta seu tempo durante consultas frios e quentes.

A postagem de blog "criação de perfil Entity Framework usando o Visual Studio 2010 Profiler" escrito pela equipe de consultoria para clientes de modelagem e dados mostra um exemplo real de como eles usaram o criador de perfil para investigar um problema de desempenho.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Esta postagem foi escrita para um aplicativo do Windows. Se você precisar criar um perfil de um aplicativo Web, o gravador de desempenho do Windows (WPR) e as ferramentas do Windows Performance Analyzer (WPA) podem funcionar melhor do que trabalhar no Visual Studio. WPR e WPA fazem parte do kit de ferramentas de desempenho do Windows, incluído no kit de avaliação e implantação do Windows ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>criação de perfil de aplicativo/banco de dados 10,2

Ferramentas como o criador de perfil interno ao Visual Studio informam onde seu aplicativo está gastando tempo.  Outro tipo de criador de perfil está disponível que executa a análise dinâmica de seu aplicativo em execução, em produção ou pré-produção, dependendo das necessidades e procura armadilhas e antipadrões comuns de acesso ao banco de dados.

Dois criadores de perfil disponíveis no mercado são o Entity Framework Profiler ( \<http://efprof.com>) e ORMProfiler ( \<http://ormprofiler.com>).

Se seu aplicativo for um aplicativo MVC usando Code First, você poderá usar o MiniProfiler da StackExchange. Scott Hanselman descreve esta ferramenta em seu blog em: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Para obter mais informações sobre a criação de perfil da atividade de banco de dados do seu aplicativo, consulte o artigo da MSDN Magazine de Julie Lerman, intitulado [a atividade de banco de dados de criação de perfil no Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>Agente de banco de dados 10,3

Se você estiver usando o Entity Framework 6, considere também usar a funcionalidade de log interna. A propriedade de banco de dados do contexto pode ser instruída para registrar sua atividade por meio de uma configuração simples de uma linha:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

Neste exemplo, a atividade do banco de dados será registrada no console do, mas a propriedade log poderá ser configurada para chamar qualquer ação&lt;cadeia de caracteres&gt; delegado.

Se você quiser habilitar o log de banco de dados sem recompilação e estiver usando Entity Framework 6,1 ou posterior, poderá fazer isso adicionando um interceptor no arquivo Web. config ou app. config do seu aplicativo.

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

## <a name="11-appendix"></a>11 apêndice

### <a name="111-a-test-environment"></a>11,1 A. ambiente de teste

Esse ambiente usa uma configuração de 2 máquinas com o banco de dados em um computador separado do aplicativo cliente. Os computadores estão no mesmo rack, portanto, a latência de rede é relativamente baixa, mas mais realista do que um ambiente de máquina única.

#### <a name="1111-app-server"></a>Servidor de aplicativos 11.1.1

##### <a name="11111-software-environment"></a>Ambiente de software 11.1.1.1

-   Ambiente de software Entity Framework 4
    -   Nome do so: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 – Ultimate.
    -   Visual Studio 2010 SP1 (somente para algumas comparações).
-   Ambiente de software Entity Framework 5 e 6
    -   Nome do so: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate.

##### <a name="11112-hardware-environment"></a>Ambiente de hardware 11.1.1.2

-   Processador duplo: Intel (R) Xeon (R) CPU L5520 W3530 @ 2.27 GHz, 2261 Mhz8 GHz, 4 núcleo (s), 84 processador (es) lógico (s).
-   2412 GB RamRAM.
-   136 GB SCSI250GB SATA 7200 rpm/s Drive dividido em 4 partições.

#### <a name="1112-db-server"></a>servidor do 11.1.2 DB

##### <a name="11121-software-environment"></a>Ambiente de software 11.1.2.1

-   Nome do so: Windows Server 2008 R 28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122-hardware-environment"></a>Ambiente de hardware 11.1.2.2

-   Processador único: Intel (R) Xeon (R) CPU L5520 @ 2.27 GHz, 2261 MhzES-1620 0 @ 3,60 GHz, 4 núcleo (s), 8 processadores lógicos.
-   824 GB RamRAM.
-   465 GB ATA500GB SATA 7200 rpm 6 GB/s Drive dividido em 4 partições.

### <a name="112-b-query-performance-comparison-tests"></a>11,2 B. testes de comparação de desempenho de consulta

O modelo Northwind foi usado para executar esses testes. Ele foi gerado a partir do banco de dados usando o designer de Entity Framework. Em seguida, o código a seguir foi usado para comparar o desempenho das opções de execução de consulta:

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

### <a name="113-c-navision-model"></a>Modelo de 11,3 C. Navision

O banco de dados do Navision é um banco de dados grande usado para demonstrar o Microsoft Dynamics – NAV. O modelo conceitual gerado contém conjuntos de entidades 1005 e conjuntos de associação 4227. O modelo usado no teste é "simples" – nenhuma herança foi adicionada a ele.

#### <a name="1131-queries-used-for-navision-tests"></a>consultas 11.3.1 usadas para testes do Navision

A lista de consultas usada com o modelo do Navision contém três categorias de consultas de Entity SQL:

##### <a name="11311-lookup"></a>pesquisa de 11.3.1.1

Uma consulta de pesquisa simples sem agregações

-   Contagem: 16232
-   Exemplo:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a>11.3.1.2 SingleAggregating

Uma consulta de BI normal com várias agregações, mas sem subtotais (consulta única)

-   Contagem: 2313
-   Exemplo:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Em que MDF\_SessionLogin\_tempo\_Max () é definido no modelo como:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Uma consulta de BI com agregações e subtotais (por meio de Union All)

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
