---
title: Tipos de entidade-EF Core
description: Como configurar e mapear tipos de entidade usando Entity Framework Core
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502449"
---
# <a name="entity-types"></a><span data-ttu-id="ec267-103">Tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="ec267-103">Entity Types</span></span>

<span data-ttu-id="ec267-104">Incluir um DbSet de um tipo no seu contexto significa que ele está incluído no modelo de EF Core; Geralmente, nos referimos a um tipo como uma *entidade*.</span><span class="sxs-lookup"><span data-stu-id="ec267-104">Including a DbSet of a type on your context means that it is included in EF Core's model; we usually refer to such a type as an *entity*.</span></span> <span data-ttu-id="ec267-105">EF Core pode ler e gravar instâncias de entidade de/para o banco de dados e, se você estiver usando um banco de dados relacional, EF Core poderá criar tabelas para suas entidades por meio de migrações.</span><span class="sxs-lookup"><span data-stu-id="ec267-105">EF Core can read and write entity instances from/to the database, and if you're using a relational database, EF Core can create tables for your entities via migrations.</span></span>

## <a name="including-types-in-the-model"></a><span data-ttu-id="ec267-106">Incluindo tipos no modelo</span><span class="sxs-lookup"><span data-stu-id="ec267-106">Including types in the model</span></span>

<span data-ttu-id="ec267-107">Por convenção, os tipos expostos nas propriedades DbSet no seu contexto são incluídos no modelo como entidades.</span><span class="sxs-lookup"><span data-stu-id="ec267-107">By convention, types that are exposed in DbSet properties on your context are included in the model as entities.</span></span> <span data-ttu-id="ec267-108">Os tipos de entidade que são especificados no método `OnModelCreating` também são incluídos, assim como os tipos que são encontrados por explorar recursivamente as propriedades de navegação de outros tipos de entidade descobertas.</span><span class="sxs-lookup"><span data-stu-id="ec267-108">Entity types that are specified in the `OnModelCreating` method are also included, as are any types that are found by recursively exploring the navigation properties of other discovered entity types.</span></span>

<span data-ttu-id="ec267-109">No exemplo de código abaixo, todos os tipos são incluídos:</span><span class="sxs-lookup"><span data-stu-id="ec267-109">In the code sample below, all types are included:</span></span>

* <span data-ttu-id="ec267-110">`Blog` está incluído porque é exposto em uma propriedade DbSet no contexto.</span><span class="sxs-lookup"><span data-stu-id="ec267-110">`Blog` is included because it's exposed in a DbSet property on the context.</span></span>
* <span data-ttu-id="ec267-111">`Post` está incluído porque é descoberto por meio da propriedade de navegação `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="ec267-111">`Post` is included because it's discovered via the `Blog.Posts` navigation property.</span></span>
* <span data-ttu-id="ec267-112">`AuditEntry` porque ele está especificado em `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="ec267-112">`AuditEntry` because it is specified in `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a><span data-ttu-id="ec267-113">Excluindo tipos do modelo</span><span class="sxs-lookup"><span data-stu-id="ec267-113">Excluding types from the model</span></span>

<span data-ttu-id="ec267-114">Se você não quiser que um tipo seja incluído no modelo, você pode excluí-lo:</span><span class="sxs-lookup"><span data-stu-id="ec267-114">If you don't want a type to be included in the model, you can exclude it:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="ec267-115">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="ec267-115">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="ec267-116">API fluente</span><span class="sxs-lookup"><span data-stu-id="ec267-116">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a><span data-ttu-id="ec267-117">Nome da tabela</span><span class="sxs-lookup"><span data-stu-id="ec267-117">Table name</span></span>

<span data-ttu-id="ec267-118">Por convenção, cada tipo de entidade será configurado para mapear para uma tabela de banco de dados com o mesmo nome que a propriedade DbSet que expõe a entidade.</span><span class="sxs-lookup"><span data-stu-id="ec267-118">By convention, each entity type will be set up to map to a database table with the same name as the DbSet property that exposes the entity.</span></span> <span data-ttu-id="ec267-119">Se não existir nenhum DbSet para a entidade especificada, o nome da classe será usado.</span><span class="sxs-lookup"><span data-stu-id="ec267-119">If no DbSet exists for the given entity, the class name is used.</span></span>

<span data-ttu-id="ec267-120">Você pode configurar manualmente o nome da tabela:</span><span class="sxs-lookup"><span data-stu-id="ec267-120">You can manually configure the table name:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="ec267-121">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="ec267-121">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="ec267-122">API fluente</span><span class="sxs-lookup"><span data-stu-id="ec267-122">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a><span data-ttu-id="ec267-123">Esquema de tabela</span><span class="sxs-lookup"><span data-stu-id="ec267-123">Table schema</span></span>

<span data-ttu-id="ec267-124">Ao usar um banco de dados relacional, as tabelas são por convenção criadas no esquema padrão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ec267-124">When using a relational database, tables are by convention created in your database's default schema.</span></span> <span data-ttu-id="ec267-125">Por exemplo, Microsoft SQL Server usará o esquema de `dbo` (o SQLite não oferece suporte a esquemas).</span><span class="sxs-lookup"><span data-stu-id="ec267-125">For example, Microsoft SQL Server will use the `dbo` schema (SQLite does not support schemas).</span></span>

<span data-ttu-id="ec267-126">Você pode configurar tabelas a serem criadas em um esquema específico da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ec267-126">You can configure tables to be created in a specific schema as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="ec267-127">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="ec267-127">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="ec267-128">API fluente</span><span class="sxs-lookup"><span data-stu-id="ec267-128">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

<span data-ttu-id="ec267-129">Em vez de especificar o esquema para cada tabela, você também pode definir o esquema padrão no nível do modelo com a API Fluent:</span><span class="sxs-lookup"><span data-stu-id="ec267-129">Rather than specifying the schema for each table, you can also define the default schema at the model level with the fluent API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

<span data-ttu-id="ec267-130">Observe que a configuração do esquema padrão também afetará outros objetos de banco de dados, como sequências.</span><span class="sxs-lookup"><span data-stu-id="ec267-130">Note that setting the default schema will also affect other database objects, such as sequences.</span></span>
