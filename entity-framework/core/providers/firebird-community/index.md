---
title: "Provedor de Banco de Dados FirebirdSQL – Core EF"
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="cf159-102">Provedor de Banco de Dados EF Core Firebird</span><span class="sxs-lookup"><span data-stu-id="cf159-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="cf159-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com FirebirdSQL.</span><span class="sxs-lookup"><span data-stu-id="cf159-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="cf159-104">O provedor é mantido como parte do [Projeto GitHub ralmsdeveloper/EntityFrameworkCore.FirebirdSQL](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="cf159-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="cf159-105">Este provedor não é mantido como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="cf159-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="cf159-106">Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="cf159-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="cf159-107">Instalar o</span><span class="sxs-lookup"><span data-stu-id="cf159-107">Install</span></span>

<span data-ttu-id="cf159-108">Instale o [pacote NuGet EntityFrameworkCore.FirebirdSQL](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="cf159-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="cf159-109">Introdução</span><span class="sxs-lookup"><span data-stu-id="cf159-109">Get Started</span></span>

<span data-ttu-id="cf159-110">Consulte a [documentação de introdução no site do projeto](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span><span class="sxs-lookup"><span data-stu-id="cf159-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="cf159-111">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="cf159-111">Supported Database Engines</span></span>

* <span data-ttu-id="cf159-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="cf159-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="cf159-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="cf159-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="cf159-114">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="cf159-114">Supported Platforms</span></span>

* <span data-ttu-id="cf159-115">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="cf159-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="cf159-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf159-116">.NET Core</span></span>

* <span data-ttu-id="cf159-117">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="cf159-117">Mono (4.2.0 onwards)</span></span>
