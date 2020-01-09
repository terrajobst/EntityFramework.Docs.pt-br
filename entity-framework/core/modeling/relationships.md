---
title: Relações-EF Core
description: Como configurar relações entre tipos de entidade ao usar Entity Framework Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 6b3e0636bfa266b78baafe1b6e318c9707294560
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502182"
---
# <a name="relationships"></a><span data-ttu-id="804c3-103">Relações</span><span class="sxs-lookup"><span data-stu-id="804c3-103">Relationships</span></span>

<span data-ttu-id="804c3-104">Uma relação define como duas entidades se relacionam entre si.</span><span class="sxs-lookup"><span data-stu-id="804c3-104">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="804c3-105">Em um banco de dados relacional, isso é representado por uma restrição FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="804c3-105">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="804c3-106">A maioria dos exemplos neste artigo usa uma relação um-para-muitos para demonstrar conceitos.</span><span class="sxs-lookup"><span data-stu-id="804c3-106">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="804c3-107">Para obter exemplos de relações um-para-um e muitos para muitos, consulte a seção [outros padrões de relação](#other-relationship-patterns) no final do artigo.</span><span class="sxs-lookup"><span data-stu-id="804c3-107">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="804c3-108">Definição de termos</span><span class="sxs-lookup"><span data-stu-id="804c3-108">Definition of terms</span></span>

<span data-ttu-id="804c3-109">Há vários termos usados para descrever as relações</span><span class="sxs-lookup"><span data-stu-id="804c3-109">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="804c3-110">**Entidade dependente:** Essa é a entidade que contém as propriedades de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="804c3-110">**Dependent entity:** This is the entity that contains the foreign key properties.</span></span> <span data-ttu-id="804c3-111">Às vezes, chamado de ' filho ' da relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-111">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="804c3-112">**Entidade principal:** Esta é a entidade que contém as propriedades de chave primária/alternativa.</span><span class="sxs-lookup"><span data-stu-id="804c3-112">**Principal entity:** This is the entity that contains the primary/alternate key properties.</span></span> <span data-ttu-id="804c3-113">Às vezes, chamado de ' pai ' da relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-113">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="804c3-114">**Chave estrangeira:** As propriedades na entidade dependente que são usadas para armazenar os valores de chave de entidade de segurança para a entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="804c3-114">**Foreign key:** The properties in the dependent entity that are used to store the principal key values for the related entity.</span></span>

* <span data-ttu-id="804c3-115">**Chave principal:** As propriedades que identificam exclusivamente a entidade principal.</span><span class="sxs-lookup"><span data-stu-id="804c3-115">**Principal key:** The properties that uniquely identify the principal entity.</span></span> <span data-ttu-id="804c3-116">Essa pode ser a chave primária ou uma chave alternativa.</span><span class="sxs-lookup"><span data-stu-id="804c3-116">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="804c3-117">**Propriedade de navegação:** Uma propriedade definida na entidade principal e/ou dependente que faz referência à entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="804c3-117">**Navigation property:** A property defined on the principal and/or dependent entity that references the related entity.</span></span>

  * <span data-ttu-id="804c3-118">**Propriedade de navegação da coleção:** Uma propriedade de navegação que contém referências a muitas entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="804c3-118">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="804c3-119">**Propriedade de navegação de referência:** Uma propriedade de navegação que mantém uma referência a uma única entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="804c3-119">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="804c3-120">**Propriedade de navegação inversa:** Ao discutir uma propriedade de navegação específica, esse termo refere-se à propriedade de navegação na outra extremidade da relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-120">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>
  
* <span data-ttu-id="804c3-121">**Relação de auto-referência:** Uma relação na qual os tipos de entidade dependente e principal são os mesmos.</span><span class="sxs-lookup"><span data-stu-id="804c3-121">**Self-referencing relationship:** A relationship in which the dependent and the principal entity types are the same.</span></span>

<span data-ttu-id="804c3-122">O código a seguir mostra uma relação um-para-muitos entre `Blog` e `Post`</span><span class="sxs-lookup"><span data-stu-id="804c3-122">The following code shows a one-to-many relationship between `Blog` and `Post`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Full)]

