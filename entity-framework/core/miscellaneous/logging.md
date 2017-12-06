---
title: Log - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 807560e563eddfb72d4286353b1403a0d2e2a441
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2017
---
# <a name="logging"></a><span data-ttu-id="459c1-102">Registrando em log</span><span class="sxs-lookup"><span data-stu-id="459c1-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="459c1-103">Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="459c1-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="459c1-104">Aplicativos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="459c1-104">ASP.NET Core applications</span></span>

<span data-ttu-id="459c1-105">EF Core integra-se automaticamente com o mechanims de registro do ASP.NET Core sempre que `AddDbContext` ou `AddDbContextPool` é usado.</span><span class="sxs-lookup"><span data-stu-id="459c1-105">EF Core integrates automatically with the logging mechanims of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="459c1-106">Portanto, ao usar o ASP.NET Core, o log deve ser configurado conforme descrito no [documentação do ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="459c1-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="459c1-107">Outros aplicativos</span><span class="sxs-lookup"><span data-stu-id="459c1-107">Other applications</span></span>

<span data-ttu-id="459c1-108">Núcleo EF log atualmente requer um ILoggerFactory que também é configurado com um ou mais ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="459c1-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="459c1-109">Provedores comuns são fornecidos nos seguintes pacotes:</span><span class="sxs-lookup"><span data-stu-id="459c1-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="459c1-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): um agente de log de console simples.</span><span class="sxs-lookup"><span data-stu-id="459c1-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="459c1-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): serviços de aplicativo do Azure oferece suporte a 'Logs de diagnóstico' e recursos de fluxo de Log.</span><span class="sxs-lookup"><span data-stu-id="459c1-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="459c1-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs de um monitor de depuração usando System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="459c1-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="459c1-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): registros de log de eventos do Windows.</span><span class="sxs-lookup"><span data-stu-id="459c1-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="459c1-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): dá suporte a EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="459c1-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="459c1-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs para um ouvinte de rastreamento usando System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="459c1-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="459c1-116">Depois de instalar o pacote apropriado (s), o aplicativo deve criar uma instância singleton/global de um LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="459c1-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="459c1-117">Por exemplo, usando o agente de log de console:</span><span class="sxs-lookup"><span data-stu-id="459c1-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="459c1-118">Esta instância singleton/global deve ser registrada com núcleo EF no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="459c1-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="459c1-119">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="459c1-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="459c1-120">É muito importante que os aplicativos não criar uma nova instância de ILoggerFactory para cada instância do contexto.</span><span class="sxs-lookup"><span data-stu-id="459c1-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="459c1-121">Isso resultará em um vazamento de memória e um baixo desempenho.</span><span class="sxs-lookup"><span data-stu-id="459c1-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="459c1-122">Filtragem que é registrado em log</span><span class="sxs-lookup"><span data-stu-id="459c1-122">Filtering what is logged</span></span>

<span data-ttu-id="459c1-123">É a maneira mais fácil para filtrar o que é registrado para configurá-lo ao registrar o ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="459c1-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="459c1-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="459c1-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="459c1-125">Neste exemplo, o log é filtrado para retornar somente as mensagens:</span><span class="sxs-lookup"><span data-stu-id="459c1-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="459c1-126">na categoria 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="459c1-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="459c1-127">o nível 'Informações'</span><span class="sxs-lookup"><span data-stu-id="459c1-127">at the 'Information' level</span></span>

<span data-ttu-id="459c1-128">Para EF Core, categorias de agente de log são definidas no `DbLoggerCategory` classe para tornar mais fácil encontrar categorias, mas eles resolver em cadeias de caracteres simples.</span><span class="sxs-lookup"><span data-stu-id="459c1-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="459c1-129">Mais detalhes sobre a infraestrutura subjacente do registro em log podem ser encontrados no [documentação de registro do ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="459c1-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>