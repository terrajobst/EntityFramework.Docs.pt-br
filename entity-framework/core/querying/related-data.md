---
title: Carregamento de dados - Core EF relacionados ao
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a><span data-ttu-id="f451d-102">Carregamento de dados relacionados</span><span class="sxs-lookup"><span data-stu-id="f451d-102">Loading Related Data</span></span>

<span data-ttu-id="f451d-103">Entity Framework Core permite que você use as propriedades de navegação em seu modelo para carregar as entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="f451d-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="f451d-104">Há três padrões de S/RM comuns usados para carregar os dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="f451d-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="f451d-105">**Carregamento adiantado** significa que os dados relacionados são carregados do banco de dados como parte da consulta inicial.</span><span class="sxs-lookup"><span data-stu-id="f451d-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="f451d-106">**Carregamento explícito** significa que os dados relacionados explicitamente carregados do banco de dados em um momento posterior.</span><span class="sxs-lookup"><span data-stu-id="f451d-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="f451d-107">**Carregamento preguiçoso** significa que os dados relacionados são carregados transparente do banco de dados quando a propriedade de navegação é acessada.</span><span class="sxs-lookup"><span data-stu-id="f451d-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="f451d-108">Carregamento preguiçoso ainda não é possível com o EF Core.</span><span class="sxs-lookup"><span data-stu-id="f451d-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="f451d-109">Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="f451d-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="f451d-110">Carregamento adiantado</span><span class="sxs-lookup"><span data-stu-id="f451d-110">Eager loading</span></span>

<span data-ttu-id="f451d-111">Você pode usar o `Include` método para especificar dados relacionados a serem incluídos nos resultados da consulta.</span><span class="sxs-lookup"><span data-stu-id="f451d-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="f451d-112">No exemplo a seguir, os blogs que são retornados nos resultados terão seus `Posts` propriedade preenchida com as postagens relacionadas.</span><span class="sxs-lookup"><span data-stu-id="f451d-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="f451d-113">Entity Framework Core será corrigir automaticamente o propriedades de navegação para outras entidades que foram previamente carregadas para a instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="f451d-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="f451d-114">Dessa forma, mesmo se você não incluir explicitamente os dados para uma propriedade de navegação, a propriedade ainda pode ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f451d-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="f451d-115">Você pode incluir dados relacionados de várias relações em uma única consulta.</span><span class="sxs-lookup"><span data-stu-id="f451d-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="f451d-116">Incluindo vários níveis</span><span class="sxs-lookup"><span data-stu-id="f451d-116">Including multiple levels</span></span>

<span data-ttu-id="f451d-117">Você pode fazer uma busca detalhada por meio de relações para incluir vários níveis de dados relacionados usando o `ThenInclude` método.</span><span class="sxs-lookup"><span data-stu-id="f451d-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="f451d-118">O exemplo a seguir carrega todos os blogs, suas postagens relacionadas e autor de cada postagem.</span><span class="sxs-lookup"><span data-stu-id="f451d-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="f451d-119">É possível encadear chamadas múltiplas para `ThenInclude` para continuar incluindo mais níveis de dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="f451d-119">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="f451d-120">Você pode combinar tudo isso para incluir dados relacionados de vários níveis e várias raízes na mesma consulta.</span><span class="sxs-lookup"><span data-stu-id="f451d-120">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="f451d-121">Você talvez queira incluir várias entidades relacionadas para uma das entidades que está sendo incluída.</span><span class="sxs-lookup"><span data-stu-id="f451d-121">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="f451d-122">Por exemplo, ao consultar `Blog`s, que você incluir `Posts` e deseja incluir o `Author` e `Tags` do `Posts`.</span><span class="sxs-lookup"><span data-stu-id="f451d-122">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="f451d-123">Para fazer isso, você precisa especificar cada incluem a partir da raiz do caminho.</span><span class="sxs-lookup"><span data-stu-id="f451d-123">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="f451d-124">Por exemplo, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="f451d-124">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="f451d-125">Isso não significa que você obterá junções redundantes, na maioria dos casos que EF serão consolidados associações durante a geração de SQL.</span><span class="sxs-lookup"><span data-stu-id="f451d-125">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="f451d-126">Ignorado inclui</span><span class="sxs-lookup"><span data-stu-id="f451d-126">Ignored includes</span></span>

