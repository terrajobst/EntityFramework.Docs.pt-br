---
title: Relações – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 3731d30a15222a18ad6c729e010b9bf0994c82b2
ms.sourcegitcommit: 83c1e2fc034e5eb1fec1ebabc8d629ffcc7c0632
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67351344"
---
# <a name="relationships"></a><span data-ttu-id="28ab1-102">Relações</span><span class="sxs-lookup"><span data-stu-id="28ab1-102">Relationships</span></span>

<span data-ttu-id="28ab1-103">Uma relação define como duas entidades se relacionam entre si.</span><span class="sxs-lookup"><span data-stu-id="28ab1-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="28ab1-104">Em um banco de dados relacional, isso é representado por uma restrição foreign key.</span><span class="sxs-lookup"><span data-stu-id="28ab1-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="28ab1-105">A maioria dos exemplos neste artigo usa uma relação um-para-muitos para demonstrar conceitos.</span><span class="sxs-lookup"><span data-stu-id="28ab1-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="28ab1-106">Para obter exemplos de relações um para um e muitos-para-muitos, consulte o [outros padrões de relação](#other-relationship-patterns) seção no final do artigo.</span><span class="sxs-lookup"><span data-stu-id="28ab1-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="28ab1-107">Definição de termos</span><span class="sxs-lookup"><span data-stu-id="28ab1-107">Definition of Terms</span></span>

<span data-ttu-id="28ab1-108">Há um número de termos usados para descrever relações</span><span class="sxs-lookup"><span data-stu-id="28ab1-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="28ab1-109">**Entidade dependente:** Essa é a entidade que contém a propriedade de chave estrangeira (s).</span><span class="sxs-lookup"><span data-stu-id="28ab1-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="28ab1-110">Às vezes chamado de "filho" da relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="28ab1-111">**Entidade principal:** Essa é a entidade que contém as propriedades de chave alternativo/primário.</span><span class="sxs-lookup"><span data-stu-id="28ab1-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="28ab1-112">Às vezes chamado de 'parent' da relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="28ab1-113">**Chave estrangeira:** Propriedade (s) em que é usada para armazenar os valores da propriedade de chave principal que a entidade está relacionada à entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="28ab1-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="28ab1-114">**Chave da entidade:** A propriedade (s) que identifica exclusivamente a entidade principal.</span><span class="sxs-lookup"><span data-stu-id="28ab1-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="28ab1-115">Isso pode ser a chave primária ou uma chave alternativa.</span><span class="sxs-lookup"><span data-stu-id="28ab1-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="28ab1-116">**Propriedade de navegação:** Uma propriedade definida na entidade dependente e/ou entidade de segurança que contém uma referência (s) para a entidade relacionada (s).</span><span class="sxs-lookup"><span data-stu-id="28ab1-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="28ab1-117">**Propriedade de navegação de coleção:** Uma propriedade de navegação que contém referências a várias entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="28ab1-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="28ab1-118">**Propriedade de navegação de referência:** Uma propriedade de navegação que contém uma referência a uma única entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="28ab1-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="28ab1-119">**Propriedade de navegação inversa:** Ao discutir uma propriedade de navegação em particular, este termo refere-se a propriedade de navegação na outra extremidade da relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="28ab1-120">A listagem de código a seguir mostra uma relação um-para-muitos entre `Blog` e `Post`</span><span class="sxs-lookup"><span data-stu-id="28ab1-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="28ab1-121">`Post` é a entidade dependente</span><span class="sxs-lookup"><span data-stu-id="28ab1-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="28ab1-122">`Blog` é a entidade principal</span><span class="sxs-lookup"><span data-stu-id="28ab1-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="28ab1-123">`Post.BlogId` é a chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="28ab1-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="28ab1-124">`Blog.BlogId` é a chave de entidade (nesse caso é uma chave primária em vez de uma chave alternativa)</span><span class="sxs-lookup"><span data-stu-id="28ab1-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="28ab1-125">`Post.Blog` é uma propriedade de navegação de referência</span><span class="sxs-lookup"><span data-stu-id="28ab1-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="28ab1-126">`Blog.Posts` é uma propriedade de navegação de coleção</span><span class="sxs-lookup"><span data-stu-id="28ab1-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="28ab1-127">`Post.Blog` é a propriedade de navegação inverso do `Blog.Posts` (e vice-versa)</span><span class="sxs-lookup"><span data-stu-id="28ab1-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="28ab1-128">Convenções</span><span class="sxs-lookup"><span data-stu-id="28ab1-128">Conventions</span></span>

