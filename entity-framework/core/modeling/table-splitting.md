---
title: Divisão de tabela-EF Core
description: Como configurar a divisão de tabela usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: c38d3ee0efa82f84a1051017ae40c9f3fdd57f1f
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781164"
---
# <a name="table-splitting"></a><span data-ttu-id="4ff97-103">Divisão de tabela</span><span class="sxs-lookup"><span data-stu-id="4ff97-103">Table Splitting</span></span>

<span data-ttu-id="4ff97-104">EF Core permite mapear duas ou mais entidades para uma única linha.</span><span class="sxs-lookup"><span data-stu-id="4ff97-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="4ff97-105">Isso é chamado de _divisão de tabela_ ou compartilhamento de _tabela_.</span><span class="sxs-lookup"><span data-stu-id="4ff97-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="4ff97-106">Configuração do</span><span class="sxs-lookup"><span data-stu-id="4ff97-106">Configuration</span></span>

<span data-ttu-id="4ff97-107">Para usar a divisão de tabela, os tipos de entidade precisam ser mapeados para a mesma tabela, ter as chaves primárias mapeadas para as mesmas colunas e pelo menos uma relação configurada entre a chave primária de um tipo de entidade e outra na mesma tabela.</span><span class="sxs-lookup"><span data-stu-id="4ff97-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="4ff97-108">Um cenário comum para divisão de tabela é usar apenas um subconjunto das colunas na tabela para maior desempenho ou encapsulamento.</span><span class="sxs-lookup"><span data-stu-id="4ff97-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="4ff97-109">Neste exemplo `Order` representa um subconjunto de `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="4ff97-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="4ff97-110">Além da configuração necessária, chamamos `Property(o => o.Status).HasColumnName("Status")` para mapear `DetailedOrder.Status` para a mesma coluna que `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="4ff97-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> <span data-ttu-id="4ff97-111">Consulte o [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obter mais contexto.</span><span class="sxs-lookup"><span data-stu-id="4ff97-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="4ff97-112">Medição de</span><span class="sxs-lookup"><span data-stu-id="4ff97-112">Usage</span></span>

<span data-ttu-id="4ff97-113">Salvar e consultar entidades usando divisão de tabela é feito da mesma maneira que outras entidades:</span><span class="sxs-lookup"><span data-stu-id="4ff97-113">Saving and querying entities using table splitting is done in the same way as other entities:</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a><span data-ttu-id="4ff97-114">Entidade dependente opcional</span><span class="sxs-lookup"><span data-stu-id="4ff97-114">Optional dependent entity</span></span>

> [!NOTE]
> <span data-ttu-id="4ff97-115">Esse recurso foi introduzido no EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="4ff97-115">This feature was introduced in EF Core 3.0.</span></span>

<span data-ttu-id="4ff97-116">Se todas as colunas usadas por uma entidade dependente forem `NULL` no banco de dados, nenhuma instância para ela será criada quando consultada.</span><span class="sxs-lookup"><span data-stu-id="4ff97-116">If all of the columns used by a dependent entity are `NULL` in the database, then no instance for it will be created when queried.</span></span> <span data-ttu-id="4ff97-117">Isso permite modelar uma entidade dependente opcional, em que a propriedade relationship no principal seria nula.</span><span class="sxs-lookup"><span data-stu-id="4ff97-117">This allows modeling an optional dependent entity, where the relationship property on the principal would be null.</span></span> <span data-ttu-id="4ff97-118">Observe que isso também aconteceria que todas as propriedades do dependente fossem opcionais e definidas como `null`, o que pode não ser esperado.</span><span class="sxs-lookup"><span data-stu-id="4ff97-118">Note that This would also happen all of the dependent's properties are optional and set to `null`, which might not be expected.</span></span>

## <a name="concurrency-tokens"></a><span data-ttu-id="4ff97-119">Tokens de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="4ff97-119">Concurrency tokens</span></span>

<span data-ttu-id="4ff97-120">Se qualquer um dos tipos de entidade que compartilham uma tabela tiver um token de simultaneidade, ele também deverá ser incluído em todos os outros tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="4ff97-120">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types as well.</span></span> <span data-ttu-id="4ff97-121">Isso é necessário para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="4ff97-121">This is necessary in order to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="4ff97-122">Para evitar a exposição do token de simultaneidade ao código de consumo, é possível a propriedade criar um como [sombra](xref:core/modeling/shadow-properties):</span><span class="sxs-lookup"><span data-stu-id="4ff97-122">To avoid exposing the concurrency token to the consuming code, it's possible the create one as a [shadow property](xref:core/modeling/shadow-properties):</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
