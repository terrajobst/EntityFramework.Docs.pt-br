---
title: Índices-EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502130"
---
# <a name="indexes"></a><span data-ttu-id="b93d5-102">Índices</span><span class="sxs-lookup"><span data-stu-id="b93d5-102">Indexes</span></span>

<span data-ttu-id="b93d5-103">Os índices são um conceito comum entre vários armazenamentos de dados.</span><span class="sxs-lookup"><span data-stu-id="b93d5-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="b93d5-104">Embora sua implementação no armazenamento de dados possa variar, elas são usadas para fazer pesquisas com base em uma coluna (ou conjunto de colunas) mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="b93d5-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

<span data-ttu-id="b93d5-105">Não é possível criar índices usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="b93d5-105">Indexes cannot be created using data annotations.</span></span> <span data-ttu-id="b93d5-106">Você pode usar a API fluente para especificar um índice em uma única coluna da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="b93d5-106">You can use the Fluent API to specify an index on a single column as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

<span data-ttu-id="b93d5-107">Você também pode especificar um índice em mais de uma coluna:</span><span class="sxs-lookup"><span data-stu-id="b93d5-107">You can also specify an index over more than one column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> <span data-ttu-id="b93d5-108">Por convenção, um índice é criado em cada propriedade (ou conjunto de propriedades) que são usados como uma chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="b93d5-108">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>
>
> <span data-ttu-id="b93d5-109">EF Core só dá suporte a um índice por conjunto distinto de propriedades.</span><span class="sxs-lookup"><span data-stu-id="b93d5-109">EF Core only supports one index per distinct set of properties.</span></span> <span data-ttu-id="b93d5-110">Se você usar a API fluente para configurar um índice em um conjunto de propriedades que já tem um índice definido, seja por convenção ou configuração anterior, você alterará a definição desse índice.</span><span class="sxs-lookup"><span data-stu-id="b93d5-110">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="b93d5-111">Isso será útil se você quiser configurar ainda mais um índice que foi criado por convenção.</span><span class="sxs-lookup"><span data-stu-id="b93d5-111">This is useful if you want to further configure an index that was created by convention.</span></span>

## <a name="index-uniqueness"></a><span data-ttu-id="b93d5-112">Exclusividade do índice</span><span class="sxs-lookup"><span data-stu-id="b93d5-112">Index uniqueness</span></span>

<span data-ttu-id="b93d5-113">Por padrão, os índices não são exclusivos: várias linhas têm permissão para ter os mesmos valores para o conjunto de colunas do índice.</span><span class="sxs-lookup"><span data-stu-id="b93d5-113">By default, indexes aren't unique: multiple rows are allowed to have the same value(s) for the index's column set.</span></span> <span data-ttu-id="b93d5-114">Você pode fazer um índice exclusivo da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="b93d5-114">You can make an index unique as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

<span data-ttu-id="b93d5-115">A tentativa de inserir mais de uma entidade com os mesmos valores para o conjunto de colunas do índice fará com que uma exceção seja gerada.</span><span class="sxs-lookup"><span data-stu-id="b93d5-115">Attempting to insert more than one entity with the same values for the index's column set will cause an exception to be thrown.</span></span>

## <a name="index-name"></a><span data-ttu-id="b93d5-116">Nome do índice</span><span class="sxs-lookup"><span data-stu-id="b93d5-116">Index name</span></span>

