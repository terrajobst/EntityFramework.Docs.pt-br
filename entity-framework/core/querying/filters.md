---
title: Filtros de consulta global – EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 73efe62262cf45cc1841d7a86cf59249cf07c5ea
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996659"
---
# <a name="global-query-filters"></a><span data-ttu-id="b2a8b-102">Filtros de consulta global</span><span class="sxs-lookup"><span data-stu-id="b2a8b-102">Global Query Filters</span></span>

<span data-ttu-id="b2a8b-103">Os filtros de consulta global são predicados de consulta LINQ (uma expressão booliana normalmente passada ao operador de consulta LINQ *Where*) aplicado aos Tipos de Entidade no modelo de metadados (normalmente, *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="b2a8b-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="b2a8b-104">Esses filtros são aplicados automaticamente em qualquer consulta LINQ que envolva esses Tipos de Entidade, incluindo Tipos de Entidade referenciados indiretamente, como usando as referências de propriedade de navegação direta ou de Inclusão.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="b2a8b-105">Alguns aplicativos comuns desse recurso são:</span><span class="sxs-lookup"><span data-stu-id="b2a8b-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="b2a8b-106">**Exclusão reversível** – um Tipo de Entidade define uma propriedade *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="b2a8b-107">**Multilocação** – um Tipo de Entidade define uma propriedade *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="b2a8b-108">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b2a8b-108">Example</span></span>

<span data-ttu-id="b2a8b-109">O exemplo a seguir mostra como usar filtros de consulta global para implementar a exclusão reversível e comportamentos de consulta de multilocação em um modelo simples de blogs.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="b2a8b-110">Veja o [exemplo](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="b2a8b-111">Primeiro, defina as entidades:</span><span class="sxs-lookup"><span data-stu-id="b2a8b-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="b2a8b-112">Observe a declaração de um campo __tenantId_ na entidade _Blog_.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="b2a8b-113">Isso será usado para associar cada instância de Blog a um locatário específico.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="b2a8b-114">Também definido é uma propriedade _IsDeleted_ no tipo de entidade _Post_.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="b2a8b-115">Isso é usado para controlar se uma instância _Post_ foi "excluída de maneira reversível".</span><span class="sxs-lookup"><span data-stu-id="b2a8b-115">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="b2a8b-116">Ou seja, a instância está marcada como excluída sem remover fisicamente os dados subjacentes.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-116">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="b2a8b-117">Em seguida, configure os filtros de consulta no _OnModelCreating_ usando a API ```HasQueryFilter```.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="b2a8b-118">As expressões de predicado passadas para as chamadas _HasQueryFilter_ agora serão automaticamente aplicadas a quaisquer consultas LINQ para esses tipos.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="b2a8b-119">Observe o uso de um campo de nível de instância de DbContext: ```_tenantId``` usado para definir o locatário atual.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="b2a8b-120">Os filtros de nível de modelo usarão o valor da instância de contexto correta (ou seja, a instância que está executando a consulta).</span><span class="sxs-lookup"><span data-stu-id="b2a8b-120">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="b2a8b-121">Como desabilitar filtros</span><span class="sxs-lookup"><span data-stu-id="b2a8b-121">Disabling Filters</span></span>

<span data-ttu-id="b2a8b-122">Os filtros podem ser desabilitados para consultas LINQ individuais usando o operador ```IgnoreQueryFilters()```.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-122">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="b2a8b-123">Limitações</span><span class="sxs-lookup"><span data-stu-id="b2a8b-123">Limitations</span></span>

<span data-ttu-id="b2a8b-124">Os filtros de consulta global têm as seguintes limitações:</span><span class="sxs-lookup"><span data-stu-id="b2a8b-124">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="b2a8b-125">Os filtros não contêm referências a propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-125">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="b2a8b-126">Os filtros podem ser definidos somente para o Tipo de Entidade raiz de uma hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="b2a8b-126">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
