---
title: Configuração baseada em código – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
caps.latest.revision: 3
ms.openlocfilehash: d6a33434e582fcd7ce756b447d7f2cbab4ca43ec
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39119938"
---
# <a name="code-based-configuration"></a><span data-ttu-id="a7b34-102">Configuração baseada em código</span><span class="sxs-lookup"><span data-stu-id="a7b34-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="a7b34-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a7b34-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="a7b34-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="a7b34-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="a7b34-105">Configuração de um aplicativo do Entity Framework pode ser especificada em um arquivo de configuração (app.config/web.config) ou por meio de código.</span><span class="sxs-lookup"><span data-stu-id="a7b34-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="a7b34-106">A última opção é conhecida como configuração baseada em código.</span><span class="sxs-lookup"><span data-stu-id="a7b34-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="a7b34-107">Configuração em um arquivo de configuração é descrita em uma [artigo separado](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="a7b34-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="a7b34-108">O arquivo de configuração tem precedência sobre a configuração baseada em código.</span><span class="sxs-lookup"><span data-stu-id="a7b34-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="a7b34-109">Em outras palavras, se uma opção de configuração está definida no código e no arquivo de configuração, a configuração no arquivo de configuração é usada.</span><span class="sxs-lookup"><span data-stu-id="a7b34-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="a7b34-110">Usando DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="a7b34-110">Using DbConfiguration</span></span>  

<span data-ttu-id="a7b34-111">Configuração baseada em código no EF6 e superior é obtida com a criação de uma subclasse de System.Data.Entity.Config.DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="a7b34-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="a7b34-112">As diretrizes a seguir devem ser seguidas ao subclasses DbConfiguration:</span><span class="sxs-lookup"><span data-stu-id="a7b34-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="a7b34-113">Crie apenas uma classe DbConfiguration para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a7b34-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="a7b34-114">Essa classe especifica configurações de domínio de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a7b34-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="a7b34-115">Coloque sua classe DbConfiguration no mesmo assembly que sua classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="a7b34-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="a7b34-116">(Consulte a *DbConfiguration movendo* seção se você quiser alterar isso.)</span><span class="sxs-lookup"><span data-stu-id="a7b34-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="a7b34-117">Fornecer um construtor público sem parâmetros de sua classe DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="a7b34-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="a7b34-118">Definir opções de configuração chamando os métodos protegidos DbConfiguration, de dentro desse construtor.</span><span class="sxs-lookup"><span data-stu-id="a7b34-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="a7b34-119">As diretrizes a seguir permite que o EF descubram e usem sua configuração automaticamente por ambas as ferramentas que precisa acessar o seu modelo e quando seu aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="a7b34-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="a7b34-120">Exemplo</span><span class="sxs-lookup"><span data-stu-id="a7b34-120">Example</span></span>  

<span data-ttu-id="a7b34-121">Uma classe derivada de DbConfiguration pode ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="a7b34-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDBConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="a7b34-122">Essa classe define EF para usar a estratégia de execução do SQL Azure - para repetir automaticamente as operações de banco de dados com falha - e usar o banco de dados Local para bancos de dados que são criados por convenção do Code First.</span><span class="sxs-lookup"><span data-stu-id="a7b34-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="a7b34-123">Movendo DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="a7b34-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="a7b34-124">Há casos em que não é possível colocar sua classe DbConfiguration no mesmo assembly como sua classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="a7b34-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="a7b34-125">Por exemplo, você pode ter duas classes DbContext cada em diferentes assemblies.</span><span class="sxs-lookup"><span data-stu-id="a7b34-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="a7b34-126">Há duas opções para lidar com isso.</span><span class="sxs-lookup"><span data-stu-id="a7b34-126">There are two options for handling this.</span></span>  

<span data-ttu-id="a7b34-127">A primeira opção é usar o arquivo de configuração para especificar a instância DbConfiguration usar.</span><span class="sxs-lookup"><span data-stu-id="a7b34-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="a7b34-128">Para fazer isso, defina o atributo codeConfigurationType da seção entityFramework.</span><span class="sxs-lookup"><span data-stu-id="a7b34-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="a7b34-129">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a7b34-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="a7b34-130">O valor de codeConfigurationType deve ser o assembly e o nome qualificado do namespace da sua classe DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="a7b34-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="a7b34-131">A segunda opção é colocar DbConfigurationTypeAttribute em sua classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="a7b34-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="a7b34-132">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a7b34-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="a7b34-133">O valor passado para o atributo pode ser seu tipo DbConfiguration - conforme mostrado acima - ou o assembly e namespace qualificado a cadeia de caracteres de nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="a7b34-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="a7b34-134">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a7b34-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="a7b34-135">Configurar DbConfiguration explicitamente</span><span class="sxs-lookup"><span data-stu-id="a7b34-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="a7b34-136">Há algumas situações em que configurações podem ser necessárias antes de qualquer tipo DbContext tem sido usado.</span><span class="sxs-lookup"><span data-stu-id="a7b34-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="a7b34-137">Exemplos incluem:</span><span class="sxs-lookup"><span data-stu-id="a7b34-137">Examples of this include:</span></span>  

