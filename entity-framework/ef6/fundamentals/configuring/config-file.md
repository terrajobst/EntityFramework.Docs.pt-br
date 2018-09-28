---
title: Arquivo de configuração - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: faba4e406b9f26f5bed6149f75c59da362d84692
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415777"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="9a935-102">Arquivo de configuração</span><span class="sxs-lookup"><span data-stu-id="9a935-102">Configuration File Settings</span></span>
<span data-ttu-id="9a935-103">Entity Framework permite uma série de configurações seja especificada do arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="9a935-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="9a935-104">Em geral o EF segue um princípio de 'convenção em detrimento da configuração': todas as configurações discutidas neste post têm um comportamento padrão, você só precisa se preocupar sobre como alterar a configuração, quando o padrão não atendem às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="9a935-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="9a935-105">Uma alternativa baseada em código</span><span class="sxs-lookup"><span data-stu-id="9a935-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="9a935-106">Todas essas configurações também podem ser aplicadas usando código.</span><span class="sxs-lookup"><span data-stu-id="9a935-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="9a935-107">A partir do EF6, apresentamos [configuração baseada em código](code-based.md), que fornece uma maneira centralizada da aplicação de configuração do código.</span><span class="sxs-lookup"><span data-stu-id="9a935-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="9a935-108">Antes do EF6, ainda pode ser aplicada a configuração do código, mas você precisa usar várias APIs para configurar diferentes áreas.</span><span class="sxs-lookup"><span data-stu-id="9a935-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="9a935-109">A opção de arquivo de configuração permite que essas configurações sejam alteradas com facilidade durante a implantação sem atualizar seu código.</span><span class="sxs-lookup"><span data-stu-id="9a935-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="9a935-110">A seção de configuração do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="9a935-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="9a935-111">Começando com o ef4.1 em diante, você pode definir o inicializador de banco de dados para um contexto usando o **appSettings** seção do arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="9a935-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="9a935-112">No EF 4.3, apresentamos personalizado **entityFramework** seção para lidar com as novas configurações.</span><span class="sxs-lookup"><span data-stu-id="9a935-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="9a935-113">Entity Framework ainda reconhecerá os inicializadores de banco de dados definidos usando o formato antigo, mas recomendamos a migração para o novo formato sempre que possível.</span><span class="sxs-lookup"><span data-stu-id="9a935-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="9a935-114">O **entityFramework** seção foi adicionada automaticamente ao arquivo de configuração do seu projeto quando você instalou o pacote EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="9a935-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a><span data-ttu-id="9a935-115">Cadeias de caracteres de conexão</span><span class="sxs-lookup"><span data-stu-id="9a935-115">Connection Strings</span></span>  

