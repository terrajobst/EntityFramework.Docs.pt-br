---
title: Provedor de Banco de Dados de InMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413001"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="9cb99-102">Provedor de Banco de Dados na Memória do EF Core</span><span class="sxs-lookup"><span data-stu-id="9cb99-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="9cb99-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="9cb99-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="9cb99-104">Isso pode ser útil para testes, embora o provedor SQLite, no modo em memória, seja uma substituição de teste mais adequada para bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="9cb99-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="9cb99-105">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="9cb99-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="9cb99-106">Instalar o</span><span class="sxs-lookup"><span data-stu-id="9cb99-106">Install</span></span>

<span data-ttu-id="9cb99-107">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="9cb99-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9cb99-108">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="9cb99-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="9cb99-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cb99-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="9cb99-110">Introdução</span><span class="sxs-lookup"><span data-stu-id="9cb99-110">Get Started</span></span>

<span data-ttu-id="9cb99-111">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="9cb99-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="9cb99-112">Testar com InMemory</span><span class="sxs-lookup"><span data-stu-id="9cb99-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="9cb99-113">Testes de Aplicativo de Exemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="9cb99-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="9cb99-114">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="9cb99-114">Supported Database Engines</span></span>

<span data-ttu-id="9cb99-115">Banco de dados de memória em processo (criado apenas para fins de teste)</span><span class="sxs-lookup"><span data-stu-id="9cb99-115">In-process memory database (designed for testing purposes only)</span></span>
