---
title: EF Core de registro em log
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: e8adc39ec01ff75112b03446a488df6199cc7041
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416578"
---
# <a name="logging"></a><span data-ttu-id="9f0c3-102">Registro em log</span><span class="sxs-lookup"><span data-stu-id="9f0c3-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="9f0c3-103">Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-103">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="9f0c3-104">ASP.NET Core aplicativos</span><span class="sxs-lookup"><span data-stu-id="9f0c3-104">ASP.NET Core applications</span></span>

<span data-ttu-id="9f0c3-105">O EF Core se integra automaticamente com os mecanismos de registro em log de ASP.NET Core sempre que `AddDbContext` ou `AddDbContextPool` for usado.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="9f0c3-106">Portanto, ao usar ASP.NET Core, o registro em log deve ser configurado conforme descrito na [documentação do ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="9f0c3-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="9f0c3-107">Outros aplicativos</span><span class="sxs-lookup"><span data-stu-id="9f0c3-107">Other applications</span></span>

<span data-ttu-id="9f0c3-108">O log de EF Core requer um ILoggerFactory que seja configurado com um ou mais provedores de registro em log.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="9f0c3-109">Provedores comuns são fornecidos nos seguintes pacotes:</span><span class="sxs-lookup"><span data-stu-id="9f0c3-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="9f0c3-110">[Microsoft. Extensions. Logging. console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): um agente de log simples do console.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="9f0c3-111">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): dá suporte aos recursos ' logs de diagnóstico ' e ' fluxo de log ' dos serviços de Azure app.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="9f0c3-112">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): logs em um monitor de depurador usando System. Diagnostics. Debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="9f0c3-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="9f0c3-113">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): logs no log de eventos do Windows.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="9f0c3-114">[Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): dá suporte a EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="9f0c3-115">[Microsoft. Extensions. Logging. rastreáte](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): registra em um ouvinte de rastreamento usando `System.Diagnostics.TraceSource.TraceEvent()`.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="9f0c3-116">Depois de instalar os pacotes apropriados, o aplicativo deve criar uma instância singleton/global de um LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="9f0c3-117">Por exemplo, usando o agente de log do console:</span><span class="sxs-lookup"><span data-stu-id="9f0c3-117">For example, using the console logger:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="9f0c3-118">Versão 3.x</span><span class="sxs-lookup"><span data-stu-id="9f0c3-118">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="9f0c3-119">Versão 2.x</span><span class="sxs-lookup"><span data-stu-id="9f0c3-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="9f0c3-120">O exemplo de código a seguir usa um Construtor `ConsoleLoggerProvider` que foi obsoleto na versão 2,2 e substituído em 3,0.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="9f0c3-121">É seguro ignorar e suprimir os avisos ao usar o 2,2.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="9f0c3-122">Essa instância singleton/global deve ser registrada com EF Core no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="9f0c3-123">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9f0c3-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="9f0c3-124">É muito importante que os aplicativos não criem uma nova instância ILoggerFactory para cada instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="9f0c3-125">Isso resultará em um vazamento de memória e um baixo desempenho.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="9f0c3-126">Filtrando o que é registrado</span><span class="sxs-lookup"><span data-stu-id="9f0c3-126">Filtering what is logged</span></span>

<span data-ttu-id="9f0c3-127">O aplicativo pode controlar o que é registrado Configurando um filtro no ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="9f0c3-128">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9f0c3-128">For example:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="9f0c3-129">Versão 3.x</span><span class="sxs-lookup"><span data-stu-id="9f0c3-129">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="9f0c3-130">Versão 2.x</span><span class="sxs-lookup"><span data-stu-id="9f0c3-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="9f0c3-131">O exemplo de código a seguir usa um Construtor `ConsoleLoggerProvider` que foi obsoleto na versão 2,2 e substituído em 3,0.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="9f0c3-132">É seguro ignorar e suprimir os avisos ao usar o 2,2.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

<span data-ttu-id="9f0c3-133">Neste exemplo, o log é filtrado para retornar apenas mensagens:</span><span class="sxs-lookup"><span data-stu-id="9f0c3-133">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="9f0c3-134">na categoria ' Microsoft. EntityFrameworkCore. Database. Command '</span><span class="sxs-lookup"><span data-stu-id="9f0c3-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="9f0c3-135">no nível de "informações"</span><span class="sxs-lookup"><span data-stu-id="9f0c3-135">at the 'Information' level</span></span>

<span data-ttu-id="9f0c3-136">Por EF Core, as categorias de agente são definidas na classe `DbLoggerCategory` para facilitar a localização de categorias, mas elas são resolvidas para cadeias de caracteres simples.</span><span class="sxs-lookup"><span data-stu-id="9f0c3-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="9f0c3-137">Mais detalhes sobre a infraestrutura de log subjacente podem ser encontrados na [documentação de log de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="9f0c3-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
