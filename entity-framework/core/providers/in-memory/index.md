---
title: "Provedor de Banco de Dados de InMemory – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="79424-102">Provedor de Banco de Dados na Memória do EF Core</span><span class="sxs-lookup"><span data-stu-id="79424-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="79424-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="79424-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="79424-104">Isso pode ser útil para testes, embora o provedor SQLite, no modo em memória, seja uma substituição de teste mais adequada para bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="79424-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="79424-105">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="79424-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="79424-106">Instalar o</span><span class="sxs-lookup"><span data-stu-id="79424-106">Install</span></span>

<span data-ttu-id="79424-107">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="79424-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="79424-108">Introdução</span><span class="sxs-lookup"><span data-stu-id="79424-108">Get Started</span></span>

<span data-ttu-id="79424-109">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="79424-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="79424-110">Testar com InMemory</span><span class="sxs-lookup"><span data-stu-id="79424-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="79424-111">Testes de Aplicativo de Exemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="79424-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="79424-112">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="79424-112">Supported Database Engines</span></span>

* <span data-ttu-id="79424-113">Banco de dados na memória interno (projetado somente para fins de teste)</span><span class="sxs-lookup"><span data-stu-id="79424-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="79424-114">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="79424-114">Supported Platforms</span></span>

* <span data-ttu-id="79424-115">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="79424-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="79424-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="79424-116">.NET Core</span></span>

* <span data-ttu-id="79424-117">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="79424-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="79424-118">Plataforma Universal do Windows</span><span class="sxs-lookup"><span data-stu-id="79424-118">Universal Windows Platform</span></span>
