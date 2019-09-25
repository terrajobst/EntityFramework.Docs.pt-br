---
title: Portando de EF6 para EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 42e40ce769a67a987883027e1807ec7eaeb4ad7a
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198034"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="3867b-102">Portando de EF6 para EF Core</span><span class="sxs-lookup"><span data-stu-id="3867b-102">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="3867b-103">Devido às mudanças fundamentais no EF Core, não recomendamos tentar mover um aplicativo EF6 para o EF Core, a menos que você tenha um motivo convincente para fazer essa alteração.</span><span class="sxs-lookup"><span data-stu-id="3867b-103">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="3867b-104">Você deve entender a mudança de EF6 para EF Core como portabilidade, em vez de uma upgrade.</span><span class="sxs-lookup"><span data-stu-id="3867b-104">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3867b-105">Antes de você começar o processo de portabilidade, é importante validar se o EF Core atende aos requisitos de acesso a dados do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3867b-105">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="3867b-106">Recursos ausentes</span><span class="sxs-lookup"><span data-stu-id="3867b-106">Missing features</span></span>

<span data-ttu-id="3867b-107">Verifique se o EF Core possui todos os recursos que você precisa usar em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3867b-107">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="3867b-108">Confira [Comparação de recursos](xref:efcore-and-ef6/index) para obter uma comparação detalhada entre o conjunto de recursos do EF Core e do EF6.</span><span class="sxs-lookup"><span data-stu-id="3867b-108">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="3867b-109">Se algum dos recursos obrigatórios estiver ausente, garanta que você possa compensar a falta desses recursos antes da portabilidade para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="3867b-109">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="3867b-110">Alterações de comportamento</span><span class="sxs-lookup"><span data-stu-id="3867b-110">Behavior changes</span></span>

<span data-ttu-id="3867b-111">Esta é uma lista parcial de algumas mudanças no comportamento entre o EF6 e o EF Core.</span><span class="sxs-lookup"><span data-stu-id="3867b-111">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="3867b-112">É importante tê-las em mente ao portar seu aplicativo, já que elas podem mudar a forma como seu aplicativo se comporta, mas não aparecerão como erros de compilação após a troca para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="3867b-112">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="3867b-113">Comportamento de DbSet.Add/Attach e graph</span><span class="sxs-lookup"><span data-stu-id="3867b-113">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="3867b-114">No EF6, chamar `DbSet.Add()` em uma entidade resulta em uma pesquisa recursiva de todas as entidades referenciadas em suas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="3867b-114">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="3867b-115">Quaisquer entidades encontradas e que ainda não são rastreadas pelo contexto também são marcadas como adicionadas.</span><span class="sxs-lookup"><span data-stu-id="3867b-115">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="3867b-116">O `DbSet.Attach()` se comporta da mesma forma, mas todas as entidades são marcadas como inalteradas.</span><span class="sxs-lookup"><span data-stu-id="3867b-116">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="3867b-117">**O EF Core realiza uma pesquisa recursiva semelhante, mas com algumas regras ligeiramente diferentes.**</span><span class="sxs-lookup"><span data-stu-id="3867b-117">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="3867b-118">A entidade raiz está sempre no estado solicitado (adicionado para `DbSet.Add` e inalterado para `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="3867b-118">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="3867b-119">**Para entidades encontradas durante a pesquisa recursiva de propriedades de navegação:**</span><span class="sxs-lookup"><span data-stu-id="3867b-119">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="3867b-120">**Se a chave primária da entidade for gerada pelo repositório**</span><span class="sxs-lookup"><span data-stu-id="3867b-120">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="3867b-121">Se a chave primária não for definida com um valor, o estado será definido como adicionado.</span><span class="sxs-lookup"><span data-stu-id="3867b-121">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="3867b-122">O valor da chave primária será considerado "não definido" se for atribuído a ele o valor padrão de CLR do tipo de propriedade (por exemplo, `0` para `int`, `null` para `string` etc.).</span><span class="sxs-lookup"><span data-stu-id="3867b-122">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="3867b-123">Se a chave primária for definida com um valor, o estado será definido como inalterado.</span><span class="sxs-lookup"><span data-stu-id="3867b-123">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="3867b-124">Se a chave primária não for gerada pelo banco de dados, a entidade será colocada no mesmo estado que a raiz.</span><span class="sxs-lookup"><span data-stu-id="3867b-124">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="3867b-125">Inicialização de banco de dados Code First</span><span class="sxs-lookup"><span data-stu-id="3867b-125">Code First database initialization</span></span>

<span data-ttu-id="3867b-126">**O EF6 realiza uma quantidade significativa de mágica ao selecionar a conexão de banco de dados e inicializar o banco de dados. Essas regras incluem:**</span><span class="sxs-lookup"><span data-stu-id="3867b-126">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="3867b-127">Se nenhuma configuração for realizada, o EF6 selecionará um banco de dados no SQL Express ou LocalDb.</span><span class="sxs-lookup"><span data-stu-id="3867b-127">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="3867b-128">Se uma cadeia de conexão com o mesmo nome que o contexto estiver no arquivo `App/Web.config` dos aplicativos, essa conexão será usada.</span><span class="sxs-lookup"><span data-stu-id="3867b-128">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="3867b-129">Se o banco de dados não existir, ele será criado.</span><span class="sxs-lookup"><span data-stu-id="3867b-129">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="3867b-130">Se nenhuma das tabelas do modelo existir no banco de dados, o esquema do modelo atual será adicionado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3867b-130">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="3867b-131">Se as migrações estiverem habilitadas, elas serão usadas para criar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3867b-131">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="3867b-132">Se o banco de dados existir e o EF6 tiver criado o esquema anteriormente, o esquema será verificado quanto à compatibilidade com o modelo atual.</span><span class="sxs-lookup"><span data-stu-id="3867b-132">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="3867b-133">Uma exceção será lançada se o modelo tiver mudado desde a criação do esquema.</span><span class="sxs-lookup"><span data-stu-id="3867b-133">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="3867b-134">**O EF Core não faz nenhuma parte dessa mágica.**</span><span class="sxs-lookup"><span data-stu-id="3867b-134">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="3867b-135">A conexão de banco de dados precisa ser configurada explicitamente no código.</span><span class="sxs-lookup"><span data-stu-id="3867b-135">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="3867b-136">Nenhuma inicialização é realizada.</span><span class="sxs-lookup"><span data-stu-id="3867b-136">No initialization is performed.</span></span> <span data-ttu-id="3867b-137">Você deve usar `DbContext.Database.Migrate()` para aplicar migrações (ou `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` para criar/excluir o banco de dados sem usar migrações).</span><span class="sxs-lookup"><span data-stu-id="3867b-137">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="3867b-138">Convenção de nomenclatura de tabela Code First</span><span class="sxs-lookup"><span data-stu-id="3867b-138">Code First table naming convention</span></span>

<span data-ttu-id="3867b-139">O EF6 executa o nome de classe da entidade por meio de um serviço de pluralização para calcular o nome de tabela padrão para a qual a entidade é mapeada.</span><span class="sxs-lookup"><span data-stu-id="3867b-139">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="3867b-140">O EF Core usa o nome da propriedade `DbSet` à qual a entidade é exposta no contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="3867b-140">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="3867b-141">Se a entidade não tiver uma propriedade `DbSet`, o nome de classe será usado.</span><span class="sxs-lookup"><span data-stu-id="3867b-141">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
