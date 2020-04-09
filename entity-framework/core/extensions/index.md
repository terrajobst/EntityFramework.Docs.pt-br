---
title: Ferramentas e Extensões – EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634235"
---
# <a name="ef-core-tools--extensions"></a>Ferramentas e Extensões do EF Core

Essas ferramentas e extensões oferecem funcionalidades adicionais para o Entity Framework Core 2.1 e as versões posteriores.

> [!IMPORTANT]  
> As extensões são compostas de diversas fontes e não são mantidas como parte do projeto do Entity Framework Core. Ao considerar uma extensão de terceiros, avalie a qualidade, o licenciamento, a compatibilidade, o suporte, entre outras coisas, para garantir que ela atenda às suas necessidades. Em particular, uma extensão criada para uma versão mais antiga do EF Core pode precisar de atualização para que funcione com as versões mais recentes.

## <a name="tools"></a>Ferramentas

### <a name="llblgen-pro"></a>LLBLGen Pro

O LLBLGen Pro é uma solução de modelagem de entidade compatível com o Entity Framework e o Entity Framework Core. Com ele, é possível definir facilmente o modelo de entidade e mapeá-lo em seu banco de dados usando as abordagens Model First ou Database First. Assim, você pode começar a escrever consultas imediatamente. Para o EF Core: 2.

