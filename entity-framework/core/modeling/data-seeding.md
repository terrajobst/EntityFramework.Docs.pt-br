---
title: Propagação de dados-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 5c056c600f696ad1443ddb7b8c95c4b0ead06d21
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417222"
---
# <a name="data-seeding"></a><span data-ttu-id="dc33a-102">Propagação de dados</span><span class="sxs-lookup"><span data-stu-id="dc33a-102">Data Seeding</span></span>

<span data-ttu-id="dc33a-103">A propagação de dados é o processo de preenchimento de um banco de dados com um conjunto inicial.</span><span class="sxs-lookup"><span data-stu-id="dc33a-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="dc33a-104">Há várias maneiras que isso pode ser feito em EF Core:</span><span class="sxs-lookup"><span data-stu-id="dc33a-104">There are several ways this can be accomplished in EF Core:</span></span>

* <span data-ttu-id="dc33a-105">Dados de semente de modelo</span><span class="sxs-lookup"><span data-stu-id="dc33a-105">Model seed data</span></span>
* <span data-ttu-id="dc33a-106">Personalização de migração manual</span><span class="sxs-lookup"><span data-stu-id="dc33a-106">Manual migration customization</span></span>
* <span data-ttu-id="dc33a-107">Lógica de inicialização personalizada</span><span class="sxs-lookup"><span data-stu-id="dc33a-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="dc33a-108">Dados de semente de modelo</span><span class="sxs-lookup"><span data-stu-id="dc33a-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="dc33a-109">Este recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="dc33a-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="dc33a-110">Ao contrário de EF6, no EF Core, os dados de propagação podem ser associados a um tipo de entidade como parte da configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="dc33a-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="dc33a-111">Em seguida, EF Core [migrações](xref:core/managing-schemas/migrations/index) podem calcular automaticamente quais operações de inserção, atualização ou exclusão precisam ser aplicadas ao atualizar o banco de dados para uma nova versão do modelo.</span><span class="sxs-lookup"><span data-stu-id="dc33a-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="dc33a-112">As migrações só consideram alterações de modelo ao determinar qual operação deve ser executada para obter os dados de semente para o estado desejado.</span><span class="sxs-lookup"><span data-stu-id="dc33a-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="dc33a-113">Assim, qualquer alteração nos dados executados fora das migrações pode ser perdida ou causar um erro.</span><span class="sxs-lookup"><span data-stu-id="dc33a-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="dc33a-114">Por exemplo, isso configurará os dados semente de um `Blog` no `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="dc33a-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="dc33a-115">Para adicionar entidades que têm uma relação, os valores de chave estrangeira precisam ser especificados:</span><span class="sxs-lookup"><span data-stu-id="dc33a-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="dc33a-116">Se o tipo de entidade tiver qualquer propriedade no estado de sombra, uma classe anônima poderá ser usada para fornecer os valores:</span><span class="sxs-lookup"><span data-stu-id="dc33a-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="dc33a-117">Tipos de entidade de propriedade podem ser propagados de maneira semelhante:</span><span class="sxs-lookup"><span data-stu-id="dc33a-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="dc33a-118">Consulte o [projeto de exemplo completo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) para obter mais contexto.</span><span class="sxs-lookup"><span data-stu-id="dc33a-118">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="dc33a-119">Depois que os dados tiverem sido adicionados ao modelo, as [migrações](xref:core/managing-schemas/migrations/index) deverão ser usadas para aplicar as alterações.</span><span class="sxs-lookup"><span data-stu-id="dc33a-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="dc33a-120">Se você precisar aplicar migrações como parte de uma implantação automatizada, poderá [criar um script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) que pode ser visualizado antes da execução.</span><span class="sxs-lookup"><span data-stu-id="dc33a-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="dc33a-121">Como alternativa, você pode usar o `context.Database.EnsureCreated()` para criar um novo banco de dados que contenha a semente, por exemplo, para um banco de dado de teste ou ao usar o provedor na memória ou qualquer banco de dados que não seja de relação.</span><span class="sxs-lookup"><span data-stu-id="dc33a-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="dc33a-122">Observe que, se o banco de dados já existir, `EnsureCreated()` não atualizará o esquema nem os dados de semente no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="dc33a-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="dc33a-123">Para bancos de dados relacionais, você não deve chamar `EnsureCreated()` se planeja usar migrações.</span><span class="sxs-lookup"><span data-stu-id="dc33a-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

### <a name="limitations-of-model-seed-data"></a><span data-ttu-id="dc33a-124">Limitações dos dados de semente do modelo</span><span class="sxs-lookup"><span data-stu-id="dc33a-124">Limitations of model seed data</span></span>

