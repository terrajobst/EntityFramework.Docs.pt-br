---
title: Provedor do Azure Cosmos DB – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 683436aa485d2fef9aa8bf6c6ff02b00dfeb28cf
ms.sourcegitcommit: 2caec1e63f2ce1d9439ef6193df5a77da2fedd0f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317571"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>Provedor do Azure Cosmos DB no EF Core

>[!NOTE]
> Este provedor é novo no EF Core 3.0.

Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Azure Cosmos DB. O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

É extremamente recomendado familiarizar-se com a [documentação do Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) antes de ler esta seção.

## <a name="install"></a>Instalar o

Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a>Introdução

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) deste artigo no GitHub.

Assim como para outros provedores, a primeira etapa é chamar `UseCosmos`:

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Aqui, o ponto de extremidade e a chave são embutidos no código para simplificar, mas em um aplicativo de produção, eles devem ser [armazenados com segurança](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)

Neste exemplo, `Order` é uma entidade simples com uma referência ao [tipo próprio](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

O processo de salvar e consultar dados segue o padrão de EF normal:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Chamar `EnsureCreated` é necessário para criar as coleções necessárias e inserir os [dados de semente](../../modeling/data-seeding.md), se eles estiverem presentes no modelo. No entanto, `EnsureCreated` só deve ser chamado durante a implantação, não durante a operação normal, pois isso pode causar problemas de desempenho.

## <a name="cosmos-specific-model-customization"></a>Personalização de modelo específica do Cosmos

Por padrão, todos os tipos de entidade são mapeados para o mesmo contêiner, nomeados segundo o contexto derivado (neste caso, `"OrderContext"`). Para alterar o nome do contêiner padrão, use `HasDefaultContainer`:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Para mapear um tipo de entidade para um contêiner diferente, use `ToContainer`:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Para identificar o tipo de entidade que um determinado item representa, o EF Core adiciona um valor de discriminador mesmo quando não há nenhum tipo de entidade derivada. O nome e o valor do discriminador [podem ser alterados](../../modeling/inheritance.md).

## <a name="embedded-entities"></a>Entidades inseridas

Para o Cosmos, as entidades próprias são inseridas no mesmo item que o proprietário. Para alterar o nome de uma propriedade, use `ToJsonProperty`:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

Com essa configuração, a ordem do exemplo acima é armazenada da seguinte forma:

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
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
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
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```
