---
title: Provedor de Banco de Dados de InMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993314"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="2fb13-102">Provedor de Banco de Dados na Memória do EF Core</span><span class="sxs-lookup"><span data-stu-id="2fb13-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="2fb13-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="2fb13-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="2fb13-104">Isso pode ser útil para testes, embora o provedor SQLite, no modo em memória, seja uma substituição de teste mais adequada para bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="2fb13-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="2fb13-105">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2fb13-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="2fb13-106">Instalar o</span><span class="sxs-lookup"><span data-stu-id="2fb13-106">Install</span></span>

<span data-ttu-id="2fb13-107">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="2fb13-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="2fb13-108">Introdução</span><span class="sxs-lookup"><span data-stu-id="2fb13-108">Get Started</span></span>

<span data-ttu-id="2fb13-109">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="2fb13-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="2fb13-110">Testar com InMemory</span><span class="sxs-lookup"><span data-stu-id="2fb13-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="2fb13-111">Testes de Aplicativo de Exemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="2fb13-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="2fb13-112">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="2fb13-112">Supported Database Engines</span></span>

* <span data-ttu-id="2fb13-113">Banco de dados na memória interno (projetado somente para fins de teste)</span><span class="sxs-lookup"><span data-stu-id="2fb13-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="2fb13-114">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="2fb13-114">Supported Platforms</span></span>

* <span data-ttu-id="2fb13-115">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="2fb13-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="2fb13-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fb13-116">.NET Core</span></span>

* <span data-ttu-id="2fb13-117">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="2fb13-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="2fb13-118">Plataforma Universal do Windows</span><span class="sxs-lookup"><span data-stu-id="2fb13-118">Universal Windows Platform</span></span>
