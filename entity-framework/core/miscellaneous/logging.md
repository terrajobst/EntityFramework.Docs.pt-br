---
title: EF Core de registro em log
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 6a8499f9f0220087e76f2e0b3a75ce551c4ddb80
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197510"
---
# <a name="logging"></a>Registrando em log

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) deste artigo no GitHub.

## <a name="aspnet-core-applications"></a>ASP.NET Core aplicativos

O EF Core integra-se automaticamente com os mecanismos de log `AddDbContext` do `AddDbContextPool` ASP.NET Core sempre que ou é usado. Portanto, ao usar ASP.NET Core, o registro em log deve ser configurado conforme descrito na [documentação do ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Outros aplicativos

O log de EF Core requer um ILoggerFactory que seja configurado com um ou mais provedores de registro em log. Provedores comuns são fornecidos nos seguintes pacotes:

* [Microsoft. Extensions. Logging. console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Um agente de log simples do console.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Dá suporte aos recursos ' logs de diagnóstico ' e ' fluxo de log ' dos serviços de Azure App.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Registra em um monitor de depurador usando System. Diagnostics. Debug. WriteLine ().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Registra no log de eventos do Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Dá suporte a EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Faz logon em um ouvinte `System.Diagnostics.TraceSource.TraceEvent()`de rastreamento usando.

Depois de instalar os pacotes apropriados, o aplicativo deve criar uma instância singleton/global de um LoggerFactory. Por exemplo, usando o agente de log do console:

# <a name="version-30tabv3"></a>[Versão 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[Versão 2. x](#tab/v2)

> [!NOTE]
> O exemplo de código a seguir `ConsoleLoggerProvider` usa um construtor que foi obsoleto na versão 2,2 e substituído em 3,0. É seguro ignorar e suprimir os avisos ao usar o 2,2.

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

# <a name="version-30tabv3"></a>[Versão 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[Versão 2. x](#tab/v2)

> [!NOTE]
> O exemplo de código a seguir `ConsoleLoggerProvider` usa um construtor que foi obsoleto na versão 2,2 e substituído em 3,0. É seguro ignorar e suprimir os avisos ao usar o 2,2.

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

Por EF Core, as categorias de agente são definidas `DbLoggerCategory` na classe para facilitar a localização de categorias, mas elas são resolvidas para cadeias de caracteres simples.

Mais detalhes sobre a infraestrutura de log subjacente podem ser encontrados na [documentação de log de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
