---
title: Provedor de Azure Cosmos DB-trabalhando com dados não estruturados-EF Core
description: Como trabalhar com Azure Cosmos DB dados não estruturados usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 0bfccbfd3af6e209967004752b5a3947d644544b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655510"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Trabalhando com dados não estruturados no provedor EF Core Azure Cosmos DB

O EF Core foi projetado para facilitar o trabalho com dados que seguem um esquema definido no modelo. No entanto, um dos pontos fortes da Azure Cosmos DB é a flexibilidade na forma dos dados armazenados.

## <a name="accessing-the-raw-json"></a>Acessando o JSON bruto

É possível acessar as propriedades que não são rastreadas por EF Core por meio de uma propriedade especial no [estado de sombra](../../modeling/shadow-properties.md) chamado `"__jObject"` que contém um `JObject` que representa os dados recebidos da loja e os dados que serão armazenados:

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
> A propriedade `"__jObject"` faz parte da infraestrutura de EF Core e só deve ser usada como último recurso, pois provavelmente terá um comportamento diferente em versões futuras.

> [!NOTE]
> As alterações na entidade substituirão os valores armazenados em `"__jObject"` durante `SaveChanges`.

## <a name="using-cosmosclient"></a>Usando CosmosClient

Para desacoplar completamente de EF Core obter o objeto [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) que faz [parte do SDK do Azure Cosmos DB](/azure/cosmos-db/sql-api-get-started) do `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Valores de propriedade ausentes

No exemplo anterior, removemos a propriedade `"TrackingNumber"` da ordem. Devido a como a indexação funciona no Cosmos DB, as consultas que fazem referência à propriedade ausente em outro lugar do que na projeção podem retornar resultados inesperados. Por exemplo:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

A consulta classificada realmente não retorna nenhum resultado. Isso significa que deve tomar cuidado para sempre preencher as propriedades mapeadas por EF Core ao trabalhar diretamente com o repositório.

> [!NOTE]
> Esse comportamento pode ser alterado em versões futuras do cosmos. Por exemplo, no momento, se a política de indexação definir o índice composto {ID/? ASC, TrackingNumber/? ASC)}, então uma consulta o tem ' ORDER BY c.Id ASC, c. Discriminator ASC __'__ retornaria itens que não têm a propriedade `"TrackingNumber"`.
