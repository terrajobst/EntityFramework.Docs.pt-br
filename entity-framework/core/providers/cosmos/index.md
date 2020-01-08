---
title: Provedor do Azure Cosmos DB – EF Core
description: Documentação para o provedor de banco de dados que permite que o Entity Framework Core seja usado com a API do SQL do Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 6903aab4911f7478afe3d8987a791ae1c5ccebce
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502208"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="cdff6-103">Provedor do Azure Cosmos DB no EF Core</span><span class="sxs-lookup"><span data-stu-id="cdff6-103">EF Core Azure Cosmos DB Provider</span></span>

> [!NOTE]
> <span data-ttu-id="cdff6-104">Este provedor é novo no EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="cdff6-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="cdff6-105">Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cdff6-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="cdff6-106">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="cdff6-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="cdff6-107">É extremamente recomendado familiarizar-se com a [documentação do Azure Cosmos DB](/azure/cosmos-db/introduction) antes de ler esta seção.</span><span class="sxs-lookup"><span data-stu-id="cdff6-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

> [!NOTE]
> <span data-ttu-id="cdff6-108">Esse provedor funciona somente com a API do SQL do Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cdff6-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="cdff6-109">Instalar o</span><span class="sxs-lookup"><span data-stu-id="cdff6-109">Install</span></span>

<span data-ttu-id="cdff6-110">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="cdff6-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

### <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="cdff6-111">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="cdff6-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studiotabvs"></a>[<span data-ttu-id="cdff6-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cdff6-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="cdff6-113">Introdução</span><span class="sxs-lookup"><span data-stu-id="cdff6-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="cdff6-114">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="cdff6-114">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="cdff6-115">Assim como para outros provedores, a primeira etapa é chamar [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span><span class="sxs-lookup"><span data-stu-id="cdff6-115">Like for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="cdff6-116">Aqui, o ponto de extremidade e a chave são embutidos no código para simplificar, mas em um aplicativo de produção, eles devem ser [armazenados com segurança](/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="cdff6-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="cdff6-117">Neste exemplo, `Order` é uma entidade simples com uma referência ao [tipo próprio](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="cdff6-117">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="cdff6-118">O processo de salvar e consultar dados segue o padrão de EF normal:</span><span class="sxs-lookup"><span data-stu-id="cdff6-118">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="cdff6-119">Será necessário chamar [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) para criar os contêineres obrigatórios e inserir os [dados de semente](../../modeling/data-seeding.md) (se presentes no modelo).</span><span class="sxs-lookup"><span data-stu-id="cdff6-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="cdff6-120">No entanto, `EnsureCreatedAsync` só deve ser chamado durante a implantação, não durante a operação normal, pois isso pode causar problemas de desempenho.</span><span class="sxs-lookup"><span data-stu-id="cdff6-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="cdff6-121">Personalização de modelo específica do Cosmos</span><span class="sxs-lookup"><span data-stu-id="cdff6-121">Cosmos-specific model customization</span></span>

