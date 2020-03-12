---
title: Implementações .NET compatíveis – EF Core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413061"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="ca240-102">Implementações .NET compatíveis com Core EF</span><span class="sxs-lookup"><span data-stu-id="ca240-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="ca240-103">Queremos que o EF Core esteja disponível para os desenvolvedores em todas as implementações modernas do .NET e ainda estamos trabalhando para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="ca240-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="ca240-104">Embora o suporte para EF Core no .NET Core esteja coberto pelo teste automatizado e saibamos que muitos aplicativos utilizam o EF Core com êxito, o Mono, o Xamarin e o UWP têm alguns problemas.</span><span class="sxs-lookup"><span data-stu-id="ca240-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="ca240-105">Visão geral</span><span class="sxs-lookup"><span data-stu-id="ca240-105">Overview</span></span>

<span data-ttu-id="ca240-106">A tabela a seguir fornece diretrizes para cada implementação do .NET:</span><span class="sxs-lookup"><span data-stu-id="ca240-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="ca240-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="ca240-107">EF Core</span></span>                       | <span data-ttu-id="ca240-108">2.1 e 3.1</span><span class="sxs-lookup"><span data-stu-id="ca240-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="ca240-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="ca240-109">.NET Standard</span></span>                 | <span data-ttu-id="ca240-110">2.0</span><span class="sxs-lookup"><span data-stu-id="ca240-110">2.0</span></span>         |
| <span data-ttu-id="ca240-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca240-111">.NET Core</span></span>                     | <span data-ttu-id="ca240-112">2.0</span><span class="sxs-lookup"><span data-stu-id="ca240-112">2.0</span></span>         |
| <span data-ttu-id="ca240-113">.NET Framework<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="ca240-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="ca240-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="ca240-114">4.7.2</span></span>       |
| <span data-ttu-id="ca240-115">Mono</span><span class="sxs-lookup"><span data-stu-id="ca240-115">Mono</span></span>                          | <span data-ttu-id="ca240-116">5.4</span><span class="sxs-lookup"><span data-stu-id="ca240-116">5.4</span></span>         |
| <span data-ttu-id="ca240-117">Xamarin.iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="ca240-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="ca240-118">10.14</span><span class="sxs-lookup"><span data-stu-id="ca240-118">10.14</span></span>       |
| <span data-ttu-id="ca240-119">Xamarin.Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="ca240-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="ca240-120">8.0</span><span class="sxs-lookup"><span data-stu-id="ca240-120">8.0</span></span>         |
| <span data-ttu-id="ca240-121">UWP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="ca240-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="ca240-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="ca240-122">10.0.16299</span></span>  |
| <span data-ttu-id="ca240-123">Unity<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="ca240-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="ca240-124">2018.1</span><span class="sxs-lookup"><span data-stu-id="ca240-124">2018.1</span></span>      |

<span data-ttu-id="ca240-125"><sup>(1)</sup> Consulte a seção sobre o [.NET Framework](#net-framework) abaixo.</span><span class="sxs-lookup"><span data-stu-id="ca240-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="ca240-126"><sup>(2)</sup> Há problemas e limitações conhecidas com o Xamarin que podem impedir que alguns aplicativos desenvolvidos usando o EF Core funcionem corretamente.</span><span class="sxs-lookup"><span data-stu-id="ca240-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="ca240-127">Verifique a lista de [problemas ativos](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) para obter soluções alternativas.</span><span class="sxs-lookup"><span data-stu-id="ca240-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="ca240-128"><sup>(3)</sup> O EF Core 2.0.1 e versões mais recentes são recomendadas.</span><span class="sxs-lookup"><span data-stu-id="ca240-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="ca240-129">Instalar o [pacote do .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span><span class="sxs-lookup"><span data-stu-id="ca240-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="ca240-130">Confira a seção sobre a [Plataforma Universal do Windows](#universal-windows-platform) deste artigo.</span><span class="sxs-lookup"><span data-stu-id="ca240-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="ca240-131"><sup>(4)</sup> Há limitações e problemas conhecidos com o Unity.</span><span class="sxs-lookup"><span data-stu-id="ca240-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="ca240-132">Verifique a lista de [problemas ativos](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span><span class="sxs-lookup"><span data-stu-id="ca240-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="ca240-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="ca240-133">.NET Framework</span></span>

<span data-ttu-id="ca240-134">Os aplicativos para .NET Framework podem precisar de alterações para funcionar com as bibliotecas do .NET Standard:</span><span class="sxs-lookup"><span data-stu-id="ca240-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="ca240-135">Edite o arquivo de projeto e verifique se que a seguinte entrada aparece no grupo de propriedades inicial:</span><span class="sxs-lookup"><span data-stu-id="ca240-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="ca240-136">Para projetos de teste, verifique também se a seguinte entrada está presente:</span><span class="sxs-lookup"><span data-stu-id="ca240-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="ca240-137">Se quiser usar uma versão mais antiga do Visual Studio, [atualize o cliente NuGet para a versão 3.6.0](https://www.nuget.org/downloads). Assim, a versão anterior funcionará com as bibliotecas do .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="ca240-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="ca240-138">Também recomendamos migrar de packages.config do NuGet para PackageReference, se possível.</span><span class="sxs-lookup"><span data-stu-id="ca240-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="ca240-139">Adicione a propriedade a seguir ao arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="ca240-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="ca240-140">Plataforma Universal do Windows</span><span class="sxs-lookup"><span data-stu-id="ca240-140">Universal Windows Platform</span></span>

<span data-ttu-id="ca240-141">As versões anteriores do EF Core e da UWP do .NET tiveram vários problemas de compatibilidade, especialmente com aplicativos compilados com a cadeia de ferramentas do .NET Native.</span><span class="sxs-lookup"><span data-stu-id="ca240-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="ca240-142">A nova versão da UWP do .NET adiciona suporte para o .NET Standard 2.0 e contém o .NET Native 2.0, que corrige a maioria dos problemas de compatibilidade relatados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ca240-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="ca240-143">O EF Core 2.0.1 foi testado mais rigorosamente com a UWP, mas o teste não foi automatizado.</span><span class="sxs-lookup"><span data-stu-id="ca240-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="ca240-144">Ao usar o EF Core na UWP:</span><span class="sxs-lookup"><span data-stu-id="ca240-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="ca240-145">Para otimizar o desempenho de consultas, evite tipos anônimos nas consultas LINQ.</span><span class="sxs-lookup"><span data-stu-id="ca240-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="ca240-146">A implantação de um aplicativo da UWP na loja de aplicativos exige a compilação do aplicativo com o .NET Native.</span><span class="sxs-lookup"><span data-stu-id="ca240-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="ca240-147">Consultas com tipos anônimos tem um desempenho pior no .NET Native.</span><span class="sxs-lookup"><span data-stu-id="ca240-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="ca240-148">Para otimizar o desempenho de `SaveChanges()`, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) e implemente [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) e [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) nos tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="ca240-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="ca240-149">Relatar problemas</span><span class="sxs-lookup"><span data-stu-id="ca240-149">Report issues</span></span>

<span data-ttu-id="ca240-150">Para qualquer combinação que não funcione conforme o esperado, recomendamos criar novos problemas no [rastreador de problemas do EF Core](https://github.com/aspnet/entityframeworkcore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="ca240-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="ca240-151">Para problemas específicos de Xamarin use o rastreador de problemas para [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) ou [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span><span class="sxs-lookup"><span data-stu-id="ca240-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
