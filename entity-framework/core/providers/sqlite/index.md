---
title: "Provedor de Banco de Dados SQLite – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="363e2-102">Provedor de Banco de Dados EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="363e2-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="363e2-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com SQLite.</span><span class="sxs-lookup"><span data-stu-id="363e2-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="363e2-104">O provedor é mantido como parte do [projeto EntityFramework GitHub](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="363e2-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="363e2-105">Instalar o</span><span class="sxs-lookup"><span data-stu-id="363e2-105">Install</span></span>

<span data-ttu-id="363e2-106">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="363e2-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="363e2-107">Introdução</span><span class="sxs-lookup"><span data-stu-id="363e2-107">Get Started</span></span>

<span data-ttu-id="363e2-108">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="363e2-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="363e2-109">SQLite Local em UWP</span><span class="sxs-lookup"><span data-stu-id="363e2-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="363e2-110">Aplicativo .NET Core para Novo Banco de Dados SQLite</span><span class="sxs-lookup"><span data-stu-id="363e2-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="363e2-111">Aplicativo de Exemplo Unicorn Clicker</span><span class="sxs-lookup"><span data-stu-id="363e2-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="363e2-112">Aplicativo de Exemplo Unicorn Packer</span><span class="sxs-lookup"><span data-stu-id="363e2-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="363e2-113">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="363e2-113">Supported Database Engines</span></span>

* <span data-ttu-id="363e2-114">SQLite (3.7 em diante)</span><span class="sxs-lookup"><span data-stu-id="363e2-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="363e2-115">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="363e2-115">Supported Platforms</span></span>

* <span data-ttu-id="363e2-116">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="363e2-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="363e2-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="363e2-117">.NET Core</span></span>

* <span data-ttu-id="363e2-118">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="363e2-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="363e2-119">Plataforma Universal do Windows</span><span class="sxs-lookup"><span data-stu-id="363e2-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="363e2-120">Limitações</span><span class="sxs-lookup"><span data-stu-id="363e2-120">Limitations</span></span>

<span data-ttu-id="363e2-121">Consulte [Limitações do SQLite](limitations.md) para algumas limitações importantes do provedor SQLite.</span><span class="sxs-lookup"><span data-stu-id="363e2-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
