---
title: "Provedor de Banco de Dados SQLite – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="70380-102">Provedor de Banco de Dados EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="70380-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="70380-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com SQLite.</span><span class="sxs-lookup"><span data-stu-id="70380-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="70380-104">O provedor é mantido como parte do [projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="70380-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="70380-105">Instalar o</span><span class="sxs-lookup"><span data-stu-id="70380-105">Install</span></span>

<span data-ttu-id="70380-106">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="70380-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="70380-107">Introdução</span><span class="sxs-lookup"><span data-stu-id="70380-107">Get Started</span></span>

<span data-ttu-id="70380-108">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="70380-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="70380-109">SQLite Local em UWP</span><span class="sxs-lookup"><span data-stu-id="70380-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="70380-110">Aplicativo .NET Core para Novo Banco de Dados SQLite</span><span class="sxs-lookup"><span data-stu-id="70380-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="70380-111">Aplicativo de Exemplo Unicorn Clicker</span><span class="sxs-lookup"><span data-stu-id="70380-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="70380-112">Aplicativo de Exemplo Unicorn Packer</span><span class="sxs-lookup"><span data-stu-id="70380-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="70380-113">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="70380-113">Supported Database Engines</span></span>

* <span data-ttu-id="70380-114">SQLite (3.7 em diante)</span><span class="sxs-lookup"><span data-stu-id="70380-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="70380-115">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="70380-115">Supported Platforms</span></span>

* <span data-ttu-id="70380-116">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="70380-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="70380-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="70380-117">.NET Core</span></span>

* <span data-ttu-id="70380-118">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="70380-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="70380-119">Plataforma Universal do Windows</span><span class="sxs-lookup"><span data-stu-id="70380-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="70380-120">Limitações</span><span class="sxs-lookup"><span data-stu-id="70380-120">Limitations</span></span>

<span data-ttu-id="70380-121">Consulte [Limitações do SQLite](limitations.md) para algumas limitações importantes do provedor SQLite.</span><span class="sxs-lookup"><span data-stu-id="70380-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