<span data-ttu-id="28ab1-129">Por convenção, uma relação será criada quando há uma propriedade de navegação descoberta em um tipo.</span><span class="sxs-lookup"><span data-stu-id="28ab1-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="28ab1-130">Uma propriedade é considerada uma propriedade de navegação, se o tipo que ele aponta não pode ser mapeado como um tipo escalar pelo provedor de banco de dados atual.</span><span class="sxs-lookup"><span data-stu-id="28ab1-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="28ab1-131">As relações que são descobertas pela convenção sempre serão direcionada para a chave primária da entidade principal.</span><span class="sxs-lookup"><span data-stu-id="28ab1-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="28ab1-132">Para direcionar para uma chave alternativa, uma configuração adicional deve ser executada usando a API do Fluent.</span><span class="sxs-lookup"><span data-stu-id="28ab1-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="28ab1-133">Relações totalmente definidas</span><span class="sxs-lookup"><span data-stu-id="28ab1-133">Fully Defined Relationships</span></span>

<span data-ttu-id="28ab1-134">O padrão mais comum para relações é ter as propriedades de navegação definidas em ambas as extremidades da relação e uma propriedade de chave estrangeira definida na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="28ab1-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="28ab1-135">Se um par de propriedades de navegação é encontrado entre dois tipos, eles serão configurados como propriedades de navegação inversa da mesma relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="28ab1-136">Se a entidade dependente contém uma propriedade chamada `<primary key property name>`, `<navigation property name><primary key property name>`, ou `<principal entity name><primary key property name>` e em seguida, ele será configurado como a chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="28ab1-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="28ab1-137">Se houver várias propriedades de navegação, definidas entre dois tipos (ou seja, mais de um par distinto de navegações que apontam para si), em seguida, nenhuma relação será criada por convenção, e você precisará configurá-las para identificar manualmente como o par de propriedades de navegação para cima.</span><span class="sxs-lookup"><span data-stu-id="28ab1-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="28ab1-138">Nenhuma propriedade de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="28ab1-138">No Foreign Key Property</span></span>

<span data-ttu-id="28ab1-139">Embora seja recomendável ter uma propriedade de chave estrangeira definida na classe de entidade dependente, não é necessário.</span><span class="sxs-lookup"><span data-stu-id="28ab1-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="28ab1-140">Se nenhuma propriedade de chave estrangeira for encontrada, uma propriedade de chave estrangeira de sombra será introduzida com o nome `<navigation property name><principal key property name>` (consulte [propriedades de sombra](shadow-properties.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="28ab1-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="28ab1-141">Propriedade de navegação único</span><span class="sxs-lookup"><span data-stu-id="28ab1-141">Single Navigation Property</span></span>

<span data-ttu-id="28ab1-142">Incluindo apenas uma propriedade de navegação (nenhuma navegação inversa e nenhuma propriedade de chave estrangeira) é suficiente para ter uma relação definida por convenção.</span><span class="sxs-lookup"><span data-stu-id="28ab1-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="28ab1-143">Você também pode ter uma propriedade de navegação e uma propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="28ab1-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="28ab1-144">Excluir em cascata</span><span class="sxs-lookup"><span data-stu-id="28ab1-144">Cascade Delete</span></span>

