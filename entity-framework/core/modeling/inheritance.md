---
title: Herança – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995890"
---
# <a name="inheritance"></a><span data-ttu-id="45fa6-102">Herança</span><span class="sxs-lookup"><span data-stu-id="45fa6-102">Inheritance</span></span>

<span data-ttu-id="45fa6-103">Herança no modelo do EF é usada para controlar como a herança em classes de entidade é representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="45fa6-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="45fa6-104">Convenções</span><span class="sxs-lookup"><span data-stu-id="45fa6-104">Conventions</span></span>

<span data-ttu-id="45fa6-105">Por convenção, é responsabilidade do provedor de banco de dados para determinar como herança será representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="45fa6-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="45fa6-106">Ver [herança (banco de dados relacional)](relational/inheritance.md) de como isso é tratado com um provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="45fa6-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="45fa6-107">EF só irá configurar a herança se dois ou mais tipos herdados estão explicitamente incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="45fa6-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="45fa6-108">O EF não examinará para tipos de base ou derivados caso contrário, não foram incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="45fa6-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="45fa6-109">Você pode incluir tipos no modelo, expondo um *DbSet<TEntity>*  para cada tipo na hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="45fa6-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="45fa6-110">Se você não quiser expor uma *DbSet<TEntity>*  para uma ou mais entidades na hierarquia, você pode usar a API Fluent para garantir que eles são incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="45fa6-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="45fa6-111">E se você não confie em convenções, você pode especificar o tipo base explicitamente usando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="45fa6-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="45fa6-112">Você pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.</span><span class="sxs-lookup"><span data-stu-id="45fa6-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="45fa6-113">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="45fa6-113">Data Annotations</span></span>

<span data-ttu-id="45fa6-114">É possível usar as anotações de dados para configurar a herança.</span><span class="sxs-lookup"><span data-stu-id="45fa6-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="45fa6-115">API fluente</span><span class="sxs-lookup"><span data-stu-id="45fa6-115">Fluent API</span></span>

<span data-ttu-id="45fa6-116">A API Fluent para herança depende do provedor de banco de dados que você está usando.</span><span class="sxs-lookup"><span data-stu-id="45fa6-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="45fa6-117">Ver [herança (banco de dados relacional)](relational/inheritance.md) para a configuração que você pode executar para um provedor de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="45fa6-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
