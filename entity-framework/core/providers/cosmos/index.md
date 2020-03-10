---
title: Provedor do Azure Cosmos DB – EF Core
description: Documentação para o provedor de banco de dados que permite que o Entity Framework Core seja usado com a API do SQL do Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 74284bf78f404e376436a1ef5d5933186c85ae49
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413051"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>Provedor do Azure Cosmos DB no EF Core

> [!NOTE]
> Este provedor é novo no EF Core 3.0.

Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Azure Cosmos DB. O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

É extremamente recomendado familiarizar-se com a [documentação do Azure Cosmos DB](/azure/cosmos-db/introduction) antes de ler esta seção.

> [!NOTE]
> Esse provedor funciona somente com a API do SQL do Azure Cosmos DB.

## <a name="install"></a>Instalar o

Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Introdução

> [!TIP]  
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) deste artigo no GitHub.

Assim como para outros provedores, a primeira etapa é chamar [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Aqui, o ponto de extremidade e a chave são embutidos no código para simplificar, mas em um aplicativo de produção, eles devem ser [armazenados com segurança](/aspnet/core/security/app-secrets#secret-manager).

Neste exemplo, `Order` é uma entidade simples com uma referência ao [tipo próprio](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

O processo de salvar e consultar dados segue o padrão de EF normal:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Será necessário chamar [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) para criar os contêineres obrigatórios e inserir os [dados de semente](../../modeling/data-seeding.md) (se presentes no modelo). No entanto, `EnsureCreatedAsync` só deve ser chamado durante a implantação, não durante a operação normal, pois isso pode causar problemas de desempenho.

## <a name="cosmos-specific-model-customization"></a>Personalização de modelo específica do Cosmos

Por padrão, todos os tipos de entidade são mapeados para o mesmo contêiner, nomeados segundo o contexto derivado (neste caso, `"OrderContext"`). Para alterar o nome do contêiner padrão, use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Para mapear um tipo de entidade para um contêiner diferente, use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Para identificar o tipo de entidade que um determinado item representa, o EF Core adiciona um valor de discriminador mesmo quando não há nenhum tipo de entidade derivada. O nome e o valor do discriminador [podem ser alterados](../../modeling/inheritance.md).

Se nenhum outro tipo de entidade for armazenado no mesmo contêiner, o discriminador poderá ser removido chamando [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a>Chaves de partição

Por padrão, o EF Core criará contêineres com a chave de partição definida como `"__partitionKey"` sem fornecer qualquer valor a ele ao inserir itens. Mas, para aproveitar totalmente os recursos de desempenho do Azure Cosmos, deve-se usar uma [chave de partição cuidadosamente selecionada](/azure/cosmos-db/partition-data). Ela pode ser configurada chamando [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
>A propriedade de chave de partição pode ser de qualquer tipo, contanto que ela seja [convertida em cadeia de caracteres](xref:core/modeling/value-conversions).

Uma vez configurada, a propriedade da chave da partição sempre deve ter um valor não nulo. Ao emitir uma consulta, uma condição poderá ser adicionada para torná-la de partição única.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a>Entidades inseridas

Para o Cosmos, as entidades próprias são inseridas no mesmo item que o proprietário. Para alterar o nome de uma propriedade, use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

Com essa configuração, a ordem do exemplo acima é armazenada da seguinte forma:

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
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

Coleções de entidades próprias também são inseridas. Para o próximo exemplo, usaremos a classe `Distributor` com uma coleção de `StreetAddress`:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

As entidades próprias não precisam fornecer valores de chave explícitos para serem armazenadas:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Elas serão persistidas da seguinte forma:

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

Internamente, o EF Core sempre precisa ter valores de chave exclusivos para todas as entidades rastreadas. A chave primária criada por padrão para coleções de tipos próprios é composta pelas propriedades de chave estrangeira que apontam para o proprietário e por uma propriedade `int` correspondente ao índice na matriz JSON. Para recuperar esses valores, a API de entrada pode ser usada:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> Quando necessário, a chave primária padrão para os tipos de entidade própria pode ser alterada, mas os valores da chave devem ser fornecidos explicitamente.

## <a name="working-with-disconnected-entities"></a>Trabalhando com entidades desconectadas

Cada item precisa ter um valor `id` exclusivo para a chave de partição especificada. Por padrão, o EF Core gera o valor concatenando os valores do discriminador e da chave primária, usando '|' como delimitador. Os valores da chave são gerados apenas quando a entidade entra no estado `Added`. Isso poderá ser um problema ao [anexar entidades](../../saving/disconnected-entities.md) se elas não tiverem uma propriedade `id` no tipo .NET para armazenar o valor.

Para contornar essa limitação, é possível criar e definir o valor de `id` manualmente ou marcar a entidade como adicionada primeiro e, em seguida, alterá-la para o estado desejado:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Este é o JSON resultante:

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        }
    ],
    "_rid": "JBwtAN8oNYEBAAAAAAAAAA==",
    "_self": "dbs/JBwtAA==/colls/JBwtAN8oNYE=/docs/JBwtAN8oNYEBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-9377-d7a1ae7c01d5\"",
    "_attachments": "attachments/",
    "_ts": 1572917100
}
```
