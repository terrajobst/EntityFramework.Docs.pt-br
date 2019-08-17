---
title: Portando de EF6 para requisitos de validação de EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565352"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="23b6b-102">Antes de portar de EF6 para EF Core: Validar os requisitos do aplicativo</span><span class="sxs-lookup"><span data-stu-id="23b6b-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="23b6b-103">Antes de iniciar o processo de portabilidade, é importante validar que EF Core atende aos requisitos de acesso de dados para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="23b6b-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="23b6b-104">Recursos ausentes</span><span class="sxs-lookup"><span data-stu-id="23b6b-104">Missing features</span></span>

<span data-ttu-id="23b6b-105">Verifique se EF Core tem todos os recursos que você precisa usar em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="23b6b-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="23b6b-106">Consulte [comparação de recursos](../features.md) para obter uma comparação detalhada de como o conjunto de recursos no EF Core se compara com EF6.</span><span class="sxs-lookup"><span data-stu-id="23b6b-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="23b6b-107">Se algum recurso necessário estiver ausente, verifique se você pode compensar a falta desses recursos antes de portar para EF Core.</span><span class="sxs-lookup"><span data-stu-id="23b6b-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="23b6b-108">Alterações de comportamento</span><span class="sxs-lookup"><span data-stu-id="23b6b-108">Behavior changes</span></span>

<span data-ttu-id="23b6b-109">Esta é uma lista não exaustiva de algumas alterações no comportamento entre EF6 e EF Core.</span><span class="sxs-lookup"><span data-stu-id="23b6b-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="23b6b-110">É importante ter isso em mente como sua porta de seu aplicativo, pois eles podem mudar a forma com que seu aplicativo se comporta, mas não aparecerá como erros de compilação após a troca para EF Core.</span><span class="sxs-lookup"><span data-stu-id="23b6b-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="23b6b-111">DbSet. Adicionar/anexar e comportamento de grafo</span><span class="sxs-lookup"><span data-stu-id="23b6b-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="23b6b-112">No EF6, a `DbSet.Add()` chamada em uma entidade resulta em uma pesquisa recursiva de todas as entidades referenciadas em suas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="23b6b-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="23b6b-113">Todas as entidades que são encontradas e ainda não são rastreadas pelo contexto também são marcadas como adicionadas.</span><span class="sxs-lookup"><span data-stu-id="23b6b-113">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="23b6b-114">`DbSet.Attach()`comporta-se, exceto que todas as entidades são marcadas como inalteradas.</span><span class="sxs-lookup"><span data-stu-id="23b6b-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="23b6b-115">**EF Core executa uma pesquisa recursiva semelhante, mas com algumas regras ligeiramente diferentes.**</span><span class="sxs-lookup"><span data-stu-id="23b6b-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="23b6b-116">A entidade raiz sempre está no estado solicitado (adicionada para `DbSet.Add` e inalterada para `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="23b6b-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="23b6b-117">**Para entidades que são encontradas durante a pesquisa recursiva de propriedades de navegação:**</span><span class="sxs-lookup"><span data-stu-id="23b6b-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="23b6b-118">**Se a chave primária da entidade for gerada pelo armazenamento**</span><span class="sxs-lookup"><span data-stu-id="23b6b-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="23b6b-119">Se a chave primária não estiver definida como um valor, o estado será definido como adicionado.</span><span class="sxs-lookup"><span data-stu-id="23b6b-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="23b6b-120">O valor da chave primária será considerado "não definido" se for atribuído o valor padrão do CLR para o tipo de propriedade (por `0` exemplo `int`, `null` para `string`, para, etc.).</span><span class="sxs-lookup"><span data-stu-id="23b6b-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="23b6b-121">Se a chave primária for definida como um valor, o estado será definido como inalterado.</span><span class="sxs-lookup"><span data-stu-id="23b6b-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="23b6b-122">Se a chave primária não for gerada pelo banco de dados, a entidade será colocada no mesmo estado que a raiz.</span><span class="sxs-lookup"><span data-stu-id="23b6b-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="23b6b-123">Inicialização do banco de dados Code First</span><span class="sxs-lookup"><span data-stu-id="23b6b-123">Code First database initialization</span></span>

<span data-ttu-id="23b6b-124">**O EF6 tem uma quantidade significativa de mágica que ele realiza em relação à seleção da conexão de banco de dados e à inicialização do banco de dados. Algumas dessas regras incluem:**</span><span class="sxs-lookup"><span data-stu-id="23b6b-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="23b6b-125">Se nenhuma configuração for executada, o EF6 selecionará um banco de dados no SQL Express ou LocalDb.</span><span class="sxs-lookup"><span data-stu-id="23b6b-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="23b6b-126">Se uma cadeia de conexão com o mesmo nome que o contexto estiver no arquivo `App/Web.config` de aplicativos, essa conexão será usada.</span><span class="sxs-lookup"><span data-stu-id="23b6b-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="23b6b-127">Se o banco de dados não existir, ele será criado.</span><span class="sxs-lookup"><span data-stu-id="23b6b-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="23b6b-128">Se nenhuma das tabelas do modelo existir no banco de dados, o esquema para o modelo atual será adicionado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="23b6b-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="23b6b-129">Se as migrações estiverem habilitadas, elas serão usadas para criar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="23b6b-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="23b6b-130">Se o banco de dados existir e EF6 tiver criado o esquema anteriormente, o esquema será verificado quanto à compatibilidade com o modelo atual.</span><span class="sxs-lookup"><span data-stu-id="23b6b-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="23b6b-131">Uma exceção será gerada se o modelo tiver sido alterado desde que o esquema foi criado.</span><span class="sxs-lookup"><span data-stu-id="23b6b-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="23b6b-132">**EF Core não realiza nenhuma dessas mágicas.**</span><span class="sxs-lookup"><span data-stu-id="23b6b-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="23b6b-133">A conexão de banco de dados deve ser explicitamente configurada no código.</span><span class="sxs-lookup"><span data-stu-id="23b6b-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="23b6b-134">Nenhuma inicialização é executada.</span><span class="sxs-lookup"><span data-stu-id="23b6b-134">No initialization is performed.</span></span> <span data-ttu-id="23b6b-135">Você deve usar `DbContext.Database.Migrate()` para aplicar migrações (ou `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` para criar/excluir o banco de dados sem usar as migrações).</span><span class="sxs-lookup"><span data-stu-id="23b6b-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="23b6b-136">Code First Convenção de nomenclatura de tabela</span><span class="sxs-lookup"><span data-stu-id="23b6b-136">Code First table naming convention</span></span>

<span data-ttu-id="23b6b-137">EF6 executa o nome da classe de entidade por meio de um serviço de pluralização para calcular o nome de tabela padrão ao qual a entidade está mapeada.</span><span class="sxs-lookup"><span data-stu-id="23b6b-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="23b6b-138">EF Core usa o nome da `DbSet` Propriedade na qual a entidade é exposta no contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="23b6b-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="23b6b-139">Se a entidade não tiver uma `DbSet` Propriedade, o nome da classe será usado.</span><span class="sxs-lookup"><span data-stu-id="23b6b-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