- <span data-ttu-id="a7b34-138">Usando DbModelBuilder para criar um modelo sem contexto</span><span class="sxs-lookup"><span data-stu-id="a7b34-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="a7b34-139">Usando algum outro código de framework/utilitário que utiliza um DbContext em que esse contexto é usado antes de seu contexto de aplicativo é usado</span><span class="sxs-lookup"><span data-stu-id="a7b34-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="a7b34-140">Em tais situações EF não conseguir descobrir a configuração automaticamente e, em vez disso, você deve fazer o seguinte:</span><span class="sxs-lookup"><span data-stu-id="a7b34-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="a7b34-141">Defina o tipo de DbConfiguration no arquivo de configuração, conforme descrito na *DbConfiguration movendo* seção acima</span><span class="sxs-lookup"><span data-stu-id="a7b34-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="a7b34-142">Chame o método DbConfiguration.SetConfiguration estático durante a inicialização do aplicativo</span><span class="sxs-lookup"><span data-stu-id="a7b34-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="a7b34-143">Substituindo DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="a7b34-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="a7b34-144">Há algumas situações em que você precisa substituir a configuração definida no DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="a7b34-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="a7b34-145">Isso não é normalmente feito por desenvolvedores de aplicativos, mas em vez disso, por provedores de terceiros e plug-ins que não é possível usar uma classe derivada de DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="a7b34-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="a7b34-146">Para isso, o EntityFramework permite que um manipulador de eventos a ser registrado que pode modificar a configuração existente antes que ele é bloqueado.</span><span class="sxs-lookup"><span data-stu-id="a7b34-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="a7b34-147">Ele também fornece um método de açúcar especificamente para a substituição de qualquer serviço retornado pelo localizador de serviço do EF.</span><span class="sxs-lookup"><span data-stu-id="a7b34-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="a7b34-148">Isso é como ele se destina a ser usado:</span><span class="sxs-lookup"><span data-stu-id="a7b34-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="a7b34-149">Na inicialização do aplicativo (antes do EF é usado) o plug-in ou o provedor deve se registrar para o método de manipulador de eventos para este evento.</span><span class="sxs-lookup"><span data-stu-id="a7b34-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="a7b34-150">(Observe que isso deve ocorrer antes que o aplicativo usa o EF).</span><span class="sxs-lookup"><span data-stu-id="a7b34-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="a7b34-151">O manipulador de eventos faz uma chamada para replaceservice tenha para cada serviço que precisa ser substituído.</span><span class="sxs-lookup"><span data-stu-id="a7b34-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="a7b34-152">Por exemplo, repalce IDbConnectionFactory e DbProviderService, você deve registrar um manipulador algo assim:</span><span class="sxs-lookup"><span data-stu-id="a7b34-152">For example, to repalce IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="a7b34-153">No código acima MyProviderServices e MyConnectionFactory representam suas implementações do serviço.</span><span class="sxs-lookup"><span data-stu-id="a7b34-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="a7b34-154">Você também pode adicionar manipuladores de dependência adicional para obter o mesmo efeito.</span><span class="sxs-lookup"><span data-stu-id="a7b34-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="a7b34-155">Observe que você pode também encapsular DbProviderFactory dessa maneira, mas fazer então afetará apenas EF e não os usos do DbProviderFactory fora do EF.</span><span class="sxs-lookup"><span data-stu-id="a7b34-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="a7b34-156">Por esse motivo provavelmente você desejará continuar a encapsular o DbProviderFactory quando você tem antes.</span><span class="sxs-lookup"><span data-stu-id="a7b34-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="a7b34-157">Você também deve ter em mente os serviços que são executados externamente para seu aplicativo - por exemplo, ao executar migrações no Console do Gerenciador de pacote.</span><span class="sxs-lookup"><span data-stu-id="a7b34-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="a7b34-158">Quando você executa migre do console, que ele tentará localizar seu DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="a7b34-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="a7b34-159">No entanto, se deseja ou não obterá o serviço encapsulado depende de onde o manipulador de eventos registrado por ele.</span><span class="sxs-lookup"><span data-stu-id="a7b34-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="a7b34-160">Se ele estiver registrado como parte da construção do seu DbConfiguration, em seguida, o código deve ser executado e o serviço deve obter encapsulado.</span><span class="sxs-lookup"><span data-stu-id="a7b34-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="a7b34-161">Normalmente, isso não será o caso, e isso significa que as ferramentas não obterá o serviço encapsulado.</span><span class="sxs-lookup"><span data-stu-id="a7b34-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
