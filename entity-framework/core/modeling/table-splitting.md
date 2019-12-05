---
title: Divisão de tabela-EF Core
description: Como configurar a divisão de tabela usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824561"
---
# <a name="table-splitting"></a><span data-ttu-id="b02de-103">Divisão de tabela</span><span class="sxs-lookup"><span data-stu-id="b02de-103">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="b02de-104">Esse recurso é novo no EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="b02de-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="b02de-105">EF Core permite mapear duas ou mais entidades para uma única linha.</span><span class="sxs-lookup"><span data-stu-id="b02de-105">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="b02de-106">Isso é chamado de _divisão de tabela_ ou compartilhamento de _tabela_.</span><span class="sxs-lookup"><span data-stu-id="b02de-106">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="b02de-107">Configuração do</span><span class="sxs-lookup"><span data-stu-id="b02de-107">Configuration</span></span>

<span data-ttu-id="b02de-108">Para usar a divisão de tabela, os tipos de entidade precisam ser mapeados para a mesma tabela, ter as chaves primárias mapeadas para as mesmas colunas e pelo menos uma relação configurada entre a chave primária de um tipo de entidade e outra na mesma tabela.</span><span class="sxs-lookup"><span data-stu-id="b02de-108">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="b02de-109">Um cenário comum para divisão de tabela é usar apenas um subconjunto das colunas na tabela para maior desempenho ou encapsulamento.</span><span class="sxs-lookup"><span data-stu-id="b02de-109">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="b02de-110">Neste exemplo `Order` representa um subconjunto de `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="b02de-110">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="b02de-111">Além da configuração necessária, chamamos `Property(o => o.Status).HasColumnName("Status")` para mapear `DetailedOrder.Status` para a mesma coluna que `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="b02de-111">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="b02de-112">Consulte o [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obter mais contexto.</span><span class="sxs-lookup"><span data-stu-id="b02de-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="b02de-113">Medição de</span><span class="sxs-lookup"><span data-stu-id="b02de-113">Usage</span></span>

<span data-ttu-id="b02de-114">Salvar e consultar entidades usando divisão de tabela é feito da mesma maneira que outras entidades.</span><span class="sxs-lookup"><span data-stu-id="b02de-114">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="b02de-115">E, a partir do EF Core 3,0, a referência de entidade dependente pode ser `null`.</span><span class="sxs-lookup"><span data-stu-id="b02de-115">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="b02de-116">Se todas as colunas usadas pela entidade dependente forem `NULL` é o banco de dados, nenhuma instância para ela será criada quando consultada.</span><span class="sxs-lookup"><span data-stu-id="b02de-116">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="b02de-117">Isso também aconteceria que todas as propriedades fossem opcionais e definidas como `null`, o que pode não ser esperado.</span><span class="sxs-lookup"><span data-stu-id="b02de-117">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="b02de-118">Tokens de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="b02de-118">Concurrency tokens</span></span>

<span data-ttu-id="b02de-119">Se qualquer um dos tipos de entidade que compartilham uma tabela tiver um token de simultaneidade, ele deverá ser incluído em todos os outros tipos de entidade para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela for atualizada.</span><span class="sxs-lookup"><span data-stu-id="b02de-119">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="b02de-120">Para evitar expô-lo ao código de consumo, é possível criar um no estado de sombra.</span><span class="sxs-lookup"><span data-stu-id="b02de-120">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
