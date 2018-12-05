---
title: Propagação de dados – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 8f28dfea12461572ade8fbf3910ebd216dafb389
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857423"
---
# <a name="data-seeding"></a><span data-ttu-id="a8ce3-102">Propagação de dados</span><span class="sxs-lookup"><span data-stu-id="a8ce3-102">Data Seeding</span></span>

<span data-ttu-id="a8ce3-103">Propagação de dados é o processo de preenchimento de um banco de dados com um conjunto inicial de dados.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="a8ce3-104">Há várias maneiras, que isso pode ser feito no EF Core:</span><span class="sxs-lookup"><span data-stu-id="a8ce3-104">There are several ways this can be accomplished in EF Core:</span></span>
* <span data-ttu-id="a8ce3-105">Dados de propagação de modelo</span><span class="sxs-lookup"><span data-stu-id="a8ce3-105">Model seed data</span></span>
* <span data-ttu-id="a8ce3-106">Personalização da migração manual</span><span class="sxs-lookup"><span data-stu-id="a8ce3-106">Manual migration customization</span></span>
* <span data-ttu-id="a8ce3-107">Lógica de inicialização personalizada</span><span class="sxs-lookup"><span data-stu-id="a8ce3-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="a8ce3-108">Dados de propagação de modelo</span><span class="sxs-lookup"><span data-stu-id="a8ce3-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="a8ce3-109">Este recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="a8ce3-110">Diferentemente do EF6 no EF Core, a propagação de dados pode ser associada com um tipo de entidade como parte da configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="a8ce3-111">Em seguida, o EF Core [migrações](xref:core/managing-schemas/migrations/index) pode calcular automaticamente o que inserir, atualizar ou excluir operações precisa ser aplicado ao atualizar o banco de dados para uma nova versão do modelo.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="a8ce3-112">Migrações considera somente as alterações de modelo ao determinar qual operação deve ser executada para obter os dados de semente para o estado desejado.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="a8ce3-113">Portanto, quaisquer alterações de dados executadas fora de migrações podem ser perdidas ou causar um erro.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="a8ce3-114">Por exemplo, isso irá configurar dados de semente para uma `Blog` em `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="a8ce3-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="a8ce3-115">Para adicionar entidades que têm uma relação de valores de chave estrangeira precisa ser especificado:</span><span class="sxs-lookup"><span data-stu-id="a8ce3-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="a8ce3-116">Se o tipo de entidade tem todas as propriedades em uma classe anônima pode ser usada para fornecer os valores de estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="a8ce3-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="a8ce3-117">Propriedade entidade tipos podem ser propagados de maneira semelhante:</span><span class="sxs-lookup"><span data-stu-id="a8ce3-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="a8ce3-118">Consulte a [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) para obter mais contexto.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="a8ce3-119">Depois que os dados tiverem sido adicionados ao modelo, [migrações](xref:core/managing-schemas/migrations/index) deve ser usado para aplicar as alterações.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="a8ce3-120">Se você precisar aplicar migrações como parte de uma implantação automatizada, você pode [criar um script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) que pode ser visualizado antes da execução.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="a8ce3-121">Como alternativa, você pode usar `context.Database.EnsureCreated()` para criar um novo banco de dados que contém os dados de semente, por exemplo, para um banco de dados de teste ou ao usar o provedor na memória ou qualquer banco de dados não-relação.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="a8ce3-122">Observe que, se o banco de dados já existe, `EnsureCreated()` não atualizará os dados de semente nem de esquema no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="a8ce3-123">Para bancos de dados relacionais, você não deve chamar `EnsureCreated()` se você planeja usar as migrações.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

<span data-ttu-id="a8ce3-124">Esse tipo de dados de propagação é gerenciado pelas migrações e o script para atualizar os dados que já está no banco de dados precisa ser gerado sem se conectar ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-124">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="a8ce3-125">Isso impõe algumas restrições:</span><span class="sxs-lookup"><span data-stu-id="a8ce3-125">This imposes some restrictions:</span></span>
* <span data-ttu-id="a8ce3-126">O valor de chave primária precisa ser especificado mesmo que ele normalmente é gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-126">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="a8ce3-127">Ele será usado para detectar alterações de dados entre as migrações.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-127">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="a8ce3-128">Anteriormente dados propagados serão removidos se a chave primária é alterada de alguma forma.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-128">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="a8ce3-129">Portanto, esse recurso é mais útil para dados estáticos que não tem deve ser alterada fora de migrações e não dependem mais nada no banco de dados, por exemplo CEPs.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-129">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="a8ce3-130">Se seu cenário inclui qualquer um dos seguintes, é recomendável usar a lógica de inicialização personalizada descrita na última seção:</span><span class="sxs-lookup"><span data-stu-id="a8ce3-130">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>
* <span data-ttu-id="a8ce3-131">Dados temporários para teste</span><span class="sxs-lookup"><span data-stu-id="a8ce3-131">Temporary data for testing</span></span>
* <span data-ttu-id="a8ce3-132">Dados que depende do estado do banco de dados</span><span class="sxs-lookup"><span data-stu-id="a8ce3-132">Data that depends on database state</span></span>
* <span data-ttu-id="a8ce3-133">Dados que precisam de valores de chave a ser gerado pelo banco de dados, incluindo as entidades que usam chaves alternativas como a identidade</span><span class="sxs-lookup"><span data-stu-id="a8ce3-133">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="a8ce3-134">Dados que requerem transformação personalizada (que não é tratado por [conversões de valor](xref:core/modeling/value-conversions)), como um hash de senha</span><span class="sxs-lookup"><span data-stu-id="a8ce3-134">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="a8ce3-135">Dados que exigem chamadas à API externa, como a criação de usuários e funções do ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="a8ce3-135">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="a8ce3-136">Personalização da migração manual</span><span class="sxs-lookup"><span data-stu-id="a8ce3-136">Manual migration customization</span></span>

<span data-ttu-id="a8ce3-137">Quando uma migração é adicionada as alterações aos dados especificados com `HasData` são transformados em chamadas para `InsertData()`, `UpdateData()`, e `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-137">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="a8ce3-138">Uma maneira de trabalhar em torno de algumas das limitações das `HasData` é adicionar manualmente essas chamadas ou [operações personalizadas](xref:core/managing-schemas/migrations/operations) para a migração em vez disso.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-138">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="a8ce3-139">Lógica de inicialização personalizada</span><span class="sxs-lookup"><span data-stu-id="a8ce3-139">Custom initialization logic</span></span>