<span data-ttu-id="f451d-127">Se você alterar a consulta para que ele não retorna instâncias do tipo de entidade que a consulta foi iniciada com o, os operadores de inclusão são ignorados.</span><span class="sxs-lookup"><span data-stu-id="f451d-127">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="f451d-128">No exemplo a seguir, os operadores de inclusão se baseiam o `Blog`, mas, em seguida, o `Select` operador é usado para alterar a consulta para retornar um tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="f451d-128">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="f451d-129">Nesse caso, os operadores de inclusão não têm efeito.</span><span class="sxs-lookup"><span data-stu-id="f451d-129">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="f451d-130">Por padrão, o núcleo de EF registrará um aviso quando inclui operadores são ignorados.</span><span class="sxs-lookup"><span data-stu-id="f451d-130">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="f451d-131">Consulte [log](../miscellaneous/logging.md) para obter mais informações sobre como exibir a saída de log.</span><span class="sxs-lookup"><span data-stu-id="f451d-131">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="f451d-132">Você pode alterar o comportamento quando um operador include é ignorado para gerar ou não faça nada.</span><span class="sxs-lookup"><span data-stu-id="f451d-132">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="f451d-133">Isso é feito ao configurar as opções para o contexto - normalmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f451d-133">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="f451d-134">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="f451d-134">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="f451d-135">Esse recurso foi introduzido no EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="f451d-135">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="f451d-136">Você pode carregar explicitamente uma propriedade de navegação por meio de `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="f451d-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="f451d-137">Você pode carregar também explicitamente uma propriedade de navegação executando uma consulta separada que retorna as entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="f451d-137">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="f451d-138">Se o controle de alterações está habilitada, ao carregar uma entidade, Core EF automaticamente definir as propriedades de navegação da entidade para se referir a qualquer entidade que já carregada recentemente carregado e definir as propriedades de navegação das entidades já carregado para referir-se a entidade carregado recentemente.</span><span class="sxs-lookup"><span data-stu-id="f451d-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="f451d-139">Consultando entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="f451d-139">Querying related entities</span></span>

<span data-ttu-id="f451d-140">Você também pode obter uma consulta LINQ que representa o conteúdo de uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="f451d-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="f451d-141">Isso permite que você faça coisas como a execução de um operador de agregação sobre as entidades relacionadas sem carregá-los na memória.</span><span class="sxs-lookup"><span data-stu-id="f451d-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="f451d-142">Você também pode filtrar quais entidades relacionadas são carregadas na memória.</span><span class="sxs-lookup"><span data-stu-id="f451d-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="f451d-143">Carregamento preguiçoso</span><span class="sxs-lookup"><span data-stu-id="f451d-143">Lazy loading</span></span>

<span data-ttu-id="f451d-144">Carregamento preguiçoso ainda não é suportado por núcleo EF.</span><span class="sxs-lookup"><span data-stu-id="f451d-144">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="f451d-145">Você pode exibir o [item de carregamento lento em nossa lista de pendências](https://github.com/aspnet/EntityFramework/issues/3797) para controlar esse recurso.</span><span class="sxs-lookup"><span data-stu-id="f451d-145">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="f451d-146">Serialização e dados relacionados</span><span class="sxs-lookup"><span data-stu-id="f451d-146">Related data and serialization</span></span>

<span data-ttu-id="f451d-147">Como propriedades de navegação de automaticamente a correção do EF principal será você pode acabar com ciclos em seu gráfico de objeto.</span><span class="sxs-lookup"><span data-stu-id="f451d-147">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="f451d-148">Por exemplo, o carregamento de um blog e está relacionado postagens resultará em um objeto de blog que referencia uma coleção de postagens.</span><span class="sxs-lookup"><span data-stu-id="f451d-148">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="f451d-149">Cada essas postagens terá uma referência de volta para o blog.</span><span class="sxs-lookup"><span data-stu-id="f451d-149">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="f451d-150">Algumas estruturas de serialização não permitir que esses ciclos.</span><span class="sxs-lookup"><span data-stu-id="f451d-150">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="f451d-151">Por exemplo, Json.NET lançará a exceção a seguir se encontrou um ciclo.</span><span class="sxs-lookup"><span data-stu-id="f451d-151">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="f451d-152">Newtonsoft.Json.JsonSerializationException: Self referenciando loop detectado para a propriedade 'Blog' com tipo 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="f451d-152">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="f451d-153">Se você estiver usando o ASP.NET Core, você pode configurar Json.NET para ignorar ciclos que encontrar no gráfico de objeto.</span><span class="sxs-lookup"><span data-stu-id="f451d-153">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="f451d-154">Isso é feito o `ConfigureServices(...)` método `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="f451d-154">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
