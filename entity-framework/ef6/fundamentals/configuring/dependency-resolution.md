---
title: Resolução de dependência - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 45681bb0cedecd502b1968b90b7f682d3257dd23
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998156"
---
# <a name="dependency-resolution"></a><span data-ttu-id="4abe4-102">Resolução de dependência</span><span class="sxs-lookup"><span data-stu-id="4abe4-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="4abe4-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="4abe4-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="4abe4-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="4abe4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="4abe4-105">Começando com o EF6, o Entity Framework contém um mecanismo de propósito geral para a obtenção de implementações dos serviços exigidos por ele.</span><span class="sxs-lookup"><span data-stu-id="4abe4-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="4abe4-106">Ou seja, quando o EF usa uma instância de algumas interfaces ou classes base ele pedirá uma implementação concreta da classe de interface ou base de dados de usar.</span><span class="sxs-lookup"><span data-stu-id="4abe4-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="4abe4-107">Isso é obtido por meio do uso da interface IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="4abe4-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="4abe4-108">O método GetService normalmente é chamado pelo EF e é tratado por uma implementação de IDbDependencyResolver fornecido pelo EF ou pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4abe4-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="4abe4-109">Quando chamado, o argumento de tipo é o tipo de classe de interface ou base do serviço que está sendo solicitado e o objeto de chave é nulo ou um objeto que fornece informações contextuais sobre o serviço solicitado.</span><span class="sxs-lookup"><span data-stu-id="4abe4-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="4abe4-110">Este artigo não contém todos os detalhes sobre como implementar IDbDependencyResolver, mas em vez disso, atua como uma referência para os tipos de serviço (ou seja, as interface e a base de dados de tipos de classe) para que o EF chamadas GetService e a semântica do objeto chave para cada um deles chamadas.</span><span class="sxs-lookup"><span data-stu-id="4abe4-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="4abe4-111">Este documento será mantido atualizado conforme serviços adicionais são adicionados.</span><span class="sxs-lookup"><span data-stu-id="4abe4-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="4abe4-112">Serviços resolvidos</span><span class="sxs-lookup"><span data-stu-id="4abe4-112">Services resolved</span></span>  

