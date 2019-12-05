---
title: Tipos de entidade de subunidade-EF Core
description: Como configurar tipos de entidade sem o uso de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 129e24b154ba32583435aeb742dbf478350344e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824666"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="515e4-103">Tipos de entidade sem chave</span><span class="sxs-lookup"><span data-stu-id="515e4-103">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="515e4-104">Esse recurso foi adicionado no EF Core 2,1 sob o nome dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="515e4-104">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="515e4-105">No EF Core 3,0, o conceito foi renomeado para tipos de entidade de subunidade.</span><span class="sxs-lookup"><span data-stu-id="515e4-105">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="515e4-106">Além dos tipos de entidade regulares, um modelo de EF Core pode conter _tipos de entidade sem_chave, que podem ser usados para realizar consultas de banco de dados em relação a data que não contém valores de chaves.</span><span class="sxs-lookup"><span data-stu-id="515e4-106">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="515e4-107">Características de tipos de entidade de subtipo</span><span class="sxs-lookup"><span data-stu-id="515e4-107">Keyless entity types characteristics</span></span>

<span data-ttu-id="515e4-108">Os tipos de entidade keyless dão suporte a muitos dos mesmos recursos de mapeamento que os tipos de entidade regular, como o mapeamento de herança e as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="515e4-108">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="515e4-109">Em repositórios relacionais, eles podem configurar as colunas por meio de métodos da API fluentes ou anotações de dados e objetos de banco de dados de destino.</span><span class="sxs-lookup"><span data-stu-id="515e4-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="515e4-110">No entanto, eles são diferentes dos tipos de entidade regulares, pois:</span><span class="sxs-lookup"><span data-stu-id="515e4-110">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="515e4-111">Não é possível definir uma chave.</span><span class="sxs-lookup"><span data-stu-id="515e4-111">Cannot have a key defined.</span></span>
- <span data-ttu-id="515e4-112">Nunca são rastreadas para alterações no _DbContext_ e, portanto, nunca são inseridas, atualizadas ou excluídas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="515e4-112">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="515e4-113">Nunca são descobertos pela convenção.</span><span class="sxs-lookup"><span data-stu-id="515e4-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="515e4-114">Dá suporte apenas a um subconjunto de recursos de mapeamento de navegação, especificamente:</span><span class="sxs-lookup"><span data-stu-id="515e4-114">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="515e4-115">Eles nunca podem agir como o final principal de uma relação.</span><span class="sxs-lookup"><span data-stu-id="515e4-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="515e4-116">Eles podem não ter navegações para entidades pertencentes</span><span class="sxs-lookup"><span data-stu-id="515e4-116">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="515e4-117">Eles só podem conter Propriedades de navegação de referência apontando para entidades regulares.</span><span class="sxs-lookup"><span data-stu-id="515e4-117">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="515e4-118">As entidades não podem conter Propriedades de navegação para tipos de entidade sem nenhum.</span><span class="sxs-lookup"><span data-stu-id="515e4-118">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="515e4-119">Precisa ser configurado com `.HasNoKey()` chamada de método.</span><span class="sxs-lookup"><span data-stu-id="515e4-119">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="515e4-120">Pode ser mapeado para uma _consulta de definição_.</span><span class="sxs-lookup"><span data-stu-id="515e4-120">May be mapped to a _defining query_.</span></span> <span data-ttu-id="515e4-121">Uma consulta de definição é uma consulta declarada no modelo que atua como uma fonte de dados para um tipo de entidade sem modelo.</span><span class="sxs-lookup"><span data-stu-id="515e4-121">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="515e4-122">Cenários de uso</span><span class="sxs-lookup"><span data-stu-id="515e4-122">Usage scenarios</span></span>

