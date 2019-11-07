---
title: Incluindo & excluindo tipos-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655734"
---
# <a name="including--excluding-types"></a><span data-ttu-id="61551-102">Incluir e Excluir Tipos</span><span class="sxs-lookup"><span data-stu-id="61551-102">Including & Excluding Types</span></span>

<span data-ttu-id="61551-103">A inclusão de um tipo no modelo significa que o EF tem metadados sobre esse tipo e tentará ler e gravar instâncias de/para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61551-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="61551-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="61551-104">Conventions</span></span>

<span data-ttu-id="61551-105">Por convenção, os tipos expostos em Propriedades `DbSet` no seu contexto são incluídos em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="61551-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="61551-106">Além disso, os tipos mencionados no método `OnModelCreating` também são incluídos.</span><span class="sxs-lookup"><span data-stu-id="61551-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="61551-107">Finalmente, todos os tipos que são encontrados pela exploração recursiva das propriedades de navegação de tipos descobertos também são incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="61551-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="61551-108">**Por exemplo, na listagem de código a seguir, todos os três tipos são descobertos:**</span><span class="sxs-lookup"><span data-stu-id="61551-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="61551-109">`Blog` porque ele é exposto em uma propriedade `DbSet` no contexto</span><span class="sxs-lookup"><span data-stu-id="61551-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="61551-110">`Post` porque ele é descoberto por meio da propriedade de navegação `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="61551-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="61551-111">`AuditEntry` porque ele é mencionado em `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="61551-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="61551-112">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="61551-112">Data Annotations</span></span>

<span data-ttu-id="61551-113">Você pode usar as anotações de dados para excluir um tipo do modelo.</span><span class="sxs-lookup"><span data-stu-id="61551-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="61551-114">API fluente</span><span class="sxs-lookup"><span data-stu-id="61551-114">Fluent API</span></span>

<span data-ttu-id="61551-115">Você pode usar a API fluente para excluir um tipo do modelo.</span><span class="sxs-lookup"><span data-stu-id="61551-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