<span data-ttu-id="4abe4-113">Salvo indicação contrária qualquer objeto retornado deve ser thread-safe, pois ele pode ser usado como um singleton.</span><span class="sxs-lookup"><span data-stu-id="4abe4-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="4abe4-114">Em muitos casos, que o objeto retornado é uma fábrica de caso em que a sua própria fábrica deve ser thread-safe, mas o objeto retornado de fábrica não precisa ser thread-safe, pois uma nova instância é solicitada de fábrica para cada uso.</span><span class="sxs-lookup"><span data-stu-id="4abe4-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="4abe4-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="4abe4-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="4abe4-116">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-117">**Objeto retornado**: um inicializador de banco de dados para o tipo de contexto fornecido</span><span class="sxs-lookup"><span data-stu-id="4abe4-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="4abe4-118">**Chave**: não usado; será nulo</span><span class="sxs-lookup"><span data-stu-id="4abe4-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="4abe4-119">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="4abe4-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="4abe4-120">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="4abe4-121">**Objeto retornado**: uma fábrica para criar um gerador SQL que pode ser usado para migrações e outras ações que fazem com que um banco de dados a serem criados, como a criação de banco de dados com os inicializadores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4abe4-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="4abe4-122">**Chave**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.NET especificando o tipo de banco de dados para o qual o SQL será gerado.</span><span class="sxs-lookup"><span data-stu-id="4abe4-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="4abe4-123">Por exemplo, o gerador de SQL do SQL Server é retornado para a chave "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="4abe4-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="4abe4-124">Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="4abe4-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="4abe4-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="4abe4-126">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-127">**Objeto retornado**: provedor do EF a ser usado para um nome invariável do provedor determinado</span><span class="sxs-lookup"><span data-stu-id="4abe4-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="4abe4-128">**Chave**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.NET especificando o tipo de banco de dados para o qual um provedor é necessária.</span><span class="sxs-lookup"><span data-stu-id="4abe4-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="4abe4-129">Por exemplo, o provedor do SQL Server é retornado para a chave "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="4abe4-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="4abe4-130">Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="4abe4-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="4abe4-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="4abe4-132">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-133">**Objeto retornado**: A fábrica de conexão que será usada quando o EF cria uma conexão de banco de dados por convenção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="4abe4-134">Ou seja, quando nenhuma conexão ou a cadeia de caracteres de conexão é fornecida para o EF, e nenhuma cadeia de caracteres de conexão pode ser encontrada no App. config ou Web. config, esse serviço é usado para criar uma conexão por convenção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="4abe4-135">Alterando a fábrica de conexão pode permitir que o EF usar um tipo diferente do banco de dados (por exemplo, SQL Server Compact Edition) por padrão.</span><span class="sxs-lookup"><span data-stu-id="4abe4-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="4abe4-136">**Chave**: não usado; será nulo</span><span class="sxs-lookup"><span data-stu-id="4abe4-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="4abe4-137">Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="4abe4-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="4abe4-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="4abe4-139">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-140">**Objeto retornado**: um serviço que pode gerar um token de manifesto do provedor de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="4abe4-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="4abe4-141">Normalmente, esse serviço é usado de duas maneiras.</span><span class="sxs-lookup"><span data-stu-id="4abe4-141">This service is typically used in two ways.</span></span> <span data-ttu-id="4abe4-142">Primeiro, ele pode ser usado para evitar o Code First se conectar ao banco de dados ao criar um modelo.</span><span class="sxs-lookup"><span data-stu-id="4abe4-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="4abe4-143">Em segundo lugar, ele pode ser usado para forçar o Code First para criar um modelo para uma versão específica do banco de dados - por exemplo, para forçar um modelo para o SQL Server 2005, mesmo que, às vezes, o SQL Server 2008 é usado.</span><span class="sxs-lookup"><span data-stu-id="4abe4-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="4abe4-144">**Tempo de vida do objeto**: Singleton – o mesmo objeto pode ser usada várias vezes e, simultaneamente, por threads diferentes</span><span class="sxs-lookup"><span data-stu-id="4abe4-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="4abe4-145">**Chave**: não usado; será nulo</span><span class="sxs-lookup"><span data-stu-id="4abe4-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="4abe4-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="4abe4-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="4abe4-147">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-148">**Objeto retornado**: um serviço que pode obter uma fábrica de provedor de uma determinada conexão.</span><span class="sxs-lookup"><span data-stu-id="4abe4-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="4abe4-149">No .NET 4.5, o provedor é publicamente acessível a partir de conexão.</span><span class="sxs-lookup"><span data-stu-id="4abe4-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="4abe4-150">No .NET 4 a implementação padrão desse serviço usa alguns heurística para encontrar o provedor correspondente.</span><span class="sxs-lookup"><span data-stu-id="4abe4-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="4abe4-151">Se eles falharem, em seguida, uma nova implementação desse serviço pode ser registrado para fornecer uma resolução apropriadas.</span><span class="sxs-lookup"><span data-stu-id="4abe4-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="4abe4-152">**Chave**: não usado; será nulo</span><span class="sxs-lookup"><span data-stu-id="4abe4-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="4abe4-153">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="4abe4-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="4abe4-154">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-155">**Objeto retornado**: uma fábrica que irá gerar uma chave de cache de modelo para um determinado contexto.</span><span class="sxs-lookup"><span data-stu-id="4abe4-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="4abe4-156">Por padrão, o EF armazenará em cache um modelo para cada tipo de DbContext por provedor.</span><span class="sxs-lookup"><span data-stu-id="4abe4-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="4abe4-157">Uma implementação diferente do serviço pode ser usada para adicionar outras informações, como o nome do esquema, para a chave de cache.</span><span class="sxs-lookup"><span data-stu-id="4abe4-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="4abe4-158">**Chave**: não usado; será nulo</span><span class="sxs-lookup"><span data-stu-id="4abe4-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="4abe4-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="4abe4-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="4abe4-160">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-161">**Objeto retornado**: provedor espacial do EF uma que adiciona suporte para o provedor do EF básico para tipos espaciais, geography e geometry.</span><span class="sxs-lookup"><span data-stu-id="4abe4-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="4abe4-162">**Chave**: DbSptialServices é solicitado de duas maneiras.</span><span class="sxs-lookup"><span data-stu-id="4abe4-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="4abe4-163">Primeiro, específico do provedor de serviços espaciais são solicitados usando um objeto DbProviderInfo (que contém o invariável de token de manifesto e o nome) como a chave.</span><span class="sxs-lookup"><span data-stu-id="4abe4-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="4abe4-164">Em segundo lugar, DbSpatialServices podem ser solicitados sem chave.</span><span class="sxs-lookup"><span data-stu-id="4abe4-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="4abe4-165">Isso é usado para resolver o "global espacial provedor" que é usado durante a criação de tipos de DbGeography ou DbGeometry autônomos.</span><span class="sxs-lookup"><span data-stu-id="4abe4-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="4abe4-166">Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="4abe4-167">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="4abe4-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="4abe4-168">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-169">**Objeto retornado**: uma fábrica para criar um serviço que permite que um provedor implementar novas tentativas ou outro comportamento quando consultas e comandos são executados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4abe4-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="4abe4-170">Se nenhuma implementação for fornecida, em seguida, EF será simplesmente execute os comandos e propagar todas as exceções geradas.</span><span class="sxs-lookup"><span data-stu-id="4abe4-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="4abe4-171">Para o SQL Server, esse serviço é usado para fornecer uma política de repetição que é especialmente útil ao executar em servidores de banco de dados baseado em nuvem, como SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="4abe4-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="4abe4-172">**Chave**: ExecutionStrategyKey um objeto que contém o nome invariável do provedor e, opcionalmente, um nome de servidor para o qual a estratégia de execução será usada.</span><span class="sxs-lookup"><span data-stu-id="4abe4-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="4abe4-173">Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="4abe4-174">Func < DbConnection, cadeia de caracteres, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="4abe4-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="4abe4-175">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-176">**Objeto retornado**: uma fábrica que permite que um provedor configurar o mapeamento do HistoryContext ao `__MigrationHistory` tabela usada pelo migrações do EF.</span><span class="sxs-lookup"><span data-stu-id="4abe4-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="4abe4-177">O HistoryContext é um código primeiro DbContext e pode ser configurado usando a API fluente normal para alterar coisas como o nome da tabela e as especificações de mapeamento de coluna.</span><span class="sxs-lookup"><span data-stu-id="4abe4-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="4abe4-178">**Chave**: não usado; será nulo</span><span class="sxs-lookup"><span data-stu-id="4abe4-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="4abe4-179">Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="4abe4-180">DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="4abe4-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="4abe4-181">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-182">**Objeto retornado**: provedor de ADO.NET a ser usado para um nome invariável do provedor determinado.</span><span class="sxs-lookup"><span data-stu-id="4abe4-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="4abe4-183">**Chave**: uma cadeia de caracteres que contém o nome invariável do provedor ADO.NET</span><span class="sxs-lookup"><span data-stu-id="4abe4-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="4abe4-184">Esse serviço não é geralmente alterado diretamente, pois a implementação padrão usa o registro do provedor ADO.NET normal.</span><span class="sxs-lookup"><span data-stu-id="4abe4-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="4abe4-185">Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="4abe4-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="4abe4-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="4abe4-187">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-188">**Objeto retornado**: um serviço que é usado para determinar um nome invariável do provedor para um determinado tipo de DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="4abe4-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="4abe4-189">A implementação padrão desse serviço usa o registro do provedor ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="4abe4-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="4abe4-190">Isso significa que se o provedor ADO.NET não estiver registrado da maneira normal, porque o DbProviderFactory está sendo resolvido pelo EF, em seguida, também será necessário resolver esse serviço.</span><span class="sxs-lookup"><span data-stu-id="4abe4-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="4abe4-191">**Chave**: instância o DbProviderFactory para o qual um nome invariável é necessário.</span><span class="sxs-lookup"><span data-stu-id="4abe4-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="4abe4-192">Para obter mais detalhes sobre relacionadas ao provedor de serviços no EF6, consulte o [modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md) seção.</span><span class="sxs-lookup"><span data-stu-id="4abe4-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="4abe4-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="4abe4-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="4abe4-194">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-195">**Objeto retornado**: um cache de assemblies que contêm exibições pré-geradas.</span><span class="sxs-lookup"><span data-stu-id="4abe4-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="4abe4-196">Uma substituição geralmente é usada para permitir que o EF saber quais assemblies contêm exibições pré-geradas sem fazer qualquer descoberta.</span><span class="sxs-lookup"><span data-stu-id="4abe4-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="4abe4-197">**Chave**: não usado; será nulo</span><span class="sxs-lookup"><span data-stu-id="4abe4-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="4abe4-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="4abe4-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="4abe4-199">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-200">**Objeto retornado**: um serviço usado pelo EF Pluralizar e singularizar nomes.</span><span class="sxs-lookup"><span data-stu-id="4abe4-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="4abe4-201">Por padrão, um serviço de pluralização em inglês é usado.</span><span class="sxs-lookup"><span data-stu-id="4abe4-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="4abe4-202">**Chave**: não usado; será nulo</span><span class="sxs-lookup"><span data-stu-id="4abe4-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="4abe4-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="4abe4-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="4abe4-204">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="4abe4-205">**Objetos retornados**: qualquer interceptores que devem ser registrados quando o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="4abe4-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="4abe4-206">Observe que esses objetos são solicitados usando a chamada GetServices e todos os interceptadores retornados por qualquer resolvedor de dependência serão registrada.</span><span class="sxs-lookup"><span data-stu-id="4abe4-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="4abe4-207">**Chave**: não usado; será nulo.</span><span class="sxs-lookup"><span data-stu-id="4abe4-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="4abe4-208">Func < System.Data.Entity.DbContext, uma ação < cadeia de caracteres\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="4abe4-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="4abe4-209">**Versão introduzida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="4abe4-210">**Objeto retornado**: uma fábrica que será usada para criar o formatador de log do banco de dados que será usado quando o contexto. Propriedade Database. log é definida no contexto fornecido.</span><span class="sxs-lookup"><span data-stu-id="4abe4-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="4abe4-211">**Chave**: não usado; será nulo.</span><span class="sxs-lookup"><span data-stu-id="4abe4-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="4abe4-212">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="4abe4-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="4abe4-213">**Versão introduzida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="4abe4-214">**Objeto retornado**: uma fábrica que será usada para criar instâncias de contexto para migrações quando o contexto não tem um construtor sem parâmetros acessível.</span><span class="sxs-lookup"><span data-stu-id="4abe4-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="4abe4-215">**Chave**: objeto de tipo a para o tipo do DbContext derivada para a qual uma fábrica é necessário.</span><span class="sxs-lookup"><span data-stu-id="4abe4-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="4abe4-216">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="4abe4-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="4abe4-217">**Versão introduzida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="4abe4-218">**Objeto retornado**: uma fábrica que será usada para criar os serializadores para serialização de anotações personalizadas fortemente tipadas, de modo que eles possam ser serializados e desterilized em XML para uso em migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="4abe4-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="4abe4-219">**Chave**: O nome da anotação que está sendo serializado ou desserializado.</span><span class="sxs-lookup"><span data-stu-id="4abe4-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="4abe4-220">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="4abe4-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="4abe4-221">**Versão introduzida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="4abe4-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="4abe4-222">**Objeto retornado**: uma fábrica que será usada para criar manipuladores de transações, para que possam ser aplicada a manipulação especial para situações como a manipulação de falhas de confirmação.</span><span class="sxs-lookup"><span data-stu-id="4abe4-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="4abe4-223">**Chave**: ExecutionStrategyKey um objeto que contém o nome invariável do provedor e, opcionalmente, um nome de servidor para o qual o manipulador de transações será usado.</span><span class="sxs-lookup"><span data-stu-id="4abe4-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
