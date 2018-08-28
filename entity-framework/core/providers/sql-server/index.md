---
title: Provedor de Banco de Dados do Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995663"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="83d0c-102">Provedor de Banco de Dados EF Core do Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="83d0c-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="83d0c-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Microsoft SQL Server (incluindo o SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="83d0c-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="83d0c-104">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="83d0c-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="83d0c-105">Instalar o</span><span class="sxs-lookup"><span data-stu-id="83d0c-105">Install</span></span>

<span data-ttu-id="83d0c-106">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="83d0c-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="83d0c-107">Introdução</span><span class="sxs-lookup"><span data-stu-id="83d0c-107">Get Started</span></span>

<span data-ttu-id="83d0c-108">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="83d0c-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="83d0c-109">Introdução ao .NET Framework (Console, WinForms, WPF etc.)</span><span class="sxs-lookup"><span data-stu-id="83d0c-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="83d0c-110">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83d0c-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="83d0c-111">Aplicativo de exemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="83d0c-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="83d0c-112">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="83d0c-112">Supported Database Engines</span></span>

* <span data-ttu-id="83d0c-113">Microsoft SQL Server (2008 em diante)</span><span class="sxs-lookup"><span data-stu-id="83d0c-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="83d0c-114">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="83d0c-114">Supported Platforms</span></span>

* <span data-ttu-id="83d0c-115">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="83d0c-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="83d0c-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="83d0c-116">.NET Core</span></span>

* <span data-ttu-id="83d0c-117">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="83d0c-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
