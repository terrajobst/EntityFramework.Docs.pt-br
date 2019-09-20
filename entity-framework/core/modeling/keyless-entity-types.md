---
title: Tipos de entidade de subunidade-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: b968ac9602b9aa1f1c1e3181b6b76a64394d70f0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150839"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="15d21-102">Tipos de entidade de subunidade</span><span class="sxs-lookup"><span data-stu-id="15d21-102">Keyless Entity Types</span></span>
> [!NOTE]
> <span data-ttu-id="15d21-103">Este recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="15d21-103">This feature is new in EF Core 2.1.</span></span> <span data-ttu-id="15d21-104">Antes de 3,0 eles eram conhecidos como tipos de consulta</span><span class="sxs-lookup"><span data-stu-id="15d21-104">Prior to 3.0 they were known as Query Types</span></span>

<span data-ttu-id="15d21-105">Além dos tipos de entidade regulares, um modelo de EF Core pode conter _tipos de entidade sem_chave, que podem ser usados para realizar consultas de banco de dados em relação a data que não contém valores de chaves.</span><span class="sxs-lookup"><span data-stu-id="15d21-105">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="15d21-106">Características de tipos de entidade de subtipo</span><span class="sxs-lookup"><span data-stu-id="15d21-106">Keyless entity types characteristics</span></span>

<span data-ttu-id="15d21-107">Os tipos de entidade keyless dão suporte a muitos dos mesmos recursos de mapeamento que os tipos de entidade regular, como o mapeamento de herança e as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="15d21-107">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="15d21-108">Em repositórios relacionais, eles podem configurar as colunas por meio de métodos da API fluentes ou anotações de dados e objetos de banco de dados de destino.</span><span class="sxs-lookup"><span data-stu-id="15d21-108">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="15d21-109">No entanto, eles são diferentes dos tipos de entidade regulares, pois:</span><span class="sxs-lookup"><span data-stu-id="15d21-109">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="15d21-110">Não é possível definir uma chave.</span><span class="sxs-lookup"><span data-stu-id="15d21-110">Cannot have a key defined.</span></span>
- <span data-ttu-id="15d21-111">Nunca são rastreadas para alterações no _DbContext_ e, portanto, nunca são inseridas, atualizadas ou excluídas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15d21-111">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="15d21-112">Nunca são descobertos pela convenção.</span><span class="sxs-lookup"><span data-stu-id="15d21-112">Are never discovered by convention.</span></span>
- <span data-ttu-id="15d21-113">Dá suporte apenas a um subconjunto de recursos de mapeamento de navegação, especificamente:</span><span class="sxs-lookup"><span data-stu-id="15d21-113">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="15d21-114">Eles nunca podem agir como o final principal de uma relação.</span><span class="sxs-lookup"><span data-stu-id="15d21-114">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="15d21-115">Eles podem não ter navegações para entidades pertencentes</span><span class="sxs-lookup"><span data-stu-id="15d21-115">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="15d21-116">Eles só podem conter Propriedades de navegação de referência apontando para entidades regulares.</span><span class="sxs-lookup"><span data-stu-id="15d21-116">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="15d21-117">As entidades não podem conter Propriedades de navegação para tipos de entidade sem nenhum.</span><span class="sxs-lookup"><span data-stu-id="15d21-117">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="15d21-118">Precisa ser configurado com a `.HasNoKey()` chamada de método.</span><span class="sxs-lookup"><span data-stu-id="15d21-118">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="15d21-119">Pode ser mapeado para uma _consulta de definição_.</span><span class="sxs-lookup"><span data-stu-id="15d21-119">May be mapped to a _defining query_.</span></span> <span data-ttu-id="15d21-120">Uma consulta de definição é uma consulta declarada no modelo que atua como uma fonte de dados para um tipo de entidade sem modelo.</span><span class="sxs-lookup"><span data-stu-id="15d21-120">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="15d21-121">Cenários de uso</span><span class="sxs-lookup"><span data-stu-id="15d21-121">Usage scenarios</span></span>

