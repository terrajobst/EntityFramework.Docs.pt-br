---
title: Ferramentas e Extensões – EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e70011b42818e4df1ec5b9b88d7adb9d36bb26f1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654798"
---
# <a name="ef-core-tools--extensions"></a>Ferramentas e Extensões do EF Core

Essas ferramentas e extensões oferecem funcionalidades adicionais para o Entity Framework Core 2.0 e as versões posteriores.

> [!IMPORTANT]  
> As extensões são compostas de diversas fontes e não são mantidas como parte do projeto do Entity Framework Core. Ao considerar uma extensão de terceiros, avalie a qualidade, o licenciamento, a compatibilidade, o suporte, entre outras coisas, para garantir que ela atenda às suas necessidades.

## <a name="tools"></a>Ferramentas

### <a name="llblgen-pro"></a>LLBLGen Pro

O LLBLGen Pro é uma solução de modelagem de entidade compatível com o Entity Framework e o Entity Framework Core. Com ele, é possível definir facilmente o modelo de entidade e mapeá-lo em seu banco de dados usando as abordagens Model First ou Database First. Assim, você pode começar a escrever consultas imediatamente.

[Site](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Desenvolvedor de Entidade Devart

O Desenvolvedor de Entidade é um potente designer de ORM para o ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL. Ele é compatível com a criação de modelos do EF Core visualmente, usando as primeiras abordagens do modelo ou do banco de dados e a geração de código em C # ou Visual Basic.

[Site](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

O EF Core Power Tools é uma extensão do Visual Studio 2017 que expõe várias tarefas de tempo de design do EF Core em uma interface do usuário simples. Ele inclui a engenharia reversa do DbContext e classes de entidade de bancos de dados existentes e do [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), gerenciamento de migrações de banco de dados e visualizações de modelo.

[Wiki do GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Editor Visual do Entity Framework

O Entity Framework Visual Editor é uma extensão do Visual Studio que adiciona um designer de ORM ao design visual do EF 6 e às classes do EF Core. O código é gerado usando modelos T4. Portanto, ele pode ser personalizado para atender a quaisquer necessidades. Ele é compatível com herança, associações unidirecionais e bidirecionais, enumerações e com a capacidade de atribuir código de cor às suas classes e de adicionar blocos de texto para explicar partes potencialmente misteriosas do seu design.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

O CatFactory é um mecanismo de scaffolding para o .NET Core que pode automatizar a geração de classes DbContext, entidades, configurações de mapeamento e classes de repositório de um banco de dados do SQL Server.

[Repositório do GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Gerador do Entity Framework Core da LoreSoft

O Entity Framework Core Generator (efg) é uma ferramenta de CLI do .NET Core que pode gerar modelos do EF Core de um banco de dados existente, muito parecido com `dotnet ef dbcontext scaffold`, mas que também é compatível com a [regeneração](https://efg.loresoft.com/en/latest/regeneration/) do código seguro por meio da substituição de região ou da análise dos arquivos de mapeamento. Essa ferramenta é compatível com a geração de modelos de exibição, com a validação e o código do mapeador de objetos.

[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentação](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Extensões

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Uma biblioteca de plug-in que permite registrar automaticamente as alterações de dados feitas pelo EF Core em uma tabela de histórico.

[Repositório do GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Um núcleo da porta .NET Core / .NET Standard do System.Linq.Dynamic, que inclui o suporte assíncrono com o EF Core.
System.Linq.Dynamic originado como uma amostra da Microsoft, que demonstra como criar consultas LINQ dinamicamente utilizando expressões de cadeia de caracteres em vez de código.

[Repositório do GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Uma extensão que permite armazenar os resultados das consultas do EF Core em um cache de segundo nível, para que as execuções subsequentes das mesmas consultas possam evitar o acesso ao banco de dados e recuperar os dados diretamente do cache.

[Repositório do GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Essa biblioteca permite recuperar os valores da chave primária (incluindo chaves compostas) de qualquer entidade como um dicionário.

[Repositório do GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Essa biblioteca habilita o acesso fortemente tipado aos valores originais das propriedades da entidade.

[Repositório do GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

O Geco (Generator Console) é um gerador de código simples baseado em um projeto de console, que é executado no .NET Core e usa cadeias de caracteres interpoladas em C# para geração de código. O Geco inclui um gerador de modelo reverso para o EF Core, com suporte para pluralização, singularização e modelos editáveis. Ele também fornece um gerador de script de dados de semente, um executor de script e um limpador de banco de dados.

[Repositório do GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

O LinqKit.Microsoft.EntityFrameworkCore é uma versão da biblioteca LINQKit compatível com o EF Core. O LINQKit é um conjunto gratuito de extensões para LINQ to SQL e para usuários avançados do Entity Framework. Ele habilita funcionalidades avançadas, como a criação dinâmica de expressões de predicado, e o uso de variáveis de expressão em subconsultas.  

[Repositório do GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

O NeinLinq estende provedores LINQ, como o Entity Framework, para habilitar a reutilização de funções, reescrever consultas e criar consultas dinâmicas usando predicados e seletores traduzíveis.

[Repositório do GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Um plug-in para Microsoft.EntityFrameworkCore para dar suporte ao repositório, aos padrões de unidade de trabalho e a vários bancos de dados compatíveis com a transação distribuída.

[Repositório do GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Extensões do EF Core para operações em massa (Inserir, Atualizar e Excluir).

[Repositório do GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Adiciona a pluralização de tempo de design ao EF Core.

[Repositório do GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

Um método de extensão simples que obtém a instrução SQL que o EF Core geraria para uma determinada consulta LINQ em cenários simples. O método ToSql é limitado a cenários simples porque o EF Core pode gerar mais de uma instrução SQL para uma única consulta LINQ e diferentes instruções SQL, dependendo dos valores dos parâmetros.

[Repositório do GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Reativação do atributo [Index] para o EF Core (com a extensão para criação de modelo).

[Repositório do GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Fornece um wrapper em torno do provedor de banco de dados na memória EF Core. Faz com que ele funcione mais como um provedor relacional.

[Repositório do GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Uma implementação de suporte temporal para o EF Core.

[Repositório do GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Um cache de consulta de segundo nível de alto desempenho para o EF Core.

[Repositório do GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Amplia o DbContext com recursos como: Filtro de Include, Auditoria, Cache, Consultar futuro, Exclusão em lote, Atualização em lote e muito mais.

[Site](https://entityframework-plus.net/)
[Repositório do GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Extensões do Entity Framework

Amplia o DbContext com operações em massa de alto desempenho: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge e muito mais.

[Site](https://entityframework-extensions.net/)

### <a name="reconciler"></a>Reconciler

Atualize um grafo de entidade no repositório para um determinado grafo, inserindo, atualizando e removendo as respectivas entidades.

[Repositório do GitHub](https://github.com/jtheisen/reconciler)
