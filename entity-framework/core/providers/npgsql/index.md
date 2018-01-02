---
title: "Provedor de Banco de Dados Npgsql para PostgreSQL – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="417e0-102">Provedor de Banco de Dados de EF Core Npgsql para PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="417e0-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="417e0-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="417e0-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="417e0-104">O provedor é mantido como parte do [projeto Npgsql](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="417e0-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="417e0-105">Este provedor não é mantido como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="417e0-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="417e0-106">Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="417e0-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="417e0-107">Instalar o</span><span class="sxs-lookup"><span data-stu-id="417e0-107">Install</span></span>

<span data-ttu-id="417e0-108">Instale o [pacote NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="417e0-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="417e0-109">Introdução</span><span class="sxs-lookup"><span data-stu-id="417e0-109">Get Started</span></span>

<span data-ttu-id="417e0-110">Consulte a [documentação do Npgsql](http://www.npgsql.org/efcore/index.html) para começar.</span><span class="sxs-lookup"><span data-stu-id="417e0-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="417e0-111">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="417e0-111">Supported Database Engines</span></span>

* <span data-ttu-id="417e0-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="417e0-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="417e0-113">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="417e0-113">Supported Platforms</span></span>

* <span data-ttu-id="417e0-114">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="417e0-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="417e0-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="417e0-115">.NET Core</span></span>

* <span data-ttu-id="417e0-116">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="417e0-116">Mono (4.2.0 onwards)</span></span>
