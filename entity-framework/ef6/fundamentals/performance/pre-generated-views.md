---
title: Modos de exibição de mapeamento pré-gerado - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459520"
---
# <a name="pre-generated-mapping-views"></a>Modos de exibição de mapeamento gerados previamente
Antes que o Entity Framework pode executar uma consulta ou salvar as alterações à fonte de dados, ele deve gerar um conjunto de exibições de mapeamento para acessar o banco de dados. Esses modos de exibição de mapeamento são um conjunto de instrução de Entity SQL que representam o banco de dados de uma maneira abstrata e fazem parte dos metadados que é armazenado em cache por domínio de aplicativo. Se você criar várias instâncias do mesmo contexto no mesmo domínio do aplicativo, eles serão reutilizar modos de exibição de mapeamento dos metadados em cache em vez de regenerá-los. Como geração de exibição de mapeamento é uma parte significativa do custo geral da execução da consulta primeiro, o Entity Framework permite que você gerar previamente exibições de mapeamento e incluí-los no projeto compilado. Para obter mais informações, consulte [considerações de desempenho (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>Gerando mapeamento de exibições com o EF Power Tools Community Edition

A maneira mais fácil de gerar previamente exibições é usar o [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition). Uma vez que o Power Tools instalado, você terá uma opção de menu para gerar exibições, como mostrado abaixo.

-   Para **Code First** modelos com o botão direito no arquivo de código que contém sua classe DbContext.
-   Para **EF Designer** modelos com o botão direito em seu arquivo EDMX.

![gerar exibições](~/ef6/media/generateviews.png)

Depois que o processo for concluído, você terá uma classe semelhante à seguinte gerado

![modos de exibição gerados](~/ef6/media/generatedviews.png)

Agora quando você executa seu aplicativo EF usará essa classe para carregar os modos de exibição conforme necessário. Se as alterações de modelo e você não gerar novamente esta classe EF lançará uma exceção.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Gerando mapeamento de exibições do código - EF6 em diante

A outra maneira de gerar exibições é usar as APIs que o EF oferece. Ao usar esse método, você tem a liberdade para serializar os modos de exibição, no entanto, você gosta, mas você também precisa carregar as exibições por conta própria.

> [!NOTE]
> **EF6 em diante, somente** -APIs do mostrado nesta seção foram introduzidas no Entity Framework 6. Se você estiver usando uma versão anterior, que essa informação não se aplica.

### <a name="generating-views"></a>Gerando exibições

As APIs para gerar exibições estão na classe System.Data.Entity.Core.Mapping.StorageMappingItemCollection. Você pode recuperar um StorageMappingCollection para um contexto usando o MetadataWorkspace de um ObjectContext. Se você estiver usando a API DbContext mais recente, em seguida, você pode acessá-la usando o IObjectContextAdapter como abaixo, nesse código, temos uma instância do seu DbContext derivada chamada dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Quando você tiver StorageMappingItemCollection, em seguida, você pode obter acesso aos métodos GenerateViews e ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

O primeiro método cria um dicionário com uma entrada para cada modo de exibição no mapeamento de contêiner. O segundo método calcula um valor de hash para o mapeamento de contêiner único e é usado em tempo de execução para validar que o modelo não foi alterado desde que os modos de exibição foram gerados previamente. Substituições dos dois métodos são fornecidas para cenários complexos que envolvem vários mapeamentos de contêiner.

Ao gerar os modos de exibição, você chamará o método GenerateViews e, em seguida, gravar o resultante EntitySetBase e DbMappingView. Você também precisará armazenar o hash gerado pelo método ComputeMappingHashValue.

### <a name="loading-views"></a>Modos de exibição de carregamento

Para carregar as exibições geradas pelo método GenerateViews, você pode fornecer o EF com uma classe que herda da classe abstrata DbMappingViewCache. DbMappingViewCache especifica dois métodos que você deve implementar:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

A propriedade MappingHashValue deve retornar o hash gerado pelo método ComputeMappingHashValue. Quando o EF é vai pedir para modos de exibição a ele pela primeira vez gerará e comparar o valor de hash do modelo com o hash retornado por essa propriedade. Se eles não corresponderem, em seguida, o EF lançará uma exceção de EntityCommandCompilationException.

O método GetView aceitará um EntitySetBase e você precisa retornar um DbMappingVIew que contém o EntitySql que foi gerada para que foi associado a determinado EntitySetBase no dicionário gerado pelo método GenerateViews. Se o EF solicita uma exibição que você não precisa, em seguida, GetView deve retornar nula.

Veja a seguir um extrato do DbMappingViewCache que é gerado com o Power Tools, conforme descrito acima, nele, que podemos ver uma maneira de armazenar e recuperar o EntitySql necessária.

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

Para que o uso do EF seu DbMappingViewCache adicionar use DbMappingViewCacheTypeAttribute, especificando o contexto no qual ele foi criado para. No código a seguir, associamos o BlogContext com a classe MyMappingViewCache.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Para cenários mais complexos, as instâncias de cache do modo de exibição de mapeamento podem ser fornecidas com a especificação de uma fábrica de cache do modo de exibição de mapeamento. Isso pode ser feito com a implementação da classe abstrata System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory. A instância da fábrica de cache de modo de exibição de mapeamento que é usada pode ser recuperada ou definida usando o StorageMappingItemCollection.MappingViewCacheFactoryproperty.