* <span data-ttu-id="804c3-123">`Post` é a entidade dependente</span><span class="sxs-lookup"><span data-stu-id="804c3-123">`Post` is the dependent entity</span></span>

* <span data-ttu-id="804c3-124">`Blog` é a entidade principal</span><span class="sxs-lookup"><span data-stu-id="804c3-124">`Blog` is the principal entity</span></span>

* <span data-ttu-id="804c3-125">`Post.BlogId` é a chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="804c3-125">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="804c3-126">`Blog.BlogId` é a chave principal (nesse caso, é uma chave primária em vez de uma chave alternativa)</span><span class="sxs-lookup"><span data-stu-id="804c3-126">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="804c3-127">`Post.Blog` é uma propriedade de navegação de referência</span><span class="sxs-lookup"><span data-stu-id="804c3-127">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="804c3-128">`Blog.Posts` é uma propriedade de navegação de coleção</span><span class="sxs-lookup"><span data-stu-id="804c3-128">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="804c3-129">`Post.Blog` é a propriedade de navegação inversa de `Blog.Posts` (e vice-versa)</span><span class="sxs-lookup"><span data-stu-id="804c3-129">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

## <a name="conventions"></a><span data-ttu-id="804c3-130">Convenções</span><span class="sxs-lookup"><span data-stu-id="804c3-130">Conventions</span></span>

<span data-ttu-id="804c3-131">Por padrão, uma relação será criada quando houver uma propriedade de navegação descoberta em um tipo.</span><span class="sxs-lookup"><span data-stu-id="804c3-131">By default, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="804c3-132">Uma propriedade é considerada uma propriedade de navegação se o tipo que ele aponta não puder ser mapeado como um tipo escalar pelo provedor de banco de dados atual.</span><span class="sxs-lookup"><span data-stu-id="804c3-132">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="804c3-133">As relações descobertas por convenção serão sempre direcionadas à chave primária da entidade principal.</span><span class="sxs-lookup"><span data-stu-id="804c3-133">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="804c3-134">Para direcionar uma chave alternativa, a configuração adicional deve ser executada usando a API Fluent.</span><span class="sxs-lookup"><span data-stu-id="804c3-134">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="804c3-135">Relações totalmente definidas</span><span class="sxs-lookup"><span data-stu-id="804c3-135">Fully defined relationships</span></span>

<span data-ttu-id="804c3-136">O padrão mais comum para relações é ter propriedades de navegação definidas em ambas as extremidades da relação e uma propriedade de chave estrangeira definida na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="804c3-136">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="804c3-137">Se um par de propriedades de navegação for encontrado entre dois tipos, eles serão configurados como propriedades de navegação inversas da mesma relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-137">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="804c3-138">Se a entidade dependente contiver uma propriedade com um nome correspondente a um desses padrões, ela será configurada como a chave estrangeira:</span><span class="sxs-lookup"><span data-stu-id="804c3-138">If the dependent entity contains a property with a name matching one of these patterns then it will be configured as the foreign key:</span></span>
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Full&highlight=6,15-16)]

<span data-ttu-id="804c3-139">Neste exemplo, as propriedades realçadas serão usadas para configurar a relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-139">In this example the highlighted properties will be used to configure the relationship.</span></span>

> [!NOTE]
> <span data-ttu-id="804c3-140">Se a propriedade for a chave primária ou for de um tipo não compatível com a chave principal, ela não será configurada como a chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="804c3-140">If the property is the primary key or is of a type not compatible with the principal key then it won't be configured as the foreign key.</span></span>

