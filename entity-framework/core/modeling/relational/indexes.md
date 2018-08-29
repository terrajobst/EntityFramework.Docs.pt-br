---
title: Índices – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993211"
---
# <a name="indexes"></a><span data-ttu-id="c1bb0-102">Índices</span><span class="sxs-lookup"><span data-stu-id="c1bb0-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="c1bb0-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c1bb0-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="c1bb0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c1bb0-105">Um índice em um banco de dados relacional é mapeado para o mesmo conceito de um índice no núcleo do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="c1bb0-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="c1bb0-106">Conventions</span></span>

<span data-ttu-id="c1bb0-107">Por convenção, os índices são nomeados `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="c1bb0-108">Para índices compostos `<property name>` se tornará uma lista de sublinhado separado de nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c1bb0-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="c1bb0-109">Data Annotations</span></span>

<span data-ttu-id="c1bb0-110">Índices não podem ser configurados usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c1bb0-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="c1bb0-111">Fluent API</span></span>

<span data-ttu-id="c1bb0-112">Você pode usar a API Fluent para configurar o nome de um índice.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="c1bb0-113">Você também pode especificar um filtro.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="c1bb0-114">Ao usar o provedor do SQL Server EF acrescenta um 'IS NOT NULL' Filtrar para todas as colunas que permitem valor nulas que fazem parte de um índice exclusivo.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="c1bb0-115">Para substituir essa convenção, você pode fornecer um `null` valor.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