<span data-ttu-id="b93d5-117">Por convenção, os índices criados em um banco de dados relacional são nomeados `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="b93d5-117">By convention, indexes created in a relational database are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="b93d5-118">Para índices compostos, `<property name>` se torna uma lista de nomes de propriedade separados por sublinhado.</span><span class="sxs-lookup"><span data-stu-id="b93d5-118">For composite indexes, `<property name>` becomes an underscore separated list of property names.</span></span>

<span data-ttu-id="b93d5-119">Você pode usar a API fluente para definir o nome do índice criado no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="b93d5-119">You can use the Fluent API to set the name of the index created in the database:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a><span data-ttu-id="b93d5-120">Filtro de índice</span><span class="sxs-lookup"><span data-stu-id="b93d5-120">Index filter</span></span>

<span data-ttu-id="b93d5-121">Alguns bancos de dados relacionais permitem que você especifique um índice filtrado ou parcial.</span><span class="sxs-lookup"><span data-stu-id="b93d5-121">Some relational databases allow you to specify a filtered or partial index.</span></span> <span data-ttu-id="b93d5-122">Isso permite que você indexe apenas um subconjunto dos valores de uma coluna, reduzindo o tamanho do índice e melhorando o desempenho e o uso do espaço em disco.</span><span class="sxs-lookup"><span data-stu-id="b93d5-122">This allows you to index only a subset of a column's values, reducing the index's size and improving both performance and disk space usage.</span></span> <span data-ttu-id="b93d5-123">Para obter mais informações sobre SQL Server índices filtrados, [consulte a documentação](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).</span><span class="sxs-lookup"><span data-stu-id="b93d5-123">For more information on SQL Server filtered indexes, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).</span></span>

<span data-ttu-id="b93d5-124">Você pode usar a API fluente para especificar um filtro em um índice, fornecido como uma expressão SQL:</span><span class="sxs-lookup"><span data-stu-id="b93d5-124">You can use the Fluent API to specify a filter on an index, provided as a SQL expression:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

<span data-ttu-id="b93d5-125">Ao usar o provedor de SQL Server o EF adiciona um filtro de `'IS NOT NULL'` para todas as colunas anuláveis que fazem parte de um índice exclusivo.</span><span class="sxs-lookup"><span data-stu-id="b93d5-125">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="b93d5-126">Para substituir essa convenção, você pode fornecer um valor `null`.</span><span class="sxs-lookup"><span data-stu-id="b93d5-126">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a><span data-ttu-id="b93d5-127">Colunas incluídas</span><span class="sxs-lookup"><span data-stu-id="b93d5-127">Included columns</span></span>

<span data-ttu-id="b93d5-128">Alguns bancos de dados relacionais permitem que você configure um conjunto de colunas que são incluídas no índice, mas que não fazem parte de sua "chave".</span><span class="sxs-lookup"><span data-stu-id="b93d5-128">Some relational databases allow you to configure a set of columns which get included in the index, but aren't part of its "key".</span></span> <span data-ttu-id="b93d5-129">Isso pode melhorar significativamente o desempenho da consulta quando todas as colunas na consulta são incluídas no índice como colunas de chave ou não chave, pois a tabela em si não precisa ser acessada.</span><span class="sxs-lookup"><span data-stu-id="b93d5-129">This can significantly improve query performance when all columns in the query are included in the index either as key or nonkey columns, as the table itself doesn't need to be accessed.</span></span> <span data-ttu-id="b93d5-130">Para obter mais informações sobre SQL Server colunas incluídas, [consulte a documentação](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).</span><span class="sxs-lookup"><span data-stu-id="b93d5-130">For more information on SQL Server included columns, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).</span></span>

<span data-ttu-id="b93d5-131">No exemplo a seguir, a coluna `Url` é parte da chave de índice, portanto, qualquer filtragem de consulta nessa coluna pode usar o índice.</span><span class="sxs-lookup"><span data-stu-id="b93d5-131">In the following example, the `Url` column is part of the index key, so any query filtering on that column can use the index.</span></span> <span data-ttu-id="b93d5-132">Mas, além disso, as consultas que acessam apenas as colunas `Title` e `PublishedOn` não precisarão acessar a tabela e serão executadas com mais eficiência:</span><span class="sxs-lookup"><span data-stu-id="b93d5-132">But in addition, queries accessing only the `Title` and `PublishedOn` columns will not need to access the table and will run more efficiently:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