> [!NOTE]
> <span data-ttu-id="804c3-141">Antes de EF Core 3,0, a propriedade denominada exatamente igual à propriedade da chave principal [também foi correspondida como a chave estrangeira](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span><span class="sxs-lookup"><span data-stu-id="804c3-141">Before EF Core 3.0 the property named exactly the same as the principal key property [was also matched as the foreign key](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="804c3-142">Nenhuma propriedade de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="804c3-142">No foreign key property</span></span>

<span data-ttu-id="804c3-143">Embora seja recomendável ter uma propriedade de chave estrangeira definida na classe de entidade dependente, ela não é necessária.</span><span class="sxs-lookup"><span data-stu-id="804c3-143">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="804c3-144">Se nenhuma propriedade de chave estrangeira for encontrada, uma [propriedade de chave estrangeira de sombra](shadow-properties.md) será introduzida com o nome `<navigation property name><principal key property name>` ou `<principal entity name><principal key property name>` se nenhuma navegação estiver presente no tipo dependente.</span><span class="sxs-lookup"><span data-stu-id="804c3-144">If no foreign key property is found, a [shadow foreign key property](shadow-properties.md) will be introduced with the name `<navigation property name><principal key property name>` or `<principal entity name><principal key property name>` if no navigation is present on the dependent type.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=6,15)]

<span data-ttu-id="804c3-145">Neste exemplo, a chave estrangeira de sombra é `BlogId` porque a prependência do nome de navegação seria redundante.</span><span class="sxs-lookup"><span data-stu-id="804c3-145">In this example the shadow foreign key is `BlogId` because prepending the navigation name would be redundant.</span></span>

> [!NOTE]
> <span data-ttu-id="804c3-146">Se já existir uma propriedade com o mesmo nome, o nome da propriedade de sombra será sufixado com um número.</span><span class="sxs-lookup"><span data-stu-id="804c3-146">If a property with the same name already exists then the shadow property name will be suffixed with a number.</span></span>

### <a name="single-navigation-property"></a><span data-ttu-id="804c3-147">Propriedade de navegação única</span><span class="sxs-lookup"><span data-stu-id="804c3-147">Single navigation property</span></span>

<span data-ttu-id="804c3-148">Incluir apenas uma propriedade de navegação (sem navegação inversa e nenhuma propriedade de chave estrangeira) é suficiente para ter uma relação definida por convenção.</span><span class="sxs-lookup"><span data-stu-id="804c3-148">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="804c3-149">Você também pode ter uma única propriedade de navegação e uma propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="804c3-149">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=OneNavigation&highlight=6)]

### <a name="limitations"></a><span data-ttu-id="804c3-150">Limitações</span><span class="sxs-lookup"><span data-stu-id="804c3-150">Limitations</span></span>

<span data-ttu-id="804c3-151">Quando há várias propriedades de navegação definidas entre dois tipos (ou seja, mais do que apenas um par de navegações que apontam entre si), as relações representadas pelas propriedades de navegação são ambíguas.</span><span class="sxs-lookup"><span data-stu-id="804c3-151">When there are multiple navigation properties defined between two types (that is, more than just one pair of navigations that point to each other) the relationships represented by the navigation properties are ambiguous.</span></span> <span data-ttu-id="804c3-152">Será necessário configurá-los manualmente para resolver a ambiguidade.</span><span class="sxs-lookup"><span data-stu-id="804c3-152">You will need to manually configure them to resolve the ambiguity.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="804c3-153">Exclusão em cascata</span><span class="sxs-lookup"><span data-stu-id="804c3-153">Cascade delete</span></span>

