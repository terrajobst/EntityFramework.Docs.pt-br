---
title: Implementações .NET compatíveis – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 790628c407cc4374fee4ebde8201783955afdcc3
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900324"
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementações .NET compatíveis com Core EF

Queremos que o EF Core esteja disponível em qualquer lugar em que você possa escrever o código .NET e ainda estamos trabalhando para essa meta. Embora a compatibilidade do EF Core com o .NET Core e o .NET Framework esteja coberta pelo teste automatizado e é sabido que muitos aplicativos utilizam o EF Core com êxito, o Mono, o Xamarin e o UWP têm alguns problemas.

A tabela a seguir fornece diretrizes para cada implementação do .NET:

| Implementação do .NET                                                                                                  | Status                                                             | Requisitos do EF Core 1.x                                                                                | Requisitos do EF Core 2.x <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Console](../get-started/netcore/index.md) etc.) | Totalmente compatível e recomendada                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET Framework** (WinForms, WPF, ASP.NET, [Console](../get-started/full-dotnet/index.md) etc.)                    | Totalmente compatível e recomendada. O EF6 também está disponível <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono e Xamarin**                                                                                                   | Em andamento <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Plataforma Universal do Windows**](../get-started/uwp/index.md)                                                        | Recomenda-se o EF Core 2.0.1 <sup>(4)</sup>                           | [Pacote UWP 5.x do .NET Core](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [Pacote UWP 6.x do .NET Core](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1)</sup> O EF Core 2.0 tem como destino e, portanto, requer implementações do .NET que sejam compatíveis com o [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard).

<sup>(2) </sup> Consulte [Comparar EF Core e EF6](../../efcore-and-ef6/index.md) para escolher a tecnologia certa.

<sup>(3)</sup> Há problemas e limitações conhecidas com o Xamarin que podem impedir que alguns aplicativos desenvolvidos usando o EF Core 2.0 funcionem corretamente. Verifique a lista de [problemas ativos](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) para obter soluções alternativas.

<sup>(4) </sup> Versões anteriores do EF Core e da UWP do .NET tiveram vários problemas de compatibilidade, especialmente com aplicativos compilados com a cadeia de ferramentas do .NET Native. A nova versão da UWP do .NET adiciona suporte para o .NET Standard 2.0 e contém o .NET Native 2.0, que corrige a maioria dos problemas de compatibilidade relatados anteriormente. O EF Core 2.0.1 foi testado mais rigorosamente com a UWP, mas o teste não foi automatizado.

Para qualquer combinação que não funcione conforme o esperado, recomendamos criar novos problemas no [rastreador de problemas do EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). Para problemas específicos de Xamarin use o rastreador de problemas para [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) ou [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