<span data-ttu-id="15d21-122">Alguns dos principais cenários de uso para tipos de entidade de tipo de subunidade são:</span><span class="sxs-lookup"><span data-stu-id="15d21-122">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="15d21-123">Servindo como o tipo de retorno para [consultas SQL brutas](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="15d21-123">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="15d21-124">Mapeamento para exibições de banco de dados que não contêm uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="15d21-124">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="15d21-125">Mapeamento para tabelas que não têm uma chave primária definida.</span><span class="sxs-lookup"><span data-stu-id="15d21-125">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="15d21-126">Mapeamento para consultas definidas no modelo.</span><span class="sxs-lookup"><span data-stu-id="15d21-126">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="15d21-127">Mapeando para objetos de banco de dados</span><span class="sxs-lookup"><span data-stu-id="15d21-127">Mapping to database objects</span></span>

<span data-ttu-id="15d21-128">O mapeamento de um tipo de entidade sem um objeto de banco de dados `ToTable` é `ToView` obtido usando o ou a API fluente.</span><span class="sxs-lookup"><span data-stu-id="15d21-128">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="15d21-129">Da perspectiva do EF Core, o objeto de banco de dados especificado neste método é um _exibição_, que significa que ela é tratada como uma fonte de consulta somente leitura e não pode ser o destino da atualização, inserir ou excluir operações.</span><span class="sxs-lookup"><span data-stu-id="15d21-129">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="15d21-130">No entanto, isso não significa que o objeto de banco de dados é realmente necessário para ser uma exibição de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15d21-130">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="15d21-131">Como alternativa, ele pode ser uma tabela de banco de dados que será tratada como somente leitura.</span><span class="sxs-lookup"><span data-stu-id="15d21-131">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="15d21-132">Por outro lado, para tipos de entidade regulares, EF Core pressupõe que um objeto de banco de `ToTable` dados especificado no método possa ser tratado como uma _tabela_, o que significa que ele pode ser usado como uma fonte de consulta, mas também direcionado por operações de atualização, exclusão e inserção.</span><span class="sxs-lookup"><span data-stu-id="15d21-132">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="15d21-133">Na verdade, você pode especificar o nome de uma exibição de banco de dados em `ToTable` e tudo deve funcionar bem, desde que o modo de exibição está configurado para ser atualizável no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15d21-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="15d21-134">`ToView`supõe que o objeto já existe no banco de dados e não será criado por migrações.</span><span class="sxs-lookup"><span data-stu-id="15d21-134">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="15d21-135">Exemplo</span><span class="sxs-lookup"><span data-stu-id="15d21-135">Example</span></span>

<span data-ttu-id="15d21-136">O exemplo a seguir mostra como usar tipos de entidade para consultar uma exibição de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15d21-136">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="15d21-137">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="15d21-137">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="15d21-138">Primeiro, definimos um modelo simples de Blog e Post:</span><span class="sxs-lookup"><span data-stu-id="15d21-138">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="15d21-139">Em seguida, definimos uma exibição de banco de dados simples que nos permitirá consultar o número de postagens associada a cada blog:</span><span class="sxs-lookup"><span data-stu-id="15d21-139">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="15d21-140">Em seguida, definimos uma classe para manter o resultado da exibição do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="15d21-140">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="15d21-141">Em seguida, configuramos o tipo de entidade keyless em `HasNoKey` OnModelCreating usando a API.</span><span class="sxs-lookup"><span data-stu-id="15d21-141">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="15d21-142">Usamos a API de configuração fluente para configurar o mapeamento para o tipo de entidade de automenos:</span><span class="sxs-lookup"><span data-stu-id="15d21-142">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="15d21-143">Em seguida, configuramos o `DbContext` para `DbSet<T>`incluir:</span><span class="sxs-lookup"><span data-stu-id="15d21-143">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="15d21-144">Por fim, é possível consultar a exibição de banco de dados da maneira padrão:</span><span class="sxs-lookup"><span data-stu-id="15d21-144">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="15d21-145">Observe que também definimos uma propriedade de consulta de nível de contexto (DbSet) para atuar como uma raiz para consultas nesse tipo.</span><span class="sxs-lookup"><span data-stu-id="15d21-145">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
