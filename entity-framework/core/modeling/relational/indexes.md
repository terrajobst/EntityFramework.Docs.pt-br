---
title: "Índices - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a><span data-ttu-id="3ff30-102">Índices</span><span class="sxs-lookup"><span data-stu-id="3ff30-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="3ff30-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="3ff30-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3ff30-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="3ff30-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3ff30-105">Um índice em um banco de dados relacional é mapeado para o mesmo conceito que um índice no núcleo do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3ff30-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="3ff30-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="3ff30-106">Conventions</span></span>

<span data-ttu-id="3ff30-107">Por convenção, os índices são denominados `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="3ff30-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="3ff30-108">Para índices compostos `<property name>` torna-se uma lista de sublinhado separado dos nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="3ff30-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3ff30-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="3ff30-109">Data Annotations</span></span>

<span data-ttu-id="3ff30-110">Índices não podem ser configurados usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="3ff30-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3ff30-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="3ff30-111">Fluent API</span></span>

<span data-ttu-id="3ff30-112">Você pode usar a API fluente para configurar o nome de um índice.</span><span class="sxs-lookup"><span data-stu-id="3ff30-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="3ff30-113">Você também pode especificar um filtro.</span><span class="sxs-lookup"><span data-stu-id="3ff30-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="3ff30-114">Quando usar o provedor do SQL Server EF adiciona um 'IS NOT NULL' Filtrar todas as colunas anuláveis que fazem parte de um índice exclusivo.</span><span class="sxs-lookup"><span data-stu-id="3ff30-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="3ff30-115">Para substituir essa convenção, você pode fornecer um `null` valor.</span><span class="sxs-lookup"><span data-stu-id="3ff30-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
