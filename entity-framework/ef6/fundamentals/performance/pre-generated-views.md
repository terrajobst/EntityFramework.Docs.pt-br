---
title: Exibições de mapeamento geradas previamente-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419387"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="fd10c-102">Exibições de mapeamento geradas previamente</span><span class="sxs-lookup"><span data-stu-id="fd10c-102">Pre-generated mapping views</span></span>
<span data-ttu-id="fd10c-103">Antes que o Entity Framework possa executar uma consulta ou salvar as alterações na fonte de dados, ele deve gerar um conjunto de exibições de mapeamento para acessar o banco de dado.</span><span class="sxs-lookup"><span data-stu-id="fd10c-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="fd10c-104">Essas exibições de mapeamento são um conjunto de Entity SQL instrução que representam o banco de dados de forma abstrata e fazem parte dos metadados que são armazenados em cache por domínio de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fd10c-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="fd10c-105">Se você criar várias instâncias do mesmo contexto no mesmo domínio de aplicativo, elas reutilizarão os modos de exibição de mapeamento dos metadados em cache em vez de gerá-los novamente.</span><span class="sxs-lookup"><span data-stu-id="fd10c-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="fd10c-106">Como a geração de exibição de mapeamento é uma parte significativa do custo geral da execução da primeira consulta, a Entity Framework permite gerar previamente exibições de mapeamento e incluí-las no projeto compilado.</span><span class="sxs-lookup"><span data-stu-id="fd10c-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span><span data-ttu-id="fd10c-107"> Para obter mais informações, consulte  [performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="fd10c-107"> For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="fd10c-108">Gerando exibições de mapeamento com o EF Power Tools Community Edition</span><span class="sxs-lookup"><span data-stu-id="fd10c-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="fd10c-109">A maneira mais fácil de gerar previamente modos de exibição é usar o [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span><span class="sxs-lookup"><span data-stu-id="fd10c-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="fd10c-110">Depois de instalar o Power Tools, você terá uma opção de menu para gerar exibições, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fd10c-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="fd10c-111">Para modelos de **Code First** , clique com o botão direito do mouse no arquivo de código que contém sua classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="fd10c-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="fd10c-112">Para modelos do **designer do EF** , clique com o botão direito do mouse no arquivo edmx.</span><span class="sxs-lookup"><span data-stu-id="fd10c-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![gerar exibições](~/ef6/media/generateviews.png)

<span data-ttu-id="fd10c-114">Quando o processo for concluído, você terá uma classe semelhante à gerada a seguir</span><span class="sxs-lookup"><span data-stu-id="fd10c-114">Once the process is finished you will have a class similar to the following generated</span></span>

![Exibições geradas](~/ef6/media/generatedviews.png)

<span data-ttu-id="fd10c-116">Agora, quando você executar o aplicativo, o EF usará essa classe para carregar exibições conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="fd10c-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="fd10c-117">Se seu modelo for alterado e você não gerar novamente essa classe, o EF gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="fd10c-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="fd10c-118">Gerando exibições de mapeamento do Code-EF6 em diante</span><span class="sxs-lookup"><span data-stu-id="fd10c-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="fd10c-119">A outra maneira de gerar modos de exibição é usar as APIs que o EF fornece.</span><span class="sxs-lookup"><span data-stu-id="fd10c-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="fd10c-120">Ao usar esse método, você tem a liberdade de serializar as exibições como desejar, mas também precisa carregar as exibições por conta própria.</span><span class="sxs-lookup"><span data-stu-id="fd10c-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="fd10c-121">**Somente EF6 em diante** – as APIs mostradas nesta seção foram introduzidas no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="fd10c-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fd10c-122">Se você estiver usando uma versão anterior, essas informações não serão aplicáveis.</span><span class="sxs-lookup"><span data-stu-id="fd10c-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="fd10c-123">Geração de exibições</span><span class="sxs-lookup"><span data-stu-id="fd10c-123">Generating Views</span></span>

<span data-ttu-id="fd10c-124">As APIs para gerar modos de exibição estão na classe System. Data. Entity. Core. Mapping. StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="fd10c-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="fd10c-125">Você pode recuperar um StorageMappingCollection para um contexto usando o MetadataWorkspace de um ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="fd10c-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="fd10c-126">Se você estiver usando a API DbContext mais nova, poderá acessá-la usando o IObjectContextAdapter como mostrado abaixo, neste código, temos uma instância de seu DbContext derivado chamado dbContext:</span><span class="sxs-lookup"><span data-stu-id="fd10c-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="fd10c-127">Depois de ter o StorageMappingItemCollection, você pode obter acesso aos métodos GenerateViews e ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="fd10c-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="fd10c-128">O primeiro método cria um dicionário com uma entrada para cada exibição no mapeamento de contêiner.</span><span class="sxs-lookup"><span data-stu-id="fd10c-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="fd10c-129">O segundo método computa um valor de hash para o mapeamento de contêiner único e é usado em tempo de execução para validar que o modelo não foi alterado desde que as exibições foram geradas previamente.</span><span class="sxs-lookup"><span data-stu-id="fd10c-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="fd10c-130">As substituições dos dois métodos são fornecidas para cenários complexos que envolvem vários mapeamentos de contêiner.</span><span class="sxs-lookup"><span data-stu-id="fd10c-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="fd10c-131">Ao gerar exibições, você chamará o método GenerateViews e gravará o EntitySetBase resultante e o DbMappingView.</span><span class="sxs-lookup"><span data-stu-id="fd10c-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="fd10c-132">Você também precisará armazenar o hash gerado pelo método ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="fd10c-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="fd10c-133">Carregando exibições</span><span class="sxs-lookup"><span data-stu-id="fd10c-133">Loading Views</span></span>

<span data-ttu-id="fd10c-134">Para carregar as exibições geradas pelo método GenerateViews, você pode fornecer o EF com uma classe que herda da classe abstrata DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="fd10c-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="fd10c-135">DbMappingViewCache especifica dois métodos que devem ser implementados:</span><span class="sxs-lookup"><span data-stu-id="fd10c-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="fd10c-136">A propriedade MappingHashValue deve retornar o hash gerado pelo método ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="fd10c-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="fd10c-137">Quando o EF vai solicitar exibições, ele primeiro irá gerar e comparar o valor de hash do modelo com o hash retornado por essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="fd10c-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="fd10c-138">Se eles não corresponderem, o EF gerará uma exceção EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="fd10c-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="fd10c-139">O método GetView aceitará um EntitySetBase e você precisará retornar um DbMappingVIew contendo o EntitySql que foi gerado para o que estava associado ao EntitySetBase fornecido no dicionário gerado pelo método GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="fd10c-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="fd10c-140">Se o EF solicitar uma exibição que você não tenha, GetView deverá retornar NULL.</span><span class="sxs-lookup"><span data-stu-id="fd10c-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="fd10c-141">Veja a seguir uma extração do DbMappingViewCache que é gerado com o Power Tools, conforme descrito acima, que vemos uma maneira de armazenar e recuperar o EntitySql necessário.</span><span class="sxs-lookup"><span data-stu-id="fd10c-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

<span data-ttu-id="fd10c-142">Para que o EF use seu DbMappingViewCache, você adiciona usar o DbMappingViewCacheTypeAttribute, especificando o contexto para o qual ele foi criado.</span><span class="sxs-lookup"><span data-stu-id="fd10c-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="fd10c-143">No código abaixo, associamos o BlogContext à classe MyMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="fd10c-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="fd10c-144">Para cenários mais complexos, o mapeamento de instâncias de cache de exibição pode ser fornecido especificando uma fábrica de cache de exibição de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="fd10c-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="fd10c-145">Isso pode ser feito com a implementação da classe abstrata System. Data. Entity. Infrastructure. MappingViews. DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="fd10c-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="fd10c-146">A instância da fábrica de cache do modo de exibição de mapeamento que é usada pode ser recuperada ou definida usando o StorageMappingItemCollection. MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="fd10c-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
