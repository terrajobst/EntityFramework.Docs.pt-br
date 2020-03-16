---
title: Glossário de Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402197"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="0ae29-102">Glossário de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0ae29-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="0ae29-103">Code First</span><span class="sxs-lookup"><span data-stu-id="0ae29-103">Code First</span></span>
<span data-ttu-id="0ae29-104">Criar um modelo de Entity Framework usando código.</span><span class="sxs-lookup"><span data-stu-id="0ae29-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="0ae29-105">O modelo pode direcionar um banco de dados existente ou um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0ae29-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="0ae29-106">Contexto</span><span class="sxs-lookup"><span data-stu-id="0ae29-106">Context</span></span>
<span data-ttu-id="0ae29-107">Uma classe que representa uma sessão com o banco de dados, permitindo que você consulte e salve os mesmos.</span><span class="sxs-lookup"><span data-stu-id="0ae29-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="0ae29-108">Um contexto deriva da classe DbContext ou ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="0ae29-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="0ae29-109">Convenção (Code First)</span><span class="sxs-lookup"><span data-stu-id="0ae29-109">Convention (Code First)</span></span>
<span data-ttu-id="0ae29-110">Uma regra que Entity Framework usa para inferir a forma do modelo de suas classes.</span><span class="sxs-lookup"><span data-stu-id="0ae29-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="0ae29-111">Database First</span><span class="sxs-lookup"><span data-stu-id="0ae29-111">Database First</span></span>
<span data-ttu-id="0ae29-112">Criar um modelo de Entity Framework, usando o designer do EF, que tem como alvo um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="0ae29-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="0ae29-113">Carregamento adiantado</span><span class="sxs-lookup"><span data-stu-id="0ae29-113">Eager loading</span></span>
<span data-ttu-id="0ae29-114">Um padrão de carregar dados relacionados em que uma consulta para um tipo de entidade também carrega entidades relacionadas como parte da consulta.</span><span class="sxs-lookup"><span data-stu-id="0ae29-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="0ae29-115">EF Designer</span><span class="sxs-lookup"><span data-stu-id="0ae29-115">EF Designer</span></span>
<span data-ttu-id="0ae29-116">Um designer visual no Visual Studio que permite criar um modelo de Entity Framework usando caixas e linhas.</span><span class="sxs-lookup"><span data-stu-id="0ae29-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="0ae29-117">Entidade</span><span class="sxs-lookup"><span data-stu-id="0ae29-117">Entity</span></span>
<span data-ttu-id="0ae29-118">Uma classe ou um objeto que representa dados de aplicativos, como clientes, produtos e pedidos.</span><span class="sxs-lookup"><span data-stu-id="0ae29-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="0ae29-119">Modelo de dados da entidade</span><span class="sxs-lookup"><span data-stu-id="0ae29-119">Entity Data Model</span></span>
<span data-ttu-id="0ae29-120">Um modelo que descreve as entidades e as relações entre elas.</span><span class="sxs-lookup"><span data-stu-id="0ae29-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="0ae29-121">O EF usa o EDM para descrever o modelo conceitual no qual os programas de desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="0ae29-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="0ae29-122">O EDM se baseia no modelo de relacionamento de entidade introduzido pela Dr. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="0ae29-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="0ae29-123">O EDM foi originalmente desenvolvido com o principal objetivo de se tornar o modelo de dados comum em um conjunto de tecnologias de desenvolvedor e de servidor da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0ae29-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="0ae29-124">O EDM também é usado como parte do protocolo OData.</span><span class="sxs-lookup"><span data-stu-id="0ae29-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="0ae29-125">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="0ae29-125">Explicit loading</span></span>
<span data-ttu-id="0ae29-126">Um padrão de carregar dados relacionados em que objetos relacionados são carregados chamando uma API.</span><span class="sxs-lookup"><span data-stu-id="0ae29-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0ae29-127">API fluente</span><span class="sxs-lookup"><span data-stu-id="0ae29-127">Fluent API</span></span>
<span data-ttu-id="0ae29-128">Uma API que pode ser usada para configurar um modelo de Code First.</span><span class="sxs-lookup"><span data-stu-id="0ae29-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="0ae29-129">Associação de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="0ae29-129">Foreign key association</span></span>
<span data-ttu-id="0ae29-130">Uma associação entre entidades em que uma propriedade que representa a chave estrangeira é incluída na classe da entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="0ae29-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="0ae29-131">Por exemplo, Product contém uma propriedade CategoryId.</span><span class="sxs-lookup"><span data-stu-id="0ae29-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="0ae29-132">Identificando relação</span><span class="sxs-lookup"><span data-stu-id="0ae29-132">Identifying relationship</span></span>
<span data-ttu-id="0ae29-133">Uma relação em que a chave primária da entidade de segurança é parte da chave primária da entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="0ae29-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="0ae29-134">Nesse tipo de relação, a entidade dependente não pode existir sem a entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="0ae29-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="0ae29-135">Associação independente</span><span class="sxs-lookup"><span data-stu-id="0ae29-135">Independent association</span></span>
<span data-ttu-id="0ae29-136">Uma associação entre entidades em que não há nenhuma propriedade representando a chave estrangeira na classe da entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="0ae29-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="0ae29-137">Por exemplo, uma classe Product contém uma relação com Category, mas sem Propriedade CategoryId.</span><span class="sxs-lookup"><span data-stu-id="0ae29-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="0ae29-138">Entity Framework rastreia o estado da Associação independentemente do estado das entidades nas duas extremidades de associação.</span><span class="sxs-lookup"><span data-stu-id="0ae29-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="0ae29-139">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="0ae29-139">Lazy loading</span></span>
<span data-ttu-id="0ae29-140">Um padrão de carregar dados relacionados em que objetos relacionados são carregados automaticamente quando uma propriedade de navegação é acessada.</span><span class="sxs-lookup"><span data-stu-id="0ae29-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="0ae29-141">Model First</span><span class="sxs-lookup"><span data-stu-id="0ae29-141">Model First</span></span>
<span data-ttu-id="0ae29-142">Criar um modelo de Entity Framework, usando o designer do EF, que é usado para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0ae29-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="0ae29-143">Propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="0ae29-143">Navigation property</span></span>
<span data-ttu-id="0ae29-144">Uma propriedade de uma entidade que faz referência a outra entidade.</span><span class="sxs-lookup"><span data-stu-id="0ae29-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="0ae29-145">Por exemplo, Product contém uma propriedade de navegação Category e Category contém uma propriedade de navegação Products.</span><span class="sxs-lookup"><span data-stu-id="0ae29-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="0ae29-146">POCO</span><span class="sxs-lookup"><span data-stu-id="0ae29-146">POCO</span></span>
<span data-ttu-id="0ae29-147">Acrônimo para objeto CLR Plain antigo.</span><span class="sxs-lookup"><span data-stu-id="0ae29-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="0ae29-148">Uma classe de usuário simples que não tem nenhuma dependência com nenhuma estrutura.</span><span class="sxs-lookup"><span data-stu-id="0ae29-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="0ae29-149">No contexto do EF, uma classe de entidade que não é derivada de EntityObject, implementa qualquer interface ou transporta qualquer atributo definido no EF.</span><span class="sxs-lookup"><span data-stu-id="0ae29-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="0ae29-150">Essas classes de entidade que são dissociadas da estrutura de persistência também são consideradas "Persistence que desconhecem".</span><span class="sxs-lookup"><span data-stu-id="0ae29-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="0ae29-151">Inverter relação</span><span class="sxs-lookup"><span data-stu-id="0ae29-151">Relationship inverse</span></span>
<span data-ttu-id="0ae29-152">A extremidade oposta de uma relação, por exemplo, produto. Categoria e categoria. Remessa.</span><span class="sxs-lookup"><span data-stu-id="0ae29-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="0ae29-153">Entidade de rastreamento automático</span><span class="sxs-lookup"><span data-stu-id="0ae29-153">Self-tracking entity</span></span>
<span data-ttu-id="0ae29-154">Uma entidade criada com base em um modelo de geração de código que ajuda com o desenvolvimento de N camadas.</span><span class="sxs-lookup"><span data-stu-id="0ae29-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="0ae29-155">Tipo de tabela por concreto (TPC)</span><span class="sxs-lookup"><span data-stu-id="0ae29-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="0ae29-156">Um método de mapear a herança em que cada tipo não abstrato na hierarquia é mapeado para uma tabela separada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0ae29-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="0ae29-157">Tabela por hierarquia (TPH)</span><span class="sxs-lookup"><span data-stu-id="0ae29-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="0ae29-158">Um método de mapear a herança em que todos os tipos na hierarquia são mapeados para a mesma tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0ae29-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="0ae29-159">Uma ou mais colunas discriminadoras são usadas para identificar a qual tipo cada linha está associada.</span><span class="sxs-lookup"><span data-stu-id="0ae29-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="0ae29-160">Tabela por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="0ae29-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="0ae29-161">Um método de mapear a herança em que as propriedades comuns de todos os tipos na hierarquia são mapeadas para a mesma tabela no banco de dados, mas as propriedades exclusivas de cada tipo são mapeadas para uma tabela separada.</span><span class="sxs-lookup"><span data-stu-id="0ae29-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="0ae29-162">Descoberta de tipo</span><span class="sxs-lookup"><span data-stu-id="0ae29-162">Type discovery</span></span>
<span data-ttu-id="0ae29-163">O processo de identificar os tipos que devem fazer parte de um modelo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0ae29-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
