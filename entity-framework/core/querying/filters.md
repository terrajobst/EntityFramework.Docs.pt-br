---
title: Filtros de consulta global – EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053896"
---
# <a name="global-query-filters"></a><span data-ttu-id="a5970-102">Filtros de consulta global</span><span class="sxs-lookup"><span data-stu-id="a5970-102">Global Query Filters</span></span>

<span data-ttu-id="a5970-103">Os filtros de consulta global são predicados de consulta LINQ (uma expressão booliana normalmente passada ao operador de consulta LINQ *Where*) aplicado aos Tipos de Entidade no modelo de metadados (normalmente, *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="a5970-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="a5970-104">Esses filtros são aplicados automaticamente em qualquer consulta LINQ que envolva esses Tipos de Entidade, incluindo Tipos de Entidade referenciados indiretamente, como usando as referências de propriedade de navegação direta ou de Inclusão.</span><span class="sxs-lookup"><span data-stu-id="a5970-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="a5970-105">Alguns aplicativos comuns desse recurso são:</span><span class="sxs-lookup"><span data-stu-id="a5970-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="a5970-106">**Exclusão reversível** – um Tipo de Entidade define uma propriedade *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="a5970-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="a5970-107">**Multilocação** – um Tipo de Entidade define uma propriedade *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="a5970-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="a5970-108">Exemplo</span><span class="sxs-lookup"><span data-stu-id="a5970-108">Example</span></span>

<span data-ttu-id="a5970-109">O exemplo a seguir mostra como usar filtros de consulta global para implementar a exclusão reversível e comportamentos de consulta de multilocação em um modelo simples de blogs.</span><span class="sxs-lookup"><span data-stu-id="a5970-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="a5970-110">Veja o [exemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="a5970-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="a5970-111">Primeiro, defina as entidades:</span><span class="sxs-lookup"><span data-stu-id="a5970-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="a5970-112">Observe a declaração de um campo __tenantId_ na entidade _Blog_.</span><span class="sxs-lookup"><span data-stu-id="a5970-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="a5970-113">Isso será usado para associar cada instância de Blog a um locatário específico.</span><span class="sxs-lookup"><span data-stu-id="a5970-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="a5970-114">Também definido é uma propriedade _IsDeleted_ no tipo de entidade _Post_.</span><span class="sxs-lookup"><span data-stu-id="a5970-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="a5970-115">Isso é usado para controlar se uma instância _Post_ foi "excluída de maneira reversível".</span><span class="sxs-lookup"><span data-stu-id="a5970-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="a5970-116">Ou seja, A instância está marcada como excluída sem remover fisicamente os dados subjacentes.</span><span class="sxs-lookup"><span data-stu-id="a5970-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="a5970-117">Em seguida, configure os filtros de consulta no _OnModelCreating_ usando a API ```HasQueryFilter```.</span><span class="sxs-lookup"><span data-stu-id="a5970-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="a5970-118">As expressões de predicado passadas para as chamadas _HasQueryFilter_ agora serão automaticamente aplicadas a quaisquer consultas LINQ para esses tipos.</span><span class="sxs-lookup"><span data-stu-id="a5970-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="a5970-119">Observe o uso de um campo de nível de instância de DbContext: ```_tenantId``` usado para definir o locatário atual.</span><span class="sxs-lookup"><span data-stu-id="a5970-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="a5970-120">Filtros no nível de modelo usarão o valor da instância do contexto correto.</span><span class="sxs-lookup"><span data-stu-id="a5970-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="a5970-121">Ou seja, A instância que está executando a consulta.</span><span class="sxs-lookup"><span data-stu-id="a5970-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="a5970-122">Como desabilitar filtros</span><span class="sxs-lookup"><span data-stu-id="a5970-122">Disabling Filters</span></span>

<span data-ttu-id="a5970-123">Os filtros podem ser desabilitados para consultas LINQ individuais usando o operador ```IgnoreQueryFilters()```.</span><span class="sxs-lookup"><span data-stu-id="a5970-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="a5970-124">Limitações</span><span class="sxs-lookup"><span data-stu-id="a5970-124">Limitations</span></span>

<span data-ttu-id="a5970-125">Os filtros de consulta global têm as seguintes limitações:</span><span class="sxs-lookup"><span data-stu-id="a5970-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="a5970-126">Os filtros não contêm referências a propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="a5970-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="a5970-127">Os filtros podem ser definidos somente para o Tipo de Entidade raiz de uma hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="a5970-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
