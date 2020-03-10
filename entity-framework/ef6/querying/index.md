---
title: Consultando e Localizando entidades – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412971"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="a55aa-102">Consultando e Localizando entidades</span><span class="sxs-lookup"><span data-stu-id="a55aa-102">Querying and Finding Entities</span></span>
<span data-ttu-id="a55aa-103">Este tópico aborda as várias maneiras que você pode consultar dados usando o Entity Framework, incluindo o LINQ e o método Find.</span><span class="sxs-lookup"><span data-stu-id="a55aa-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="a55aa-104">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="a55aa-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="a55aa-105">Localizando entidades usando uma consulta</span><span class="sxs-lookup"><span data-stu-id="a55aa-105">Finding entities using a query</span></span>  

<span data-ttu-id="a55aa-106">O DbSet e o IDbSet implementam o IQueryable e, portanto, podem ser usados como o ponto de partida para escrever uma consulta LINQ no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a55aa-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="a55aa-107">Este não é o local apropriado para uma discussão aprofundada do LINQ, mas aqui estão alguns exemplos simples:</span><span class="sxs-lookup"><span data-stu-id="a55aa-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

<span data-ttu-id="a55aa-108">Observe que o DbSet e o IDbSet sempre criam consultas no banco de dados e sempre envolverão uma viagem de ida e volta para o banco de dados, mesmo se as entidades retornadas já existirem no contexto.</span><span class="sxs-lookup"><span data-stu-id="a55aa-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="a55aa-109">Uma consulta é executada no banco de dados quando:</span><span class="sxs-lookup"><span data-stu-id="a55aa-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="a55aa-110">Ela é enumerada por uma instrução **foreach** (C#) ou **For Each** (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="a55aa-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="a55aa-111">Ela é enumerada por uma operação de coleção, como [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary) ou [ToList](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="a55aa-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="a55aa-112">Operadores LINQ, como [First](https://msdn.microsoft.com/library/bb291976) ou [Any](https://msdn.microsoft.com/library/bb337697) são especificados na parte mais externa da consulta.</span><span class="sxs-lookup"><span data-stu-id="a55aa-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="a55aa-113">Os seguintes métodos são chamados: o método de extensão [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) em um DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx) e Database.ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="a55aa-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="a55aa-114">Quando os resultados são retornados do banco de dados, objetos que não existem no contexto são anexados ao contexto.</span><span class="sxs-lookup"><span data-stu-id="a55aa-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="a55aa-115">Se um objeto já estiver no contexto, o objeto existente será retornado (os valores atuais e originais das propriedades do objeto na entrada **não** serão substituídos pelos valores do banco de dados).</span><span class="sxs-lookup"><span data-stu-id="a55aa-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="a55aa-116">Quando você executa uma consulta, as entidades que foram adicionadas ao contexto, mas ainda não foram salvas no banco de dados não são retornadas como parte do conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="a55aa-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="a55aa-117">Para obter os dados que estão no contexto, consulte [Dados locais](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="a55aa-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="a55aa-118">Se uma consulta não retornar nenhuma linha do banco de dados, o resultado será uma coleção vazia, em vez de **null**.</span><span class="sxs-lookup"><span data-stu-id="a55aa-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="a55aa-119">Localizando entidades usando chaves primárias</span><span class="sxs-lookup"><span data-stu-id="a55aa-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="a55aa-120">O método Find no DbSet usa o valor da chave primária para tentar localizar uma entidade rastreada pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="a55aa-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="a55aa-121">Se a entidade não for encontrada no contexto, uma consulta será enviada ao banco de dados para localizar a entidade lá.</span><span class="sxs-lookup"><span data-stu-id="a55aa-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="a55aa-122">Null será retornado se a entidade não for encontrada no contexto ou no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a55aa-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="a55aa-123">Find é diferente de usar uma consulta de duas maneiras significativas:</span><span class="sxs-lookup"><span data-stu-id="a55aa-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="a55aa-124">Uma viagem de ida e volta para o banco de dados será feita apenas se a entidade com a determinada chave não for localizada no contexto.</span><span class="sxs-lookup"><span data-stu-id="a55aa-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="a55aa-125">Find retornará entidades que estão com o estado Adicionada.</span><span class="sxs-lookup"><span data-stu-id="a55aa-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="a55aa-126">Isto é, Find retornará entidades que foram adicionadas ao contexto, mas ainda não foram salvas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a55aa-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="a55aa-127">Localizando uma entidade pela chave primária</span><span class="sxs-lookup"><span data-stu-id="a55aa-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="a55aa-128">O código a seguir mostra alguns usos de Find:</span><span class="sxs-lookup"><span data-stu-id="a55aa-128">The following code shows some uses of Find:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="a55aa-129">Localizando uma entidade pela chave primária composta</span><span class="sxs-lookup"><span data-stu-id="a55aa-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="a55aa-130">O Entity Framework permite que as entidades tenham chaves compostas, essa é uma chave composta por mais de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="a55aa-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="a55aa-131">Por exemplo, você pode ter uma entidade BlogSettings que representa as configurações do usuário para um blog específico.</span><span class="sxs-lookup"><span data-stu-id="a55aa-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="a55aa-132">Como um usuário teria apenas uma BlogSettings para cada blog, você poderia optar por tornar a chave primária BlogSettings em uma combinação de BlogId e Username.</span><span class="sxs-lookup"><span data-stu-id="a55aa-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="a55aa-133">O código a seguir tenta localizar a BlogSettings com BlogId = 3 e Username = "johndoe1987":</span><span class="sxs-lookup"><span data-stu-id="a55aa-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="a55aa-134">Observe que quando tiver chaves compostas, você precisará usar ColumnAttribute ou a API fluente para especificar uma ordem para as propriedades da chave composta.</span><span class="sxs-lookup"><span data-stu-id="a55aa-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="a55aa-135">A chamada para Find deve usar essa ordem quando especificando os valores que formam a chave.</span><span class="sxs-lookup"><span data-stu-id="a55aa-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