<span data-ttu-id="804c3-154">Por convenção, a exclusão em cascata será definida como *em cascata* para relações necessárias e *ClientSetNull* para relações opcionais.</span><span class="sxs-lookup"><span data-stu-id="804c3-154">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="804c3-155">*Cascade* significa que as entidades dependentes também são excluídas.</span><span class="sxs-lookup"><span data-stu-id="804c3-155">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="804c3-156">*ClientSetNull* significa que as entidades dependentes que não estão carregadas na memória permanecerão inalteradas e deverão ser excluídas manualmente ou atualizadas para apontar para uma entidade principal válida.</span><span class="sxs-lookup"><span data-stu-id="804c3-156">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="804c3-157">Para entidades que são carregadas na memória, EF Core tentará definir as propriedades de chave estrangeira como NULL.</span><span class="sxs-lookup"><span data-stu-id="804c3-157">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="804c3-158">Consulte a seção [relações obrigatórias e opcionais](#required-and-optional-relationships) para obter a diferença entre as relações obrigatórias e opcionais.</span><span class="sxs-lookup"><span data-stu-id="804c3-158">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="804c3-159">Consulte [exclusão em cascata](../saving/cascade-delete.md) para obter mais detalhes sobre os diferentes comportamentos de exclusão e os padrões usados pela Convenção.</span><span class="sxs-lookup"><span data-stu-id="804c3-159">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="manual-configuration"></a><span data-ttu-id="804c3-160">Configuração manual</span><span class="sxs-lookup"><span data-stu-id="804c3-160">Manual configuration</span></span>

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="804c3-161">API fluente</span><span class="sxs-lookup"><span data-stu-id="804c3-161">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="804c3-162">Para configurar uma relação na API fluente, você começa identificando as propriedades de navegação que compõem a relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="804c3-163">`HasOne` ou `HasMany` identifica a propriedade de navegação no tipo de entidade em que você está iniciando a configuração.</span><span class="sxs-lookup"><span data-stu-id="804c3-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="804c3-164">Em seguida, você encadea uma chamada para `WithOne` ou `WithMany` para identificar a navegação inversa.</span><span class="sxs-lookup"><span data-stu-id="804c3-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="804c3-165">os `WithOne` de /`HasOne`são usados para propriedades de navegação de referência e `HasMany`/de `WithMany` são usados para propriedades de navegação de coleção.</span><span class="sxs-lookup"><span data-stu-id="804c3-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=8-10)]

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="804c3-166">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="804c3-166">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="804c3-167">Você pode usar as anotações de dados para configurar como as propriedades de navegação nas entidades dependentes e de entidade emparelham.</span><span class="sxs-lookup"><span data-stu-id="804c3-167">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="804c3-168">Isso normalmente é feito quando há mais de um par de propriedades de navegação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="804c3-168">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?name=InverseProperty&highlight=20,23)]

> [!NOTE]
> <span data-ttu-id="804c3-169">Você só pode usar [Required] em Propriedades na entidade dependente para afetar a necessidade da relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-169">You can only use [Required] on properties on the dependent entity to impact the requiredness of the relationship.</span></span> <span data-ttu-id="804c3-170">[Obrigatório] na navegação da entidade principal geralmente é ignorado, mas pode fazer com que a entidade se torne a dependente.</span><span class="sxs-lookup"><span data-stu-id="804c3-170">[Required] on the navigation from the principal entity is usually ignored, but it may cause the entity to become the dependent one.</span></span>

> [!NOTE]
> <span data-ttu-id="804c3-171">As anotações de dados `[ForeignKey]` e `[InverseProperty]` estão disponíveis no namespace `System.ComponentModel.DataAnnotations.Schema`.</span><span class="sxs-lookup"><span data-stu-id="804c3-171">The data annotations `[ForeignKey]` and `[InverseProperty]` are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span> <span data-ttu-id="804c3-172">`[Required]` está disponível no namespace `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="804c3-172">`[Required]` is available in the `System.ComponentModel.DataAnnotations` namespace.</span></span>

---

### <a name="single-navigation-property"></a><span data-ttu-id="804c3-173">Propriedade de navegação única</span><span class="sxs-lookup"><span data-stu-id="804c3-173">Single navigation property</span></span>

<span data-ttu-id="804c3-174">Se você tiver apenas uma propriedade de navegação, haverá sobrecargas sem parâmetros de `WithOne` e `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="804c3-174">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="804c3-175">Isso indica que há uma referência ou uma coleção conceitualmente na outra extremidade da relação, mas não há nenhuma propriedade de navegação incluída na classe de entidade.</span><span class="sxs-lookup"><span data-stu-id="804c3-175">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?name=OneNavigation&highlight=8-10)]

