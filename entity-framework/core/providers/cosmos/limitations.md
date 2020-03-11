---
title: Provedor de Azure Cosmos DB-limitações-EF Core
description: As limitações do provedor de Azure Cosmos DB de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417205"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="f68ed-103">Limitações do provedor de Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="f68ed-103">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="f68ed-104">O provedor Cosmos tem uma série de limitações.</span><span class="sxs-lookup"><span data-stu-id="f68ed-104">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="f68ed-105">Muitas dessas limitações são um resultado de limitações no mecanismo de banco de dados Cosmos subjacente e não são específicas do EF.</span><span class="sxs-lookup"><span data-stu-id="f68ed-105">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="f68ed-106">Mas a maioria simplesmente [não foi implementada ainda](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="f68ed-106">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="f68ed-107">Limitações temporárias</span><span class="sxs-lookup"><span data-stu-id="f68ed-107">Temporary limitations</span></span>

- <span data-ttu-id="f68ed-108">Mesmo se houver apenas um tipo de entidade sem herança mapeada para um contêiner, ele ainda terá uma propriedade discriminadora.</span><span class="sxs-lookup"><span data-stu-id="f68ed-108">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="f68ed-109">Tipos de entidade com chaves de partição não funcionam corretamente em alguns cenários</span><span class="sxs-lookup"><span data-stu-id="f68ed-109">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="f68ed-110">Não há suporte para chamadas de `Include`</span><span class="sxs-lookup"><span data-stu-id="f68ed-110">`Include` calls are not supported</span></span>
- <span data-ttu-id="f68ed-111">Não há suporte para chamadas de `Join`</span><span class="sxs-lookup"><span data-stu-id="f68ed-111">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="f68ed-112">Limitações do SDK do Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f68ed-112">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="f68ed-113">Somente métodos assíncronos são fornecidos</span><span class="sxs-lookup"><span data-stu-id="f68ed-113">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="f68ed-114">Como não há nenhuma versão de sincronização dos métodos de nível baixo EF Core depende, a funcionalidade correspondente é implementada no momento chamando `.Wait()` no `Task`retornado.</span><span class="sxs-lookup"><span data-stu-id="f68ed-114">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="f68ed-115">Isso significa que usar métodos como `SaveChanges`ou `ToList` em vez de suas contrapartes assíncronas pode levar a um deadlock em seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="f68ed-115">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="f68ed-116">Limitações de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f68ed-116">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="f68ed-117">Você pode ver a visão geral completa de [Azure Cosmos DB recursos com suporte](/azure/cosmos-db/modeling-data), essas são as diferenças mais notáveis em comparação com um banco de dados relacional:</span><span class="sxs-lookup"><span data-stu-id="f68ed-117">You can see the full overview of [Azure Cosmos DB supported features](/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="f68ed-118">Não há suporte para transações iniciadas pelo cliente</span><span class="sxs-lookup"><span data-stu-id="f68ed-118">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="f68ed-119">Algumas consultas entre partições não têm suporte ou muito mais lentamente, dependendo dos operadores envolvidos</span><span class="sxs-lookup"><span data-stu-id="f68ed-119">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>
