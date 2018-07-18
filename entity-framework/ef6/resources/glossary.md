---
title: Glossário do Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
caps.latest.revision: 3
ms.openlocfilehash: cbd61838afc23dfb37cee7c624c65476c5270099
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39119936"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="645b0-102">Glossário do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="645b0-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="645b0-103">Code First</span><span class="sxs-lookup"><span data-stu-id="645b0-103">Code First</span></span>
<span data-ttu-id="645b0-104">Criando um modelo do Entity Framework usando código.</span><span class="sxs-lookup"><span data-stu-id="645b0-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="645b0-105">O modelo pode ter como destino e o banco de dados existente ou um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="645b0-105">The model can target and existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="645b0-106">Contexto</span><span class="sxs-lookup"><span data-stu-id="645b0-106">Context</span></span>
<span data-ttu-id="645b0-107">Uma classe que representa uma sessão com o banco de dados, permitindo que você consultar e salvar os dados.</span><span class="sxs-lookup"><span data-stu-id="645b0-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="645b0-108">Um contexto deriva da classe DbContext ou ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="645b0-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="645b0-109">Convenção (Code First)</span><span class="sxs-lookup"><span data-stu-id="645b0-109">Convention (Code First)</span></span>
<span data-ttu-id="645b0-110">Uma regra que usa o Entity Framework para inferir a forma do modelo para você de suas classes.</span><span class="sxs-lookup"><span data-stu-id="645b0-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="645b0-111">Primeiro banco de dados</span><span class="sxs-lookup"><span data-stu-id="645b0-111">Database First</span></span>
<span data-ttu-id="645b0-112">Criando um modelo do Entity Framework, usando o Designer de EF, que tem como alvo um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="645b0-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="645b0-113">Carregamento adiantado</span><span class="sxs-lookup"><span data-stu-id="645b0-113">Eager loading</span></span>
<span data-ttu-id="645b0-114">Um padrão de carregamento de dados relacionados em que uma consulta para um tipo de entidade também carrega entidades relacionadas como parte da consulta.</span><span class="sxs-lookup"><span data-stu-id="645b0-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="645b0-115">Designer EF</span><span class="sxs-lookup"><span data-stu-id="645b0-115">EF Designer</span></span>
<span data-ttu-id="645b0-116">Um designer visual no Visual Studio que permite que você crie um modelo do Entity Framework usando as caixas e linhas.</span><span class="sxs-lookup"><span data-stu-id="645b0-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="645b0-117">Entidade</span><span class="sxs-lookup"><span data-stu-id="645b0-117">Entity</span></span>
<span data-ttu-id="645b0-118">Uma classe ou objeto que representa os dados de aplicativo, como clientes, produtos e pedidos.</span><span class="sxs-lookup"><span data-stu-id="645b0-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="645b0-119">Modelo de Dados de Entidade</span><span class="sxs-lookup"><span data-stu-id="645b0-119">Entity Data Model</span></span>
<span data-ttu-id="645b0-120">Um modelo que descreve entidades e as relações entre eles.</span><span class="sxs-lookup"><span data-stu-id="645b0-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="645b0-121">O EF usa EDM para descrever o modelo conceitual em relação ao qual os programas do desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="645b0-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="645b0-122">EDM se baseia no modelo de relação de entidade, introduzido pela recuperação de desastre. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="645b0-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="645b0-123">O EDM foi desenvolvido originalmente com o principal objetivo de se tornar o modelo de dados comum de um conjunto de tecnologias de desenvolvedor e o servidor da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="645b0-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="645b0-124">O EDM também é usado como parte do protocolo OData.</span><span class="sxs-lookup"><span data-stu-id="645b0-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="645b0-125">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="645b0-125">Explicit loading</span></span>
<span data-ttu-id="645b0-126">Um padrão de carregamento de dados relacionados em que os objetos relacionados são carregados, chamando uma API.</span><span class="sxs-lookup"><span data-stu-id="645b0-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="645b0-127">API fluente</span><span class="sxs-lookup"><span data-stu-id="645b0-127">Fluent API</span></span>
<span data-ttu-id="645b0-128">Uma API que pode ser usada para configurar um modelo Code First.</span><span class="sxs-lookup"><span data-stu-id="645b0-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="645b0-129">Associação de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="645b0-129">Foreign key association</span></span>
<span data-ttu-id="645b0-130">Uma associação entre entidades em que uma propriedade que representa a chave estrangeira está incluída na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="645b0-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="645b0-131">Por exemplo, o produto contém uma propriedade CategoryId.</span><span class="sxs-lookup"><span data-stu-id="645b0-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="645b0-132">Relação de identificação</span><span class="sxs-lookup"><span data-stu-id="645b0-132">Identifying relationship</span></span>
<span data-ttu-id="645b0-133">Uma relação em que a chave primária da entidade de segurança é parte da chave primária da entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="645b0-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="645b0-134">Nesse tipo de relação, a entidade dependente não pode existir sem a entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="645b0-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="645b0-135">Associação independente</span><span class="sxs-lookup"><span data-stu-id="645b0-135">Independent association</span></span>
<span data-ttu-id="645b0-136">Uma associação entre entidades em que não há nenhuma propriedade que representa a chave estrangeira na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="645b0-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="645b0-137">Por exemplo, uma classe Product contém uma relação de categoria, mas nenhuma propriedade CategoryId.</span><span class="sxs-lookup"><span data-stu-id="645b0-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="645b0-138">Entity Framework controla o estado da associação independentemente do estado das entidades nas extremidades de associação de dois.</span><span class="sxs-lookup"><span data-stu-id="645b0-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="645b0-139">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="645b0-139">Lazy loading</span></span>
<span data-ttu-id="645b0-140">Um padrão de carregamento de dados relacionados em que os objetos relacionados são carregados automaticamente quando uma propriedade de navegação é acessada.</span><span class="sxs-lookup"><span data-stu-id="645b0-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="645b0-141">Model First</span><span class="sxs-lookup"><span data-stu-id="645b0-141">Model First</span></span>
<span data-ttu-id="645b0-142">Criar um modelo do Entity Framework, usando o Designer de EF, o que, em seguida, é usado para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="645b0-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="645b0-143">Propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="645b0-143">Navigation property</span></span>
<span data-ttu-id="645b0-144">Uma propriedade de uma entidade que faz referência a outra entidade.</span><span class="sxs-lookup"><span data-stu-id="645b0-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="645b0-145">Por exemplo, o produto contém uma propriedade de navegação de categoria e categoria contém uma propriedade de navegação de produtos.</span><span class="sxs-lookup"><span data-stu-id="645b0-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="645b0-146">POCO</span><span class="sxs-lookup"><span data-stu-id="645b0-146">POCO</span></span>
<span data-ttu-id="645b0-147">Acrônimo para objeto Plain Old CLR.</span><span class="sxs-lookup"><span data-stu-id="645b0-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="645b0-148">Uma classe de usuário simples que não tem nenhuma dependência com qualquer estrutura.</span><span class="sxs-lookup"><span data-stu-id="645b0-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="645b0-149">No contexto do EF, um uma classe de entidade que não é derivado de EntityObject, implementar quaisquer interfaces ou executa qualquer atributo definido no EF.</span><span class="sxs-lookup"><span data-stu-id="645b0-149">In the context of EF, a an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="645b0-150">Essas classes de entidade que são separados da estrutura de persistência também devem ser "com ignorância de persistência".</span><span class="sxs-lookup"><span data-stu-id="645b0-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="645b0-151">Inverso da relação</span><span class="sxs-lookup"><span data-stu-id="645b0-151">Relationship inverse</span></span>
<span data-ttu-id="645b0-152">A extremidade oposta de uma relação, por exemplo, o produto. Categoria e categoria. Produto.</span><span class="sxs-lookup"><span data-stu-id="645b0-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="645b0-153">Entidade de autocontrole</span><span class="sxs-lookup"><span data-stu-id="645b0-153">Self-tracking entity</span></span>
<span data-ttu-id="645b0-154">Entidade criada a partir de um modelo de geração de código que ajuda com o desenvolvimento de N camadas.</span><span class="sxs-lookup"><span data-stu-id="645b0-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="645b0-155">Tipo de tabela por concreto (TPC)</span><span class="sxs-lookup"><span data-stu-id="645b0-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="645b0-156">Um método de mapeamento de herança em que cada tipo de não-abstrata na hierarquia é mapeado para separar a tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="645b0-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="645b0-157">Tabela por hierarquia (TPH)</span><span class="sxs-lookup"><span data-stu-id="645b0-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="645b0-158">Um método de mapeamento de herança em que todos os tipos na hierarquia são mapeados para a mesma tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="645b0-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="645b0-159">Um discriminador coluna (s) é usado para identificar qual tipo cada linha está associado.</span><span class="sxs-lookup"><span data-stu-id="645b0-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="645b0-160">Tabela por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="645b0-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="645b0-161">Um método de mapeamento de herança em que as propriedades comuns de todos os tipos na hierarquia são mapeadas para a mesma tabela no banco de dados, mas propriedades exclusivas para cada tipo são mapeadas para uma tabela separada.</span><span class="sxs-lookup"><span data-stu-id="645b0-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="645b0-162">Descoberta do tipo</span><span class="sxs-lookup"><span data-stu-id="645b0-162">Type discovery</span></span>
<span data-ttu-id="645b0-163">O processo de identificar os tipos que devem fazer parte de um modelo do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="645b0-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
