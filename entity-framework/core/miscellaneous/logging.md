---
title: EF Core de registro em log
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 1a3863ee5f508c1fd393d4ec2c25c46ab8634f00
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502091"
---
# <a name="logging"></a>Registrando em log

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) deste artigo no GitHub.

## <a name="aspnet-core-applications"></a>ASP.NET Core aplicativos

O EF Core se integra automaticamente com os mecanismos de registro em log de ASP.NET Core sempre que `AddDbContext` ou `AddDbContextPool` for usado. Portanto, ao usar ASP.NET Core, o registro em log deve ser configurado conforme descrito na [documentação do ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Outros aplicativos

O log de EF Core requer um ILoggerFactory que seja configurado com um ou mais provedores de registro em log. Provedores comuns são fornecidos nos seguintes pacotes:

* [Microsoft. Extensions. Logging. console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): um agente de log simples do console.
* [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): dá suporte aos recursos ' logs de diagnóstico ' e ' fluxo de log ' dos serviços de Azure app.
* [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): logs em um monitor de depurador usando System. Diagnostics. Debug. WriteLine ().
* [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): logs no log de eventos do Windows.
* [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): dá suporte a EventSource/EventListener.
* [Microsoft. Extensions. Logging. rastreáte](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): registra em um ouvinte de rastreamento usando `System.Diagnostics.TraceSource.TraceEvent()`.

Depois de instalar os pacotes apropriados, o aplicativo deve criar uma instância singleton/global de um LoggerFactory. Por exemplo, usando o agente de log do console:

### <a name="version-30tabv3"></a>[Versão 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2xtabv2"></a>[Versão 2.x](#tab/v2)

> [!NOTE]
> O exemplo de código a seguir usa um Construtor `ConsoleLoggerProvider` que foi obsoleto na versão 2,2 e substituído em 3,0. É seguro ignorar e suprimir os avisos ao usar o 2,2.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

Essa instância singleton/global deve ser registrada com EF Core no `DbContextOptionsBuilder`. Por exemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> É muito importante que os aplicativos não criem uma nova instância ILoggerFactory para cada instância de contexto. Isso resultará em um vazamento de memória e um baixo desempenho.

## <a name="filtering-what-is-logged"></a>Filtrando o que é registrado

O aplicativo pode controlar o que é registrado Configurando um filtro no ILoggerProvider. Por exemplo:

### <a name="version-30tabv3"></a>[Versão 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2xtabv2"></a>[Versão 2.x](#tab/v2)

> [!NOTE]
> O exemplo de código a seguir usa um Construtor `ConsoleLoggerProvider` que foi obsoleto na versão 2,2 e substituído em 3,0. É seguro ignorar e suprimir os avisos ao usar o 2,2.

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

Neste exemplo, o log é filtrado para retornar apenas mensagens:

* na categoria ' Microsoft. EntityFrameworkCore. Database. Command '
* no nível de "informações"

Por EF Core, as categorias de agente são definidas na classe `DbLoggerCategory` para facilitar a localização de categorias, mas elas são resolvidas para cadeias de caracteres simples.

Mais detalhes sobre a infraestrutura de log subjacente podem ser encontrados na [documentação de log de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