<span data-ttu-id="cdff6-122">Por padrão, todos os tipos de entidade são mapeados para o mesmo contêiner, nomeados segundo o contexto derivado (neste caso, `"OrderContext"`).</span><span class="sxs-lookup"><span data-stu-id="cdff6-122">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="cdff6-123">Para alterar o nome do contêiner padrão, use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span><span class="sxs-lookup"><span data-stu-id="cdff6-123">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="cdff6-124">Para mapear um tipo de entidade para um contêiner diferente, use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span><span class="sxs-lookup"><span data-stu-id="cdff6-124">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="cdff6-125">Para identificar o tipo de entidade que um determinado item representa, o EF Core adiciona um valor de discriminador mesmo quando não há nenhum tipo de entidade derivada.</span><span class="sxs-lookup"><span data-stu-id="cdff6-125">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="cdff6-126">O nome e o valor do discriminador [podem ser alterados](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="cdff6-126">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

<span data-ttu-id="cdff6-127">Se nenhum outro tipo de entidade for armazenado no mesmo contêiner, o discriminador poderá ser removido chamando [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span><span class="sxs-lookup"><span data-stu-id="cdff6-127">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="cdff6-128">Chaves de partição</span><span class="sxs-lookup"><span data-stu-id="cdff6-128">Partition keys</span></span>

<span data-ttu-id="cdff6-129">Por padrão, o EF Core criará contêineres com a chave de partição definida como `"__partitionKey"` sem fornecer qualquer valor a ele ao inserir itens.</span><span class="sxs-lookup"><span data-stu-id="cdff6-129">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="cdff6-130">Mas, para aproveitar totalmente os recursos de desempenho do Azure Cosmos, deve-se usar uma [chave de partição cuidadosamente selecionada](/azure/cosmos-db/partition-data).</span><span class="sxs-lookup"><span data-stu-id="cdff6-130">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="cdff6-131">Ela pode ser configurada chamando [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span><span class="sxs-lookup"><span data-stu-id="cdff6-131">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
><span data-ttu-id="cdff6-132">A propriedade de chave de partição pode ser de qualquer tipo, contanto que ela seja [convertida em cadeia de caracteres](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="cdff6-132">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="cdff6-133">Uma vez configurada, a propriedade da chave da partição sempre deve ter um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="cdff6-133">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="cdff6-134">Ao emitir uma consulta, uma condição poderá ser adicionada para torná-la de partição única.</span><span class="sxs-lookup"><span data-stu-id="cdff6-134">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="cdff6-135">Entidades inseridas</span><span class="sxs-lookup"><span data-stu-id="cdff6-135">Embedded entities</span></span>

<span data-ttu-id="cdff6-136">Para o Cosmos, as entidades próprias são inseridas no mesmo item que o proprietário.</span><span class="sxs-lookup"><span data-stu-id="cdff6-136">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="cdff6-137">Para alterar o nome de uma propriedade, use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span><span class="sxs-lookup"><span data-stu-id="cdff6-137">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="cdff6-138">Com essa configuração, a ordem do exemplo acima é armazenada da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="cdff6-138">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="cdff6-139">Coleções de entidades próprias também são inseridas.</span><span class="sxs-lookup"><span data-stu-id="cdff6-139">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="cdff6-140">Para o próximo exemplo, usaremos a classe `Distributor` com uma coleção de `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="cdff6-140">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="cdff6-141">As entidades próprias não precisam fornecer valores de chave explícitos para serem armazenadas:</span><span class="sxs-lookup"><span data-stu-id="cdff6-141">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="cdff6-142">Elas serão persistidas da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="cdff6-142">They will be persisted in this way:</span></span>

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

<span data-ttu-id="cdff6-143">Internamente, o EF Core sempre precisa ter valores de chave exclusivos para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="cdff6-143">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="cdff6-144">A chave primária criada por padrão para coleções de tipos próprios é composta pelas propriedades de chave estrangeira que apontam para o proprietário e por uma propriedade `int` correspondente ao índice na matriz JSON.</span><span class="sxs-lookup"><span data-stu-id="cdff6-144">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="cdff6-145">Para recuperar esses valores, a API de entrada pode ser usada:</span><span class="sxs-lookup"><span data-stu-id="cdff6-145">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="cdff6-146">Quando necessário, a chave primária padrão para os tipos de entidade própria pode ser alterada, mas os valores da chave devem ser fornecidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="cdff6-146">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="cdff6-147">Trabalhando com entidades desconectadas</span><span class="sxs-lookup"><span data-stu-id="cdff6-147">Working with disconnected entities</span></span>

<span data-ttu-id="cdff6-148">Cada item precisa ter um valor `id` exclusivo para a chave de partição especificada.</span><span class="sxs-lookup"><span data-stu-id="cdff6-148">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="cdff6-149">Por padrão, o EF Core gera o valor concatenando os valores do discriminador e da chave primária, usando '|' como delimitador.</span><span class="sxs-lookup"><span data-stu-id="cdff6-149">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="cdff6-150">Os valores da chave são gerados apenas quando a entidade entra no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="cdff6-150">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="cdff6-151">Isso poderá ser um problema ao [anexar entidades](../../saving/disconnected-entities.md) se elas não tiverem uma propriedade `id` no tipo .NET para armazenar o valor.</span><span class="sxs-lookup"><span data-stu-id="cdff6-151">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="cdff6-152">Para contornar essa limitação, é possível criar e definir o valor de `id` manualmente ou marcar a entidade como adicionada primeiro e, em seguida, alterá-la para o estado desejado:</span><span class="sxs-lookup"><span data-stu-id="cdff6-152">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="cdff6-153">Este é o JSON resultante:</span><span class="sxs-lookup"><span data-stu-id="cdff6-153">This is the resulting JSON:</span></span>

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
