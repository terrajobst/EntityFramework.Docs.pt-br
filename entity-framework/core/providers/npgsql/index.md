---
title: "Provedor de Banco de Dados Npgsql para PostgreSQL – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="19039-102">Provedor de Banco de Dados de EF Core Npgsql para PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="19039-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="19039-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="19039-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="19039-104">O provedor é mantido como parte do [projeto Npgsql](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="19039-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="19039-105">Este provedor não é mantido como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="19039-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="19039-106">Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="19039-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="19039-107">Instalar o</span><span class="sxs-lookup"><span data-stu-id="19039-107">Install</span></span>

<span data-ttu-id="19039-108">Instale o [pacote NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="19039-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="19039-109">Introdução</span><span class="sxs-lookup"><span data-stu-id="19039-109">Get Started</span></span>

<span data-ttu-id="19039-110">Consulte a [documentação do Npgsql](http://www.npgsql.org/efcore/index.html) para começar.</span><span class="sxs-lookup"><span data-stu-id="19039-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="19039-111">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="19039-111">Supported Database Engines</span></span>

* <span data-ttu-id="19039-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="19039-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="19039-113">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="19039-113">Supported Platforms</span></span>

* <span data-ttu-id="19039-114">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="19039-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="19039-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="19039-115">.NET Core</span></span>

* <span data-ttu-id="19039-116">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="19039-116">Mono (4.2.0 onwards)</span></span>
