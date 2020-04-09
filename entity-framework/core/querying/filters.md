---
title: Filtros de consulta global – EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417725"
---
# <a name="global-query-filters"></a><span data-ttu-id="25c6a-102">Filtros de consulta global</span><span class="sxs-lookup"><span data-stu-id="25c6a-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="25c6a-103">Essa funcionalidade foi introduzida no EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="25c6a-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="25c6a-104">Os filtros de consulta global são predicados de consulta LINQ (uma expressão booliana normalmente passada ao operador de consulta LINQ *Where*) aplicado aos Tipos de Entidade no modelo de metadados (normalmente, *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="25c6a-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="25c6a-105">Esses filtros são aplicados automaticamente em qualquer consulta LINQ que envolva esses Tipos de Entidade, incluindo Tipos de Entidade referenciados indiretamente, como usando as referências de propriedade de navegação direta ou de Inclusão.</span><span class="sxs-lookup"><span data-stu-id="25c6a-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="25c6a-106">Alguns aplicativos comuns desse recurso são:</span><span class="sxs-lookup"><span data-stu-id="25c6a-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="25c6a-107">**Exclusão reversível** – um Tipo de Entidade define uma propriedade *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="25c6a-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="25c6a-108">**Multi-locação** - Um Tipo de Entidade define uma propriedade *TenantId.*</span><span class="sxs-lookup"><span data-stu-id="25c6a-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="25c6a-109">Exemplo</span><span class="sxs-lookup"><span data-stu-id="25c6a-109">Example</span></span>

<span data-ttu-id="25c6a-110">O exemplo a seguir mostra como usar filtros de consulta global para implementar a exclusão reversível e comportamentos de consulta de multilocação em um modelo simples de blogs.</span><span class="sxs-lookup"><span data-stu-id="25c6a-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="25c6a-111">Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="25c6a-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="25c6a-112">Primeiro, defina as entidades:</span><span class="sxs-lookup"><span data-stu-id="25c6a-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="25c6a-113">Anote a declaração de um campo _inquilino_ na entidade _blog._</span><span class="sxs-lookup"><span data-stu-id="25c6a-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="25c6a-114">Isso será usado para associar cada instância de Blog a um locatário específico.</span><span class="sxs-lookup"><span data-stu-id="25c6a-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="25c6a-115">Também definido é uma propriedade _IsDeleted_ no tipo de entidade _Post_.</span><span class="sxs-lookup"><span data-stu-id="25c6a-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="25c6a-116">Isso é usado para controlar se uma instância _Post_ foi "excluída de maneira reversível".</span><span class="sxs-lookup"><span data-stu-id="25c6a-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="25c6a-117">Ou seja, a instância está marcada como excluída sem remover fisicamente os dados subjacentes.</span><span class="sxs-lookup"><span data-stu-id="25c6a-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="25c6a-118">Em seguida, configure os filtros de consulta no _OnModelCreating_ usando a API `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="25c6a-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="25c6a-119">As expressões de predicado passadas para as chamadas _HasQueryFilter_ agora serão automaticamente aplicadas a quaisquer consultas LINQ para esses tipos.</span><span class="sxs-lookup"><span data-stu-id="25c6a-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="25c6a-120">Observe o uso de um campo de nível de instância de DbContext: `_tenantId` usado para definir o locatário atual.</span><span class="sxs-lookup"><span data-stu-id="25c6a-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="25c6a-121">Os filtros de nível de modelo usarão o valor da instância de contexto correta (ou seja, a instância que está executando a consulta).</span><span class="sxs-lookup"><span data-stu-id="25c6a-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="25c6a-122">Atualmente, não é possível definir vários filtros de consulta na mesma entidade - apenas o último será aplicado.</span><span class="sxs-lookup"><span data-stu-id="25c6a-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="25c6a-123">No entanto, você pode definir um único filtro com múltiplas condições usando o operador lógico _E_ [ `&&` (em C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span><span class="sxs-lookup"><span data-stu-id="25c6a-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="25c6a-124">Como desabilitar filtros</span><span class="sxs-lookup"><span data-stu-id="25c6a-124">Disabling Filters</span></span>

<span data-ttu-id="25c6a-125">Os filtros podem ser desabilitados para consultas LINQ individuais usando o operador `IgnoreQueryFilters()`.</span><span class="sxs-lookup"><span data-stu-id="25c6a-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="25c6a-126">Limitações</span><span class="sxs-lookup"><span data-stu-id="25c6a-126">Limitations</span></span>

<span data-ttu-id="25c6a-127">Os filtros de consulta global têm as seguintes limitações:</span><span class="sxs-lookup"><span data-stu-id="25c6a-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="25c6a-128">Os filtros podem ser definidos somente para o Tipo de Entidade raiz de uma hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="25c6a-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
