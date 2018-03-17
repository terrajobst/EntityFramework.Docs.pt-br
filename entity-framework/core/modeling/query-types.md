---
title: Tipos de consulta - Core EF
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a><span data-ttu-id="718b6-102">Tipos de consulta</span><span class="sxs-lookup"><span data-stu-id="718b6-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="718b6-103">Esse recurso é novo no EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="718b6-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="718b6-104">Tipos de consulta são tipos de resultados de consulta somente leitura que podem ser adicionados ao modelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="718b6-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="718b6-105">Tipos de consulta habilitar consultas ad hoc (como tipos anônimos), mas são mais flexíveis porque eles podem ter a configuração de mapeamento especificada.</span><span class="sxs-lookup"><span data-stu-id="718b6-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="718b6-106">Eles são conceitualmente semelhantes aos tipos de entidade em que:</span><span class="sxs-lookup"><span data-stu-id="718b6-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="718b6-107">Eles são POCO tipos c# que são adicionados ao modelo, no ```OnModelCreating``` usando o ```ModelBuilder.Query``` método, ou por meio de uma propriedade DbContext "set" (para tipos de consulta essa propriedade é digitada como ```DbQuery<T>``` em vez de ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="718b6-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather than ```DbSet<T>```).</span></span>
- <span data-ttu-id="718b6-108">Eles oferecem suporte muito as mesmas capacidades de mapeamento de tipos de entidade normal.</span><span class="sxs-lookup"><span data-stu-id="718b6-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="718b6-109">Por exemplo, o mapeamento de herança, navegações (consulte limitiations abaixo) e, em repositórios relacionais, a capacidade de configurar os objetos de esquema de banco de dados de destino via ```ToTable```, ```HasColumn``` api fluente métodos (ou as anotações de dados).</span><span class="sxs-lookup"><span data-stu-id="718b6-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="718b6-110">Tipos de consulta são diferentes da entidade tipos em que eles:</span><span class="sxs-lookup"><span data-stu-id="718b6-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="718b6-111">Não exigem uma chave a ser definido.</span><span class="sxs-lookup"><span data-stu-id="718b6-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="718b6-112">Nunca são rastreadas pelo rastreador de alteração.</span><span class="sxs-lookup"><span data-stu-id="718b6-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="718b6-113">Nunca são descobertos por convenção.</span><span class="sxs-lookup"><span data-stu-id="718b6-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="718b6-114">Suporte apenas a um subconjunto de recursos de mapeamento de navegação - especificamente, eles nunca podem agir como a extremidade principal de uma relação.</span><span class="sxs-lookup"><span data-stu-id="718b6-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="718b6-115">Pode ser mapeado para um _definir consulta_ -A definição de consulta é uma consulta secundária que atua de uma fonte de dados para um tipo de consulta.</span><span class="sxs-lookup"><span data-stu-id="718b6-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="718b6-116">Estes são alguns dos cenários de uso principal para tipos de consulta:</span><span class="sxs-lookup"><span data-stu-id="718b6-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="718b6-117">Mapeando para exibições de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="718b6-117">Mapping to database views.</span></span>
- <span data-ttu-id="718b6-118">Mapeando para tabelas que não têm uma chave primária definida.</span><span class="sxs-lookup"><span data-stu-id="718b6-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="718b6-119">Serve como o tipo de retorno para ad-hoc ```FromSql()``` consultas.</span><span class="sxs-lookup"><span data-stu-id="718b6-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="718b6-120">Mapeamento para consultas definidas no modelo.</span><span class="sxs-lookup"><span data-stu-id="718b6-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="718b6-121">Mapeamento de um tipo de consulta para um modo de exibição de banco de dados é obtido usando o ```ToTable``` API fluente.</span><span class="sxs-lookup"><span data-stu-id="718b6-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="718b6-122">Exemplo</span><span class="sxs-lookup"><span data-stu-id="718b6-122">Example</span></span>

<span data-ttu-id="718b6-123">O exemplo a seguir mostra como usar o tipo de consulta para consultar uma exibição de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="718b6-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="718b6-124">Veja o [exemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="718b6-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="718b6-125">Primeiro, definimos um modelo simples de Blog e Post:</span><span class="sxs-lookup"><span data-stu-id="718b6-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="718b6-126">Em seguida, definimos uma exibição de banco de dados simples que permitirá que o número de postagens associado com cada blog de consulta:</span><span class="sxs-lookup"><span data-stu-id="718b6-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="718b6-127">Em seguida, definimos uma classe para armazenar o resultado da exibição do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="718b6-127">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="718b6-128">Em seguida, podemos configurar o tipo de consulta em _OnModelCreating_ usando o ```modelBuilder.Query<T>``` API.</span><span class="sxs-lookup"><span data-stu-id="718b6-128">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="718b6-129">Usamos APIs de configuração fluente padrão para configurar o mapeamento para o tipo de consulta:</span><span class="sxs-lookup"><span data-stu-id="718b6-129">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="718b6-130">Por fim, é possível consultar a exibição de banco de dados da maneira padrão:</span><span class="sxs-lookup"><span data-stu-id="718b6-130">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="718b6-131">Observe que também, definimos uma propriedade de nível de consulta de contexto (DbQuery) para atuar como uma raiz para consultas em relação a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="718b6-131">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
