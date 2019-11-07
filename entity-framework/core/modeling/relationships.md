---
title: Relações-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655672"
---
# <a name="relationships"></a><span data-ttu-id="16c8d-102">Relações</span><span class="sxs-lookup"><span data-stu-id="16c8d-102">Relationships</span></span>

<span data-ttu-id="16c8d-103">Uma relação define como duas entidades se relacionam entre si.</span><span class="sxs-lookup"><span data-stu-id="16c8d-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="16c8d-104">Em um banco de dados relacional, isso é representado por uma restrição FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="16c8d-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="16c8d-105">A maioria dos exemplos neste artigo usa uma relação um-para-muitos para demonstrar conceitos.</span><span class="sxs-lookup"><span data-stu-id="16c8d-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="16c8d-106">Para obter exemplos de relações um-para-um e muitos para muitos, consulte a seção [outros padrões de relação](#other-relationship-patterns) no final do artigo.</span><span class="sxs-lookup"><span data-stu-id="16c8d-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="16c8d-107">Definição de termos</span><span class="sxs-lookup"><span data-stu-id="16c8d-107">Definition of Terms</span></span>

<span data-ttu-id="16c8d-108">Há vários termos usados para descrever as relações</span><span class="sxs-lookup"><span data-stu-id="16c8d-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="16c8d-109">**Entidade dependente:** Esta é a entidade que contém as propriedades de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16c8d-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="16c8d-110">Às vezes, chamado de ' filho ' da relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="16c8d-111">**Entidade principal:** Esta é a entidade que contém as propriedades de chave primária/alternativa.</span><span class="sxs-lookup"><span data-stu-id="16c8d-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="16c8d-112">Às vezes, chamado de ' pai ' da relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="16c8d-113">**Chave estrangeira:** As propriedades na entidade dependente que é usada para armazenar os valores da propriedade de chave principal à qual a entidade está relacionada.</span><span class="sxs-lookup"><span data-stu-id="16c8d-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="16c8d-114">**Chave principal:** As propriedades que identificam exclusivamente a entidade principal.</span><span class="sxs-lookup"><span data-stu-id="16c8d-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="16c8d-115">Essa pode ser a chave primária ou uma chave alternativa.</span><span class="sxs-lookup"><span data-stu-id="16c8d-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="16c8d-116">**Propriedade de navegação:** Uma propriedade definida na entidade principal e/ou dependente que contém uma referência (s) para as entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16c8d-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="16c8d-117">**Propriedade de navegação da coleção:** Uma propriedade de navegação que contém referências a muitas entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16c8d-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="16c8d-118">**Propriedade de navegação de referência:** Uma propriedade de navegação que mantém uma referência a uma única entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="16c8d-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="16c8d-119">**Propriedade de navegação inversa:** Ao discutir uma propriedade de navegação específica, esse termo refere-se à propriedade de navegação na outra extremidade da relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="16c8d-120">A listagem de código a seguir mostra uma relação um-para-muitos entre `Blog` e `Post`</span><span class="sxs-lookup"><span data-stu-id="16c8d-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="16c8d-121">`Post` é a entidade dependente</span><span class="sxs-lookup"><span data-stu-id="16c8d-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="16c8d-122">`Blog` é a entidade principal</span><span class="sxs-lookup"><span data-stu-id="16c8d-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="16c8d-123">`Post.BlogId` é a chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="16c8d-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="16c8d-124">`Blog.BlogId` é a chave principal (nesse caso, é uma chave primária em vez de uma chave alternativa)</span><span class="sxs-lookup"><span data-stu-id="16c8d-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="16c8d-125">`Post.Blog` é uma propriedade de navegação de referência</span><span class="sxs-lookup"><span data-stu-id="16c8d-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="16c8d-126">`Blog.Posts` é uma propriedade de navegação de coleção</span><span class="sxs-lookup"><span data-stu-id="16c8d-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="16c8d-127">`Post.Blog` é a propriedade de navegação inversa de `Blog.Posts` (e vice-versa)</span><span class="sxs-lookup"><span data-stu-id="16c8d-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="16c8d-128">Convenções</span><span class="sxs-lookup"><span data-stu-id="16c8d-128">Conventions</span></span>

