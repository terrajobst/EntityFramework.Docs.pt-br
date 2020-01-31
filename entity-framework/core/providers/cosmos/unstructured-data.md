---
title: Provedor de Azure Cosmos DB-trabalhando com dados não estruturados-EF Core
description: Como trabalhar com Azure Cosmos DB dados não estruturados usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 69f979d46174ff56310b334f28438ac271f45155
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888090"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="d10e7-103">Trabalhando com dados não estruturados no provedor EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d10e7-103">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="d10e7-104">O EF Core foi projetado para facilitar o trabalho com dados que seguem um esquema definido no modelo.</span><span class="sxs-lookup"><span data-stu-id="d10e7-104">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="d10e7-105">No entanto, um dos pontos fortes da Azure Cosmos DB é a flexibilidade na forma dos dados armazenados.</span><span class="sxs-lookup"><span data-stu-id="d10e7-105">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="d10e7-106">Acessando o JSON bruto</span><span class="sxs-lookup"><span data-stu-id="d10e7-106">Accessing the raw JSON</span></span>

<span data-ttu-id="d10e7-107">É possível acessar as propriedades que não são rastreadas por EF Core por meio de uma propriedade especial no [estado de sombra](../../modeling/shadow-properties.md) chamado `"__jObject"` que contém um `JObject` que representa os dados recebidos da loja e os dados que serão armazenados:</span><span class="sxs-lookup"><span data-stu-id="d10e7-107">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
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
> <span data-ttu-id="d10e7-108">A propriedade `"__jObject"` faz parte da infraestrutura de EF Core e só deve ser usada como último recurso, pois provavelmente terá um comportamento diferente em versões futuras.</span><span class="sxs-lookup"><span data-stu-id="d10e7-108">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="d10e7-109">As alterações na entidade substituirão os valores armazenados em `"__jObject"` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="d10e7-109">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="d10e7-110">Usando CosmosClient</span><span class="sxs-lookup"><span data-stu-id="d10e7-110">Using CosmosClient</span></span>

<span data-ttu-id="d10e7-111">Para desacoplar completamente de EF Core obter o objeto [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) que faz [parte do SDK do Azure Cosmos DB](/azure/cosmos-db/sql-api-get-started) do `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="d10e7-111">To decouple completely from EF Core get the [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) object that is [part of the Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="d10e7-112">Valores de propriedade ausentes</span><span class="sxs-lookup"><span data-stu-id="d10e7-112">Missing property values</span></span>

<span data-ttu-id="d10e7-113">No exemplo anterior, removemos a propriedade `"TrackingNumber"` da ordem.</span><span class="sxs-lookup"><span data-stu-id="d10e7-113">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="d10e7-114">Devido a como a indexação funciona no Cosmos DB, as consultas que fazem referência à propriedade ausente em outro lugar do que na projeção podem retornar resultados inesperados.</span><span class="sxs-lookup"><span data-stu-id="d10e7-114">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="d10e7-115">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d10e7-115">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="d10e7-116">A consulta classificada realmente não retorna nenhum resultado.</span><span class="sxs-lookup"><span data-stu-id="d10e7-116">The sorted query actually returns no results.</span></span> <span data-ttu-id="d10e7-117">Isso significa que deve tomar cuidado para sempre preencher as propriedades mapeadas por EF Core ao trabalhar diretamente com o repositório.</span><span class="sxs-lookup"><span data-stu-id="d10e7-117">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="d10e7-118">Esse comportamento pode ser alterado em versões futuras do cosmos.</span><span class="sxs-lookup"><span data-stu-id="d10e7-118">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="d10e7-119">Por exemplo, no momento, se a política de indexação definir o índice composto {ID/?</span><span class="sxs-lookup"><span data-stu-id="d10e7-119">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="d10e7-120">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="d10e7-120">ASC, TrackingNumber/?</span></span> <span data-ttu-id="d10e7-121">ASC)}, então uma consulta que tem ' ORDER BY c.Id ASC, c. Discriminator ASC __'__ retornaria itens que não têm a propriedade `"TrackingNumber"`.</span><span class="sxs-lookup"><span data-stu-id="d10e7-121">ASC)}, then a query that has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>
