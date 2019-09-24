---
title: Implementações .NET compatíveis – EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: ac3cf3d0a84200bbf4ba7ec18b9115e06d1748f4
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149217"
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementações .NET compatíveis com Core EF

Queremos que o EF Core esteja disponível para os desenvolvedores em todas as implementações modernas do .NET e ainda estamos trabalhando para essa finalidade. Embora o suporte para EF Core no .NET Core esteja coberto pelo teste automatizado e saibamos que muitos aplicativos utilizam o EF Core com êxito, o Mono, o Xamarin e o UWP têm alguns problemas.

## <a name="overview"></a>Visão geral

A tabela a seguir fornece diretrizes para cada implementação do .NET:

| EF Core                       | 1.x    | 2.x        | 3.x             |
|:------------------------------|:-------|:-----------|:----------------|
| .NET Standard                 | 1.3    | 2.0        | 2.1             |
| .NET Core                     | 1.0    | 2.0        | 3.0             |
| .NET Framework<sup>(1)</sup>  | 4.5.1  | 4.7.2      | (sem suporte) |
| Mono                          | 4.6    | 5.4        | 6.4             |
| Xamarin.iOS<sup>(2)</sup>     | 10.0   | 10.14      | 12.16           |
| Xamarin.Android<sup>(2)</sup> | 7.0    | 8.0        | 10.0            |
| UWP<sup>(3)</sup>             | 10.0   | 10.0.16299 | TBD             |
| Unity<sup>(4)</sup>           | 2018.1 | 2018.1     | TBD             |

<sup>(1)</sup> Consulte a seção sobre o [.NET Framework](#net-framework) abaixo.

<sup>(2)</sup> Há problemas e limitações conhecidas com o Xamarin que podem impedir que alguns aplicativos desenvolvidos usando o EF Core funcionem corretamente. Verifique a lista de [problemas ativos](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) para obter soluções alternativas.

<sup>(3)</sup> O EF Core 2.0.1 e versões mais recentes são recomendadas. Instalar o [pacote do .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/). Confira a seção sobre a [Plataforma Universal do Windows](#universal-windows-platform) deste artigo.

<sup>(4)</sup> Há limitações e problemas conhecidos com o Unity. Verifique a lista de [problemas ativos](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).

## <a name="net-framework"></a>.NET Framework

Os aplicativos para .NET Framework podem precisar de alterações para funcionar com as bibliotecas do .NET Standard:

Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

Para projetos de teste, verifique também se a seguinte entrada está presente:

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

Se quiser usar uma versão mais antiga do Visual Studio, [atualize o cliente NuGet para a versão 3.6.0](https://www.nuget.org/downloads). Assim, a versão anterior funcionará com as bibliotecas do .NET Standard 2.0.

Também recomendamos migrar de packages.config do NuGet para PackageReference, se possível. Adicione a propriedade a seguir ao arquivo de projeto:

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a>Plataforma Universal do Windows

As versões anteriores do EF Core e da UWP do .NET tiveram vários problemas de compatibilidade, especialmente com aplicativos compilados com a cadeia de ferramentas do .NET Native. A nova versão da UWP do .NET adiciona suporte para o .NET Standard 2.0 e contém o .NET Native 2.0, que corrige a maioria dos problemas de compatibilidade relatados anteriormente. O EF Core 2.0.1 foi testado mais rigorosamente com a UWP, mas o teste não foi automatizado.

Ao usar o EF Core na UWP:

* Para otimizar o desempenho de consultas, evite tipos anônimos nas consultas LINQ. A implantação de um aplicativo da UWP na loja de aplicativos exige a compilação do aplicativo com o .NET Native. Consultas com tipos anônimos tem um desempenho pior no .NET Native.

* Para otimizar o desempenho de `SaveChanges()`, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) e implemente [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) e [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) nos tipos de entidade.

## <a name="report-issues"></a>Relatar problemas

Para qualquer combinação que não funcione conforme o esperado, recomendamos criar novos problemas no [rastreador de problemas do EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). Para problemas específicos de Xamarin use o rastreador de problemas para [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) ou [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
