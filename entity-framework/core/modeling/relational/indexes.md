---
title: Índices (banco de dados relacional)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824570"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="1f60d-102">Índices (banco de dados relacional)</span><span class="sxs-lookup"><span data-stu-id="1f60d-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="1f60d-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="1f60d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1f60d-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="1f60d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1f60d-105">Um índice em um banco de dados relacional é mapeado para o mesmo conceito de um índice no núcleo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1f60d-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="1f60d-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="1f60d-106">Conventions</span></span>

<span data-ttu-id="1f60d-107">Por convenção, os índices são nomeados `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="1f60d-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="1f60d-108">Para índices compostos `<property name>` se torna uma lista de nomes de propriedade separados por sublinhado.</span><span class="sxs-lookup"><span data-stu-id="1f60d-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1f60d-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="1f60d-109">Data Annotations</span></span>

<span data-ttu-id="1f60d-110">Os índices não podem ser configurados usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="1f60d-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1f60d-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="1f60d-111">Fluent API</span></span>

<span data-ttu-id="1f60d-112">Você pode usar a API Fluent para configurar o nome de um índice.</span><span class="sxs-lookup"><span data-stu-id="1f60d-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="1f60d-113">Você também pode especificar um filtro.</span><span class="sxs-lookup"><span data-stu-id="1f60d-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="1f60d-114">Ao usar o provedor de SQL Server o EF adiciona um filtro de `'IS NOT NULL'` para todas as colunas anuláveis que fazem parte de um índice exclusivo.</span><span class="sxs-lookup"><span data-stu-id="1f60d-114">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="1f60d-115">Para substituir essa convenção, você pode fornecer um valor `null`.</span><span class="sxs-lookup"><span data-stu-id="1f60d-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="1f60d-116">Incluir colunas em índices de SQL Server</span><span class="sxs-lookup"><span data-stu-id="1f60d-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="1f60d-117">Você pode configurar [índices com colunas incluídas](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) para melhorar significativamente o desempenho da consulta quando todas as colunas na consulta são incluídas no índice como colunas de chave ou não chave.</span><span class="sxs-lookup"><span data-stu-id="1f60d-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