### <a name="foreign-key"></a><span data-ttu-id="804c3-176">Chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="804c3-176">Foreign key</span></span>

#### <a name="fluent-api-simple-keytabfluent-api-simple-key"></a>[<span data-ttu-id="804c3-177">API Fluent (chave simples)</span><span class="sxs-lookup"><span data-stu-id="804c3-177">Fluent API (simple key)</span></span>](#tab/fluent-api-simple-key)

<span data-ttu-id="804c3-178">Você pode usar a API fluente para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação:</span><span class="sxs-lookup"><span data-stu-id="804c3-178">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?name=ForeignKey&highlight=11)]

#### <a name="fluent-api-composite-keytabfluent-api-composite-key"></a>[<span data-ttu-id="804c3-179">API Fluent (chave composta)</span><span class="sxs-lookup"><span data-stu-id="804c3-179">Fluent API (composite key)</span></span>](#tab/fluent-api-composite-key)

<span data-ttu-id="804c3-180">Você pode usar a API Fluent para configurar quais propriedades devem ser usadas como as propriedades de chave estrangeira de composição para uma determinada relação:</span><span class="sxs-lookup"><span data-stu-id="804c3-180">You can use the Fluent API to configure which properties should be used as the composite foreign key properties for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?name=CompositeForeignKey&highlight=13)]

#### <a name="data-annotations-simple-keytabdata-annotations-simple-key"></a>[<span data-ttu-id="804c3-181">Anotações de dados (chave simples)</span><span class="sxs-lookup"><span data-stu-id="804c3-181">Data annotations (simple key)</span></span>](#tab/data-annotations-simple-key)

<span data-ttu-id="804c3-182">Você pode usar as anotações de dados para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-182">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="804c3-183">Isso normalmente é feito quando a propriedade de chave estrangeira não é descoberta pela Convenção:</span><span class="sxs-lookup"><span data-stu-id="804c3-183">This is typically done when the foreign key property is not discovered by convention:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?name=ForeignKey&highlight=17)]

> [!TIP]  
> <span data-ttu-id="804c3-184">A anotação `[ForeignKey]` pode ser colocada em qualquer propriedade de navegação na relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-184">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="804c3-185">Ele não precisa ir para a propriedade de navegação na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="804c3-185">It does not need to go on the navigation property in the dependent entity class.</span></span>

> [!NOTE]
> <span data-ttu-id="804c3-186">A propriedade especificada usando `[ForeignKey]` em uma propriedade de navegação não precisa existir no tipo dependente.</span><span class="sxs-lookup"><span data-stu-id="804c3-186">The property specified using `[ForeignKey]` on a navigation property doesn't need to exist the the dependent type.</span></span> <span data-ttu-id="804c3-187">Nesse caso, o nome especificado será usado para criar uma chave estrangeira de sombra.</span><span class="sxs-lookup"><span data-stu-id="804c3-187">In this case the specified name will be used to create a shadow foreign key.</span></span>

---

#### <a name="shadow-foreign-key"></a><span data-ttu-id="804c3-188">Chave estrangeira de sombra</span><span class="sxs-lookup"><span data-stu-id="804c3-188">Shadow foreign key</span></span>