<span data-ttu-id="28ab1-145">Por convenção, a exclusão em cascata será definida como *Cascade* para as relações necessárias e *ClientSetNull* para relações opcionais.</span><span class="sxs-lookup"><span data-stu-id="28ab1-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="28ab1-146">*CASCADE* significa entidades dependentes também serão excluídas.</span><span class="sxs-lookup"><span data-stu-id="28ab1-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="28ab1-147">*ClientSetNull* significa que as entidades dependentes que não são carregadas na memória permanecerá inalterado e deve ser excluídas manualmente ou atualizada para apontar para uma entidade principal válida.</span><span class="sxs-lookup"><span data-stu-id="28ab1-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="28ab1-148">Para entidades que são carregadas na memória, o EF Core tentará definir as propriedades de chave estrangeiras como nulo.</span><span class="sxs-lookup"><span data-stu-id="28ab1-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="28ab1-149">Consulte a [relações obrigatórios e opcionais](#required-and-optional-relationships) seção para a diferença entre as relações obrigatórios e opcionais.</span><span class="sxs-lookup"><span data-stu-id="28ab1-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="28ab1-150">Ver [exclusão em cascata](../saving/cascade-delete.md) para obter mais detalhes sobre os diferentes comportamentos e os padrões usados pela convenção de exclusão.</span><span class="sxs-lookup"><span data-stu-id="28ab1-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="28ab1-151">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="28ab1-151">Data Annotations</span></span>

<span data-ttu-id="28ab1-152">Há duas anotações de dados que podem ser usadas para configurar as relações, `[ForeignKey]` e `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="28ab1-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="28ab1-153">Eles estão disponíveis no `System.ComponentModel.DataAnnotations.Schema` namespace.</span><span class="sxs-lookup"><span data-stu-id="28ab1-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="28ab1-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="28ab1-154">[ForeignKey]</span></span>

<span data-ttu-id="28ab1-155">Você pode usar as anotações de dados para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="28ab1-156">Normalmente, isso é feito quando a propriedade de chave estrangeira não for descoberta por convenção.</span><span class="sxs-lookup"><span data-stu-id="28ab1-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="28ab1-157">O `[ForeignKey]` anotação pode ser colocada em uma das propriedades de navegação na relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="28ab1-158">Ele não precisa ir na propriedade de navegação na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="28ab1-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="28ab1-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="28ab1-159">[InverseProperty]</span></span>

<span data-ttu-id="28ab1-160">Você pode usar as anotações de dados para configurar como as propriedades de navegação em entidades dependentes e principais ao emparelhar.</span><span class="sxs-lookup"><span data-stu-id="28ab1-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="28ab1-161">Normalmente, isso é feito quando há mais de um par de propriedades de navegação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="28ab1-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="28ab1-162">API fluente</span><span class="sxs-lookup"><span data-stu-id="28ab1-162">Fluent API</span></span>

<span data-ttu-id="28ab1-163">Para configurar uma relação na API do Fluent, inicie identificando as propriedades de navegação que compõem a relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="28ab1-164">`HasOne` ou `HasMany` identifica a propriedade de navegação no tipo de entidade que você estiver iniciando a configuração em.</span><span class="sxs-lookup"><span data-stu-id="28ab1-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="28ab1-165">Uma chamada para então encadear `WithOne` ou `WithMany` para identificar a navegação inversa.</span><span class="sxs-lookup"><span data-stu-id="28ab1-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="28ab1-166">`HasOne`/`WithOne` são usados para as propriedades de navegação de referência e `HasMany` / `WithMany` são usados para as propriedades de navegação de coleção.</span><span class="sxs-lookup"><span data-stu-id="28ab1-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="28ab1-167">Propriedade de navegação único</span><span class="sxs-lookup"><span data-stu-id="28ab1-167">Single Navigation Property</span></span>

<span data-ttu-id="28ab1-168">Se você tiver apenas uma propriedade de navegação, há sobrecargas sem parâmetro de `WithOne` e `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="28ab1-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="28ab1-169">Isso indica que há conceitualmente uma referência ou uma coleção na outra extremidade da relação, mas não há nenhuma propriedade de navegação incluída na classe de entidade.</span><span class="sxs-lookup"><span data-stu-id="28ab1-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="28ab1-170">Chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="28ab1-170">Foreign Key</span></span>

<span data-ttu-id="28ab1-171">Você pode usar a API Fluent para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="28ab1-172">A listagem de código a seguir mostra como configurar uma chave estrangeira composta.</span><span class="sxs-lookup"><span data-stu-id="28ab1-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="28ab1-173">Você pode usar a sobrecarga de cadeia de caracteres de `HasForeignKey(...)` para configurar uma propriedade de sombra como uma chave estrangeira (consulte [propriedades de sombra](shadow-properties.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="28ab1-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="28ab1-174">É recomendável adicionar explicitamente a propriedade de sombra para o modelo antes de usá-lo como uma chave estrangeira (conforme mostrado abaixo).</span><span class="sxs-lookup"><span data-stu-id="28ab1-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="28ab1-175">Sem a propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="28ab1-175">Without Navigation Property</span></span>

<span data-ttu-id="28ab1-176">Você não precisa necessariamente fornecem uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="28ab1-177">Você simplesmente pode fornecer uma chave estrangeira em um lado da relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="28ab1-178">Chave da entidade</span><span class="sxs-lookup"><span data-stu-id="28ab1-178">Principal Key</span></span>

<span data-ttu-id="28ab1-179">Se você quiser que a chave estrangeira para fazer referência a uma propriedade que não seja a chave primária, você pode usar a API Fluent para configurar a propriedade de chave de entidade de segurança para a relação.</span><span class="sxs-lookup"><span data-stu-id="28ab1-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="28ab1-180">A propriedade que você configurar como a chave será a entidade de segurança automaticamente ser configurado como uma chave alternativa (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).</span><span class="sxs-lookup"><span data-stu-id="28ab1-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="28ab1-181">A listagem de código a seguir mostra como configurar uma chave de entidade composta.</span><span class="sxs-lookup"><span data-stu-id="28ab1-181">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="28ab1-182">A ordem em que você especificar propriedades de chave de entidade de segurança deve corresponder à ordem na qual eles são especificados para a chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="28ab1-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="28ab1-183">Relações obrigatórios e opcionais</span><span class="sxs-lookup"><span data-stu-id="28ab1-183">Required and Optional Relationships</span></span>

<span data-ttu-id="28ab1-184">Você pode usar a API Fluent para configurar se a relação é obrigatório ou opcional.</span><span class="sxs-lookup"><span data-stu-id="28ab1-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="28ab1-185">Por fim, isso controla se a propriedade de chave estrangeira é obrigatório ou opcional.</span><span class="sxs-lookup"><span data-stu-id="28ab1-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="28ab1-186">Isso é mais útil quando você estiver usando uma chave estrangeira do estado de sombra.</span><span class="sxs-lookup"><span data-stu-id="28ab1-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="28ab1-187">Se você tiver uma propriedade de chave estrangeira em sua classe de entidade, o requiredness da relação é determinado com base em se a propriedade de chave estrangeira é obrigatória ou opcional (consulte [propriedades obrigatórias e opcionais](required-optional.md) para obter mais informações informações).</span><span class="sxs-lookup"><span data-stu-id="28ab1-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="28ab1-188">Excluir em cascata</span><span class="sxs-lookup"><span data-stu-id="28ab1-188">Cascade Delete</span></span>

<span data-ttu-id="28ab1-189">Você pode usar a API Fluent para configurar o comportamento de exclusão em cascata para uma determinada relação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="28ab1-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="28ab1-190">Ver [exclusão em cascata](../saving/cascade-delete.md) na seção salvando dados para uma discussão detalhada de cada opção.</span><span class="sxs-lookup"><span data-stu-id="28ab1-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="28ab1-191">Outros padrões de relação</span><span class="sxs-lookup"><span data-stu-id="28ab1-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="28ab1-192">Um para um</span><span class="sxs-lookup"><span data-stu-id="28ab1-192">One-to-one</span></span>

<span data-ttu-id="28ab1-193">Relações um-para-um têm uma propriedade de navegação de referência em ambos os lados.</span><span class="sxs-lookup"><span data-stu-id="28ab1-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="28ab1-194">Eles seguem as mesmas convenções de relações um-para-muitos, mas um índice exclusivo é introduzido na propriedade de chave estrangeira para garantir que apenas um dependente está relacionado a cada entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="28ab1-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="28ab1-195">O EF escolherá uma das entidades para ser dependente com base em sua capacidade de detectar uma propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="28ab1-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="28ab1-196">Se a entidade errada é escolhida como o dependente, você pode usar a API Fluent para corrigir isso.</span><span class="sxs-lookup"><span data-stu-id="28ab1-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="28ab1-197">Ao configurar a relação com a API Fluent, que você usar o `HasOne` e `WithOne` métodos.</span><span class="sxs-lookup"><span data-stu-id="28ab1-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="28ab1-198">Ao configurar a chave estrangeira, você precisa especificar o tipo de entidade dependente - Observe o parâmetro genérico fornecido para `HasForeignKey` na lista abaixo.</span><span class="sxs-lookup"><span data-stu-id="28ab1-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="28ab1-199">Em uma relação um-para-muitos, é claro que a entidade com a navegação de referência é dependente e o com a coleção é a entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="28ab1-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="28ab1-200">Mas isso não é então uma relação individual -, portanto, a necessidade de defini-lo explicitamente.</span><span class="sxs-lookup"><span data-stu-id="28ab1-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="28ab1-201">Muitos-para-muitos</span><span class="sxs-lookup"><span data-stu-id="28ab1-201">Many-to-many</span></span>

<span data-ttu-id="28ab1-202">Ainda não há suporte para relações muitos-para-muitos sem uma classe de entidade para representar a tabela de junção.</span><span class="sxs-lookup"><span data-stu-id="28ab1-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="28ab1-203">No entanto, você pode representar uma relação muitos-para-muitos com a inclusão de uma classe de entidade para a tabela de junção e duas relações de um-para-muitos separado mapeamento.</span><span class="sxs-lookup"><span data-stu-id="28ab1-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
