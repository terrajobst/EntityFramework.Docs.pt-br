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
# <a name="logging"></a>Registrando em log

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) no GitHub.

## <a name="aspnet-core-applications"></a>Aplicativos do ASP.NET Core

EF Core integra-se automaticamente com o mechanims de registro do ASP.NET Core sempre que `AddDbContext` ou `AddDbContextPool` é usado. Portanto, ao usar o ASP.NET Core, o log deve ser configurado conforme descrito no [documentação do ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Outros aplicativos

Núcleo EF log atualmente requer um ILoggerFactory que também é configurado com um ou mais ILoggerProvider. Provedores comuns são fornecidos nos seguintes pacotes:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): um agente de log de console simples.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): serviços de aplicativo do Azure oferece suporte a 'Logs de diagnóstico' e recursos de fluxo de Log.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs de um monitor de depuração usando System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): registros de log de eventos do Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): dá suporte a EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs para um ouvinte de rastreamento usando System.Diagnostics.TraceSource.TraceEvent().

Depois de instalar o pacote apropriado (s), o aplicativo deve criar uma instância singleton/global de um LoggerFactory. Por exemplo, usando o agente de log de console:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Esta instância singleton/global deve ser registrada com núcleo EF no `DbContextOptionsBuilder`. Por exemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> É muito importante que os aplicativos não criar uma nova instância de ILoggerFactory para cada instância do contexto. Isso resultará em um vazamento de memória e um baixo desempenho.

## <a name="filtering-what-is-logged"></a>Filtragem que é registrado em log

É a maneira mais fácil para filtrar o que é registrado para configurá-lo ao registrar o ILoggerProvider. Por exemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

Neste exemplo, o log é filtrado para retornar somente as mensagens:
 * na categoria 'Microsoft.EntityFrameworkCore.Database.Command'
 * o nível 'Informações'

Para EF Core, categorias de agente de log são definidas no `DbLoggerCategory` classe para tornar mais fácil encontrar categorias, mas eles resolver em cadeias de caracteres simples.

Mais detalhes sobre a infraestrutura subjacente do registro em log podem ser encontrados no [documentação de registro do ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).