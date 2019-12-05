---
title: Herança-EF Core
description: Como configurar a herança de tipo de entidade usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824681"
---
# <a name="inheritance"></a><span data-ttu-id="5d5a7-103">{1&gt;Herança&lt;1}</span><span class="sxs-lookup"><span data-stu-id="5d5a7-103">Inheritance</span></span>

<span data-ttu-id="5d5a7-104">A herança no modelo do EF é usada para controlar como a herança nas classes de entidade é representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="5d5a7-105">Convenções</span><span class="sxs-lookup"><span data-stu-id="5d5a7-105">Conventions</span></span>

<span data-ttu-id="5d5a7-106">Por padrão, cabe ao provedor de banco de dados determinar como a herança será representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="5d5a7-107">Consulte [herança (banco de dados relacional)](relational/inheritance.md) para saber como isso é tratado com um provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="5d5a7-108">O EF só configurará a herança se dois ou mais tipos herdados forem explicitamente incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="5d5a7-109">O EF não examinará os tipos base ou derivados que não foram incluídos de outra forma no modelo.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="5d5a7-110">Você pode incluir tipos no modelo expondo um *DbSet\<TEntity >* para cada tipo na hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="5d5a7-111">Se você não quiser expor um *DbSet\<TEntity >* para uma ou mais entidades na hierarquia, poderá usar a API fluente para garantir que elas sejam incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="5d5a7-112">E se você não depender de convenções, poderá especificar o tipo base explicitamente usando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="5d5a7-113">Você pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5d5a7-114">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="5d5a7-114">Data Annotations</span></span>

<span data-ttu-id="5d5a7-115">Você não pode usar anotações de dados para configurar a herança.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5d5a7-116">API fluente</span><span class="sxs-lookup"><span data-stu-id="5d5a7-116">Fluent API</span></span>

<span data-ttu-id="5d5a7-117">A API fluente para herança depende do provedor de banco de dados que você está usando.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="5d5a7-118">Consulte [herança (banco de dados relacional)](relational/inheritance.md) para a configuração que você pode executar para um provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="5d5a7-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
