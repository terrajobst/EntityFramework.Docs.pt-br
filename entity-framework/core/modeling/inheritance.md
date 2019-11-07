---
title: Herança-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655683"
---
# <a name="inheritance"></a><span data-ttu-id="1e833-102">Herança</span><span class="sxs-lookup"><span data-stu-id="1e833-102">Inheritance</span></span>

<span data-ttu-id="1e833-103">A herança no modelo do EF é usada para controlar como a herança nas classes de entidade é representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1e833-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="1e833-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="1e833-104">Conventions</span></span>

<span data-ttu-id="1e833-105">Por convenção, cabe ao provedor de banco de dados determinar como a herança será representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1e833-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="1e833-106">Consulte [herança (banco de dados relacional)](relational/inheritance.md) para saber como isso é tratado com um provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="1e833-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="1e833-107">O EF só configurará a herança se dois ou mais tipos herdados forem explicitamente incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="1e833-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="1e833-108">O EF não examinará os tipos base ou derivados que não foram incluídos de outra forma no modelo.</span><span class="sxs-lookup"><span data-stu-id="1e833-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="1e833-109">Você pode incluir tipos no modelo expondo um *DbSet\<TEntity >* para cada tipo na hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="1e833-109">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="1e833-110">Se você não quiser expor um *DbSet\<TEntity >* para uma ou mais entidades na hierarquia, poderá usar a API fluente para garantir que elas sejam incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="1e833-110">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="1e833-111">E se você não depender de convenções, poderá especificar o tipo base explicitamente usando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="1e833-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="1e833-112">Você pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.</span><span class="sxs-lookup"><span data-stu-id="1e833-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1e833-113">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="1e833-113">Data Annotations</span></span>

<span data-ttu-id="1e833-114">Você não pode usar anotações de dados para configurar a herança.</span><span class="sxs-lookup"><span data-stu-id="1e833-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1e833-115">API fluente</span><span class="sxs-lookup"><span data-stu-id="1e833-115">Fluent API</span></span>

<span data-ttu-id="1e833-116">A API fluente para herança depende do provedor de banco de dados que você está usando.</span><span class="sxs-lookup"><span data-stu-id="1e833-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="1e833-117">Consulte [herança (banco de dados relacional)](relational/inheritance.md) para a configuração que você pode executar para um provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="1e833-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