<span data-ttu-id="16c8d-129">Por convenção, uma relação será criada quando houver uma propriedade de navegação descoberta em um tipo.</span><span class="sxs-lookup"><span data-stu-id="16c8d-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="16c8d-130">Uma propriedade é considerada uma propriedade de navegação se o tipo que ele aponta não puder ser mapeado como um tipo escalar pelo provedor de banco de dados atual.</span><span class="sxs-lookup"><span data-stu-id="16c8d-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="16c8d-131">As relações descobertas por convenção serão sempre direcionadas à chave primária da entidade principal.</span><span class="sxs-lookup"><span data-stu-id="16c8d-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="16c8d-132">Para direcionar uma chave alternativa, a configuração adicional deve ser executada usando a API Fluent.</span><span class="sxs-lookup"><span data-stu-id="16c8d-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="16c8d-133">Relações totalmente definidas</span><span class="sxs-lookup"><span data-stu-id="16c8d-133">Fully Defined Relationships</span></span>

<span data-ttu-id="16c8d-134">O padrão mais comum para relações é ter propriedades de navegação definidas em ambas as extremidades da relação e uma propriedade de chave estrangeira definida na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="16c8d-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="16c8d-135">Se um par de propriedades de navegação for encontrado entre dois tipos, eles serão configurados como propriedades de navegação inversas da mesma relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="16c8d-136">Se a entidade dependente contiver uma propriedade chamada `<primary key property name>`, `<navigation property name><primary key property name>`ou `<principal entity name><primary key property name>`, ela será configurada como a chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16c8d-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="16c8d-137">Se houver várias propriedades de navegação definidas entre dois tipos (ou seja, mais de um par distinto de navegações que apontam entre si), nenhuma relação será criada por convenção e você precisará configurá-las manualmente para identificar como o Propriedades de navegação emparelhadas.</span><span class="sxs-lookup"><span data-stu-id="16c8d-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="16c8d-138">Nenhuma propriedade de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="16c8d-138">No Foreign Key Property</span></span>

<span data-ttu-id="16c8d-139">Embora seja recomendável ter uma propriedade de chave estrangeira definida na classe de entidade dependente, ela não é necessária.</span><span class="sxs-lookup"><span data-stu-id="16c8d-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="16c8d-140">Se nenhuma propriedade de chave estrangeira for encontrada, uma propriedade de chave estrangeira de sombra será introduzida com o nome `<navigation property name><principal key property name>` (consulte [Propriedades de sombra](shadow-properties.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="16c8d-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="16c8d-141">Propriedade de navegação única</span><span class="sxs-lookup"><span data-stu-id="16c8d-141">Single Navigation Property</span></span>

<span data-ttu-id="16c8d-142">Incluir apenas uma propriedade de navegação (sem navegação inversa e nenhuma propriedade de chave estrangeira) é suficiente para ter uma relação definida por convenção.</span><span class="sxs-lookup"><span data-stu-id="16c8d-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="16c8d-143">Você também pode ter uma única propriedade de navegação e uma propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16c8d-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="16c8d-144">Excluir em cascata</span><span class="sxs-lookup"><span data-stu-id="16c8d-144">Cascade Delete</span></span>