<span data-ttu-id="dc33a-125">Esse tipo de dados de semente é gerenciado por migrações e o script para atualizar os dados que já estão no banco de dados precisa ser gerado sem se conectar ao banco de dado.</span><span class="sxs-lookup"><span data-stu-id="dc33a-125">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="dc33a-126">Isso impõe algumas restrições:</span><span class="sxs-lookup"><span data-stu-id="dc33a-126">This imposes some restrictions:</span></span>

* <span data-ttu-id="dc33a-127">O valor da chave primária precisa ser especificado mesmo que seja geralmente gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dc33a-127">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="dc33a-128">Ele será usado para detectar alterações de dados entre as migrações.</span><span class="sxs-lookup"><span data-stu-id="dc33a-128">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="dc33a-129">Os dados propagados anteriormente serão removidos se a chave primária for alterada de qualquer forma.</span><span class="sxs-lookup"><span data-stu-id="dc33a-129">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="dc33a-130">Portanto, esse recurso é mais útil para dados estáticos que não devem ser alterados fora das migrações e não dependem de mais nada no banco de dados, por exemplo, CEPs.</span><span class="sxs-lookup"><span data-stu-id="dc33a-130">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="dc33a-131">Se seu cenário incluir qualquer um dos seguintes, é recomendável usar a lógica de inicialização personalizada descrita na última seção:</span><span class="sxs-lookup"><span data-stu-id="dc33a-131">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>

* <span data-ttu-id="dc33a-132">Dados temporários para teste</span><span class="sxs-lookup"><span data-stu-id="dc33a-132">Temporary data for testing</span></span>
* <span data-ttu-id="dc33a-133">Dados que dependem do estado do banco de dado</span><span class="sxs-lookup"><span data-stu-id="dc33a-133">Data that depends on database state</span></span>
* <span data-ttu-id="dc33a-134">Dados que precisam de valores de chave a serem gerados pelo banco de dado, incluindo entidades que usam chaves alternativas como a identidade</span><span class="sxs-lookup"><span data-stu-id="dc33a-134">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="dc33a-135">Dados que exigem transformação personalizada (que não é manipulada por [conversões de valor](xref:core/modeling/value-conversions)), como alguns hashes de senha</span><span class="sxs-lookup"><span data-stu-id="dc33a-135">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="dc33a-136">Dados que exigem chamadas para a API externa, como ASP.NET Core funções de identidade e criação de usuários</span><span class="sxs-lookup"><span data-stu-id="dc33a-136">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="dc33a-137">Personalização de migração manual</span><span class="sxs-lookup"><span data-stu-id="dc33a-137">Manual migration customization</span></span>

<span data-ttu-id="dc33a-138">Quando uma migração é adicionada, as alterações nos dados especificados com `HasData` são transformadas em chamadas para `InsertData()`, `UpdateData()`e `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="dc33a-138">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="dc33a-139">Uma maneira de trabalhar com algumas das limitações do `HasData` é adicionar manualmente essas chamadas ou [operações personalizadas](xref:core/managing-schemas/migrations/operations) à migração.</span><span class="sxs-lookup"><span data-stu-id="dc33a-139">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="dc33a-140">Lógica de inicialização personalizada</span><span class="sxs-lookup"><span data-stu-id="dc33a-140">Custom initialization logic</span></span>

<span data-ttu-id="dc33a-141">Uma maneira simples e eficiente de executar a propagação de dados é usar [`DbContext.SaveChanges()`](xref:core/saving/index) antes que a lógica do aplicativo principal comece a execução.</span><span class="sxs-lookup"><span data-stu-id="dc33a-141">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="dc33a-142">O código de propagação não deve fazer parte da execução normal do aplicativo, pois isso pode causar problemas de simultaneidade quando várias instâncias estão em execução e também exigiria que o aplicativo tenha permissão para modificar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dc33a-142">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="dc33a-143">Dependendo das restrições de sua implantação, o código de inicialização pode ser executado de maneiras diferentes:</span><span class="sxs-lookup"><span data-stu-id="dc33a-143">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>

* <span data-ttu-id="dc33a-144">Executando o aplicativo de inicialização localmente</span><span class="sxs-lookup"><span data-stu-id="dc33a-144">Running the initialization app locally</span></span>
* <span data-ttu-id="dc33a-145">Implantando o aplicativo de inicialização com o aplicativo principal, invocando a rotina de inicialização e desabilitando ou removendo o aplicativo de inicialização.</span><span class="sxs-lookup"><span data-stu-id="dc33a-145">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="dc33a-146">Normalmente, isso pode ser automatizado usando [perfis de publicação](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="dc33a-146">This can usually be automated by using [publish profiles](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
