---
title: "Provedor de Banco de Dados Pomelo MySQL – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="0f17d-102">Provedor de Banco de Dados EF Core Pomelo para MySQL</span><span class="sxs-lookup"><span data-stu-id="0f17d-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="0f17d-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com MySQL.</span><span class="sxs-lookup"><span data-stu-id="0f17d-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="0f17d-104">O provedor é mantido como parte do [Projeto da Pomelo Foundation](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="0f17d-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="0f17d-105">Este provedor não é mantido como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0f17d-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="0f17d-106">Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="0f17d-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="0f17d-107">Instalar o</span><span class="sxs-lookup"><span data-stu-id="0f17d-107">Install</span></span>

<span data-ttu-id="0f17d-108">Instale o [pacote NuGet Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="0f17d-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="0f17d-109">Introdução</span><span class="sxs-lookup"><span data-stu-id="0f17d-109">Get Started</span></span>

<span data-ttu-id="0f17d-110">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="0f17d-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="0f17d-111">Documentação de introdução</span><span class="sxs-lookup"><span data-stu-id="0f17d-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="0f17d-112">Aplicativo de exemplo Yuuko Blog</span><span class="sxs-lookup"><span data-stu-id="0f17d-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="0f17d-113">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="0f17d-113">Supported Database Engines</span></span>

* <span data-ttu-id="0f17d-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="0f17d-114">MySQL</span></span>
* <span data-ttu-id="0f17d-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="0f17d-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="0f17d-116">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="0f17d-116">Supported Platforms</span></span>

* <span data-ttu-id="0f17d-117">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="0f17d-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="0f17d-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f17d-118">.NET Core</span></span>

* <span data-ttu-id="0f17d-119">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="0f17d-119">Mono (4.2.0 onwards)</span></span>
