---
title: Modos de exibição de mapeamento pré-gerado - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
caps.latest.revision: 3
ms.openlocfilehash: 9e74176d02afc424118219eec8e016843333cbb8
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39119900"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="ecd3d-102">Modos de exibição de mapeamento gerados previamente</span><span class="sxs-lookup"><span data-stu-id="ecd3d-102">Pre-generated mapping views</span></span>
<span data-ttu-id="ecd3d-103">Antes que o Entity Framework pode executar uma consulta ou salvar as alterações à fonte de dados, ele deve gerar um conjunto de exibições de mapeamento para acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="ecd3d-104">Esses modos de exibição de mapeamento são um conjunto de instrução de Entity SQL que representam o banco de dados de uma maneira abstrata e fazem parte dos metadados que é armazenado em cache por domínio de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="ecd3d-105">Se você criar várias instâncias do mesmo contexto no mesmo domínio do aplicativo, eles serão reutilizar modos de exibição de mapeamento dos metadados em cache em vez de regenerá-los.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="ecd3d-106">Como geração de exibição de mapeamento é uma parte significativa do custo geral da execução da consulta primeiro, o Entity Framework permite que você gerar previamente exibições de mapeamento e incluí-los no projeto compilado.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="ecd3d-107">Para obter mais informações, consulte [considerações de desempenho (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="ecd3d-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="ecd3d-108">Gerando mapeamento de exibições com as EF Power Tools</span><span class="sxs-lookup"><span data-stu-id="ecd3d-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="ecd3d-109">A maneira mais fácil de gerar previamente exibições é usar o [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span><span class="sxs-lookup"><span data-stu-id="ecd3d-109">The easiest way to pre-generate views is to use the [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="ecd3d-110">Uma vez que o Power Tools instalado, você terá uma opção de menu para gerar exibições, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="ecd3d-111">Para **Code First** modelos com o botão direito no arquivo de código que contém sua classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="ecd3d-112">Para **EF Designer** modelos com o botão direito em seu arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generateViews](~/ef6/media/generateviews.png)

<span data-ttu-id="ecd3d-114">Depois que o processo for concluído, você terá uma classe semelhante à seguinte gerado</span><span class="sxs-lookup"><span data-stu-id="ecd3d-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generatedViews](~/ef6/media/generatedviews.png)

<span data-ttu-id="ecd3d-116">Agora quando você executa seu aplicativo EF usará essa classe para carregar os modos de exibição conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="ecd3d-117">Se as alterações de modelo e você não gerar novamente esta classe EF lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="ecd3d-118">Gerando mapeamento de exibições do código - EF6 em diante</span><span class="sxs-lookup"><span data-stu-id="ecd3d-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="ecd3d-119">A outra maneira de gerar exibições é usar as APIs que o EF oferece.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="ecd3d-120">Ao usar esse método, você tem a liberdade para serializar os modos de exibição, no entanto, você gosta, mas você também precisa carregar as exibições por conta própria.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="ecd3d-121">**EF6 em diante, somente** -APIs do mostrado nesta seção foram introduzidas no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="ecd3d-122">Se você estiver usando uma versão anterior, que essa informação não se aplica.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="ecd3d-123">Gerando exibições</span><span class="sxs-lookup"><span data-stu-id="ecd3d-123">Generating Views</span></span>

<span data-ttu-id="ecd3d-124">As APIs para gerar exibições estão na classe System.Data.Entity.Core.Mapping.StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="ecd3d-125">Você pode recuperar um StorageMappingCollection para um contexto usando o MetadataWorkspace de um ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="ecd3d-126">Se você estiver usando a API DbContext mais recente, em seguida, você pode acessá-la usando o IObjectContextAdapter como abaixo, nesse código, temos uma instância do seu DbContext derivada chamada dbContext:</span><span class="sxs-lookup"><span data-stu-id="ecd3d-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="ecd3d-127">Quando você tiver StorageMappingItemCollection, em seguida, você pode obter acesso aos métodos GenerateViews e ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="ecd3d-128">O primeiro método cria um dicionário com uma entrada para cada modo de exibição no mapeamento de contêiner.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="ecd3d-129">O segundo método calcula um valor de hash para o mapeamento de contêiner único e é usado em tempo de execução para validar que o modelo não foi alterado desde que os modos de exibição foram gerados previamente.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="ecd3d-130">Substituições dos dois métodos são fornecidas para cenários complexos que envolvem vários mapeamentos de contêiner.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="ecd3d-131">Ao gerar os modos de exibição, você chamará o método GenerateViews e, em seguida, gravar o resultante EntitySetBase e DbMappingView.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="ecd3d-132">Você também precisará armazenar o hash gerado pelo método ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="ecd3d-133">Modos de exibição de carregamento</span><span class="sxs-lookup"><span data-stu-id="ecd3d-133">Loading Views</span></span>

<span data-ttu-id="ecd3d-134">Para carregar as exibições geradas pelo método GenerateViews, você pode fornecer o EF com uma classe que herda da classe abstrata DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="ecd3d-135">DbMappingViewCache especifica dois métodos que você deve implementar:</span><span class="sxs-lookup"><span data-stu-id="ecd3d-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="ecd3d-136">A propriedade MappingHashValue deve retornar o hash gerado pelo método ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="ecd3d-137">Quando o EF é vai pedir para modos de exibição a ele pela primeira vez gerará e comparar o valor de hash do modelo com o hash retornado por essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="ecd3d-138">Se eles não corresponderem, em seguida, o EF lançará uma exceção de EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="ecd3d-139">O método GetView aceitará um EntitySetBase e você precisa retornar um DbMappingVIew que contém o EntitySql que foi gerada para que foi associado a determinado EntitySetBase no dicionário gerado pelo método GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="ecd3d-140">Se o EF solicita uma exibição que você não precisa, em seguida, GetView deve retornar nula.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="ecd3d-141">Veja a seguir um extrato do DbMappingViewCache que é gerado com o Power Tools, conforme descrito acima, nele, que podemos ver uma maneira de armazenar e recuperar o EntitySql necessária.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="ecd3d-142">Para que o uso do EF seu DbMappingViewCache adicionar use DbMappingViewCacheTypeAttribute, especificando o contexto no qual ele foi criado para.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="ecd3d-143">No código a seguir, associamos o BlogContext com a classe MyMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="ecd3d-144">Para cenários mais complexos, as instâncias de cache do modo de exibição de mapeamento podem ser fornecidas com a especificação de uma fábrica de cache do modo de exibição de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="ecd3d-145">Isso pode ser feito com a implementação da classe abstrata System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="ecd3d-146">A instância da fábrica de cache de modo de exibição de mapeamento que é usada pode ser recuperada ou definida usando o StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="ecd3d-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