<span data-ttu-id="515e4-123">Alguns dos principais cenários de uso para tipos de entidade de tipo de subunidade são:</span><span class="sxs-lookup"><span data-stu-id="515e4-123">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="515e4-124">Servindo como o tipo de retorno para [consultas SQL brutas](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="515e4-124">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="515e4-125">Mapeamento para exibições de banco de dados que não contêm uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="515e4-125">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="515e4-126">Mapeamento para tabelas que não têm uma chave primária definida.</span><span class="sxs-lookup"><span data-stu-id="515e4-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="515e4-127">Mapeamento para consultas definidas no modelo.</span><span class="sxs-lookup"><span data-stu-id="515e4-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="515e4-128">Mapeando para objetos de banco de dados</span><span class="sxs-lookup"><span data-stu-id="515e4-128">Mapping to database objects</span></span>

<span data-ttu-id="515e4-129">O mapeamento de um tipo de entidade sem um objeto de banco de dados é obtido usando o `ToTable` ou `ToView` API fluente.</span><span class="sxs-lookup"><span data-stu-id="515e4-129">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="515e4-130">Da perspectiva do EF Core, o objeto de banco de dados especificado neste método é um _exibição_, que significa que ela é tratada como uma fonte de consulta somente leitura e não pode ser o destino da atualização, inserir ou excluir operações.</span><span class="sxs-lookup"><span data-stu-id="515e4-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="515e4-131">No entanto, isso não significa que o objeto de banco de dados é realmente necessário para ser uma exibição de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="515e4-131">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="515e4-132">Como alternativa, ele pode ser uma tabela de banco de dados que será tratada como somente leitura.</span><span class="sxs-lookup"><span data-stu-id="515e4-132">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="515e4-133">Por outro lado, para tipos de entidade regulares, EF Core pressupõe que um objeto de banco de dados especificado no método `ToTable` pode ser tratado como uma _tabela_, o que significa que ele pode ser usado como uma fonte de consulta, mas também direcionado pelas operações Update, DELETE e INSERT.</span><span class="sxs-lookup"><span data-stu-id="515e4-133">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="515e4-134">Na verdade, você pode especificar o nome de uma exibição de banco de dados em `ToTable` e tudo deve funcionar bem, desde que o modo de exibição está configurado para ser atualizável no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="515e4-134">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="515e4-135">`ToView` pressupõe que o objeto já existe no banco de dados e não será criado por migrações.</span><span class="sxs-lookup"><span data-stu-id="515e4-135">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="515e4-136">Exemplo</span><span class="sxs-lookup"><span data-stu-id="515e4-136">Example</span></span>

<span data-ttu-id="515e4-137">O exemplo a seguir mostra como usar tipos de entidade para consultar uma exibição de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="515e4-137">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="515e4-138">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="515e4-138">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="515e4-139">Primeiro, definimos um modelo simples de Blog e Post:</span><span class="sxs-lookup"><span data-stu-id="515e4-139">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="515e4-140">Em seguida, definimos uma exibição de banco de dados simples que nos permitirá consultar o número de postagens associada a cada blog:</span><span class="sxs-lookup"><span data-stu-id="515e4-140">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="515e4-141">Em seguida, definimos uma classe para manter o resultado da exibição do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="515e4-141">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="515e4-142">Em seguida, configuramos o tipo de entidade keyless em _OnModelCreating_ usando a API `HasNoKey`.</span><span class="sxs-lookup"><span data-stu-id="515e4-142">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="515e4-143">Usamos a API de configuração fluente para configurar o mapeamento para o tipo de entidade de automenos:</span><span class="sxs-lookup"><span data-stu-id="515e4-143">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="515e4-144">Em seguida, configuramos o `DbContext` para incluir o `DbSet<T>`:</span><span class="sxs-lookup"><span data-stu-id="515e4-144">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="515e4-145">Por fim, é possível consultar a exibição de banco de dados da maneira padrão:</span><span class="sxs-lookup"><span data-stu-id="515e4-145">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="515e4-146">Observe que também definimos uma propriedade de consulta de nível de contexto (DbSet) para atuar como uma raiz para consultas nesse tipo.</span><span class="sxs-lookup"><span data-stu-id="515e4-146">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
