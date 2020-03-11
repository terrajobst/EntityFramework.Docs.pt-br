---
title: Resolução de dependência-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417858"
---
# <a name="dependency-resolution"></a>Resolução de dependência
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

A partir do EF6, o Entity Framework contém um mecanismo de finalidade geral para obter implementações de serviços que ele requer. Ou seja, quando o EF usa uma instância de algumas interfaces ou classes base, ele solicitará uma implementação concreta da interface ou classe base a ser usada. Isso é obtido por meio do uso da interface IDbDependencyResolver:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

O método GetService é normalmente chamado pelo EF e é tratado por uma implementação de IDbDependencyResolver fornecida pelo EF ou pelo aplicativo. Quando chamado, o argumento de tipo é a interface ou o tipo de classe base do serviço que está sendo solicitado, e o objeto de chave é nulo ou um objeto que fornece informações contextuais sobre o serviço solicitado.  

Salvo indicação em contrário, qualquer objeto retornado deve ser thread-safe, pois ele pode ser usado como um singleton. Em muitos casos, o objeto retornado é um alocador nesse caso, a própria fábrica deve ser thread-safe, mas o objeto retornado da fábrica não precisa ser thread-safe, pois uma nova instância é solicitada da fábrica para cada uso.

Este artigo não contém detalhes completos sobre como implementar IDbDependencyResolver, mas, em vez disso, atua como uma referência para os tipos de serviço (ou seja, a interface e os tipos de classe base) para os quais o EF chama GetService e a semântica do objeto de chave para cada um deles exige.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System. Data. Entity. IDatabaseInitializer < TContext\>  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: um inicializador de banco de dados para o tipo de contexto fornecido  

**Chave**: não usada; será NULL  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System. Data. Entity. Migrations. Sql. MigrationSqlGenerator\>  

**Versão introduzida**: EF 6.0.0

**Objeto retornado**: uma fábrica para criar um gerador de SQL que pode ser usado para migrações e outras ações que fazem com que um banco de dados seja criado, como a criação de banco de dados com inicializadores de banco de dados.  

**Key**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.net especificando o tipo de banco de dados para o qual o SQL será gerado. Por exemplo, o gerador de SQL SQL Server é retornado para a chave "System. Data. SqlClient".  

>[!NOTE]
> Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentitycorecommondbproviderservices"></a>System. Data. entidade. Core. Common. DbProviderServices  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: o provedor do EF a ser usado para um determinado nome invariável do provedor  

**Key**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.net especificando o tipo de banco de dados para o qual um provedor é necessário. Por exemplo, o provedor de SQL Server é retornado para a chave "System. Data. SqlClient".  

>[!NOTE]
> Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System. Data. Entity. Infrastructure. IDbConnectionFactory  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: a fábrica de conexão que será usada quando o EF criar uma conexão de banco de dados por convenção. Ou seja, quando nenhuma cadeia de conexão ou de conexão é fornecida ao EF, e nenhuma cadeia de conexão pode ser encontrada no app. config ou Web. config, esse serviço é usado para criar uma conexão por convenção. A alteração da fábrica de conexão pode permitir que o EF use um tipo diferente de banco de dados (por exemplo, SQL Server Compact Edition) por padrão.  

**Chave**: não usada; será NULL  

>[!NOTE]
> Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System. Data. Entity. Infrastructure. IManifestTokenService  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: um serviço que pode gerar um token de manifesto de provedor de uma conexão. Normalmente, esse serviço é usado de duas maneiras. Primeiro, ele pode ser usado para evitar Code First se conectar ao banco de dados ao criar um modelo. Em segundo lugar, ele pode ser usado para forçar Code First criar um modelo para uma versão de banco de dados específica, por exemplo, para forçar um modelo para SQL Server 2005, mesmo que às vezes SQL Server 2008 seja usado.  

**Tempo de vida do objeto**: singleton – o mesmo objeto pode ser usado várias vezes e simultaneamente por threads diferentes  

**Chave**: não usada; será NULL  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System. Data. Entity. Infrastructure. IDbProviderFactoryService  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: um serviço que pode obter um alocador de provedor de uma determinada conexão. No .NET 4,5, o provedor é acessível publicamente da conexão. No .NET 4, a implementação padrão desse serviço usa alguns heurísticos para localizar o provedor de correspondência. Se eles falharem, uma nova implementação desse serviço poderá ser registrada para fornecer uma resolução apropriada.  

**Chave**: não usada; será NULL  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System. Data. Entity. Infrastructure. IDbModelCacheKey\>  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: uma fábrica que irá gerar uma chave de cache de modelo para um determinado contexto. Por padrão, o EF armazena em cache um modelo por tipo DbContext por provedor. Uma implementação diferente desse serviço pode ser usada para adicionar outras informações, como nome do esquema, à chave do cache.  

**Chave**: não usada; será NULL  

## <a name="systemdataentityspatialdbspatialservices"></a>System. Data. Entity. Spatial. DbSpatialServices  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: um provedor espacial do EF que adiciona suporte ao provedor do EF básico para tipos espaciais geography e Geometry.  

