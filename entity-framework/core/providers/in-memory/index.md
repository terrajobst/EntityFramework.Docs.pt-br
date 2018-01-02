---
title: "Provedor de Banco de Dados de InMemory – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="a5bc8-102">Provedor de Banco de Dados na Memória do EF Core</span><span class="sxs-lookup"><span data-stu-id="a5bc8-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="a5bc8-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="a5bc8-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="a5bc8-104">Isso é útil ao testar o código que usa o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a5bc8-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="a5bc8-105">O provedor é mantido como parte do [projeto EntityFramework GitHub](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="a5bc8-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="a5bc8-106">Instalar o</span><span class="sxs-lookup"><span data-stu-id="a5bc8-106">Install</span></span>

<span data-ttu-id="a5bc8-107">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="a5bc8-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="a5bc8-108">Introdução</span><span class="sxs-lookup"><span data-stu-id="a5bc8-108">Get Started</span></span>

<span data-ttu-id="a5bc8-109">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="a5bc8-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="a5bc8-110">Testar com InMemory</span><span class="sxs-lookup"><span data-stu-id="a5bc8-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="a5bc8-111">Testes de Aplicativo de Exemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="a5bc8-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="a5bc8-112">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="a5bc8-112">Supported Database Engines</span></span>

* <span data-ttu-id="a5bc8-113">Banco de dados na memória interno (projetado somente para fins de teste)</span><span class="sxs-lookup"><span data-stu-id="a5bc8-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a5bc8-114">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="a5bc8-114">Supported Platforms</span></span>

* <span data-ttu-id="a5bc8-115">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="a5bc8-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="a5bc8-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5bc8-116">.NET Core</span></span>

* <span data-ttu-id="a5bc8-117">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="a5bc8-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="a5bc8-118">Plataforma Universal do Windows</span><span class="sxs-lookup"><span data-stu-id="a5bc8-118">Universal Windows Platform</span></span>