<span data-ttu-id="804c3-189">Você pode usar a sobrecarga de cadeia de caracteres de `HasForeignKey(...)` para configurar uma propriedade de sombra como uma chave estrangeira (consulte [Propriedades de sombra](shadow-properties.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="804c3-189">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="804c3-190">É recomendável adicionar explicitamente a propriedade Shadow ao modelo antes de usá-la como uma chave estrangeira (como mostrado abaixo).</span><span class="sxs-lookup"><span data-stu-id="804c3-190">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs?name=ShadowForeignKey&highlight=10,16)]

#### <a name="foreign-key-constraint-name"></a><span data-ttu-id="804c3-191">Nome da restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="804c3-191">Foreign key constraint name</span></span>

<span data-ttu-id="804c3-192">Por convenção, ao direcionar um banco de dados relacional, as restrições de chave estrangeira são nomeadas FK_<dependent type name> _<principal type name>_ <foreign key property name>.</span><span class="sxs-lookup"><span data-stu-id="804c3-192">By convention, when targeting a relational database, foreign key constraints are named FK_<dependent type name>_<principal type name>_<foreign key property name>.</span></span> <span data-ttu-id="804c3-193">Para chaves estrangeiras compostas <foreign key property name> se torna uma lista separada por sublinhado de nomes de propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="804c3-193">For composite foreign keys <foreign key property name> becomes an underscore separated list of foreign key property names.</span></span>

<span data-ttu-id="804c3-194">Você também pode configurar o nome da restrição da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="804c3-194">You can also configure the constraint name as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ConstraintName.cs?name=ConstraintName&highlight=6-7)]

### <a name="without-navigation-property"></a><span data-ttu-id="804c3-195">Sem propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="804c3-195">Without navigation property</span></span>

<span data-ttu-id="804c3-196">Você não precisa necessariamente fornecer uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="804c3-196">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="804c3-197">Você pode simplesmente fornecer uma chave estrangeira em um lado da relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-197">You can simply provide a foreign key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?name=NoNavigation&highlight=8-11)]

### <a name="principal-key"></a><span data-ttu-id="804c3-198">Chave principal</span><span class="sxs-lookup"><span data-stu-id="804c3-198">Principal key</span></span>

<span data-ttu-id="804c3-199">Se desejar que a chave estrangeira referencie uma propriedade diferente da chave primária, você poderá usar a API Fluent para configurar a propriedade principal de chave para a relação.</span><span class="sxs-lookup"><span data-stu-id="804c3-199">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="804c3-200">A propriedade que você configurar como a chave principal será automaticamente configurada como uma [chave alternativa](alternate-keys.md).</span><span class="sxs-lookup"><span data-stu-id="804c3-200">The property that you configure as the principal key will automatically be setup as an [alternate key](alternate-keys.md).</span></span>

#### <a name="simple-keytabsimple-key"></a>[<span data-ttu-id="804c3-201">Chave simples</span><span class="sxs-lookup"><span data-stu-id="804c3-201">Simple key</span></span>](#tab/simple-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

#### <a name="composite-keytabcomposite-key"></a>[<span data-ttu-id="804c3-202">Chave composta</span><span class="sxs-lookup"><span data-stu-id="804c3-202">Composite key</span></span>](#tab/composite-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=CompositePrincipalKey&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="804c3-203">A ordem na qual você especifica as propriedades da chave de entidade de segurança deve corresponder à ordem na qual elas são especificadas para a chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="804c3-203">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

---

### <a name="required-and-optional-relationships"></a><span data-ttu-id="804c3-204">Relações obrigatórias e opcionais</span><span class="sxs-lookup"><span data-stu-id="804c3-204">Required and optional relationships</span></span>

<span data-ttu-id="804c3-205">Você pode usar a API fluente para configurar se a relação é necessária ou opcional.</span><span class="sxs-lookup"><span data-stu-id="804c3-205">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="804c3-206">Por fim, isso controla se a propriedade de chave estrangeira é necessária ou opcional.</span><span class="sxs-lookup"><span data-stu-id="804c3-206">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="804c3-207">Isso é mais útil quando você está usando uma chave estrangeira de estado de sombra.</span><span class="sxs-lookup"><span data-stu-id="804c3-207">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="804c3-208">Se você tiver uma propriedade de chave estrangeira em sua classe de entidade, a exigência da relação será determinada com base em se a propriedade de chave estrangeira é necessária ou opcional (consulte [as propriedades obrigatórias e opcionais](required-optional.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="804c3-208">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=6)]

