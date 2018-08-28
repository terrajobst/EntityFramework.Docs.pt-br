---
title: Log – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: efc78fbada3c59bf9cf2c4cb694835bb5ad60e76
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996999"
---
# <a name="logging"></a><span data-ttu-id="525a8-102">Registrando em log</span><span class="sxs-lookup"><span data-stu-id="525a8-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="525a8-103">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="525a8-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="525a8-104">Aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="525a8-104">ASP.NET Core applications</span></span>

<span data-ttu-id="525a8-105">O EF Core integra-se automaticamente com os mecanismos de registro em log do ASP.NET Core sempre que `AddDbContext` ou `AddDbContextPool` é usado.</span><span class="sxs-lookup"><span data-stu-id="525a8-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="525a8-106">Portanto, ao usar o ASP.NET Core, registro em log deve ser configurado conforme descrito na [documentação do ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="525a8-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="525a8-107">Outros aplicativos</span><span class="sxs-lookup"><span data-stu-id="525a8-107">Other applications</span></span>

<span data-ttu-id="525a8-108">Registro em log no momento do EF Core requer um ILoggerFactory que também é configurado com um ou mais ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="525a8-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="525a8-109">Provedores comuns são fornecidos nos seguintes pacotes:</span><span class="sxs-lookup"><span data-stu-id="525a8-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="525a8-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): um agente de console simples.</span><span class="sxs-lookup"><span data-stu-id="525a8-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="525a8-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): recursos de fluxo de Log e de serviços de aplicativo do Azure dá suporte a 'logs de diagnóstico '.</span><span class="sxs-lookup"><span data-stu-id="525a8-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="525a8-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs para um monitor de depuração usando System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="525a8-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="525a8-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs do log de eventos do Windows.</span><span class="sxs-lookup"><span data-stu-id="525a8-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="525a8-114">[EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): dá suporte a EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="525a8-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="525a8-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs para um ouvinte de rastreamento usando System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="525a8-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="525a8-116">Depois de instalar o pacote apropriado (s), o aplicativo deve criar uma instância singleton/global de um LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="525a8-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="525a8-117">Por exemplo, usando o agente de console:</span><span class="sxs-lookup"><span data-stu-id="525a8-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="525a8-118">Esta instância singleton/global, em seguida, deve ser registrada com o EF Core no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="525a8-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="525a8-119">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="525a8-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="525a8-120">É muito importante que aplicativos não criam uma nova instância de ILoggerFactory para cada instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="525a8-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="525a8-121">Isso resultará em um vazamento de memória e desempenho ruim.</span><span class="sxs-lookup"><span data-stu-id="525a8-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="525a8-122">O que é registrado de filtragem</span><span class="sxs-lookup"><span data-stu-id="525a8-122">Filtering what is logged</span></span>

<span data-ttu-id="525a8-123">É a maneira mais fácil para filtrar o que é registrado para configurá-lo ao registrar o ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="525a8-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="525a8-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="525a8-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="525a8-125">Neste exemplo, o log é filtrado para retornar apenas as mensagens:</span><span class="sxs-lookup"><span data-stu-id="525a8-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="525a8-126">na categoria 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="525a8-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="525a8-127">no nível de 'Informações'</span><span class="sxs-lookup"><span data-stu-id="525a8-127">at the 'Information' level</span></span>

<span data-ttu-id="525a8-128">Para o EF Core, as categorias de agente são definidas no `DbLoggerCategory` classe para torná-lo mais fácil encontrar categorias, mas elas resolvem para cadeias de caracteres simples.</span><span class="sxs-lookup"><span data-stu-id="525a8-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="525a8-129">Obter mais detalhes sobre a infraestrutura subjacente do registro em log podem ser encontrados na [documentação de registro em log do ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="525a8-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
