---
title: Tipos de consulta – EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 89f5be356654dc02e353441a83e34c90fc727593
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900298"
---
# <a name="query-types"></a><span data-ttu-id="24708-102">Tipos de consulta</span><span class="sxs-lookup"><span data-stu-id="24708-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="24708-103">Este recurso é novo no EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="24708-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="24708-104">Além dos tipos de entidade, um modelo do EF Core pode conter _tipos de consulta_, que pode ser usado para executar consultas de banco de dados em relação aos dados que não são mapeados para tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="24708-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="24708-105">Tipos de consulta têm muitas semelhanças com tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="24708-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="24708-106">Eles também podem ser adicionados ao modelo ou no `OnModelCreating`, ou por meio de uma propriedade "set" em um derivada _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="24708-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="24708-107">Eles oferecem suporte a vários dos mesmos recursos de mapeamento, como herança de mapeamento, propriedades de navegação (consulte limitações abaixo) e, em repositórios relacionais, a capacidade de configurar as colunas por meio de métodos da API fluentes ou anotações de dados e objetos de banco de dados de destino.</span><span class="sxs-lookup"><span data-stu-id="24708-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="24708-108">No entanto forem diferentes da entidade tipos em que eles:</span><span class="sxs-lookup"><span data-stu-id="24708-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="24708-109">Não exigem uma chave a ser definido.</span><span class="sxs-lookup"><span data-stu-id="24708-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="24708-110">Nunca são rastreados para alterações na _DbContext_ e, portanto, são nunca inseridos, atualizados ou excluídos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="24708-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="24708-111">Nunca são descobertos pela convenção.</span><span class="sxs-lookup"><span data-stu-id="24708-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="24708-112">Somente suporta um subconjunto de recursos de mapeamento de navegação - especificamente:</span><span class="sxs-lookup"><span data-stu-id="24708-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="24708-113">Eles nunca podem agir como o final principal de uma relação.</span><span class="sxs-lookup"><span data-stu-id="24708-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="24708-114">Eles só podem conter propriedades de navegação de referência que aponta para entidades.</span><span class="sxs-lookup"><span data-stu-id="24708-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="24708-115">Entidades não podem conter propriedades de navegação para tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="24708-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="24708-116">Sejam resolvidos do _ModelBuilder_ usando o `Query` método em vez de `Entity` método.</span><span class="sxs-lookup"><span data-stu-id="24708-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="24708-117">São mapeadas na _DbContext_ por meio das propriedades do tipo `DbQuery<T>` em vez de `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="24708-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="24708-118">São mapeadas para objetos de banco de dados usando o `ToView` método, em vez de `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="24708-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="24708-119">Pode ser mapeado para um _definição de consulta_ - uma definição de consulta é uma consulta secundária declarada no modelo que funciona de uma fonte de dados para um tipo de consulta.</span><span class="sxs-lookup"><span data-stu-id="24708-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="24708-120">Estes são alguns dos cenários de uso principal para tipos de consulta:</span><span class="sxs-lookup"><span data-stu-id="24708-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="24708-121">Servindo como o tipo de retorno para o ad-hoc `FromSql()` consultas.</span><span class="sxs-lookup"><span data-stu-id="24708-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="24708-122">Mapeando para exibições de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="24708-122">Mapping to database views.</span></span>
- <span data-ttu-id="24708-123">Mapeamento para tabelas que não têm uma chave primária definida.</span><span class="sxs-lookup"><span data-stu-id="24708-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="24708-124">Mapeamento para consultas definidas no modelo.</span><span class="sxs-lookup"><span data-stu-id="24708-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="24708-125">Mapeando um tipo de consulta para um objeto de banco de dados é feito usando o `ToView` API fluente.</span><span class="sxs-lookup"><span data-stu-id="24708-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="24708-126">Da perspectiva do EF Core, o objeto de banco de dados especificado neste método é um _exibição_, que significa que ela é tratada como uma fonte de consulta somente leitura e não pode ser o destino da atualização, inserir ou excluir operações.</span><span class="sxs-lookup"><span data-stu-id="24708-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="24708-127">No entanto, isso não significa que o objeto de banco de dados é realmente necessário para ser uma exibição de banco de dados – como alternativa, ele pode ser uma tabela de banco de dados que será tratada como somente leitura.</span><span class="sxs-lookup"><span data-stu-id="24708-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="24708-128">Por outro lado, para tipos de entidade, o EF Core supõe que um objeto de banco de dados especificado na `ToTable` método pode ser tratado como um _tabela_, o que significa que ele pode ser usado como uma fonte de consulta, mas também é direcionado pela atualização, excluir e inserir operações.</span><span class="sxs-lookup"><span data-stu-id="24708-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="24708-129">Na verdade, você pode especificar o nome de uma exibição de banco de dados em `ToTable` e tudo deve funcionar bem, desde que o modo de exibição está configurado para ser atualizável no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="24708-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="24708-130">Exemplo</span><span class="sxs-lookup"><span data-stu-id="24708-130">Example</span></span>

<span data-ttu-id="24708-131">O exemplo a seguir mostra como usar o tipo de consulta para consultar uma exibição de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="24708-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="24708-132">Veja o [exemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="24708-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="24708-133">Primeiro, definimos um modelo simples de Blog e Post:</span><span class="sxs-lookup"><span data-stu-id="24708-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="24708-134">Em seguida, definimos uma exibição de banco de dados simples que nos permitirá consultar o número de postagens associada a cada blog:</span><span class="sxs-lookup"><span data-stu-id="24708-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="24708-135">Em seguida, definimos uma classe para manter o resultado da exibição do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="24708-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="24708-136">Em seguida, configuramos o tipo de consulta no _OnModelCreating_ usando o `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="24708-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="24708-137">Podemos usar APIs padrão de configuração fluente para configurar o mapeamento para o tipo de consulta:</span><span class="sxs-lookup"><span data-stu-id="24708-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="24708-138">Por fim, é possível consultar a exibição de banco de dados da maneira padrão:</span><span class="sxs-lookup"><span data-stu-id="24708-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="24708-139">Observe que também definimos uma propriedade de consulta de nível de contexto (DbQuery) para atuar como uma raiz para consultas em relação a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="24708-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