<span data-ttu-id="a8ce3-140">Uma maneira simples e poderosa para realizar a propagação de dados é usar [ `DbContext.SaveChanges()` ](xref:core/saving/index) antes que o aplicativo principal lógica inicia a execução.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-140">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="a8ce3-141">O código de propagação não deve ser parte da execução do aplicativo normal, pois isso pode causar problemas de simultaneidade quando várias instâncias estão em execução e também exigiria que o aplicativo com permissão para modificar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-141">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="a8ce3-142">Dependendo das restrições de sua implantação, o código de inicialização pode ser executado de diferentes maneiras:</span><span class="sxs-lookup"><span data-stu-id="a8ce3-142">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>
* <span data-ttu-id="a8ce3-143">Executando o aplicativo de inicialização localmente</span><span class="sxs-lookup"><span data-stu-id="a8ce3-143">Running the initialization app locally</span></span>
* <span data-ttu-id="a8ce3-144">Implantando o aplicativo de inicialização com o aplicativo principal, invocando a rotina de inicialização e desabilitando ou removendo o aplicativo de inicialização.</span><span class="sxs-lookup"><span data-stu-id="a8ce3-144">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="a8ce3-145">Normalmente, isso pode ser automatizado por meio [perfis de publicação](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="a8ce3-145">This can usually be automated by using [publish profiles](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
