---
title: Herança - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052776"
---
# <a name="inheritance"></a><span data-ttu-id="af0ac-102">Herança</span><span class="sxs-lookup"><span data-stu-id="af0ac-102">Inheritance</span></span>

<span data-ttu-id="af0ac-103">Herança no modelo EF é usada para controlar como a herança em classes de entidade é representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="af0ac-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="af0ac-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="af0ac-104">Conventions</span></span>

<span data-ttu-id="af0ac-105">Por convenção, é até o provedor de banco de dados para determinar como herança será representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="af0ac-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="af0ac-106">Consulte [herança (banco de dados relacional)](relational/inheritance.md) de como isso é tratado com um provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="af0ac-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="af0ac-107">EF apenas configurará herança se dois ou mais tipos herdados estão explicitamente incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="af0ac-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="af0ac-108">EF não irá procurar tipos base ou derivados que não foram incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="af0ac-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="af0ac-109">Você pode incluir tipos no modelo ao expor um *DbSet<TEntity>*  para cada tipo na hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="af0ac-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="af0ac-110">Se você não quiser expor um *DbSet<TEntity>*  para uma ou mais entidades na hierarquia, você pode usar a API fluente para garantir que eles estão incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="af0ac-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="af0ac-111">E se você não confiar em convenções, você pode especificar o tipo base explicitamente usando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="af0ac-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="af0ac-112">Você pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.</span><span class="sxs-lookup"><span data-stu-id="af0ac-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="af0ac-113">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="af0ac-113">Data Annotations</span></span>

<span data-ttu-id="af0ac-114">Você não pode usar as anotações de dados para configurar a herança.</span><span class="sxs-lookup"><span data-stu-id="af0ac-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="af0ac-115">API fluente</span><span class="sxs-lookup"><span data-stu-id="af0ac-115">Fluent API</span></span>

<span data-ttu-id="af0ac-116">A API fluente de herança depende do provedor de banco de dados que você está usando.</span><span class="sxs-lookup"><span data-stu-id="af0ac-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="af0ac-117">Consulte [herança (banco de dados relacional)](relational/inheritance.md) para a configuração que você pode executar para um provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="af0ac-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
