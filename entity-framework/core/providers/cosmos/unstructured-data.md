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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Trabalhando com dados não estruturados no provedor EF Core Azure Cosmos DB

O EF Core foi projetado para facilitar o trabalho com dados que seguem um esquema definido no modelo. No entanto, um dos pontos fortes da Azure Cosmos DB é a flexibilidade na forma dos dados armazenados.

## <a name="accessing-the-raw-json"></a>Acessando o JSON bruto

É possível acessar as propriedades que não são rastreadas por EF Core por meio de uma propriedade especial no [estado](../../modeling/shadow-properties.md) de sombra `"__jObject"` chamado que contém `JObject` um representando os dados recebidos da loja e os dados que serão armazenados:

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
> A `"__jObject"` Propriedade faz parte da infraestrutura de EF Core e só deve ser usada como último recurso, pois provavelmente terá um comportamento diferente em versões futuras.

> [!NOTE]
> As alterações na entidade substituirão os valores armazenados em `"__jObject"` durante `SaveChanges`.

## <a name="using-cosmosclient"></a>Usando CosmosClient

Para desacoplar completamente de EF Core obtenha `CosmosClient` o objeto que faz [parte do SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) do Azure Cosmos DB `DbContext`de:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Valores de propriedade ausentes

No exemplo anterior, removemos a `"TrackingNumber"` propriedade da ordem. Devido a como a indexação funciona no Cosmos DB, as consultas que fazem referência à propriedade ausente em outro lugar do que na projeção podem retornar resultados inesperados. Por exemplo:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

A consulta classificada realmente não retorna nenhum resultado. Isso significa que deve tomar cuidado para sempre preencher as propriedades mapeadas por EF Core ao trabalhar diretamente com o repositório.

> [!NOTE]
> Esse comportamento pode ser alterado em versões futuras do cosmos. Por exemplo, no momento, se a política de indexação definir o índice composto {ID/? ASC, TrackingNumber/? ASC)}, então uma consulta o tem ' order by c.ID ASC, c. Discriminator ASC __'__ retornaria itens que não têm `"TrackingNumber"` a propriedade.