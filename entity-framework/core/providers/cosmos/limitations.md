---
title: Provedor de Azure Cosmos DB-limitações-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150805"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="68fa5-102">Limitações do provedor de Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="68fa5-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="68fa5-103">O provedor Cosmos tem uma série de limitações.</span><span class="sxs-lookup"><span data-stu-id="68fa5-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="68fa5-104">Muitas dessas limitações são um resultado de limitações no mecanismo de banco de dados Cosmos subjacente e não são específicas do EF.</span><span class="sxs-lookup"><span data-stu-id="68fa5-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="68fa5-105">Mas a maioria simplesmente [não foi implementada ainda](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="68fa5-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="68fa5-106">Limitações temporárias</span><span class="sxs-lookup"><span data-stu-id="68fa5-106">Temporary limitations</span></span>

- <span data-ttu-id="68fa5-107">Mesmo se houver apenas um tipo de entidade sem herança mapeada para um contêiner, ele ainda terá uma propriedade discriminadora.</span><span class="sxs-lookup"><span data-stu-id="68fa5-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="68fa5-108">Tipos de entidade com chaves de partição não funcionam corretamente em alguns cenários</span><span class="sxs-lookup"><span data-stu-id="68fa5-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="68fa5-109">`Include`Não há suporte para chamadas</span><span class="sxs-lookup"><span data-stu-id="68fa5-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="68fa5-110">`Join`Não há suporte para chamadas</span><span class="sxs-lookup"><span data-stu-id="68fa5-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="68fa5-111">Limitações do SDK do Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="68fa5-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="68fa5-112">Somente métodos assíncronos são fornecidos</span><span class="sxs-lookup"><span data-stu-id="68fa5-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="68fa5-113">Como não há nenhuma versão de sincronização dos métodos de nível baixo EF Core depende, a funcionalidade correspondente é implementada no momento chamando `.Wait()` no retornado. `Task`</span><span class="sxs-lookup"><span data-stu-id="68fa5-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="68fa5-114">Isso significa que usar métodos como `SaveChanges`, ou `ToList` em vez de suas contrapartes assíncronas pode levar a um deadlock em seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="68fa5-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="68fa5-115">Limitações de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="68fa5-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="68fa5-116">Você pode ver a visão geral completa de [Azure Cosmos DB recursos com suporte](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), essas são as diferenças mais notáveis em comparação com um banco de dados relacional:</span><span class="sxs-lookup"><span data-stu-id="68fa5-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="68fa5-117">Não há suporte para transações iniciadas pelo cliente</span><span class="sxs-lookup"><span data-stu-id="68fa5-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="68fa5-118">Algumas consultas entre partições não têm suporte ou muito mais lentamente, dependendo dos operadores envolvidos</span><span class="sxs-lookup"><span data-stu-id="68fa5-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>