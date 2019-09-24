---
title: Índices-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7dcf27dedbde45302a462a4c41a811b9868e40bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197019"
---
# <a name="indexes"></a><span data-ttu-id="dc019-102">Índices</span><span class="sxs-lookup"><span data-stu-id="dc019-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="dc019-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="dc019-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="dc019-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="dc019-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="dc019-105">Um índice em um banco de dados relacional é mapeado para o mesmo conceito de um índice no núcleo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dc019-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="dc019-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="dc019-106">Conventions</span></span>

<span data-ttu-id="dc019-107">Por convenção, os índices são `IX_<type name>_<property name>`nomeados.</span><span class="sxs-lookup"><span data-stu-id="dc019-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="dc019-108">Para índices `<property name>` compostos se torna uma lista de nomes de propriedade separados por sublinhado.</span><span class="sxs-lookup"><span data-stu-id="dc019-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="dc019-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="dc019-109">Data Annotations</span></span>

<span data-ttu-id="dc019-110">Os índices não podem ser configurados usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="dc019-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="dc019-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="dc019-111">Fluent API</span></span>

<span data-ttu-id="dc019-112">Você pode usar a API Fluent para configurar o nome de um índice.</span><span class="sxs-lookup"><span data-stu-id="dc019-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="dc019-113">Você também pode especificar um filtro.</span><span class="sxs-lookup"><span data-stu-id="dc019-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="dc019-114">Ao usar o provedor de SQL Server EF, o adiciona um filtro ' IS NOT NULL ' para todas as colunas anuláveis que fazem parte de um índice exclusivo.</span><span class="sxs-lookup"><span data-stu-id="dc019-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="dc019-115">Para substituir essa convenção, você pode fornecer `null` um valor.</span><span class="sxs-lookup"><span data-stu-id="dc019-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="dc019-116">Incluir colunas em índices de SQL Server</span><span class="sxs-lookup"><span data-stu-id="dc019-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="dc019-117">Você pode configurar [índices com colunas incluídas](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) para melhorar significativamente o desempenho da consulta quando todas as colunas na consulta são incluídas no índice como colunas de chave ou não chave.</span><span class="sxs-lookup"><span data-stu-id="dc019-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