[Site](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Desenvolvedor de Entidade Devart

O Desenvolvedor de Entidade é um potente designer de ORM para o ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL. Ele é compatível com a criação de modelos do EF Core visualmente, usando as primeiras abordagens do modelo ou do banco de dados e a geração de código em C # ou Visual Basic. Para o EF Core: 2.

[Site](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a>nHydrate ORM para Entity Framework

Um ORM que cria classes extensíveis fortemente tipadas para Entity Framework. O código gerado é Entity Framework Core. Não há nenhuma diferença. Isso não é uma substituição do EF nem de um ORM personalizado. É um visual, uma camada de modelagem que permite que uma equipe gerencie esquemas de banco de dados complexos. Ele funciona bem com software SCM como o Git, permitindo o acesso de vários usuários ao seu modelo com o mínimo de conflito. O instalador rastreia as alterações do modelo e cria scripts de atualização. Para o EF Core: 3.

[Site do Github](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

O EF Core Power Tools é uma extensão do Visual Studio que expõe várias tarefas de tempo de design do EF Core em uma interface do usuário simples. Ele inclui a engenharia reversa do DbContext e classes de entidade de bancos de dados existentes e do [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), gerenciamento de migrações de banco de dados e visualizações de modelo. Para o EF Core: 2, 3.

[Wiki do GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Editor Visual do Entity Framework

O Entity Framework Visual Editor é uma extensão do Visual Studio que adiciona um designer de ORM ao design visual do EF 6 e às classes do EF Core. O código é gerado usando modelos T4. Portanto, ele pode ser personalizado para atender a quaisquer necessidades. Ele é compatível com herança, associações unidirecionais e bidirecionais, enumerações e com a capacidade de atribuir código de cor às suas classes e de adicionar blocos de texto para explicar partes potencialmente misteriosas do seu design. Para o EF Core: 2.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

O CatFactory é um mecanismo de scaffolding para o .NET Core que pode automatizar a geração de classes DbContext, entidades, configurações de mapeamento e classes de repositório de um banco de dados do SQL Server. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Gerador do Entity Framework Core da LoreSoft

O Entity Framework Core Generator (efg) é uma ferramenta de CLI do .NET Core que pode gerar modelos do EF Core de um banco de dados existente, muito parecido com `dotnet ef dbcontext scaffold`, mas que também é compatível com a [regeneração](https://efg.loresoft.com/en/latest/regeneration/) do código seguro por meio da substituição de região ou da análise dos arquivos de mapeamento. Essa ferramenta é compatível com a geração de modelos de exibição, com a validação e o código do mapeador de objetos. Para o EF Core: 2.

[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentação](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Extensões

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Uma biblioteca de plug-in que permite registrar automaticamente as alterações de dados feitas pelo EF Core em uma tabela de histórico. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Uma extensão que permite armazenar os resultados das consultas do EF Core em um cache de segundo nível, para que as execuções subsequentes das mesmas consultas possam evitar o acesso ao banco de dados e recuperar os dados diretamente do cache. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a>Geco

O Geco (Generator Console) é um gerador de código simples baseado em um projeto de console, que é executado no .NET Core e usa cadeias de caracteres interpoladas em C# para geração de código. O Geco inclui um gerador de modelo reverso para o EF Core, com suporte para pluralização, singularização e modelos editáveis. Ele também fornece um gerador de script de dados de semente, um executor de script e um limpador de banco de dados. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore.Scaffolding.Handlebars 

Permite a personalização de classes com engenharia reversa de um banco de dados existente usando a cadeia de ferramentas do Entity Framework Core com modelos de Handlebars. Para o EF Core: 2, 3.

[Repositório do GitHub](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore 

O NeinLinq estende provedores LINQ, como o Entity Framework, para habilitar a reutilização de funções, reescrever consultas e criar consultas dinâmicas usando predicados e seletores traduzíveis. Para o EF Core: 2, 3.

[Repositório do GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Um plug-in para Microsoft.EntityFrameworkCore para dar suporte ao repositório, aos padrões de unidade de trabalho e a vários bancos de dados compatíveis com a transação distribuída. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Extensões do EF Core para operações em massa (Inserir, Atualizar e Excluir). Para o EF Core: 2, 3.

[Repositório do GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Adiciona a pluralização de tempo de design. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Reativação do atributo [Index] para o EF Core (com a extensão para criação de modelo). Para o EF Core: 2, 3.

[Repositório do GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Fornece um wrapper em torno do provedor de banco de dados na memória EF Core. Faz com que ele funcione mais como um provedor relacional. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Uma implementação de suporte temporal. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>EfCoreTemporalTable

Realize consultas temporais com facilidade em seu banco de dados favorito usando métodos de extensão introduzidos: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`. Para o EF Core: 3.

[Repositório do GitHub](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a>EFCore.TimeTraveler

Permita consultas completas do Entity Framework Core ao [Histórico Temporal do SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) usando o código, as entidades e os mapeamentos do EF Core que você já definiu.  Viaje ao longo do tempo encapsulando seu código em `using (TemporalQuery.AsOf(targetDateTime)) {...}`. Para o EF Core: 3.

[Repositório do GitHub](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore.TemporalTables

Biblioteca de extensões para Entity Framework Core que permite aos desenvolvedores que usam SQL Server usar tabelas temporais com facilidade. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Um cache de consulta de segundo nível de alto desempenho. Para o EF Core: 2.

[Repositório do GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Amplia o DbContext com recursos como: Filtro de Include, Auditoria, Cache, Consultar futuro, Exclusão em lote, Atualização em lote e muito mais. Para o EF Core: 2, 3.

[Site](https://entityframework-plus.net/)
[Repositório do GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Extensões do Entity Framework

Amplia o DbContext com operações em massa de alto desempenho: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge e muito mais. Para o EF Core: 2, 3.

[Site](https://entityframework-extensions.net/)

### <a name="expressionify"></a>Expressionify

Adicione suporte para chamar métodos de extensão em lambdas LINQ. Para o EF Core: 3.1

[Repositório do GitHub](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a>XLinq

LINQ (Consulta Integrada à Linguagem) para bancos de dados relacionais. Ele permite que você use C# para escrever consultas fortemente tipadas. Para o EF Core: 3.1

- Suporte completo a C# para criação de consultas: várias instruções dentro de lambda, variáveis, funções etc.
- Nenhuma lacuna semântica com o SQL. O XLinq declara instruções SQL (como `SELECT`, `FROM`, `WHERE`) como métodos de C# de primeira classe, combinando sintaxe familiar com IntelliSense, segurança de tipo e refatoração.

Como resultado, o SQL se torna apenas mais uma biblioteca de classes expondo a própria API localmente, literalmente um *"SQL integrado à linguagem"*.

[Site](http://xlinq.live/)
