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
# <a name="pre-generated-mapping-views"></a>Exibições de mapeamento geradas previamente
Antes que o Entity Framework possa executar uma consulta ou salvar as alterações na fonte de dados, ele deve gerar um conjunto de exibições de mapeamento para acessar o banco de dado. Essas exibições de mapeamento são um conjunto de Entity SQL instrução que representam o banco de dados de forma abstrata e fazem parte dos metadados que são armazenados em cache por domínio de aplicativo. Se você criar várias instâncias do mesmo contexto no mesmo domínio de aplicativo, elas reutilizarão os modos de exibição de mapeamento dos metadados em cache em vez de gerá-los novamente. Como a geração de exibição de mapeamento é uma parte significativa do custo geral da execução da primeira consulta, a Entity Framework permite gerar previamente exibições de mapeamento e incluí-las no projeto compilado. Para obter mais informações, consulte  [performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>Gerando exibições de mapeamento com o EF Power Tools Community Edition

A maneira mais fácil de gerar previamente modos de exibição é usar o [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition). Depois de instalar o Power Tools, você terá uma opção de menu para gerar exibições, como mostrado abaixo.

-   Para modelos de **Code First** , clique com o botão direito do mouse no arquivo de código que contém sua classe DbContext.
-   Para modelos do **designer do EF** , clique com o botão direito do mouse no arquivo edmx.

![gerar exibições](~/ef6/media/generateviews.png)

Quando o processo for concluído, você terá uma classe semelhante à gerada a seguir

![Exibições geradas](~/ef6/media/generatedviews.png)

Agora, quando você executar o aplicativo, o EF usará essa classe para carregar exibições conforme necessário. Se seu modelo for alterado e você não gerar novamente essa classe, o EF gerará uma exceção.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Gerando exibições de mapeamento do Code-EF6 em diante

A outra maneira de gerar modos de exibição é usar as APIs que o EF fornece. Ao usar esse método, você tem a liberdade de serializar as exibições como desejar, mas também precisa carregar as exibições por conta própria.

> [!NOTE]
> **Somente EF6 em diante** – as APIs mostradas nesta seção foram introduzidas no Entity Framework 6. Se você estiver usando uma versão anterior, essas informações não serão aplicáveis.

### <a name="generating-views"></a>Geração de exibições

As APIs para gerar modos de exibição estão na classe System. Data. Entity. Core. Mapping. StorageMappingItemCollection. Você pode recuperar um StorageMappingCollection para um contexto usando o MetadataWorkspace de um ObjectContext. Se você estiver usando a API DbContext mais nova, poderá acessá-la usando o IObjectContextAdapter como mostrado abaixo, neste código, temos uma instância de seu DbContext derivado chamado dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Depois de ter o StorageMappingItemCollection, você pode obter acesso aos métodos GenerateViews e ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

O primeiro método cria um dicionário com uma entrada para cada exibição no mapeamento de contêiner. O segundo método computa um valor de hash para o mapeamento de contêiner único e é usado em tempo de execução para validar que o modelo não foi alterado desde que as exibições foram geradas previamente. As substituições dos dois métodos são fornecidas para cenários complexos que envolvem vários mapeamentos de contêiner.

Ao gerar exibições, você chamará o método GenerateViews e gravará o EntitySetBase resultante e o DbMappingView. Você também precisará armazenar o hash gerado pelo método ComputeMappingHashValue.

### <a name="loading-views"></a>Carregando exibições

Para carregar as exibições geradas pelo método GenerateViews, você pode fornecer o EF com uma classe que herda da classe abstrata DbMappingViewCache. DbMappingViewCache especifica dois métodos que devem ser implementados:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

A propriedade MappingHashValue deve retornar o hash gerado pelo método ComputeMappingHashValue. Quando o EF vai solicitar exibições, ele primeiro irá gerar e comparar o valor de hash do modelo com o hash retornado por essa propriedade. Se eles não corresponderem, o EF gerará uma exceção EntityCommandCompilationException.

O método GetView aceitará um EntitySetBase e você precisará retornar um DbMappingVIew contendo o EntitySql que foi gerado para o que estava associado ao EntitySetBase fornecido no dicionário gerado pelo método GenerateViews. Se o EF solicitar uma exibição que você não tenha, GetView deverá retornar NULL.

Veja a seguir uma extração do DbMappingViewCache que é gerado com o Power Tools, conforme descrito acima, que vemos uma maneira de armazenar e recuperar o EntitySql necessário.

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

Para que o EF use seu DbMappingViewCache, você adiciona usar o DbMappingViewCacheTypeAttribute, especificando o contexto para o qual ele foi criado. No código abaixo, associamos o BlogContext à classe MyMappingViewCache.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Para cenários mais complexos, o mapeamento de instâncias de cache de exibição pode ser fornecido especificando uma fábrica de cache de exibição de mapeamento. Isso pode ser feito com a implementação da classe abstrata System. Data. Entity. Infrastructure. MappingViews. DbMappingViewCacheFactory. A instância da fábrica de cache do modo de exibição de mapeamento que é usada pode ser recuperada ou definida usando o StorageMappingItemCollection. MappingViewCacheFactoryproperty.
