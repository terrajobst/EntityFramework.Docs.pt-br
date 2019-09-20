---
title: Provedor de Azure Cosmos DB-trabalhando com dados não estruturados-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150812"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="351ec-102">Trabalhando com dados não estruturados no provedor EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="351ec-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="351ec-103">O EF Core foi projetado para facilitar o trabalho com dados que seguem um esquema definido no modelo.</span><span class="sxs-lookup"><span data-stu-id="351ec-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="351ec-104">No entanto, um dos pontos fortes da Azure Cosmos DB é a flexibilidade na forma dos dados armazenados.</span><span class="sxs-lookup"><span data-stu-id="351ec-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="351ec-105">Acessando o JSON bruto</span><span class="sxs-lookup"><span data-stu-id="351ec-105">Accessing the raw JSON</span></span>

<span data-ttu-id="351ec-106">É possível acessar as propriedades que não são rastreadas por EF Core por meio de uma propriedade especial no [estado](../../modeling/shadow-properties.md) de sombra `"__jObject"` chamado que contém `JObject` um representando os dados recebidos da loja e os dados que serão armazenados:</span><span class="sxs-lookup"><span data-stu-id="351ec-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> <span data-ttu-id="351ec-107">A `"__jObject"` Propriedade faz parte da infraestrutura de EF Core e só deve ser usada como último recurso, pois provavelmente terá um comportamento diferente em versões futuras.</span><span class="sxs-lookup"><span data-stu-id="351ec-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="351ec-108">As alterações na entidade substituirão os valores armazenados em `"__jObject"` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="351ec-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="351ec-109">Usando CosmosClient</span><span class="sxs-lookup"><span data-stu-id="351ec-109">Using CosmosClient</span></span>

<span data-ttu-id="351ec-110">Para desacoplar completamente de EF Core obtenha `CosmosClient` o objeto que faz [parte do SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) do Azure Cosmos DB `DbContext`de:</span><span class="sxs-lookup"><span data-stu-id="351ec-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="351ec-111">Valores de propriedade ausentes</span><span class="sxs-lookup"><span data-stu-id="351ec-111">Missing property values</span></span>

<span data-ttu-id="351ec-112">No exemplo anterior, removemos a `"TrackingNumber"` propriedade da ordem.</span><span class="sxs-lookup"><span data-stu-id="351ec-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="351ec-113">Devido a como a indexação funciona no Cosmos DB, as consultas que fazem referência à propriedade ausente em outro lugar do que na projeção podem retornar resultados inesperados.</span><span class="sxs-lookup"><span data-stu-id="351ec-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="351ec-114">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="351ec-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="351ec-115">A consulta classificada realmente não retorna nenhum resultado.</span><span class="sxs-lookup"><span data-stu-id="351ec-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="351ec-116">Isso significa que deve tomar cuidado para sempre preencher as propriedades mapeadas por EF Core ao trabalhar diretamente com o repositório.</span><span class="sxs-lookup"><span data-stu-id="351ec-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="351ec-117">Esse comportamento pode ser alterado em versões futuras do cosmos.</span><span class="sxs-lookup"><span data-stu-id="351ec-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="351ec-118">Por exemplo, no momento, se a política de indexação definir o índice composto {ID/?</span><span class="sxs-lookup"><span data-stu-id="351ec-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="351ec-119">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="351ec-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="351ec-120">ASC)}, então uma consulta o tem ' order by c.ID ASC, c. Discriminator ASC __'__ retornaria itens que não têm `"TrackingNumber"` a propriedade.</span><span class="sxs-lookup"><span data-stu-id="351ec-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>