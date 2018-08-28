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
# <a name="logging"></a>Registrando em log

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) deste artigo no GitHub.

## <a name="aspnet-core-applications"></a>Aplicativos ASP.NET Core

O EF Core integra-se automaticamente com os mecanismos de registro em log do ASP.NET Core sempre que `AddDbContext` ou `AddDbContextPool` é usado. Portanto, ao usar o ASP.NET Core, registro em log deve ser configurado conforme descrito na [documentação do ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Outros aplicativos

Registro em log no momento do EF Core requer um ILoggerFactory que também é configurado com um ou mais ILoggerProvider. Provedores comuns são fornecidos nos seguintes pacotes:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): um agente de console simples.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): recursos de fluxo de Log e de serviços de aplicativo do Azure dá suporte a 'logs de diagnóstico '.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs para um monitor de depuração usando System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs do log de eventos do Windows.
* [EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): dá suporte a EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs para um ouvinte de rastreamento usando System.Diagnostics.TraceSource.TraceEvent().

Depois de instalar o pacote apropriado (s), o aplicativo deve criar uma instância singleton/global de um LoggerFactory. Por exemplo, usando o agente de console:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Esta instância singleton/global, em seguida, deve ser registrada com o EF Core no `DbContextOptionsBuilder`. Por exemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> É muito importante que aplicativos não criam uma nova instância de ILoggerFactory para cada instância de contexto. Isso resultará em um vazamento de memória e desempenho ruim.

## <a name="filtering-what-is-logged"></a>O que é registrado de filtragem

É a maneira mais fácil para filtrar o que é registrado para configurá-lo ao registrar o ILoggerProvider. Por exemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

Neste exemplo, o log é filtrado para retornar apenas as mensagens:
 * na categoria 'Microsoft.EntityFrameworkCore.Database.Command'
 * no nível de 'Informações'

Para o EF Core, as categorias de agente são definidas no `DbLoggerCategory` classe para torná-lo mais fácil encontrar categorias, mas elas resolvem para cadeias de caracteres simples.

Obter mais detalhes sobre a infraestrutura subjacente do registro em log podem ser encontrados na [documentação de registro em log do ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
