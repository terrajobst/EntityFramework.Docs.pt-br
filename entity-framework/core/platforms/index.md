---
title: "Implementações .NET compatíveis – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementações .NET compatíveis com Core EF

Queremos que o EF Core esteja disponível em qualquer lugar em que você possa escrever o código .NET e ainda estamos trabalhando para essa meta. A tabela a seguir apresenta orientação para cada implementação do .NET em que desejamos habilitar EF Core.

O EF Core 2.0 tem como destino o [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) e, portanto, requer implementações do .NET que deem suporte a ele.

| Implementação do .NET | Status | 1.x requer | 2.x requer
|-|-|-|-
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Console](../get-started/netcore/index.md) etc.) | **Totalmente compatível e recomendado:** coberto por testes automatizados e muitos aplicativos sabidamente os utilizam com sucesso. | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)
| **.NET Framework** (WinForms, WPF, ASP.NET, [Console](../get-started/full-dotnet/index.md) etc.) | **Totalmente compatível e recomendado:** coberto por testes automatizados e muitos aplicativos sabidamente os utilizam com sucesso. EF 6 também disponível nesta plataforma (consulte [Comparar EF Core e EF6](../../efcore-and-ef6/index.md) para escolher a tecnologia certa). | .NET Framework 4.5.1 | .NET Framework 4.6.1
| **Mono e Xamarin** | **Em andamento – problemas podem ser encontrados:** alguns testes foram executados pela equipe e os clientes do EF Core. Os usuários pioneiros relataram algum sucesso, mas [problemas foram encontrados](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) e outros provavelmente serão descobertos conforme os testes continuam. Em particular, há limitações no Xamarin.iOS que podem impedir que alguns aplicativos desenvolvidos usando EF Core 2.0 funcionem corretamente. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**Plataforma Universal do Windows**](../get-started/uwp/index.md) | **Em andamento – problemas podem ser encontrados:** alguns testes foram executados pela equipe e os clientes do EF Core. [Problemas](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) foram relatados quando compilado com a cadeia de ferramentas .NET Native, que normalmente é usada durante um build de versão e é um requisito para a implantação na Windows Store (se você não estiver usando o .NET Native ou quiser apenas experimentar, muitos dos problemas não o afetarão). | [Pacote mais recente do .NET UWP 5](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [Pacote mais recente do .NET UWP 6](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1) </sup> Nesta versão do .NET UWP adiciona suporte para .NET Standard 2.0 e contém .NET Native 2.0, que corrige a maioria dos problemas de compatibilidade relatados anteriormente, mas os testes revelaram [alguns problemas restantes](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) com o EF Core 2.0 que estamos planejando abordar em uma versão de patch futura.

Para qualquer combinação não funcione conforme o esperado, recomendamos criar novos problemas no [rastreador de problemas do EF ore](https://github.com/aspnet/entityframeworkcore/issues/new) e, para problemas do Xamarin relacionados, no [rastreador de problemas do Xamarin](https://bugzilla.xamarin.com/newbug).