**Chave**: DbSptialServices é solicitado de duas maneiras. Primeiro, os serviços espaciais específicos do provedor são solicitados usando um objeto DbProviderInfo (que contém o nome invariável e o token do manifesto) como a chave. Em segundo lugar, o DbSpatialServices pode ser solicitado sem nenhuma chave. Isso é usado para resolver o "provedor espacial global" que é usado durante a criação de tipos DbGeography ou DbGeometry autônomos.  

>[!NOTE]
> Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System. Data. Entity. Infrastructure. IDbExecutionStrategy\>  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: uma fábrica para criar um serviço que permite que um provedor implemente repetições ou outro comportamento quando consultas e comandos são executados no banco de dados. Se nenhuma implementação for fornecida, o EF simplesmente executará os comandos e propagará todas as exceções geradas. Por SQL Server esse serviço é usado para fornecer uma política de repetição que é especialmente útil ao executar em servidores de banco de dados baseados em nuvem, como SQL Azure.  

**Chave**: um objeto ExecutionStrategyKey que contém o nome invariável do provedor e, opcionalmente, um nome de servidor para o qual a estratégia de execução será usada.  

>[!NOTE]
> Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, String, System. Data. Entity. Migrations. History. HistoryContext\>  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: uma fábrica que permite que um provedor configure o mapeamento do HistoryContext para a tabela de `__MigrationHistory` usada por migrações do EF. O HistoryContext é um Code First DbContext e pode ser configurado usando a API Fluent normal para alterar itens como o nome da tabela e as especificações de mapeamento de coluna.  

**Chave**: não usada; será NULL  

>[!NOTE]
> Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdatacommondbproviderfactory"></a>System. Data. Common. DbProviderFactory  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: o provedor ADO.net a ser usado para um determinado nome invariável do provedor.  

**Chave**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.net  

>[!NOTE]
> Normalmente, esse serviço não é alterado diretamente, pois a implementação padrão usa o registro normal do provedor de ADO.NET. Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System. Data. Entity. Infrastructure. IProviderInvariantName  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: um serviço que é usado para determinar um nome invariável de provedor para um determinado tipo de DbProviderFactory. A implementação padrão desse serviço usa o registro do provedor ADO.NET. Isso significa que, se o provedor ADO.NET não estiver registrado de forma normal porque o DbProviderFactory está sendo resolvido pelo EF, também será necessário resolver esse serviço.  

**Chave**: a instância de DbProviderFactory para a qual um nome invariável é necessário.  

>[!NOTE]
> Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System. Data. Entity. Core. Mapping. ViewGeneration. IViewAssemblyCache  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: um cache dos assemblies que contêm exibições geradas previamente. Uma substituição é normalmente usada para permitir que o EF saiba quais assemblies contêm exibições geradas previamente sem fazer nenhuma descoberta.  

**Chave**: não usada; será NULL  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System. Data. Entity. Infrastructure. pluralização. IPluralizationService

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: um serviço usado pelo EF para Pluralize e singularize nomes. Por padrão, um serviço de pluralização em inglês é usado.  

**Chave**: não usada; será NULL  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System. Data. Entity. Infrastructure. Interceptoion. IDbInterceptor  

**Versão introduzida**: EF 6.0.0

**Objetos retornados**: todos os interceptores que devem ser registrados quando o aplicativo é iniciado. Observe que esses objetos são solicitados usando a chamada GetServices e todos os interceptores retornados por qualquer resolvedor de dependência serão registrados.

**Chave**: não usada; será NULL.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System. Data. Entity. DbContext, ação < cadeia de caracteres\>, System. Data. Entity. infraestrutura. Interceptation. DatabaseLogFormatter\>  

**Versão introduzida**: EF 6.0.0  

**Objeto retornado**: um alocador que será usado para criar o formatador de log de banco de dados que será usado quando o contexto. A Propriedade Database. log está definida no contexto fornecido.  

**Chave**: não usada; será NULL.  

## <a name="funcsystemdataentitydbcontext"></a>Func < System. Data. Entity. DbContext\>  

**Versão introduzida**: EF 6.1.0  

**Objeto retornado**: um alocador que será usado para criar instâncias de contexto para migrações quando o contexto não tiver um construtor acessível sem parâmetros.  

**Chave**: o objeto de tipo para o tipo de DbContext derivado para o qual uma fábrica é necessária.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System. Data. Entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\>  

**Versão introduzida**: EF 6.1.0  

**Objeto retornado**: uma fábrica que será usada para criar serializadores para a serialização de anotações personalizadas fortemente tipadas, de modo que elas possam ser serializadas e desterilizeddas em XML para uso no migrações do Code First.  

**Chave**: o nome da anotação que está sendo serializada ou desserializada.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System. Data. Entity. Infrastructure. TransactionHandler\>  

**Versão introduzida**: EF 6.1.0  

**Objeto retornado**: uma fábrica que será usada para criar manipuladores para transações, de forma que o tratamento especial possa ser aplicado a situações como tratamento de falhas de confirmação.  

**Chave**: um objeto ExecutionStrategyKey que contém o nome invariável do provedor e, opcionalmente, um nome de servidor para o qual o manipulador de transação será usado.  