<span data-ttu-id="16c8d-145">Por convenção, a exclusão em cascata será definida como *em cascata* para relações necessárias e *ClientSetNull* para relações opcionais.</span><span class="sxs-lookup"><span data-stu-id="16c8d-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="16c8d-146">*Cascade* significa que as entidades dependentes também são excluídas.</span><span class="sxs-lookup"><span data-stu-id="16c8d-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="16c8d-147">*ClientSetNull* significa que as entidades dependentes que não estão carregadas na memória permanecerão inalteradas e deverão ser excluídas manualmente ou atualizadas para apontar para uma entidade principal válida.</span><span class="sxs-lookup"><span data-stu-id="16c8d-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="16c8d-148">Para entidades que são carregadas na memória, EF Core tentará definir as propriedades de chave estrangeira como NULL.</span><span class="sxs-lookup"><span data-stu-id="16c8d-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="16c8d-149">Consulte a seção [relações obrigatórias e opcionais](#required-and-optional-relationships) para obter a diferença entre as relações obrigatórias e opcionais.</span><span class="sxs-lookup"><span data-stu-id="16c8d-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="16c8d-150">Consulte [exclusão em cascata](../saving/cascade-delete.md) para obter mais detalhes sobre os diferentes comportamentos de exclusão e os padrões usados pela Convenção.</span><span class="sxs-lookup"><span data-stu-id="16c8d-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="16c8d-151">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="16c8d-151">Data Annotations</span></span>

<span data-ttu-id="16c8d-152">Há duas anotações de dados que podem ser usadas para configurar relações, `[ForeignKey]` e `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="16c8d-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="16c8d-153">Eles estão disponíveis no namespace `System.ComponentModel.DataAnnotations.Schema`.</span><span class="sxs-lookup"><span data-stu-id="16c8d-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="16c8d-154">ForeignKey</span><span class="sxs-lookup"><span data-stu-id="16c8d-154">[ForeignKey]</span></span>

<span data-ttu-id="16c8d-155">Você pode usar as anotações de dados para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="16c8d-156">Isso normalmente é feito quando a propriedade de chave estrangeira não é descoberta pela Convenção.</span><span class="sxs-lookup"><span data-stu-id="16c8d-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="16c8d-157">A anotação `[ForeignKey]` pode ser colocada em qualquer propriedade de navegação na relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="16c8d-158">Ele não precisa ir para a propriedade de navegação na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="16c8d-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="16c8d-159">[Inversoproperty]</span><span class="sxs-lookup"><span data-stu-id="16c8d-159">[InverseProperty]</span></span>

<span data-ttu-id="16c8d-160">Você pode usar as anotações de dados para configurar como as propriedades de navegação nas entidades dependentes e de entidade emparelham.</span><span class="sxs-lookup"><span data-stu-id="16c8d-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="16c8d-161">Isso normalmente é feito quando há mais de um par de propriedades de navegação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="16c8d-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="16c8d-162">API fluente</span><span class="sxs-lookup"><span data-stu-id="16c8d-162">Fluent API</span></span>

<span data-ttu-id="16c8d-163">Para configurar uma relação na API fluente, você começa identificando as propriedades de navegação que compõem a relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="16c8d-164">`HasOne` ou `HasMany` identifica a propriedade de navegação no tipo de entidade em que você está iniciando a configuração.</span><span class="sxs-lookup"><span data-stu-id="16c8d-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="16c8d-165">Em seguida, você encadea uma chamada para `WithOne` ou `WithMany` para identificar a navegação inversa.</span><span class="sxs-lookup"><span data-stu-id="16c8d-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="16c8d-166">os `WithOne` de /`HasOne`são usados para propriedades de navegação de referência e `HasMany`/de `WithMany` são usados para propriedades de navegação de coleção.</span><span class="sxs-lookup"><span data-stu-id="16c8d-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="16c8d-167">Propriedade de navegação única</span><span class="sxs-lookup"><span data-stu-id="16c8d-167">Single Navigation Property</span></span>

<span data-ttu-id="16c8d-168">Se você tiver apenas uma propriedade de navegação, haverá sobrecargas sem parâmetros de `WithOne` e `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="16c8d-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="16c8d-169">Isso indica que há uma referência ou uma coleção conceitualmente na outra extremidade da relação, mas não há nenhuma propriedade de navegação incluída na classe de entidade.</span><span class="sxs-lookup"><span data-stu-id="16c8d-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="16c8d-170">Chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="16c8d-170">Foreign Key</span></span>

<span data-ttu-id="16c8d-171">Você pode usar a API fluente para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="16c8d-172">A listagem de código a seguir mostra como configurar uma chave estrangeira composta.</span><span class="sxs-lookup"><span data-stu-id="16c8d-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="16c8d-173">Você pode usar a sobrecarga de cadeia de caracteres de `HasForeignKey(...)` para configurar uma propriedade de sombra como uma chave estrangeira (consulte [Propriedades de sombra](shadow-properties.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="16c8d-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="16c8d-174">É recomendável adicionar explicitamente a propriedade Shadow ao modelo antes de usá-la como uma chave estrangeira (como mostrado abaixo).</span><span class="sxs-lookup"><span data-stu-id="16c8d-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="16c8d-175">Sem propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="16c8d-175">Without Navigation Property</span></span>

<span data-ttu-id="16c8d-176">Você não precisa necessariamente fornecer uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="16c8d-177">Você pode simplesmente fornecer uma chave estrangeira em um lado da relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="16c8d-178">Chave principal</span><span class="sxs-lookup"><span data-stu-id="16c8d-178">Principal Key</span></span>

<span data-ttu-id="16c8d-179">Se desejar que a chave estrangeira referencie uma propriedade diferente da chave primária, você poderá usar a API Fluent para configurar a propriedade principal de chave para a relação.</span><span class="sxs-lookup"><span data-stu-id="16c8d-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="16c8d-180">A propriedade que você configurar como a chave principal será automaticamente configurada como uma chave alternativa (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="16c8d-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

<span data-ttu-id="16c8d-181">A listagem de código a seguir mostra como configurar uma chave de entidade composta.</span><span class="sxs-lookup"><span data-stu-id="16c8d-181">The following code listing shows how to configure a composite principal key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="16c8d-182">A ordem na qual você especifica as propriedades da chave de entidade de segurança deve corresponder à ordem na qual elas são especificadas para a chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16c8d-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="16c8d-183">Relações obrigatórias e opcionais</span><span class="sxs-lookup"><span data-stu-id="16c8d-183">Required and Optional Relationships</span></span>

<span data-ttu-id="16c8d-184">Você pode usar a API fluente para configurar se a relação é necessária ou opcional.</span><span class="sxs-lookup"><span data-stu-id="16c8d-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="16c8d-185">Por fim, isso controla se a propriedade de chave estrangeira é necessária ou opcional.</span><span class="sxs-lookup"><span data-stu-id="16c8d-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="16c8d-186">Isso é mais útil quando você está usando uma chave estrangeira de estado de sombra.</span><span class="sxs-lookup"><span data-stu-id="16c8d-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="16c8d-187">Se você tiver uma propriedade de chave estrangeira em sua classe de entidade, a exigência da relação será determinada com base em se a propriedade de chave estrangeira é necessária ou opcional (consulte [as propriedades obrigatórias e opcionais](required-optional.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="16c8d-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a><span data-ttu-id="16c8d-188">Excluir em cascata</span><span class="sxs-lookup"><span data-stu-id="16c8d-188">Cascade Delete</span></span>

<span data-ttu-id="16c8d-189">Você pode usar a API Fluent para configurar o comportamento de exclusão em cascata para uma determinada relação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="16c8d-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="16c8d-190">Consulte [exclusão em cascata](../saving/cascade-delete.md) na seção salvando dados para obter uma discussão detalhada de cada opção.</span><span class="sxs-lookup"><span data-stu-id="16c8d-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="16c8d-191">Outros padrões de relação</span><span class="sxs-lookup"><span data-stu-id="16c8d-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="16c8d-192">Um para um</span><span class="sxs-lookup"><span data-stu-id="16c8d-192">One-to-one</span></span>

<span data-ttu-id="16c8d-193">Relações um para um têm uma propriedade de navegação de referência em ambos os lados.</span><span class="sxs-lookup"><span data-stu-id="16c8d-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="16c8d-194">Eles seguem as mesmas convenções que as relações um-para-muitos, mas um índice exclusivo é introduzido na propriedade Foreign Key para garantir que apenas um dependente esteja relacionado a cada entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="16c8d-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> <span data-ttu-id="16c8d-195">O EF escolherá uma das entidades como dependente, com base em sua capacidade de detectar uma propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16c8d-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="16c8d-196">Se a entidade incorreta for escolhida como dependente, você poderá usar a API fluente para corrigir isso.</span><span class="sxs-lookup"><span data-stu-id="16c8d-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="16c8d-197">Ao configurar a relação com a API Fluent, você usa os métodos `HasOne` e `WithOne`.</span><span class="sxs-lookup"><span data-stu-id="16c8d-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="16c8d-198">Ao configurar a chave estrangeira, você precisa especificar o tipo de entidade dependente-Observe o parâmetro genérico fornecido para `HasForeignKey` na lista abaixo.</span><span class="sxs-lookup"><span data-stu-id="16c8d-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="16c8d-199">Em uma relação um-para-muitos, fica claro que a entidade com a navegação de referência é a dependente e aquela com a coleção é a principal.</span><span class="sxs-lookup"><span data-stu-id="16c8d-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="16c8d-200">Mas isso não é tão em relação um-para-um-portanto, a necessidade de defini-lo explicitamente.</span><span class="sxs-lookup"><span data-stu-id="16c8d-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="16c8d-201">Muitos para muitos</span><span class="sxs-lookup"><span data-stu-id="16c8d-201">Many-to-many</span></span>

<span data-ttu-id="16c8d-202">Relações muitos para muitos sem uma classe de entidade para representar a tabela de junção ainda não têm suporte.</span><span class="sxs-lookup"><span data-stu-id="16c8d-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="16c8d-203">No entanto, você pode representar uma relação muitos para muitos incluindo uma classe de entidade para a tabela de junção e mapeando duas relações um-para-muitos separadas.</span><span class="sxs-lookup"><span data-stu-id="16c8d-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
