---
title: Provedor do Azure Cosmos DB – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: c753bb71089c91cbb26b970cddd118645fb18d56
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150723"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="b28d1-102">Provedor do Azure Cosmos DB no EF Core</span><span class="sxs-lookup"><span data-stu-id="b28d1-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="b28d1-103">Este provedor é novo no EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="b28d1-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="b28d1-104">Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b28d1-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="b28d1-105">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="b28d1-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="b28d1-106">É extremamente recomendado familiarizar-se com a [documentação do Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) antes de ler esta seção.</span><span class="sxs-lookup"><span data-stu-id="b28d1-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="b28d1-107">Instalar o</span><span class="sxs-lookup"><span data-stu-id="b28d1-107">Install</span></span>

<span data-ttu-id="b28d1-108">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="b28d1-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a><span data-ttu-id="b28d1-109">Introdução</span><span class="sxs-lookup"><span data-stu-id="b28d1-109">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="b28d1-110">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="b28d1-110">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="b28d1-111">Assim como para outros provedores, a primeira etapa é chamar `UseCosmos`: [!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]</span><span class="sxs-lookup"><span data-stu-id="b28d1-111">Like for other providers the first step is to call `UseCosmos`: [!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]</span></span>

> [!WARNING]
> <span data-ttu-id="b28d1-112">Aqui, o ponto de extremidade e a chave são embutidos no código para simplificar, mas em um aplicativo de produção, eles devem ser [armazenados com segurança](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="b28d1-112">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="b28d1-113">Neste exemplo, `Order` é uma entidade simples com uma referência ao [tipo próprio](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="b28d1-113">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="b28d1-114">O processo de salvar e consultar dados segue o padrão de EF normal: [!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]</span><span class="sxs-lookup"><span data-stu-id="b28d1-114">Saving and quering data follows the normal EF pattern: [!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b28d1-115">Chamar `EnsureCreated` é necessário para criar as coleções necessárias e inserir os [dados de semente](../../modeling/data-seeding.md), se eles estiverem presentes no modelo.</span><span class="sxs-lookup"><span data-stu-id="b28d1-115">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="b28d1-116">No entanto, `EnsureCreated` só deve ser chamado durante a implantação, não durante a operação normal, pois isso pode causar problemas de desempenho.</span><span class="sxs-lookup"><span data-stu-id="b28d1-116">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="b28d1-117">Personalização de modelo específica do Cosmos</span><span class="sxs-lookup"><span data-stu-id="b28d1-117">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="b28d1-118">Por padrão, todos os tipos de entidade são mapeados para o mesmo contêiner, nomeados segundo o contexto derivado (neste caso, `"OrderContext"`).</span><span class="sxs-lookup"><span data-stu-id="b28d1-118">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="b28d1-119">Para alterar o nome do contêiner padrão, use `HasDefaultContainer`:</span><span class="sxs-lookup"><span data-stu-id="b28d1-119">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="b28d1-120">Para mapear um tipo de entidade para um contêiner diferente, use `ToContainer`:</span><span class="sxs-lookup"><span data-stu-id="b28d1-120">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="b28d1-121">Para identificar o tipo de entidade que um determinado item representa, o EF Core adiciona um valor de discriminador mesmo quando não há nenhum tipo de entidade derivada.</span><span class="sxs-lookup"><span data-stu-id="b28d1-121">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="b28d1-122">O nome e o valor do discriminador [podem ser alterados](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="b28d1-122">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="b28d1-123">Entidades inseridas</span><span class="sxs-lookup"><span data-stu-id="b28d1-123">Embedded Entities</span></span>

<span data-ttu-id="b28d1-124">Para o Cosmos, as entidades próprias são inseridas no mesmo item que o proprietário.</span><span class="sxs-lookup"><span data-stu-id="b28d1-124">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="b28d1-125">Para alterar o nome de uma propriedade, use `ToJsonProperty`:</span><span class="sxs-lookup"><span data-stu-id="b28d1-125">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="b28d1-126">Com essa configuração, a ordem do exemplo acima é armazenada da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="b28d1-126">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="b28d1-127">Coleções de entidades próprias também são inseridas.</span><span class="sxs-lookup"><span data-stu-id="b28d1-127">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="b28d1-128">Para o próximo exemplo, usaremos a classe `Distributor` com uma coleção de `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="b28d1-128">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="b28d1-129">As entidades próprias não precisam fornecer valores de chave explícitos para serem armazenadas:</span><span class="sxs-lookup"><span data-stu-id="b28d1-129">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="b28d1-130">Elas serão persistidas da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="b28d1-130">They will be persisted in this way:</span></span>

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

<span data-ttu-id="b28d1-131">Internamente, o EF Core sempre precisa ter valores de chave exclusivos para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="b28d1-131">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="b28d1-132">A chave primária criada por padrão para coleções de tipos próprios é composta pelas propriedades de chave estrangeira que apontam para o proprietário e por uma propriedade `int` correspondente ao índice na matriz JSON.</span><span class="sxs-lookup"><span data-stu-id="b28d1-132">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="b28d1-133">Para recuperar esses valores, a API de entrada pode ser usada:</span><span class="sxs-lookup"><span data-stu-id="b28d1-133">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="b28d1-134">Quando necessário, a chave primária padrão para os tipos de entidade própria pode ser alterada, mas os valores da chave devem ser fornecidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="b28d1-134">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="b28d1-135">Trabalhando com entidades desconectadas</span><span class="sxs-lookup"><span data-stu-id="b28d1-135">Working with Disconnected Entities</span></span>

<span data-ttu-id="b28d1-136">Cada item precisa ter um valor `id` exclusivo para a chave de partição especificada.</span><span class="sxs-lookup"><span data-stu-id="b28d1-136">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="b28d1-137">Por padrão, o EF Core gera o valor concatenando os valores do discriminador e da chave primária, usando '|' como delimitador.</span><span class="sxs-lookup"><span data-stu-id="b28d1-137">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="b28d1-138">Os valores da chave são gerados apenas quando a entidade entra no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="b28d1-138">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="b28d1-139">Isso poderá ser um problema ao [anexar entidades](../../saving/disconnected-entities.md) se elas não tiverem uma propriedade `id` no tipo .NET para armazenar o valor.</span><span class="sxs-lookup"><span data-stu-id="b28d1-139">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="b28d1-140">Para contornar essa limitação, é possível criar e definir o valor de `id` manualmente ou marcar a entidade como adicionada primeiro e, em seguida, alterá-la para o estado desejado:</span><span class="sxs-lookup"><span data-stu-id="b28d1-140">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="b28d1-141">Este é o JSON resultante:</span><span class="sxs-lookup"><span data-stu-id="b28d1-141">This is the resulting JSON:</span></span>

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