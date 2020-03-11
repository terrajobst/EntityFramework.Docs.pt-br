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
# <a name="dependency-resolution"></a><span data-ttu-id="763dc-102">Resolução de dependência</span><span class="sxs-lookup"><span data-stu-id="763dc-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="763dc-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="763dc-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="763dc-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="763dc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="763dc-105">A partir do EF6, o Entity Framework contém um mecanismo de finalidade geral para obter implementações de serviços que ele requer.</span><span class="sxs-lookup"><span data-stu-id="763dc-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="763dc-106">Ou seja, quando o EF usa uma instância de algumas interfaces ou classes base, ele solicitará uma implementação concreta da interface ou classe base a ser usada.</span><span class="sxs-lookup"><span data-stu-id="763dc-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="763dc-107">Isso é obtido por meio do uso da interface IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="763dc-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="763dc-108">O método GetService é normalmente chamado pelo EF e é tratado por uma implementação de IDbDependencyResolver fornecida pelo EF ou pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="763dc-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="763dc-109">Quando chamado, o argumento de tipo é a interface ou o tipo de classe base do serviço que está sendo solicitado, e o objeto de chave é nulo ou um objeto que fornece informações contextuais sobre o serviço solicitado.</span><span class="sxs-lookup"><span data-stu-id="763dc-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="763dc-110">Salvo indicação em contrário, qualquer objeto retornado deve ser thread-safe, pois ele pode ser usado como um singleton.</span><span class="sxs-lookup"><span data-stu-id="763dc-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="763dc-111">Em muitos casos, o objeto retornado é um alocador nesse caso, a própria fábrica deve ser thread-safe, mas o objeto retornado da fábrica não precisa ser thread-safe, pois uma nova instância é solicitada da fábrica para cada uso.</span><span class="sxs-lookup"><span data-stu-id="763dc-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="763dc-112">Este artigo não contém detalhes completos sobre como implementar IDbDependencyResolver, mas, em vez disso, atua como uma referência para os tipos de serviço (ou seja, a interface e os tipos de classe base) para os quais o EF chama GetService e a semântica do objeto de chave para cada um deles exige.</span><span class="sxs-lookup"><span data-stu-id="763dc-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="763dc-113">System. Data. Entity. IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="763dc-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="763dc-114">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-115">**Objeto retornado**: um inicializador de banco de dados para o tipo de contexto fornecido</span><span class="sxs-lookup"><span data-stu-id="763dc-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="763dc-116">**Chave**: não usada; será NULL</span><span class="sxs-lookup"><span data-stu-id="763dc-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="763dc-117">Func < System. Data. Entity. Migrations. Sql. MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="763dc-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="763dc-118">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="763dc-119">**Objeto retornado**: uma fábrica para criar um gerador de SQL que pode ser usado para migrações e outras ações que fazem com que um banco de dados seja criado, como a criação de banco de dados com inicializadores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="763dc-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="763dc-120">**Key**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.net especificando o tipo de banco de dados para o qual o SQL será gerado.</span><span class="sxs-lookup"><span data-stu-id="763dc-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="763dc-121">Por exemplo, o gerador de SQL SQL Server é retornado para a chave "System. Data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="763dc-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="763dc-122">Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="763dc-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="763dc-123">System. Data. entidade. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="763dc-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="763dc-124">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-125">**Objeto retornado**: o provedor do EF a ser usado para um determinado nome invariável do provedor</span><span class="sxs-lookup"><span data-stu-id="763dc-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="763dc-126">**Key**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.net especificando o tipo de banco de dados para o qual um provedor é necessário.</span><span class="sxs-lookup"><span data-stu-id="763dc-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="763dc-127">Por exemplo, o provedor de SQL Server é retornado para a chave "System. Data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="763dc-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="763dc-128">Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="763dc-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="763dc-129">System. Data. Entity. Infrastructure. IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="763dc-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="763dc-130">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-131">**Objeto retornado**: a fábrica de conexão que será usada quando o EF criar uma conexão de banco de dados por convenção.</span><span class="sxs-lookup"><span data-stu-id="763dc-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="763dc-132">Ou seja, quando nenhuma cadeia de conexão ou de conexão é fornecida ao EF, e nenhuma cadeia de conexão pode ser encontrada no app. config ou Web. config, esse serviço é usado para criar uma conexão por convenção.</span><span class="sxs-lookup"><span data-stu-id="763dc-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="763dc-133">A alteração da fábrica de conexão pode permitir que o EF use um tipo diferente de banco de dados (por exemplo, SQL Server Compact Edition) por padrão.</span><span class="sxs-lookup"><span data-stu-id="763dc-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="763dc-134">**Chave**: não usada; será NULL</span><span class="sxs-lookup"><span data-stu-id="763dc-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="763dc-135">Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="763dc-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="763dc-136">System. Data. Entity. Infrastructure. IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="763dc-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="763dc-137">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-138">**Objeto retornado**: um serviço que pode gerar um token de manifesto de provedor de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="763dc-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="763dc-139">Normalmente, esse serviço é usado de duas maneiras.</span><span class="sxs-lookup"><span data-stu-id="763dc-139">This service is typically used in two ways.</span></span> <span data-ttu-id="763dc-140">Primeiro, ele pode ser usado para evitar Code First se conectar ao banco de dados ao criar um modelo.</span><span class="sxs-lookup"><span data-stu-id="763dc-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="763dc-141">Em segundo lugar, ele pode ser usado para forçar Code First criar um modelo para uma versão de banco de dados específica, por exemplo, para forçar um modelo para SQL Server 2005, mesmo que às vezes SQL Server 2008 seja usado.</span><span class="sxs-lookup"><span data-stu-id="763dc-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="763dc-142">**Tempo de vida do objeto**: singleton – o mesmo objeto pode ser usado várias vezes e simultaneamente por threads diferentes</span><span class="sxs-lookup"><span data-stu-id="763dc-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="763dc-143">**Chave**: não usada; será NULL</span><span class="sxs-lookup"><span data-stu-id="763dc-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="763dc-144">System. Data. Entity. Infrastructure. IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="763dc-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="763dc-145">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-146">**Objeto retornado**: um serviço que pode obter um alocador de provedor de uma determinada conexão.</span><span class="sxs-lookup"><span data-stu-id="763dc-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="763dc-147">No .NET 4,5, o provedor é acessível publicamente da conexão.</span><span class="sxs-lookup"><span data-stu-id="763dc-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="763dc-148">No .NET 4, a implementação padrão desse serviço usa alguns heurísticos para localizar o provedor de correspondência.</span><span class="sxs-lookup"><span data-stu-id="763dc-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="763dc-149">Se eles falharem, uma nova implementação desse serviço poderá ser registrada para fornecer uma resolução apropriada.</span><span class="sxs-lookup"><span data-stu-id="763dc-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="763dc-150">**Chave**: não usada; será NULL</span><span class="sxs-lookup"><span data-stu-id="763dc-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="763dc-151">Func < DbContext, System. Data. Entity. Infrastructure. IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="763dc-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="763dc-152">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-153">**Objeto retornado**: uma fábrica que irá gerar uma chave de cache de modelo para um determinado contexto.</span><span class="sxs-lookup"><span data-stu-id="763dc-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="763dc-154">Por padrão, o EF armazena em cache um modelo por tipo DbContext por provedor.</span><span class="sxs-lookup"><span data-stu-id="763dc-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="763dc-155">Uma implementação diferente desse serviço pode ser usada para adicionar outras informações, como nome do esquema, à chave do cache.</span><span class="sxs-lookup"><span data-stu-id="763dc-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="763dc-156">**Chave**: não usada; será NULL</span><span class="sxs-lookup"><span data-stu-id="763dc-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="763dc-157">System. Data. Entity. Spatial. DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="763dc-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="763dc-158">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-159">**Objeto retornado**: um provedor espacial do EF que adiciona suporte ao provedor do EF básico para tipos espaciais geography e Geometry.</span><span class="sxs-lookup"><span data-stu-id="763dc-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="763dc-160">**Chave**: DbSptialServices é solicitado de duas maneiras.</span><span class="sxs-lookup"><span data-stu-id="763dc-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="763dc-161">Primeiro, os serviços espaciais específicos do provedor são solicitados usando um objeto DbProviderInfo (que contém o nome invariável e o token do manifesto) como a chave.</span><span class="sxs-lookup"><span data-stu-id="763dc-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="763dc-162">Em segundo lugar, o DbSpatialServices pode ser solicitado sem nenhuma chave.</span><span class="sxs-lookup"><span data-stu-id="763dc-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="763dc-163">Isso é usado para resolver o "provedor espacial global" que é usado durante a criação de tipos DbGeography ou DbGeometry autônomos.</span><span class="sxs-lookup"><span data-stu-id="763dc-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="763dc-164">Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="763dc-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="763dc-165">Func < System. Data. Entity. Infrastructure. IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="763dc-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="763dc-166">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-167">**Objeto retornado**: uma fábrica para criar um serviço que permite que um provedor implemente repetições ou outro comportamento quando consultas e comandos são executados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="763dc-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="763dc-168">Se nenhuma implementação for fornecida, o EF simplesmente executará os comandos e propagará todas as exceções geradas.</span><span class="sxs-lookup"><span data-stu-id="763dc-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="763dc-169">Por SQL Server esse serviço é usado para fornecer uma política de repetição que é especialmente útil ao executar em servidores de banco de dados baseados em nuvem, como SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="763dc-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="763dc-170">**Chave**: um objeto ExecutionStrategyKey que contém o nome invariável do provedor e, opcionalmente, um nome de servidor para o qual a estratégia de execução será usada.</span><span class="sxs-lookup"><span data-stu-id="763dc-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="763dc-171">Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="763dc-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="763dc-172">Func < DbConnection, String, System. Data. Entity. Migrations. History. HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="763dc-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="763dc-173">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-174">**Objeto retornado**: uma fábrica que permite que um provedor configure o mapeamento do HistoryContext para a tabela de `__MigrationHistory` usada por migrações do EF.</span><span class="sxs-lookup"><span data-stu-id="763dc-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="763dc-175">O HistoryContext é um Code First DbContext e pode ser configurado usando a API Fluent normal para alterar itens como o nome da tabela e as especificações de mapeamento de coluna.</span><span class="sxs-lookup"><span data-stu-id="763dc-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="763dc-176">**Chave**: não usada; será NULL</span><span class="sxs-lookup"><span data-stu-id="763dc-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="763dc-177">Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="763dc-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="763dc-178">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="763dc-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="763dc-179">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-180">**Objeto retornado**: o provedor ADO.net a ser usado para um determinado nome invariável do provedor.</span><span class="sxs-lookup"><span data-stu-id="763dc-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="763dc-181">**Chave**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.net</span><span class="sxs-lookup"><span data-stu-id="763dc-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="763dc-182">Normalmente, esse serviço não é alterado diretamente, pois a implementação padrão usa o registro normal do provedor de ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="763dc-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="763dc-183">Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="763dc-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="763dc-184">System. Data. Entity. Infrastructure. IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="763dc-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="763dc-185">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-186">**Objeto retornado**: um serviço que é usado para determinar um nome invariável de provedor para um determinado tipo de DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="763dc-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="763dc-187">A implementação padrão desse serviço usa o registro do provedor ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="763dc-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="763dc-188">Isso significa que, se o provedor ADO.NET não estiver registrado de forma normal porque o DbProviderFactory está sendo resolvido pelo EF, também será necessário resolver esse serviço.</span><span class="sxs-lookup"><span data-stu-id="763dc-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="763dc-189">**Chave**: a instância de DbProviderFactory para a qual um nome invariável é necessário.</span><span class="sxs-lookup"><span data-stu-id="763dc-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="763dc-190">Para obter mais detalhes sobre os serviços relacionados ao provedor no EF6, consulte a seção [modelo do provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="763dc-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="763dc-191">System. Data. Entity. Core. Mapping. ViewGeneration. IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="763dc-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="763dc-192">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-193">**Objeto retornado**: um cache dos assemblies que contêm exibições geradas previamente.</span><span class="sxs-lookup"><span data-stu-id="763dc-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="763dc-194">Uma substituição é normalmente usada para permitir que o EF saiba quais assemblies contêm exibições geradas previamente sem fazer nenhuma descoberta.</span><span class="sxs-lookup"><span data-stu-id="763dc-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="763dc-195">**Chave**: não usada; será NULL</span><span class="sxs-lookup"><span data-stu-id="763dc-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="763dc-196">System. Data. Entity. Infrastructure. pluralização. IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="763dc-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="763dc-197">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-198">**Objeto retornado**: um serviço usado pelo EF para Pluralize e singularize nomes.</span><span class="sxs-lookup"><span data-stu-id="763dc-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="763dc-199">Por padrão, um serviço de pluralização em inglês é usado.</span><span class="sxs-lookup"><span data-stu-id="763dc-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="763dc-200">**Chave**: não usada; será NULL</span><span class="sxs-lookup"><span data-stu-id="763dc-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="763dc-201">System. Data. Entity. Infrastructure. Interceptoion. IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="763dc-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="763dc-202">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="763dc-203">**Objetos retornados**: todos os interceptores que devem ser registrados quando o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="763dc-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="763dc-204">Observe que esses objetos são solicitados usando a chamada GetServices e todos os interceptores retornados por qualquer resolvedor de dependência serão registrados.</span><span class="sxs-lookup"><span data-stu-id="763dc-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="763dc-205">**Chave**: não usada; será NULL.</span><span class="sxs-lookup"><span data-stu-id="763dc-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="763dc-206">Func < System. Data. Entity. DbContext, ação < cadeia de caracteres\>, System. Data. Entity. infraestrutura. Interceptation. DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="763dc-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="763dc-207">**Versão introduzida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="763dc-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="763dc-208">**Objeto retornado**: um alocador que será usado para criar o formatador de log de banco de dados que será usado quando o contexto. A Propriedade Database. log está definida no contexto fornecido.</span><span class="sxs-lookup"><span data-stu-id="763dc-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="763dc-209">**Chave**: não usada; será NULL.</span><span class="sxs-lookup"><span data-stu-id="763dc-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="763dc-210">Func < System. Data. Entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="763dc-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="763dc-211">**Versão introduzida**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="763dc-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="763dc-212">**Objeto retornado**: um alocador que será usado para criar instâncias de contexto para migrações quando o contexto não tiver um construtor acessível sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="763dc-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="763dc-213">**Chave**: o objeto de tipo para o tipo de DbContext derivado para o qual uma fábrica é necessária.</span><span class="sxs-lookup"><span data-stu-id="763dc-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="763dc-214">Func < System. Data. Entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="763dc-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="763dc-215">**Versão introduzida**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="763dc-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="763dc-216">**Objeto retornado**: uma fábrica que será usada para criar serializadores para a serialização de anotações personalizadas fortemente tipadas, de modo que elas possam ser serializadas e desterilizeddas em XML para uso no migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="763dc-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="763dc-217">**Chave**: o nome da anotação que está sendo serializada ou desserializada.</span><span class="sxs-lookup"><span data-stu-id="763dc-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="763dc-218">Func < System. Data. Entity. Infrastructure. TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="763dc-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="763dc-219">**Versão introduzida**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="763dc-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="763dc-220">**Objeto retornado**: uma fábrica que será usada para criar manipuladores para transações, de forma que o tratamento especial possa ser aplicado a situações como tratamento de falhas de confirmação.</span><span class="sxs-lookup"><span data-stu-id="763dc-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="763dc-221">**Chave**: um objeto ExecutionStrategyKey que contém o nome invariável do provedor e, opcionalmente, um nome de servidor para o qual o manipulador de transação será usado.</span><span class="sxs-lookup"><span data-stu-id="763dc-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
