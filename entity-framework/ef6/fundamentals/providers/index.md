---
title: Provedores do Entity Framework – EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413331"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="2bf08-102">Provedores do Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2bf08-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="2bf08-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2bf08-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="2bf08-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="2bf08-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="2bf08-105">O Entity Framework agora está sendo desenvolvido sob uma licença de software livre e o EF6 e acima não serão enviados como parte do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2bf08-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="2bf08-106">Isso tem muitas vantagens, mas também requer que os provedores do EF sejam recompilados nos assemblies do EF6.</span><span class="sxs-lookup"><span data-stu-id="2bf08-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="2bf08-107">Isso significa que os provedores do EF para EF5 e abaixo não funcionarão com o EF6 até que eles sejam recompilados.</span><span class="sxs-lookup"><span data-stu-id="2bf08-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="2bf08-108">Quais provedores estão disponíveis para o EF6?</span><span class="sxs-lookup"><span data-stu-id="2bf08-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="2bf08-109">Os provedores que sabemos que foram recompilados para o EF6 incluem:</span><span class="sxs-lookup"><span data-stu-id="2bf08-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="2bf08-110">**Provedor do Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="2bf08-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="2bf08-111">Compilado da [base de código de software livre do Entity Framework](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="2bf08-111">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="2bf08-112">Enviado como parte do [pacote NuGet EntityFramework](https://nuget.org/packages/EntityFramework)</span><span class="sxs-lookup"><span data-stu-id="2bf08-112">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="2bf08-113">**Provedor do Microsoft SQL Server Compact Edition**</span><span class="sxs-lookup"><span data-stu-id="2bf08-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="2bf08-114">Compilado da [base de código de software livre do Entity Framework](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="2bf08-114">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="2bf08-115">Enviado no [pacote NuGet EntityFramework.SqlServerCompact](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="2bf08-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="2bf08-116">**Provedores de dados do Devart dotConnect**</span><span class="sxs-lookup"><span data-stu-id="2bf08-116">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="2bf08-117">Há provedores de terceiros da [Devart](https://www.devart.com/) para uma variedade de bancos de dados, incluindo Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 e SQL Server</span><span class="sxs-lookup"><span data-stu-id="2bf08-117">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="2bf08-118">**Provedores do CDATA Software**</span><span class="sxs-lookup"><span data-stu-id="2bf08-118">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="2bf08-119">Há provedores de terceiros da [CData Software](https://www.cdata.com/ado/) para uma variedade de armazenamentos de dados, incluindo Salesforce, Armazenamento de Tabelas do Azure, MySql e muito mais</span><span class="sxs-lookup"><span data-stu-id="2bf08-119">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="2bf08-120">**Provedor do Firebird**</span><span class="sxs-lookup"><span data-stu-id="2bf08-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="2bf08-121">Disponível como um [Pacote NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)</span><span class="sxs-lookup"><span data-stu-id="2bf08-121">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="2bf08-122">**Provedor do Visual Fox Pro**</span><span class="sxs-lookup"><span data-stu-id="2bf08-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="2bf08-123">Disponível como um [Pacote NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span><span class="sxs-lookup"><span data-stu-id="2bf08-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="2bf08-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="2bf08-124">**MySQL**</span></span>
    *   [<span data-ttu-id="2bf08-125">MySQL Connector/NET para Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2bf08-125">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="2bf08-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="2bf08-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="2bf08-127">O Npgsql está disponível como um [pacote NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span><span class="sxs-lookup"><span data-stu-id="2bf08-127">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="2bf08-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="2bf08-128">**Oracle**</span></span>
    *   <span data-ttu-id="2bf08-129">O ODP.NET está disponível como um [pacote NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span><span class="sxs-lookup"><span data-stu-id="2bf08-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="2bf08-130">Observe que a inclusão nesta lista não indica o nível de funcionalidade ou suporte para um determinado provedor, apenas que um build para o EF6 foi disponibilizado.</span><span class="sxs-lookup"><span data-stu-id="2bf08-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="2bf08-131">Registrando provedores do EF</span><span class="sxs-lookup"><span data-stu-id="2bf08-131">Registering EF providers</span></span>

<span data-ttu-id="2bf08-132">Começando com o Entity Framework 6, os provedores do EF podem ser registrados usando a configuração baseada em código ou no arquivo de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2bf08-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="2bf08-133">Registro do arquivo de configuração</span><span class="sxs-lookup"><span data-stu-id="2bf08-133">Config file registration</span></span>

<span data-ttu-id="2bf08-134">O registro do provedor do EF em app.config ou web.config tem o seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="2bf08-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="2bf08-135">Observe que muitas vezes se o provedor do EF for instalado do NuGet, o pacote NuGet automaticamente adicionará esse registro ao arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="2bf08-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="2bf08-136">Se você instalar o pacote NuGet em um projeto que não é o projeto de inicialização para seu aplicativo, poderá ser necessário copiar o registro para o arquivo de configuração para seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="2bf08-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="2bf08-137">O "invariantName" nesse registro é o mesmo nome invariável usado para identificar um provedor ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="2bf08-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="2bf08-138">Isso pode ser encontrado como o atributo "invariável" em um registro de DbProviderFactories e como o atributo "providerName" em um registro de cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="2bf08-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="2bf08-139">O nome invariável a ser usado também deve ser incluído na documentação do provedor.</span><span class="sxs-lookup"><span data-stu-id="2bf08-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="2bf08-140">Alguns exemplos de nomes invariáveis são "System.Data.SqlClient" para o SQL Server e "System.Data.SqlServerCe.4.0" para o SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="2bf08-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="2bf08-141">O "tipo" desse registro é o nome qualificado pelo assembly do tipo de provedor derivado de "System.Data.Entity.Core.Common.DbProviderServices".</span><span class="sxs-lookup"><span data-stu-id="2bf08-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="2bf08-142">Por exemplo, a cadeia de caracteres a ser usada para o SQL Compact é “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span><span class="sxs-lookup"><span data-stu-id="2bf08-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="2bf08-143">O tipo a ser usado aqui deve ser incluído na documentação do provedor.</span><span class="sxs-lookup"><span data-stu-id="2bf08-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="2bf08-144">Registro baseado em código</span><span class="sxs-lookup"><span data-stu-id="2bf08-144">Code-based registration</span></span>

<span data-ttu-id="2bf08-145">Começando com o Entity Framework 6, a configuração de todo o aplicativo para o EF pode ser especificada no código.</span><span class="sxs-lookup"><span data-stu-id="2bf08-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="2bf08-146">Para obter os detalhes completos, consulte _[Configuração baseada em código do Entity Framework](https://msdn.microsoft.com/data/jj680699)_ .</span><span class="sxs-lookup"><span data-stu-id="2bf08-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="2bf08-147">A maneira normal de registrar um provedor do EF usando a configuração baseada em código é criar uma nova classe derivada de System.Data.Entity.DbConfiguration e colocá-la no mesmo assembly que a classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="2bf08-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="2bf08-148">A classe DbConfiguration deve, então, registrar o provedor em seu construtor.</span><span class="sxs-lookup"><span data-stu-id="2bf08-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="2bf08-149">Por exemplo, para registrar o provedor do SQL Compact, a classe DbConfiguration esta aparência:</span><span class="sxs-lookup"><span data-stu-id="2bf08-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

<span data-ttu-id="2bf08-150">Nesse código "SqlCeProviderServices.ProviderInvariantName" é uma conveniência para a cadeia de caracteres do nome invariável do SQL Server Compact (“System.Data.SqlServerCe.4.0”) e SqlCeProviderServices.Instance retorna a instância singleton do provedor do EF do SQL Compact.</span><span class="sxs-lookup"><span data-stu-id="2bf08-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="2bf08-151">E se o provedor necessário não estiver disponível?</span><span class="sxs-lookup"><span data-stu-id="2bf08-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="2bf08-152">Se o provedor estiver disponível para versões anteriores do EF, incentivamos você a entrar em contato com o proprietário do provedor e solicitar que ele crie uma versão do EF6.</span><span class="sxs-lookup"><span data-stu-id="2bf08-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="2bf08-153">Você deve incluir uma referência para a [documentação para o modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="2bf08-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="2bf08-154">Posso eu mesmo escrever um provedor?</span><span class="sxs-lookup"><span data-stu-id="2bf08-154">Can I write a provider myself?</span></span>

<span data-ttu-id="2bf08-155">Certamente é possível criar um provedor do EF por conta própria, embora essa não deva ser considerada uma tarefa trivial.</span><span class="sxs-lookup"><span data-stu-id="2bf08-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="2bf08-156">O link acima sobre o modelo de provedor do EF6 é um bom lugar para começar.</span><span class="sxs-lookup"><span data-stu-id="2bf08-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="2bf08-157">Você também pode considerar útil usar o código para o provedor do SQL Server e do SQL CE incluído na [base de código de software livre do EF](https://github.com/aspnet/EntityFramework6) como um ponto de partida ou para referência.</span><span class="sxs-lookup"><span data-stu-id="2bf08-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="2bf08-158">Observe que a partir do EF6 o provedor do EF está menos rigidamente acoplado ao provedor ADO.NET subjacente.</span><span class="sxs-lookup"><span data-stu-id="2bf08-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="2bf08-159">Isso torna mais fácil escrever um provedor do EF sem a necessidade de escrever ou encapsular as classes do ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="2bf08-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
