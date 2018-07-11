---
title: Portando de EF6 para EF Core – validar requisitos
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949108"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="e63d9-102">Antes de portabilidade do EF6 para EF Core: validar os requisitos do aplicativo</span><span class="sxs-lookup"><span data-stu-id="e63d9-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="e63d9-103">Antes de iniciar o processo de portabilidade é importante validar que o EF Core atende aos requisitos de acesso a dados para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e63d9-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="e63d9-104">Recursos ausentes</span><span class="sxs-lookup"><span data-stu-id="e63d9-104">Missing features</span></span>

<span data-ttu-id="e63d9-105">Certifique-se de que o EF Core tem todos os recursos que você precisa usar em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e63d9-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="e63d9-106">Ver [comparação de recursos](../features.md) para uma comparação detalhada de como o conjunto de recursos no EF Core se compara ao EF6.</span><span class="sxs-lookup"><span data-stu-id="e63d9-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="e63d9-107">Se quaisquer recursos necessários estão ausentes, certifique-se de que você pode compensar a falta desses recursos antes de portar para EF Core.</span><span class="sxs-lookup"><span data-stu-id="e63d9-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="e63d9-108">Alterações de comportamento</span><span class="sxs-lookup"><span data-stu-id="e63d9-108">Behavior changes</span></span>

<span data-ttu-id="e63d9-109">Esta é uma lista parcial de algumas alterações no comportamento entre o EF6 e EF Core.</span><span class="sxs-lookup"><span data-stu-id="e63d9-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="e63d9-110">É importante manter essas práticas como sua porta de seu aplicativo como eles podem alterar a maneira que seu aplicativo se comporta, mas não será exibida como erros de compilação após a troca para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="e63d9-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="e63d9-111">Comportamento de DbSet.Add/Attach e grafo</span><span class="sxs-lookup"><span data-stu-id="e63d9-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="e63d9-112">No EF6, chamar `DbSet.Add()` em resultados de uma entidade em uma pesquisa recursiva para todas as entidades referenciadas em suas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="e63d9-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="e63d9-113">Qualquer entidade que é encontrada e já não é controlada pelo contexto, também é marcadas como adicionadas.</span><span class="sxs-lookup"><span data-stu-id="e63d9-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="e63d9-114">`DbSet.Attach()` se comporta da mesma, exceto pelo fato de todas as entidades são marcadas como inalterado.</span><span class="sxs-lookup"><span data-stu-id="e63d9-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="e63d9-115">**O EF Core executa uma pesquisa recursiva semelhante, mas com algumas regras ligeiramente diferentes.**</span><span class="sxs-lookup"><span data-stu-id="e63d9-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="e63d9-116">A entidade raiz sempre está no estado solicitado (adicionado `DbSet.Add` e inalterado para `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="e63d9-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="e63d9-117">**Para entidades que são encontradas durante a pesquisa recursiva de propriedades de navegação:**</span><span class="sxs-lookup"><span data-stu-id="e63d9-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="e63d9-118">**Se a chave primária da entidade é geradas pelo repositório**</span><span class="sxs-lookup"><span data-stu-id="e63d9-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="e63d9-119">Se a chave primária não está definida como um valor, o estado é definido como adicionado.</span><span class="sxs-lookup"><span data-stu-id="e63d9-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="e63d9-120">O valor de chave primária é considerado "não definido" se ele é atribuído o valor de padrão do CLR para o tipo de propriedade (por exemplo, `0` para `int`, `null` para `string`, etc.).</span><span class="sxs-lookup"><span data-stu-id="e63d9-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="e63d9-121">Se a chave primária é definida como um valor, o estado é definido como inalterado.</span><span class="sxs-lookup"><span data-stu-id="e63d9-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="e63d9-122">Se a chave primária não é um banco de dados gerado, a entidade será colocada no mesmo estado, como a raiz.</span><span class="sxs-lookup"><span data-stu-id="e63d9-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="e63d9-123">Primeira inicialização do banco de dados de código</span><span class="sxs-lookup"><span data-stu-id="e63d9-123">Code First database initialization</span></span>

<span data-ttu-id="e63d9-124">**EF6 tem uma quantidade significativa de mágica, que ele executa em torno de selecionar a conexão de banco de dados e inicializar o banco de dados. Algumas dessas regras incluem:**</span><span class="sxs-lookup"><span data-stu-id="e63d9-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="e63d9-125">Se nenhuma configuração é realizada, o EF6 selecionará um banco de dados SQL Express ou LocalDb.</span><span class="sxs-lookup"><span data-stu-id="e63d9-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="e63d9-126">Se uma cadeia de caracteres de conexão com o mesmo nome que o contexto é em aplicativos de `App/Web.config` arquivo, essa conexão será usada.</span><span class="sxs-lookup"><span data-stu-id="e63d9-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="e63d9-127">Se o banco de dados não existir, ele é criado.</span><span class="sxs-lookup"><span data-stu-id="e63d9-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="e63d9-128">Se nenhuma das tabelas do modelo existir no banco de dados, o esquema para o modelo atual é adicionado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e63d9-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="e63d9-129">Se as migrações são habilitadas, eles são usados para criar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e63d9-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="e63d9-130">Se o banco de dados existe e EF6 tivesse criado anteriormente o esquema, o esquema é verificado para compatibilidade com o modelo atual.</span><span class="sxs-lookup"><span data-stu-id="e63d9-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="e63d9-131">Uma exceção é lançada se o modelo foi alterado desde que o esquema foi criado.</span><span class="sxs-lookup"><span data-stu-id="e63d9-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="e63d9-132">**O EF Core não executa essa mágica.**</span><span class="sxs-lookup"><span data-stu-id="e63d9-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="e63d9-133">A conexão de banco de dados deve ser configurado explicitamente no código.</span><span class="sxs-lookup"><span data-stu-id="e63d9-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="e63d9-134">Nenhuma inicialização será executada.</span><span class="sxs-lookup"><span data-stu-id="e63d9-134">No initialization is performed.</span></span> <span data-ttu-id="e63d9-135">Você deve usar `DbContext.Database.Migrate()` para aplicar as migrações (ou `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` para criar/excluir o banco de dados sem usar as migrações).</span><span class="sxs-lookup"><span data-stu-id="e63d9-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="e63d9-136">Tabela do primeiro código convenção de nomenclatura</span><span class="sxs-lookup"><span data-stu-id="e63d9-136">Code First table naming convention</span></span>

<span data-ttu-id="e63d9-137">EF6 é executado o nome de classe de entidade por meio de um serviço de pluralização para calcular o nome da tabela padrão que a entidade é mapeada para.</span><span class="sxs-lookup"><span data-stu-id="e63d9-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="e63d9-138">EF Core usa o nome da `DbSet` propriedade que a entidade é exposta no contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="e63d9-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="e63d9-139">Se a entidade não tiver um `DbSet` propriedade e, em seguida, o nome da classe é usada.</span><span class="sxs-lookup"><span data-stu-id="e63d9-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