> [!NOTE]
> <span data-ttu-id="804c3-209">Chamar `IsRequired(false)` também torna a propriedade de chave estrangeira opcional, a menos que esteja configurada de outra forma.</span><span class="sxs-lookup"><span data-stu-id="804c3-209">Calling `IsRequired(false)` also makes the foreign key property optional unless it's configured otherwise.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="804c3-210">Exclusão em cascata</span><span class="sxs-lookup"><span data-stu-id="804c3-210">Cascade delete</span></span>

<span data-ttu-id="804c3-211">Você pode usar a API Fluent para configurar o comportamento de exclusão em cascata para uma determinada relação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="804c3-211">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="804c3-212">Consulte [exclusão em cascata](../saving/cascade-delete.md) para obter uma discussão detalhada de cada opção.</span><span class="sxs-lookup"><span data-stu-id="804c3-212">See [Cascade Delete](../saving/cascade-delete.md) for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=6)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="804c3-213">Outros padrões de relação</span><span class="sxs-lookup"><span data-stu-id="804c3-213">Other relationship patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="804c3-214">Um-para-um</span><span class="sxs-lookup"><span data-stu-id="804c3-214">One-to-one</span></span>

<span data-ttu-id="804c3-215">Relações um para um têm uma propriedade de navegação de referência em ambos os lados.</span><span class="sxs-lookup"><span data-stu-id="804c3-215">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="804c3-216">Eles seguem as mesmas convenções que as relações um-para-muitos, mas um índice exclusivo é introduzido na propriedade Foreign Key para garantir que apenas um dependente esteja relacionado a cada entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="804c3-216">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=OneToOne&highlight=6,15-16)]

> [!NOTE]  
> <span data-ttu-id="804c3-217">O EF escolherá uma das entidades como dependente, com base em sua capacidade de detectar uma propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="804c3-217">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="804c3-218">Se a entidade incorreta for escolhida como dependente, você poderá usar a API fluente para corrigir isso.</span><span class="sxs-lookup"><span data-stu-id="804c3-218">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="804c3-219">Ao configurar a relação com a API Fluent, você usa os métodos `HasOne` e `WithOne`.</span><span class="sxs-lookup"><span data-stu-id="804c3-219">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="804c3-220">Ao configurar a chave estrangeira, você precisa especificar o tipo de entidade dependente-Observe o parâmetro genérico fornecido para `HasForeignKey` na lista abaixo.</span><span class="sxs-lookup"><span data-stu-id="804c3-220">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="804c3-221">Em uma relação um-para-muitos, fica claro que a entidade com a navegação de referência é a dependente e aquela com a coleção é a principal.</span><span class="sxs-lookup"><span data-stu-id="804c3-221">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="804c3-222">Mas isso não é tão em relação um-para-um-portanto, a necessidade de defini-lo explicitamente.</span><span class="sxs-lookup"><span data-stu-id="804c3-222">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="804c3-223">Muitos para muitos</span><span class="sxs-lookup"><span data-stu-id="804c3-223">Many-to-many</span></span>

<span data-ttu-id="804c3-224">Relações muitos para muitos sem uma classe de entidade para representar a tabela de junção ainda não têm suporte.</span><span class="sxs-lookup"><span data-stu-id="804c3-224">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="804c3-225">No entanto, você pode representar uma relação muitos para muitos incluindo uma classe de entidade para a tabela de junção e mapeando duas relações um-para-muitos separadas.</span><span class="sxs-lookup"><span data-stu-id="804c3-225">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11-14,16-19,39-46)]
