---
title: Resolução de dependência - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
caps.latest.revision: 3
ms.openlocfilehash: 88fd859b4f9a8069eeb08f32bb1d35ddcd21aec5
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120098"
---
# <a name="dependency-resolution"></a>Resolução de dependência
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Começando com o EF6, o Entity Framework contém um mecanismo de propósito geral para a obtenção de implementações dos serviços exigidos por ele. Ou seja, quando o EF usa uma instância de algumas interfaces ou classes base ele pedirá uma implementação concreta da classe de interface ou base de dados de usar. Isso é obtido por meio do uso da interface IDbDependencyResolver:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

O método GetService normalmente é chamado pelo EF e é tratado por uma implementação de IDbDependencyResolver fornecido pelo EF ou pelo aplicativo. Quando chamado, o argumento de tipo é o tipo de classe de interface ou base do serviço que está sendo solicitado e o objeto de chave é nulo ou um objeto que fornece informações contextuais sobre o serviço solicitado.  

Este artigo não contém todos os detalhes sobre como implementar IDbDependencyResolver, mas em vez disso, atua como uma referência para os tipos de serviço (ou seja, as interface e a base de dados de tipos de classe) para que o EF chamadas GetService e a semântica do objeto chave para cada um deles chamadas. Este documento será mantido atualizado conforme serviços adicionais são adicionados.  

## <a name="services-resolved"></a>Serviços resolvidos  

Salvo indicação contrária qualquer objeto retornado deve ser thread-safe, pois ele pode ser usado como um singleton. Em muitos casos, que o objeto retornado é uma fábrica de caso em que a sua própria fábrica deve ser thread-safe, mas o objeto retornado de fábrica não precisa ser thread-safe, pois uma nova instância é solicitada de fábrica para cada uso.  

### <a name="systemdataentityidatabaseinitializertcontext"></a>System.Data.Entity.IDatabaseInitializer < TContext\>  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: um inicializador de banco de dados para o tipo de contexto fornecido  

**Chave**: não usado; será nulo  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**Versão introduzida**: EF6.0.0

**Objeto retornado**: uma fábrica para criar um gerador SQL que pode ser usado para migrações e outras ações que fazem com que um banco de dados a serem criados, como a criação de banco de dados com os inicializadores de banco de dados.  

**Chave**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.NET especificando o tipo de banco de dados para o qual o SQL será gerado. Por exemplo, o gerador de SQL do SQL Server é retornado para a chave "System.Data.SqlClient".  

>[!NOTE]
> Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.  

### <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: provedor do EF a ser usado para um nome invariável do provedor determinado  

**Chave**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.NET especificando o tipo de banco de dados para o qual um provedor é necessária. Por exemplo, o provedor do SQL Server é retornado para a chave "System.Data.SqlClient".  

>[!NOTE]
> Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: A fábrica de conexão que será usada quando o EF cria uma conexão de banco de dados por convenção. Ou seja, quando nenhuma conexão ou a cadeia de caracteres de conexão é fornecida para o EF, e nenhuma cadeia de caracteres de conexão pode ser encontrada no App. config ou Web. config, esse serviço é usado para criar uma conexão por convenção. Alterando a fábrica de conexão pode permitir que o EF usar um tipo diferente do banco de dados (por exemplo, SQL Server Compact Edition) por padrão.  

**Chave**: não usado; será nulo  

>[!NOTE]
> Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: um serviço que pode gerar um token de manifesto do provedor de uma conexão. Normalmente, esse serviço é usado de duas maneiras. Primeiro, ele pode ser usado para evitar o Code First se conectar ao banco de dados ao criar um modelo. Em segundo lugar, ele pode ser usado para forçar o Code First para criar um modelo para uma versão específica do banco de dados - por exemplo, para forçar um modelo para o SQL Server 2005, mesmo que, às vezes, o SQL Server 2008 é usado.  

**Tempo de vida do objeto**: Singleton – o mesmo objeto pode ser usada várias vezes e, simultaneamente, por threads diferentes  

**Chave**: não usado; será nulo  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: um serviço que pode obter uma fábrica de provedor de uma determinada conexão. No .NET 4.5, o provedor é publicamente acessível a partir de conexão. No .NET 4 a implementação padrão desse serviço usa alguns heurística para encontrar o provedor correspondente. Se eles falharem, em seguida, uma nova implementação desse serviço pode ser registrado para fornecer uma resolução apropriadas.  

**Chave**: não usado; será nulo  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: uma fábrica que irá gerar uma chave de cache de modelo para um determinado contexto. Por padrão, o EF armazenará em cache um modelo para cada tipo de DbContext por provedor. Uma implementação diferente do serviço pode ser usada para adicionar outras informações, como o nome do esquema, para a chave de cache.  

**Chave**: não usado; será nulo  

