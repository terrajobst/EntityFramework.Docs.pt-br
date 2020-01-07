---
title: Provedor de Banco de Dados de InMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502117"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="a4b08-102">Provedor de Banco de Dados na Memória do EF Core</span><span class="sxs-lookup"><span data-stu-id="a4b08-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="a4b08-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="a4b08-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="a4b08-104">Isso pode ser útil para testes, embora o provedor SQLite, no modo em memória, seja uma substituição de teste mais adequada para bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="a4b08-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="a4b08-105">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="a4b08-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="a4b08-106">Instalar o</span><span class="sxs-lookup"><span data-stu-id="a4b08-106">Install</span></span>

<span data-ttu-id="a4b08-107">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="a4b08-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="a4b08-108">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4b08-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studiotabvs"></a>[<span data-ttu-id="a4b08-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4b08-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="a4b08-110">Introdução</span><span class="sxs-lookup"><span data-stu-id="a4b08-110">Get Started</span></span>

<span data-ttu-id="a4b08-111">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="a4b08-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="a4b08-112">Testar com InMemory</span><span class="sxs-lookup"><span data-stu-id="a4b08-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="a4b08-113">Testes de Aplicativo de Exemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="a4b08-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="a4b08-114">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="a4b08-114">Supported Database Engines</span></span>

<span data-ttu-id="a4b08-115">Banco de dados de memória em processo (criado apenas para fins de teste)</span><span class="sxs-lookup"><span data-stu-id="a4b08-115">In-process memory database (designed for testing purposes only)</span></span>