<span data-ttu-id="9a935-116">[Esta página](~/ef6/fundamentals/configuring/connection-strings.md) fornece mais detalhes sobre como o Entity Framework determina o banco de dados a ser usado, incluindo cadeias de caracteres de conexão no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="9a935-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="9a935-117">Vá de cadeias de caracteres de Conexão no padrão **connectionStrings** elemento e não exigem o **entityFramework** seção.</span><span class="sxs-lookup"><span data-stu-id="9a935-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="9a935-118">Modelos de código pela primeira vez com base em usam cadeias de caracteres de conexão ADO.NET normais.</span><span class="sxs-lookup"><span data-stu-id="9a935-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="9a935-119">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9a935-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="9a935-120">Designer EF com base em modelos use EF conexão cadeias de caracteres especiais.</span><span class="sxs-lookup"><span data-stu-id="9a935-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="9a935-121">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9a935-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="9a935-122">Tipo de configuração baseada em código (EF6 em diante)</span><span class="sxs-lookup"><span data-stu-id="9a935-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="9a935-123">Começando com o EF6, você pode especificar o DbConfiguration para ser usado para o EF [configuração baseada em código](code-based.md) em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9a935-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="9a935-124">Na maioria dos casos, você não precisa especificar essa configuração como EF descobrirá automaticamente seu DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="9a935-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="9a935-125">Para obter detalhes de quando talvez você precise especificar DbConfiguration no arquivo de configuração, consulte o **movendo DbConfiguration** seção [configuração baseada em código](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="9a935-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="9a935-126">Para definir um tipo de DbConfiguration, que você especificar o nome de tipo qualificado do assembly para o **codeConfigurationType** elemento.</span><span class="sxs-lookup"><span data-stu-id="9a935-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="9a935-127">Um nome qualificado do assembly é o nome qualificado de namespace, seguido por uma vírgula, em seguida, o assembly que reside o tipo.</span><span class="sxs-lookup"><span data-stu-id="9a935-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="9a935-128">Você pode, opcionalmente, especifique também a versão do assembly, cultura e token de chave pública.</span><span class="sxs-lookup"><span data-stu-id="9a935-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="9a935-129">Provedores de banco de dados do EF (EF6 em diante)</span><span class="sxs-lookup"><span data-stu-id="9a935-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="9a935-130">Antes do EF6, partes de específicos do Entity Framework de um provedor de banco de dados tinham que ser incluído como parte do provedor ADO.NET core.</span><span class="sxs-lookup"><span data-stu-id="9a935-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="9a935-131">Começando com o EF6, as partes específicas do EF são agora gerenciadas e registradas separadamente.</span><span class="sxs-lookup"><span data-stu-id="9a935-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="9a935-132">Normalmente, não será necessário registrar provedores por conta própria.</span><span class="sxs-lookup"><span data-stu-id="9a935-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="9a935-133">Isso será feito normalmente pelo provedor quando você o instala.</span><span class="sxs-lookup"><span data-stu-id="9a935-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="9a935-134">Provedores são registrados, incluindo uma **provedor** sob o elemento a **provedores** seção filho a **entityFramework** seção.</span><span class="sxs-lookup"><span data-stu-id="9a935-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="9a935-135">Há dois atributos obrigatórios para uma entrada do provedor:</span><span class="sxs-lookup"><span data-stu-id="9a935-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="9a935-136">**invariantName** identifica o provedor ADO.NET de núcleo que este destinos de provedor do EF</span><span class="sxs-lookup"><span data-stu-id="9a935-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="9a935-137">**tipo** é o nome de tipo qualificado do assembly da implementação do provedor do EF</span><span class="sxs-lookup"><span data-stu-id="9a935-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="9a935-138">Um nome qualificado do assembly é o nome qualificado de namespace, seguido por uma vírgula, em seguida, o assembly que reside o tipo.</span><span class="sxs-lookup"><span data-stu-id="9a935-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="9a935-139">Você pode, opcionalmente, especifique também a versão do assembly, cultura e token de chave pública.</span><span class="sxs-lookup"><span data-stu-id="9a935-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="9a935-140">Por exemplo, aqui está a entrada criada para registrar o provedor do SQL Server padrão quando você instala o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9a935-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="9a935-141">Interceptores (EF6.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="9a935-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="9a935-142">Começando com o EF6.1, você pode registrar interceptores no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="9a935-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="9a935-143">Os interceptores permitem que você execute lógica adicional quando o EF executa determinadas operações, como a execução de consultas de banco de dados, abrir conexões, etc.</span><span class="sxs-lookup"><span data-stu-id="9a935-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="9a935-144">Os interceptores são registrados, incluindo uma **interceptador** sob o elemento a **interceptores** seção filho do **entityFramework** seção.</span><span class="sxs-lookup"><span data-stu-id="9a935-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="9a935-145">Por exemplo, a configuração a seguir registra o interno **DatabaseLogger** interceptador que registrará em log todas as operações de banco de dados para o Console.</span><span class="sxs-lookup"><span data-stu-id="9a935-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="9a935-146">Operações de banco de dados de registro em log para um arquivo (EF6.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="9a935-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="9a935-147">Registrar interceptores por meio do arquivo de configuração é especialmente útil quando você deseja adicionar o registro em log a um aplicativo existente para ajudar a depurar um problema.</span><span class="sxs-lookup"><span data-stu-id="9a935-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="9a935-148">**DatabaseLogger** dá suporte ao registro em log para um arquivo, fornecendo o nome do arquivo como um parâmetro de construtor.</span><span class="sxs-lookup"><span data-stu-id="9a935-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="9a935-149">Por padrão, isso fará com que o arquivo de log seja substituído por um novo arquivo cada vez que o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="9a935-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="9a935-150">Para acrescentar em vez disso, o log de arquivo se ele já existir usar algo como:</span><span class="sxs-lookup"><span data-stu-id="9a935-150">To instead append to the log file if it already exists use something like:</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="9a935-151">Para saber mais sobre **DatabaseLogger** e registrando interceptores, consulte o postagem de blog [EF 6.1: ativar o registro em log sem recompilar](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="9a935-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="9a935-152">Fábrica de Conexão padrão primeiro código</span><span class="sxs-lookup"><span data-stu-id="9a935-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="9a935-153">A seção de configuração permite que você especifique uma fábrica de conexão padrão que o Code First deve usar para localizar um banco de dados a ser usado para um contexto.</span><span class="sxs-lookup"><span data-stu-id="9a935-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="9a935-154">O alocador de conexão padrão é usado apenas quando nenhuma cadeia de conexão tiver sido adicionada ao arquivo de configuração para um contexto.</span><span class="sxs-lookup"><span data-stu-id="9a935-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="9a935-155">Quando você instalou o pacote NuGet do EF uma fábrica de conexão padrão foi registrada que aponta para o SQL Express ou LocalDB, dependendo de qual delas você instalou.</span><span class="sxs-lookup"><span data-stu-id="9a935-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="9a935-156">Para definir uma fábrica de conexão, você especifica o nome de tipo qualificado do assembly para o **defaultConnectionFactory** elemento.</span><span class="sxs-lookup"><span data-stu-id="9a935-156">To set a connection factory, you specify the assembly qualified type name in the **defaultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="9a935-157">Um nome qualificado do assembly é o nome qualificado de namespace, seguido por uma vírgula, em seguida, o assembly que reside o tipo.</span><span class="sxs-lookup"><span data-stu-id="9a935-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="9a935-158">Você pode, opcionalmente, especifique também a versão do assembly, cultura e token de chave pública.</span><span class="sxs-lookup"><span data-stu-id="9a935-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="9a935-159">Aqui está um exemplo de como definir sua própria fábrica de conexão padrão:</span><span class="sxs-lookup"><span data-stu-id="9a935-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="9a935-160">O exemplo acima exige a fábrica personalizada para ter um construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="9a935-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="9a935-161">Se necessário, você pode especificar parâmetros do construtor usando o **parâmetros** elemento.</span><span class="sxs-lookup"><span data-stu-id="9a935-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="9a935-162">Por exemplo, SqlCeConnectionFactory, que está incluído no Entity Framework, requer que você forneça um nome invariável do provedor para o construtor.</span><span class="sxs-lookup"><span data-stu-id="9a935-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="9a935-163">O nome invariável do provedor identifica a versão do SQL Compact você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="9a935-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="9a935-164">A configuração a seguir fará com que os contextos usar a versão do SQL Compact 4.0 por padrão.</span><span class="sxs-lookup"><span data-stu-id="9a935-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="9a935-165">Se você não definir uma fábrica de conexão padrão, o Code First usa SqlConnectionFactory, apontando para `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="9a935-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="9a935-166">SqlConnectionFactory também tem um construtor que permite substituir partes da cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="9a935-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="9a935-167">Se você quiser usar uma instância do SQL Server diferente de `.\SQLEXPRESS` você pode usar esse construtor para definir o servidor.</span><span class="sxs-lookup"><span data-stu-id="9a935-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="9a935-168">A configuração a seguir fará com que Code First usar **MyDatabaseServer** para contextos que não têm uma cadeia de caracteres de conexão explícita definida.</span><span class="sxs-lookup"><span data-stu-id="9a935-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="9a935-169">Por padrão, supõe-se que os argumentos de construtor são do tipo cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="9a935-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="9a935-170">Você pode usar o atributo de tipo para alterar isso.</span><span class="sxs-lookup"><span data-stu-id="9a935-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="9a935-171">Inicializadores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="9a935-171">Database Initializers</span></span>  

<span data-ttu-id="9a935-172">Inicializadores de banco de dados são configurados em uma base por contexto.</span><span class="sxs-lookup"><span data-stu-id="9a935-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="9a935-173">Elas podem ser definidas no arquivo de configuração usando o **contexto** elemento.</span><span class="sxs-lookup"><span data-stu-id="9a935-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="9a935-174">Esse elemento usa o nome qualificado do assembly para identificar o contexto que está sendo configurado.</span><span class="sxs-lookup"><span data-stu-id="9a935-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="9a935-175">Por padrão, o Code First contextos são configurados para usar o inicializador de CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="9a935-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="9a935-176">Há um **disableDatabaseInitialization** atributo as **contexto** elemento que pode ser usado para desabilitar a inicialização do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9a935-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="9a935-177">Por exemplo, a configuração a seguir desabilita a inicialização do banco de dados para o contexto de Blogging.BlogContext definido em myAssembly. dll.</span><span class="sxs-lookup"><span data-stu-id="9a935-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="9a935-178">Você pode usar o **databaseInitializer** elemento para definir um inicializador personalizado.</span><span class="sxs-lookup"><span data-stu-id="9a935-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="9a935-179">Parâmetros do construtor usam a mesma sintaxe como fábricas de conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="9a935-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

<span data-ttu-id="9a935-180">Você pode configurar um dos inicializadores de banco de dados genérico que estão incluídos no Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9a935-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="9a935-181">O **tipo** atributo usa o formato do .NET Framework para tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="9a935-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="9a935-182">Por exemplo, se você estiver usando as migrações Code First, você pode configurar o banco de dados a serem migrados automaticamente usando o `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicializador.</span><span class="sxs-lookup"><span data-stu-id="9a935-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