### <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: provedor espacial do EF uma que adiciona suporte para o provedor do EF básico para tipos espaciais, geography e geometry.  

**Chave**: DbSptialServices é solicitado de duas maneiras. Primeiro, específico do provedor de serviços espaciais são solicitados usando um objeto DbProviderInfo (que contém o invariável de token de manifesto e o nome) como a chave. Em segundo lugar, DbSpatialServices podem ser solicitados sem chave. Isso é usado para resolver o "global espacial provedor" que é usado durante a criação de tipos de DbGeography ou DbGeometry autônomos.  

>[!NOTE]
> Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: uma fábrica para criar um serviço que permite que um provedor implementar novas tentativas ou outro comportamento quando consultas e comandos são executados no banco de dados. Se nenhuma implementação for fornecida, em seguida, EF será simplesmente execute os comandos e propagar todas as exceções geradas. Para o SQL Server, esse serviço é usado para fornecer uma política de repetição que é especialmente útil ao executar em servidores de banco de dados baseado em nuvem, como SQL Azure.  

**Chave**: ExecutionStrategyKey um objeto que contém o nome invariável do provedor e, opcionalmente, um nome de servidor para o qual a estratégia de execução será usada.  

>[!NOTE]
> Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, cadeia de caracteres, System.Data.Entity.Migrations.History.HistoryContext\>  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: uma fábrica que permite que um provedor configurar o mapeamento do HistoryContext ao `__MigrationHistory` tabela usada pelo migrações do EF. O HistoryContext é um código primeiro DbContext e pode ser configurado usando a API fluente normal para alterar coisas como o nome da tabela e as especificações de mapeamento de coluna.  

**Chave**: não usado; será nulo  

>[!NOTE]
> Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.  

### <a name="systemdatacommondbproviderfactory"></a>DbProviderFactory  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: provedor de ADO.NET a ser usado para um nome invariável do provedor determinado.  

**Chave**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.NET  

>[!NOTE]
> Esse serviço não é geralmente alterado diretamente, pois a implementação padrão usa o registro do provedor ADO.NET normal. Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: um serviço que é usado para determinar um nome invariável do provedor para um determinado tipo de DbProviderFactory. A implementação padrão desse serviço usa o registro do provedor ADO.NET. Isso significa que se o provedor ADO.NET não estiver registrado da maneira normal, porque o DbProviderFactory está sendo resolvido pelo EF, em seguida, também será necessário resolver esse serviço.  

**Chave**: instância o DbProviderFactory para o qual um nome invariável é necessário.  

>[!NOTE]
> Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: um cache de assemblies que contêm exibições pré-geradas. Uma substituição geralmente é usada para permitir que o EF saber quais assemblies contêm exibições pré-geradas sem fazer qualquer descoberta.  

**Chave**: não usado; será nulo  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: um serviço usado pelo EF Pluralizar e singularizar nomes. Por padrão, um serviço de pluralização em inglês é usado.  

**Chave**: não usado; será nulo  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**Versão introduzida**: EF6.0.0

**Objetos retornados**: qualquer interceptores que devem ser registrados quando o aplicativo é iniciado. Observe que esses objetos são solicitados usando a chamada GetServices e todos os interceptadores retornados por qualquer resolvedor de dependência serão registrada.

**Chave**: não usado; será nulo.  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System.Data.Entity.DbContext, uma ação < cadeia de caracteres\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**Versão introduzida**: EF6.0.0  

**Objeto retornado**: uma fábrica que será usada para criar o formatador de log do banco de dados que será usado quando o contexto. Propriedade Database. log é definida no contexto fornecido.  

**Chave**: não usado; será nulo.  

### <a name="funcsystemdataentitydbcontext"></a>Func < System.Data.Entity.DbContext\>  

**Versão introduzida**: EF6.1.0  

**Objeto retornado**: uma fábrica que será usada para criar instâncias de contexto para migrações quando o contexto não tem um construtor sem parâmetros acessível.  

**Chave**: objeto de tipo a para o tipo do DbContext derivada para a qual uma fábrica é necessário.  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**Versão introduzida**: EF6.1.0  

**Objeto retornado**: uma fábrica que será usada para criar os serializadores para serialização de anotações personalizadas fortemente tipadas, de modo que eles possam ser serializados e desterilized em XML para uso em migrações do Code First.  

**Chave**: O nome da anotação que está sendo serializado ou desserializado.  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System.Data.Entity.Infrastructure.TransactionHandler\>  

**Versão introduzida**: EF6.1.0  

**Objeto retornado**: uma fábrica que será usada para criar manipuladores de transações, para que possam ser aplicada a manipulação especial para situações como a manipulação de falhas de confirmação.  

**Chave**: ExecutionStrategyKey um objeto que contém o nome invariável do provedor e, opcionalmente, um nome de servidor para o qual o manipulador de transações será usado.  
