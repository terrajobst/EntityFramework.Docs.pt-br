---
title: "Provedor de Banco de Dados do IBM Data Server (DB2) – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="a12c8-102">Provedores de Banco de Dados do EF Core do IBM Data Server (DB2)</span><span class="sxs-lookup"><span data-stu-id="a12c8-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="a12c8-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com IBM Data Server.</span><span class="sxs-lookup"><span data-stu-id="a12c8-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="a12c8-104">O provedor é mantido pela IBM.</span><span class="sxs-lookup"><span data-stu-id="a12c8-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="a12c8-105">Este provedor não é mantido como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a12c8-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="a12c8-106">Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="a12c8-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="a12c8-107">Instalar o</span><span class="sxs-lookup"><span data-stu-id="a12c8-107">Install</span></span>

<span data-ttu-id="a12c8-108">Para trabalhar com o IBM Data Server no Windows, instale o [pacote NuGet IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="a12c8-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="a12c8-109">Para trabalhar com o IBM Data Server no Linux, instale o [pacote NuGet IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span><span class="sxs-lookup"><span data-stu-id="a12c8-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="a12c8-110">Introdução</span><span class="sxs-lookup"><span data-stu-id="a12c8-110">Get Started</span></span>

[<span data-ttu-id="a12c8-111">Introdução ao Provedor .NET da IBM para .NET Core</span><span class="sxs-lookup"><span data-stu-id="a12c8-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="a12c8-112">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="a12c8-112">Supported Database Engines</span></span>

* <span data-ttu-id="a12c8-113">zOS</span><span class="sxs-lookup"><span data-stu-id="a12c8-113">zOS</span></span>
* <span data-ttu-id="a12c8-114">LUW incluindo IBM dashDB</span><span class="sxs-lookup"><span data-stu-id="a12c8-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="a12c8-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="a12c8-115">IBM I</span></span>
* <span data-ttu-id="a12c8-116">Informix</span><span class="sxs-lookup"><span data-stu-id="a12c8-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a12c8-117">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="a12c8-117">Supported Platforms</span></span>

* <span data-ttu-id="a12c8-118">.NET Framework (4.6 em diante)</span><span class="sxs-lookup"><span data-stu-id="a12c8-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="a12c8-119">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a12c8-119">.NET Core</span></span>
