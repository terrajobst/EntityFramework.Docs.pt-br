---
title: Especificação de CSDL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 438af83b8a1ad51ee8414341181412e950d0e117
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460144"
---
# <a name="csdl-specification"></a><span data-ttu-id="6e225-102">Especificação de CSDL</span><span class="sxs-lookup"><span data-stu-id="6e225-102">CSDL Specification</span></span>
<span data-ttu-id="6e225-103">Linguagem de definição de esquema conceitual (CSDL) é uma linguagem baseada em XML que descreve as entidades, relações e funções que compõem um modelo conceitual de um aplicativo controlado por dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="6e225-104">Esse modelo conceitual pode ser usado pelo Entity Framework ou do WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="6e225-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="6e225-105">Os metadados que é descrito com a CSDL é usado pelo Entity Framework para mapear entidades e relações que são definidas em um modelo conceitual para uma fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="6e225-106">Para obter mais informações, consulte [especificação de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) e [especificação de MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="6e225-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="6e225-107">CSDL é a implementação do Entity Framework do modelo de dados de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="6e225-108">Em um aplicativo do Entity Framework, os metadados de modelo conceitual é carregado de um arquivo. CSDL (escrito em CSDL) em uma instância da System.Data.Metadata.Edm.EdmItemCollection em são acessados por meio de métodos no Classe System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="6e225-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="6e225-109">Entity Framework usa metadados do modelo conceitual para converter consultas no modelo conceitual para comandos de específico da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="6e225-110">O EF Designer armazena informações de modelo conceitual em um arquivo. edmx em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="6e225-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="6e225-111">No momento da compilação, o EF Designer usa as informações em um arquivo. edmx para criar o arquivo. CSDL que é necessário pelo Entity Framework em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="6e225-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="6e225-112">Versões do CSDL são diferenciadas por namespaces XML.</span><span class="sxs-lookup"><span data-stu-id="6e225-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="6e225-113">Versão CSDL</span><span class="sxs-lookup"><span data-stu-id="6e225-113">CSDL Version</span></span> | <span data-ttu-id="6e225-114">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="6e225-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="6e225-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="6e225-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="6e225-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="6e225-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="6e225-117">V3 CSDL</span><span class="sxs-lookup"><span data-stu-id="6e225-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="6e225-118">Elemento Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-118">Association Element (CSDL)</span></span>

<span data-ttu-id="6e225-119">Uma **associação** elemento define uma relação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="6e225-120">Uma associação deve especificar os tipos de entidade envolvidos na relação e o número possível de tipos de entidade em cada extremidade da relação, que é conhecida como a multiplicidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="6e225-121">A multiplicidade de uma extremidade de associação pode ter um valor de um (1), zero ou um (entre 0 e 1) ou muitas (\*).</span><span class="sxs-lookup"><span data-stu-id="6e225-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="6e225-122">Essas informações são especificadas em dois elementos filho de término.</span><span class="sxs-lookup"><span data-stu-id="6e225-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="6e225-123">Instâncias do tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras, se elas são expostas em um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="6e225-124">Em um aplicativo, uma instância de uma associação representa uma associação específica entre instâncias de tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="6e225-125">Instâncias de associação são agrupadas logicamente em um conjunto de associações.</span><span class="sxs-lookup"><span data-stu-id="6e225-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="6e225-126">Uma **associação** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-127">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-128">End (elementos exatamente 2)</span><span class="sxs-lookup"><span data-stu-id="6e225-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="6e225-129">ReferentialConstraint (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="6e225-130">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-131">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-131">Applicable Attributes</span></span>

<span data-ttu-id="6e225-132">A tabela a seguir descreve os atributos que podem ser aplicados para o **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="6e225-133">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-133">Attribute Name</span></span> | <span data-ttu-id="6e225-134">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-134">Is Required</span></span> | <span data-ttu-id="6e225-135">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="6e225-136">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-136">**Name**</span></span>       | <span data-ttu-id="6e225-137">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-137">Yes</span></span>         | <span data-ttu-id="6e225-138">O nome da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-139">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="6e225-140">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-141">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-142">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-142">Example</span></span>

<span data-ttu-id="6e225-143">A exemplo a seguir mostra uma **associação** elemento que define o **CustomerOrders** associação quando as chaves estrangeiras não foram expostas no **cliente** e  **Ordem** tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="6e225-144">O **multiplicidade** valores para cada **final** da associação indicam que muitos **pedidos** pode ser associado com um **cliente**, mas somente um **Customer** pode ser associado com um **ordem**.</span><span class="sxs-lookup"><span data-stu-id="6e225-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="6e225-145">Além disso, o **OnDelete** elemento indica que todos os **pedidos** que estão relacionados a um determinado **cliente** e terem sido carregados para o ObjectContext será excluído Se o **cliente** é excluído.</span><span class="sxs-lookup"><span data-stu-id="6e225-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="6e225-146">A exemplo a seguir mostra uma **associação** elemento que define o **CustomerOrders** associação quando foram expostas em chaves estrangeiras a **cliente** e  **Ordem** tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="6e225-147">Com chaves estrangeiras expostas, a relação entre as entidades é gerenciada com um **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="6e225-148">Um elemento AssociationSetMapping correspondente não é necessário para mapear esta associação para a fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="6e225-149">Elemento AssociationSet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="6e225-150">O **AssociationSet** elemento na linguagem de definição de esquema conceitual (CSDL) é um contêiner lógico para instâncias do mesmo tipo de associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="6e225-151">Um conjunto de associações fornece uma definição para o agrupamento de instâncias de associação para que eles possam ser mapeadas para uma fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="6e225-152">O **AssociationSet** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-153">Documentação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6e225-154">End (exatamente dois elementos necessários)</span><span class="sxs-lookup"><span data-stu-id="6e225-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="6e225-155">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="6e225-156">O **associação** atributo especifica o tipo de associação que contém um conjunto de associações.</span><span class="sxs-lookup"><span data-stu-id="6e225-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="6e225-157">Os conjuntos de entidades que compõem as extremidades de um conjunto de associações são especificados com exatamente dois filhos **final** elementos.</span><span class="sxs-lookup"><span data-stu-id="6e225-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-158">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-158">Applicable Attributes</span></span>

<span data-ttu-id="6e225-159">A tabela a seguir descreve os atributos que podem ser aplicados para o **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="6e225-160">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-160">Attribute Name</span></span>  | <span data-ttu-id="6e225-161">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-161">Is Required</span></span> | <span data-ttu-id="6e225-162">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-163">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-163">**Name**</span></span>        | <span data-ttu-id="6e225-164">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-164">Yes</span></span>         | <span data-ttu-id="6e225-165">O nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6e225-165">The name of the entity set.</span></span> <span data-ttu-id="6e225-166">O valor da **nome** atributo não pode ser o mesmo que o valor da **associação** atributo.</span><span class="sxs-lookup"><span data-stu-id="6e225-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="6e225-167">**Associação**</span><span class="sxs-lookup"><span data-stu-id="6e225-167">**Association**</span></span> | <span data-ttu-id="6e225-168">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-168">Yes</span></span>         | <span data-ttu-id="6e225-169">O nome totalmente qualificado da associação que definir a associação contém instâncias de.</span><span class="sxs-lookup"><span data-stu-id="6e225-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="6e225-170">A associação deve estar no mesmo namespace que o conjunto de associações.</span><span class="sxs-lookup"><span data-stu-id="6e225-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-171">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="6e225-172">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-173">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-174">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-174">Example</span></span>

<span data-ttu-id="6e225-175">A exemplo a seguir mostra uma **EntityContainer** elemento com dois **AssociationSet** elementos:</span><span class="sxs-lookup"><span data-stu-id="6e225-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="6e225-176">Elemento CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="6e225-177">O **CollectionType** elemento na linguagem de definição de esquema conceitual (CSDL) Especifica que um parâmetro de função ou função de tipo de retorno é uma coleção.</span><span class="sxs-lookup"><span data-stu-id="6e225-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="6e225-178">O **CollectionType** elemento pode ser um filho do elemento de parâmetro ou o elemento de ReturnType (função).</span><span class="sxs-lookup"><span data-stu-id="6e225-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="6e225-179">O tipo de coleção pode ser especificado usando o **tipo** atributo ou um dos seguintes elementos filhos:</span><span class="sxs-lookup"><span data-stu-id="6e225-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="6e225-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="6e225-180">**CollectionType**</span></span>
-   <span data-ttu-id="6e225-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="6e225-181">ReferenceType</span></span>
-   <span data-ttu-id="6e225-182">RowType</span><span class="sxs-lookup"><span data-stu-id="6e225-182">RowType</span></span>
-   <span data-ttu-id="6e225-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="6e225-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-184">Um modelo não validará se o tipo de uma coleção é especificado com ambos os **tipo** atributo e um elemento filho.</span><span class="sxs-lookup"><span data-stu-id="6e225-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-185">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-185">Applicable Attributes</span></span>

<span data-ttu-id="6e225-186">A tabela a seguir descreve os atributos que podem ser aplicados para o **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6e225-187">Observe que o **DefaultValue**, **MaxLength**, **FixedLength**, **precisão**, **escala**,  **Unicode**, e **agrupamento** atributos só são aplicáveis a coleções de **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="6e225-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="6e225-188">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-188">Attribute Name</span></span>                                                          | <span data-ttu-id="6e225-189">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-189">Is Required</span></span> | <span data-ttu-id="6e225-190">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-191">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-191">**Type**</span></span>                                                                | <span data-ttu-id="6e225-192">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-192">No</span></span>          | <span data-ttu-id="6e225-193">O tipo de coleção.</span><span class="sxs-lookup"><span data-stu-id="6e225-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6e225-194">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="6e225-194">**Nullable**</span></span>                                                            | <span data-ttu-id="6e225-195">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-195">No</span></span>          | <span data-ttu-id="6e225-196">**Verdadeiro** (o valor padrão) ou **falso** dependendo se a propriedade pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="6e225-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="6e225-197">> V1 CSDL, uma propriedade de tipo complexo deve ter `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6e225-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="6e225-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6e225-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="6e225-199">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-199">No</span></span>          | <span data-ttu-id="6e225-200">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="6e225-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="6e225-202">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-202">No</span></span>          | <span data-ttu-id="6e225-203">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="6e225-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="6e225-205">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-205">No</span></span>          | <span data-ttu-id="6e225-206">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="6e225-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="6e225-207">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="6e225-207">**Precision**</span></span>                                                           | <span data-ttu-id="6e225-208">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-208">No</span></span>          | <span data-ttu-id="6e225-209">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="6e225-210">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="6e225-210">**Scale**</span></span>                                                               | <span data-ttu-id="6e225-211">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-211">No</span></span>          | <span data-ttu-id="6e225-212">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6e225-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6e225-213">**SRID**</span></span>                                                                | <span data-ttu-id="6e225-214">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-214">No</span></span>          | <span data-ttu-id="6e225-215">Identificador de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="6e225-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6e225-216">Válido somente para as propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="6e225-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="6e225-217">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="6e225-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="6e225-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6e225-218">**Unicode**</span></span>                                                             | <span data-ttu-id="6e225-219">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-219">No</span></span>          | <span data-ttu-id="6e225-220">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="6e225-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="6e225-221">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="6e225-221">**Collation**</span></span>                                                           | <span data-ttu-id="6e225-222">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-222">No</span></span>          | <span data-ttu-id="6e225-223">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="6e225-224">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6e225-225">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-226">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-227">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-227">Example</span></span>

<span data-ttu-id="6e225-228">O exemplo a seguir mostra uma função definida pelo modelo que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de **pessoa** tipos de entidade (conforme especificado com o **ElementType** atributo).</span><span class="sxs-lookup"><span data-stu-id="6e225-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="6e225-229">O exemplo a seguir mostra uma função definida modelo que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de linhas (conforme especificado na **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="6e225-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="6e225-230">O exemplo a seguir mostra uma função definida modelo que usa o **CollectionType** elemento para especificar que a função aceita como um parâmetro de uma coleção de **departamento** tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="6e225-231">Elemento ComplexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="6e225-232">Um **ComplexType** elemento define uma estrutura de dados composta **EdmSimpleType** propriedades ou outros tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="6e225-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="6e225-233">Um tipo complexo pode ser uma propriedade de um tipo de entidade ou outro tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="6e225-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="6e225-234">Um tipo complexo é semelhante a um tipo de entidade em que um tipo complexo define os dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="6e225-235">No entanto, há algumas diferenças entre tipos complexos e tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="6e225-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="6e225-236">Tipos complexos não têm identidades (ou chaves) e, portanto, não podem existir independente.</span><span class="sxs-lookup"><span data-stu-id="6e225-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="6e225-237">Tipos complexos podem existir somente como propriedades de tipos de entidade ou outros tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="6e225-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="6e225-238">Tipos complexos não podem participar de associações.</span><span class="sxs-lookup"><span data-stu-id="6e225-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="6e225-239">Nem o final de uma associação pode ser um tipo complexo e, portanto, as propriedades de navegação não podem ser definidas para tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="6e225-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="6e225-240">Uma propriedade de tipo complexo não pode ter um valor nulo, embora as propriedades escalares de um tipo complexo podem cada ser definidas como null.</span><span class="sxs-lookup"><span data-stu-id="6e225-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="6e225-241">Um **ComplexType** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-242">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-243">Propriedade (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="6e225-244">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="6e225-245">A tabela a seguir descreve os atributos que podem ser aplicados para o **ComplexType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="6e225-246">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="6e225-247">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-247">Is Required</span></span> | <span data-ttu-id="6e225-248">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-249">Nome</span><span class="sxs-lookup"><span data-stu-id="6e225-249">Name</span></span>                                                                                                           | <span data-ttu-id="6e225-250">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-250">Yes</span></span>         | <span data-ttu-id="6e225-251">O nome do tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="6e225-251">The name of the complex type.</span></span> <span data-ttu-id="6e225-252">O nome de um tipo complexo não pode ser igual ao nome de outro tipo complexo, tipo de entidade ou associação que está dentro do escopo do modelo.</span><span class="sxs-lookup"><span data-stu-id="6e225-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="6e225-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="6e225-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="6e225-254">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-254">No</span></span>          | <span data-ttu-id="6e225-255">O nome de outro tipo complexo que é o tipo base do tipo complexo que está sendo definido.</span><span class="sxs-lookup"><span data-stu-id="6e225-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="6e225-256">> Se esse atributo não é aplicável na CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="6e225-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="6e225-257">Não há suporte para a herança para tipos complexos nessa versão.</span><span class="sxs-lookup"><span data-stu-id="6e225-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="6e225-258">Abstrato</span><span class="sxs-lookup"><span data-stu-id="6e225-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="6e225-259">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-259">No</span></span>          | <span data-ttu-id="6e225-260">**Verdadeiro** ou **falso** (o valor padrão), dependendo se o tipo complexo é um tipo abstrato.</span><span class="sxs-lookup"><span data-stu-id="6e225-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="6e225-261">> Se esse atributo não é aplicável na CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="6e225-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="6e225-262">Tipos complexos em que a versão não podem ser de tipos abstratos.</span><span class="sxs-lookup"><span data-stu-id="6e225-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="6e225-263">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ComplexType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="6e225-264">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-265">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-266">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-266">Example</span></span>

<span data-ttu-id="6e225-267">O exemplo a seguir mostra um tipo complexo, **endereço**, com o **EdmSimpleType** as propriedades **StreetAddress**, **Cidade**,  **StateOrProvince**, **país**, e **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="6e225-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="6e225-268">Para definir o tipo complexo **endereço** (acima) como uma propriedade de um tipo de entidade, você deve declarar o tipo de propriedade na definição de tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="6e225-269">A exemplo a seguir mostra a **endereço** propriedade como um tipo complexo em um tipo de entidade (**Publisher**):</span><span class="sxs-lookup"><span data-stu-id="6e225-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="6e225-270">Elemento DefiningExpression (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="6e225-271">O **DefiningExpression** elemento na linguagem de definição de esquema conceitual (CSDL) contém uma expressão de Entity SQL que define uma função no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="6e225-272">Para fins de validação, um **DefiningExpression** elemento pode conter conteúdo arbitrário.</span><span class="sxs-lookup"><span data-stu-id="6e225-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="6e225-273">No entanto, Entity Framework irá acionar uma exceção em tempo de execução se um **DefiningExpression** elemento não contém Entity SQL válido.</span><span class="sxs-lookup"><span data-stu-id="6e225-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-274">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-274">Applicable Attributes</span></span>

<span data-ttu-id="6e225-275">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **DefiningExpression** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="6e225-276">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-277">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6e225-278">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-278">Example</span></span>

<span data-ttu-id="6e225-279">O exemplo a seguir usa uma **DefiningExpression** elemento para definir uma função que retorna o número de anos desde um livro foi publicado.</span><span class="sxs-lookup"><span data-stu-id="6e225-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="6e225-280">O conteúdo a **DefiningExpression** elemento é escrito em Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="6e225-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="6e225-281">Elemento dependente (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="6e225-282">O **dependentes** elemento na linguagem de definição de esquema conceitual (CSDL) é um elemento filho ao elemento ReferentialConstraint e define o final dependente de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="6e225-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="6e225-283">Um **ReferentialConstraint** elemento define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="6e225-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6e225-284">Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados podem fazer referência a chave primária de outra tabela, uma propriedade (ou propriedades) de um tipo de entidade podem fazer referência à chave de entidade de outro tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6e225-285">O tipo de entidade que é referenciado é chamado de *final principal* da restrição.</span><span class="sxs-lookup"><span data-stu-id="6e225-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6e225-286">O tipo de entidade que faz referência a extremidade de entidade é chamado de *final dependente* da restrição.</span><span class="sxs-lookup"><span data-stu-id="6e225-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="6e225-287">**PropertyRef** elementos são usados para especificar quais teclas a extremidade de entidade de referência.</span><span class="sxs-lookup"><span data-stu-id="6e225-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="6e225-288">O **dependentes** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-289">PropertyRef (um ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6e225-290">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-291">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-291">Applicable Attributes</span></span>

<span data-ttu-id="6e225-292">A tabela a seguir descreve os atributos que podem ser aplicados para o **dependentes** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="6e225-293">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-293">Attribute Name</span></span> | <span data-ttu-id="6e225-294">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-294">Is Required</span></span> | <span data-ttu-id="6e225-295">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="6e225-296">**Função**</span><span class="sxs-lookup"><span data-stu-id="6e225-296">**Role**</span></span>       | <span data-ttu-id="6e225-297">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-297">Yes</span></span>         | <span data-ttu-id="6e225-298">O nome do tipo de entidade na extremidade dependente da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-299">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **dependentes** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="6e225-300">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-301">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-302">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-302">Example</span></span>

<span data-ttu-id="6e225-303">A exemplo a seguir mostra uma **ReferentialConstraint** elemento que está sendo usado como parte da definição da **PublishedBy** associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="6e225-304">O **PublisherId** propriedade da **livro** tipo de entidade constitui o final dependente da restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="6e225-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="6e225-305">Elemento de documentação (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="6e225-306">O **documentação** elemento na linguagem de definição de esquema conceitual (CSDL) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai.</span><span class="sxs-lookup"><span data-stu-id="6e225-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="6e225-307">Em um arquivo. edmx, quando o **documentação** elemento é um filho de um elemento que aparece como um objeto na superfície de design do Designer de EF (como uma entidade, associação ou propriedade), o conteúdo do **documentação**  elemento será exibido no Visual Studio **propriedades** janela para o objeto.</span><span class="sxs-lookup"><span data-stu-id="6e225-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="6e225-308">O **documentação** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-309">**Resumo**: uma breve descrição do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="6e225-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="6e225-310">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-310">(zero or one element)</span></span>
-   <span data-ttu-id="6e225-311">**LongDescription**: uma descrição abrangente do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="6e225-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="6e225-312">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-312">(zero or one element)</span></span>
-   <span data-ttu-id="6e225-313">Elementos de anotação.</span><span class="sxs-lookup"><span data-stu-id="6e225-313">Annotation elements.</span></span> <span data-ttu-id="6e225-314">(zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-315">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-315">Applicable Attributes</span></span>

<span data-ttu-id="6e225-316">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **documentação** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="6e225-317">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-318">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6e225-319">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-319">Example</span></span>

<span data-ttu-id="6e225-320">A exemplo a seguir mostra a **documentação** como um elemento filho de um elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="6e225-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="6e225-321">Se o trecho a seguir foram no CSDL conteúdo de um. edmx arquivo, o conteúdo do **resumo** e **LongDescription** elementos apareceria no Visual Studio **propriedades** janela quando você clica no `Customer` tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="6e225-322">Elemento end (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-322">End Element (CSDL)</span></span>

<span data-ttu-id="6e225-323">O **final** elemento na linguagem de definição de esquema conceitual (CSDL) pode ser um filho do elemento de associação ou o elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="6e225-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="6e225-324">Em cada caso, a função do **final** elemento é diferente e os atributos aplicáveis são diferentes.</span><span class="sxs-lookup"><span data-stu-id="6e225-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="6e225-325">Elemento final como um filho do elemento de associação</span><span class="sxs-lookup"><span data-stu-id="6e225-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="6e225-326">Uma **final** elemento (como um filho de **associação** elemento) identifica o tipo de entidade em uma extremidade de uma associação e o número de instâncias de tipo de entidade que podem existir na fim de uma associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="6e225-327">Termina de associação são definidas como parte de uma associação; uma associação deve ter exatamente duas termina de associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="6e225-328">Instâncias do tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras se são expostos em um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="6e225-329">Uma **final** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-330">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-331">OnDelete (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="6e225-332">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="6e225-333">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-333">Applicable Attributes</span></span>

<span data-ttu-id="6e225-334">A tabela a seguir descreve os atributos que podem ser aplicados para o **final** elemento quando ele for o filho de um **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="6e225-335">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-335">Attribute Name</span></span>   | <span data-ttu-id="6e225-336">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-336">Is Required</span></span> | <span data-ttu-id="6e225-337">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-338">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-338">**Type**</span></span>         | <span data-ttu-id="6e225-339">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-339">Yes</span></span>         | <span data-ttu-id="6e225-340">O nome do tipo de entidade em uma extremidade da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="6e225-341">**Função**</span><span class="sxs-lookup"><span data-stu-id="6e225-341">**Role**</span></span>         | <span data-ttu-id="6e225-342">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-342">No</span></span>          | <span data-ttu-id="6e225-343">Um nome para o final da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-343">A name for the association end.</span></span> <span data-ttu-id="6e225-344">Se nenhum nome for fornecido, será usado o nome do tipo de entidade no final da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="6e225-345">**Multiplicidade**</span><span class="sxs-lookup"><span data-stu-id="6e225-345">**Multiplicity**</span></span> | <span data-ttu-id="6e225-346">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-346">Yes</span></span>         | <span data-ttu-id="6e225-347">**1**, **entre 0 e 1**, ou **\*** dependendo do número de instâncias do tipo de entidade que podem estar no final da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="6e225-348">**1** indica essa instância de tipo exatamente uma entidade existe no final da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="6e225-349">**entre 0 e 1** indica que zero ou uma instância de tipo de entidade existe no final da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="6e225-350">**\*** indica que zero, um ou mais instâncias de tipo de entidade existem no final da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-351">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **final** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="6e225-352">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-353">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6e225-354">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-354">Example</span></span>

<span data-ttu-id="6e225-355">A exemplo a seguir mostra uma **associação** elemento que define o **CustomerOrders** associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="6e225-356">O **multiplicidade** valores para cada **final** da associação indicam que muitos **pedidos** pode ser associado com um **cliente**, mas somente um **Customer** pode ser associado com um **ordem**.</span><span class="sxs-lookup"><span data-stu-id="6e225-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="6e225-357">Além disso, o **OnDelete** elemento indica que todos os **pedidos** que estão relacionados a um determinado **cliente** e que tiverem sido carregados no ObjectContext será excluído, se o **cliente** é excluído.</span><span class="sxs-lookup"><span data-stu-id="6e225-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="6e225-358">Elemento final como um filho do elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="6e225-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="6e225-359">O **final** elemento Especifica uma extremidade de um conjunto de associações.</span><span class="sxs-lookup"><span data-stu-id="6e225-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="6e225-360">O **AssociationSet** elemento deve conter dois **final** elementos.</span><span class="sxs-lookup"><span data-stu-id="6e225-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="6e225-361">As informações contidas em um **final** elemento é usado em uma associação definida como uma fonte de dados de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="6e225-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="6e225-362">Uma **final** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-363">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-364">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-365">Elementos de anotação devem aparecer após todos os outros elementos filho.</span><span class="sxs-lookup"><span data-stu-id="6e225-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="6e225-366">Elementos de anotação só são permitidos no CSDL v2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="6e225-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6e225-367">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-367">Applicable Attributes</span></span>

<span data-ttu-id="6e225-368">A tabela a seguir descreve os atributos que podem ser aplicados para o **final** elemento quando ele for o filho de um **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="6e225-369">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-369">Attribute Name</span></span> | <span data-ttu-id="6e225-370">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-370">Is Required</span></span> | <span data-ttu-id="6e225-371">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="6e225-372">**EntitySet**</span></span>  | <span data-ttu-id="6e225-373">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-373">Yes</span></span>         | <span data-ttu-id="6e225-374">O nome da **EntitySet** elemento que define uma extremidade do pai **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="6e225-375">O **EntitySet** elemento deve ser definido no mesmo contêiner de entidade como o pai **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="6e225-376">**Função**</span><span class="sxs-lookup"><span data-stu-id="6e225-376">**Role**</span></span>       | <span data-ttu-id="6e225-377">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-377">No</span></span>          | <span data-ttu-id="6e225-378">O nome da associação de conjunto final.</span><span class="sxs-lookup"><span data-stu-id="6e225-378">The name of the association set end.</span></span> <span data-ttu-id="6e225-379">Se o **função** atributo não for usado, o nome da extremidade da associação de conjunto será o nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6e225-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6e225-380">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **final** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="6e225-381">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-382">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6e225-383">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-383">Example</span></span>

<span data-ttu-id="6e225-384">A exemplo a seguir mostra uma **EntityContainer** elemento com dois **AssociationSet** elementos, cada um com dois **final** elementos:</span><span class="sxs-lookup"><span data-stu-id="6e225-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="6e225-385">Elemento EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="6e225-386">O **EntityContainer** elemento na linguagem de definição de esquema conceitual (CSDL) é um contêiner lógico para conjuntos de entidades, conjuntos de associações e importações de função.</span><span class="sxs-lookup"><span data-stu-id="6e225-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="6e225-387">Um contêiner de entidade do modelo conceitual mapeia para um contêiner de entidade do modelo de armazenamento por meio do elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="6e225-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="6e225-388">Um contêiner de entidade do modelo de armazenamento descreve a estrutura do banco de dados: tabelas de descrevem conjuntos de entidades, conjuntos de associações descrevem restrições de chave estrangeira e importações de função descrevem os procedimentos armazenados em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="6e225-389">Uma **EntityContainer** elemento pode ter zero ou mais elementos de documentação.</span><span class="sxs-lookup"><span data-stu-id="6e225-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="6e225-390">Se um **documentação** elemento estiver presente, ela deve preceder todos **EntitySet**, **AssociationSet**, e **FunctionImport** elementos.</span><span class="sxs-lookup"><span data-stu-id="6e225-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="6e225-391">Uma **EntityContainer** elemento pode ter zero ou mais dos seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="6e225-392">EntitySet</span></span>
-   <span data-ttu-id="6e225-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="6e225-393">AssociationSet</span></span>
-   <span data-ttu-id="6e225-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="6e225-394">FunctionImport</span></span>
-   <span data-ttu-id="6e225-395">Elementos de anotação</span><span class="sxs-lookup"><span data-stu-id="6e225-395">Annotation elements</span></span>

<span data-ttu-id="6e225-396">Você pode estender uma **EntityContainer** elemento para incluir o conteúdo de outro **EntityContainer** que esteja dentro do mesmo namespace.</span><span class="sxs-lookup"><span data-stu-id="6e225-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="6e225-397">Para incluir o conteúdo de outro **EntityContainer**, em que a referência **EntityContainer** elemento, defina o valor da **estende** de atributo para o nome da  **EntityContainer** elemento que você deseja incluir.</span><span class="sxs-lookup"><span data-stu-id="6e225-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="6e225-398">Todos os elementos filho dos incluídos **EntityContainer** elemento será tratado como elementos filho de referência **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-399">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-399">Applicable Attributes</span></span>

<span data-ttu-id="6e225-400">A tabela a seguir descreve os atributos que podem ser aplicados para o **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="6e225-401">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-401">Attribute Name</span></span> | <span data-ttu-id="6e225-402">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-402">Is Required</span></span> | <span data-ttu-id="6e225-403">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="6e225-404">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-404">**Name**</span></span>       | <span data-ttu-id="6e225-405">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-405">Yes</span></span>         | <span data-ttu-id="6e225-406">O nome do contêiner de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="6e225-407">**Estende**</span><span class="sxs-lookup"><span data-stu-id="6e225-407">**Extends**</span></span>    | <span data-ttu-id="6e225-408">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-408">No</span></span>          | <span data-ttu-id="6e225-409">O nome do outro contêiner de entidade dentro do mesmo namespace.</span><span class="sxs-lookup"><span data-stu-id="6e225-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-410">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="6e225-411">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-412">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-413">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-413">Example</span></span>

<span data-ttu-id="6e225-414">A exemplo a seguir mostra uma **EntityContainer** elemento que define três conjuntos de entidade e duas associações.</span><span class="sxs-lookup"><span data-stu-id="6e225-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="6e225-415">Elemento EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="6e225-416">O **EntitySet** elemento na linguagem de definição de esquema conceitual é um contêiner lógico para instâncias de um tipo de entidade e instâncias de qualquer tipo que é derivado do tipo de objeto.</span><span class="sxs-lookup"><span data-stu-id="6e225-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="6e225-417">A relação entre um tipo de entidade e um conjunto de entidades é análoga a relação entre uma linha e uma tabela no banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="6e225-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="6e225-418">Como uma linha, um tipo de entidade define um conjunto de dados relacionados e, como uma tabela, um conjunto de entidades contém instâncias dessa definição.</span><span class="sxs-lookup"><span data-stu-id="6e225-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="6e225-419">Um conjunto de entidades fornece uma compilação para o agrupamento de instâncias do tipo de entidade para que eles possam ser mapeadas para as estruturas de dados relacionados em uma fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="6e225-420">Mais de um conjunto de entidades para um tipo de entidade específico podem ser definidas.</span><span class="sxs-lookup"><span data-stu-id="6e225-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-421">O EF Designer não oferece suporte a modelos conceituais que contêm vários conjuntos de entidades por tipo.</span><span class="sxs-lookup"><span data-stu-id="6e225-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="6e225-422">O **EntitySet** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-423">Elemento Documentation (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6e225-424">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-425">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-425">Applicable Attributes</span></span>

<span data-ttu-id="6e225-426">A tabela a seguir descreve os atributos que podem ser aplicados para o **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="6e225-427">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-427">Attribute Name</span></span> | <span data-ttu-id="6e225-428">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-428">Is Required</span></span> | <span data-ttu-id="6e225-429">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-430">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-430">**Name**</span></span>       | <span data-ttu-id="6e225-431">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-431">Yes</span></span>         | <span data-ttu-id="6e225-432">O nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6e225-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="6e225-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="6e225-433">**EntityType**</span></span> | <span data-ttu-id="6e225-434">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-434">Yes</span></span>         | <span data-ttu-id="6e225-435">O nome totalmente qualificado do tipo de entidade para a qual o conjunto de entidades contém instâncias.</span><span class="sxs-lookup"><span data-stu-id="6e225-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-436">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="6e225-437">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-438">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-439">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-439">Example</span></span>

<span data-ttu-id="6e225-440">A exemplo a seguir mostra uma **EntityContainer** elemento com três **EntitySet** elementos:</span><span class="sxs-lookup"><span data-stu-id="6e225-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

<span data-ttu-id="6e225-441">É possível definir vários conjuntos de entidades por tipo (MEST).</span><span class="sxs-lookup"><span data-stu-id="6e225-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="6e225-442">O exemplo a seguir define um contêiner de entidade com dois conjuntos de entidades para o **livro** tipo de entidade:</span><span class="sxs-lookup"><span data-stu-id="6e225-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="6e225-443">Elemento EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="6e225-444">O **EntityType** elemento representa a estrutura de um conceito de nível superior, como um cliente ou pedido, em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="6e225-445">Um tipo de entidade é um modelo para instâncias de tipos de entidade em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6e225-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="6e225-446">Cada modelo contém as informações a seguir:</span><span class="sxs-lookup"><span data-stu-id="6e225-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="6e225-447">Um nome exclusivo.</span><span class="sxs-lookup"><span data-stu-id="6e225-447">A unique name.</span></span> <span data-ttu-id="6e225-448">(Obrigatório.)</span><span class="sxs-lookup"><span data-stu-id="6e225-448">(Required.)</span></span>
-   <span data-ttu-id="6e225-449">Uma chave de entidade que é definida por uma ou mais propriedades.</span><span class="sxs-lookup"><span data-stu-id="6e225-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="6e225-450">(Obrigatório.)</span><span class="sxs-lookup"><span data-stu-id="6e225-450">(Required.)</span></span>
-   <span data-ttu-id="6e225-451">Propriedades que contêm dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-451">Properties for containing data.</span></span> <span data-ttu-id="6e225-452">(Opcional).</span><span class="sxs-lookup"><span data-stu-id="6e225-452">(Optional.)</span></span>
-   <span data-ttu-id="6e225-453">Propriedades de navegação que permitem a navegação de uma das extremidades de uma associação para a outra extremidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="6e225-454">(Opcional).</span><span class="sxs-lookup"><span data-stu-id="6e225-454">(Optional.)</span></span>

<span data-ttu-id="6e225-455">Em um aplicativo, uma instância de um tipo de entidade representa um objeto específico (como um cliente ou uma ordem específica.)</span><span class="sxs-lookup"><span data-stu-id="6e225-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="6e225-456">Cada instância de um tipo de entidade deve ter uma chave de entidade exclusivo dentro de um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6e225-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="6e225-457">Duas instâncias do tipo de entidade são consideradas iguais somente se são do mesmo tipo e os valores das chaves de entidade são os mesmos.</span><span class="sxs-lookup"><span data-stu-id="6e225-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="6e225-458">Uma **EntityType** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-459">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-460">Chave (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="6e225-461">Propriedade (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="6e225-462">NavigationProperty (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="6e225-463">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-464">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-464">Applicable Attributes</span></span>

<span data-ttu-id="6e225-465">A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="6e225-466">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="6e225-467">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-467">Is Required</span></span> | <span data-ttu-id="6e225-468">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-469">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="6e225-470">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-470">Yes</span></span>         | <span data-ttu-id="6e225-471">O nome do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="6e225-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="6e225-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="6e225-473">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-473">No</span></span>          | <span data-ttu-id="6e225-474">O nome de outro tipo de entidade que é o tipo base do tipo de entidade que está sendo definido.</span><span class="sxs-lookup"><span data-stu-id="6e225-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="6e225-475">**Abstrato**</span><span class="sxs-lookup"><span data-stu-id="6e225-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="6e225-476">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-476">No</span></span>          | <span data-ttu-id="6e225-477">**Verdadeiro** ou **falso**, dependendo se o tipo de entidade é um tipo abstrato.</span><span class="sxs-lookup"><span data-stu-id="6e225-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="6e225-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="6e225-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="6e225-479">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-479">No</span></span>          | <span data-ttu-id="6e225-480">**Verdadeiro** ou **falso** dependendo se o tipo de entidade é um tipo de entidade aberto.</span><span class="sxs-lookup"><span data-stu-id="6e225-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="6e225-481">> O **OpenType** atributo só é aplicável aos tipos de entidade que são definidos em modelos conceituais que são usados com o ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="6e225-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="6e225-482">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="6e225-483">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-484">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-485">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-485">Example</span></span>

<span data-ttu-id="6e225-486">A exemplo a seguir mostra uma **EntityType** elemento com três **propriedade** elementos e duas **NavigationProperty** elementos:</span><span class="sxs-lookup"><span data-stu-id="6e225-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="6e225-487">Elemento EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="6e225-488">O **EnumType** elemento representa um tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="6e225-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="6e225-489">Uma **EnumType** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-490">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-491">Membro (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="6e225-492">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-493">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-493">Applicable Attributes</span></span>

<span data-ttu-id="6e225-494">A tabela a seguir descreve os atributos que podem ser aplicados para o **EnumType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="6e225-495">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-495">Attribute Name</span></span>     | <span data-ttu-id="6e225-496">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-496">Is Required</span></span> | <span data-ttu-id="6e225-497">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-498">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-498">**Name**</span></span>           | <span data-ttu-id="6e225-499">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-499">Yes</span></span>         | <span data-ttu-id="6e225-500">O nome do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="6e225-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="6e225-501">**IsFlags**</span></span>        | <span data-ttu-id="6e225-502">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-502">No</span></span>          | <span data-ttu-id="6e225-503">**Verdadeiro** ou **falso**, dependendo se o tipo de enumeração pode ser usado como um conjunto de sinalizadores.</span><span class="sxs-lookup"><span data-stu-id="6e225-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="6e225-504">O valor padrão é **falsos.**.</span><span class="sxs-lookup"><span data-stu-id="6e225-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="6e225-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="6e225-505">**UnderlyingType**</span></span> | <span data-ttu-id="6e225-506">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-506">No</span></span>          | <span data-ttu-id="6e225-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** ou **Edm.SByte** definindo o intervalo de valores do tipo.</span><span class="sxs-lookup"><span data-stu-id="6e225-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="6e225-508">O tipo dos elementos de enumeração subjacente padrão é **Edm.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="6e225-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-509">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EnumType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="6e225-510">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-511">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-512">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-512">Example</span></span>

<span data-ttu-id="6e225-513">A exemplo a seguir mostra uma **EnumType** elemento com três **membro** elementos:</span><span class="sxs-lookup"><span data-stu-id="6e225-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="6e225-514">Elemento de função (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-514">Function Element (CSDL)</span></span>

<span data-ttu-id="6e225-515">O **função** elemento na linguagem de definição de esquema conceitual (CSDL) é usado para definir ou declare as funções no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="6e225-516">Uma função é definida usando um elemento DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="6e225-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="6e225-517">Um **função** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-518">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-519">Parâmetro (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="6e225-520">DefiningExpression (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="6e225-521">ReturnType (função) (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="6e225-522">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="6e225-523">Retornar de um tipo para uma função deve ser especificado com qualquer um de **ReturnType** elemento (função) ou o **ReturnType** atributo (veja abaixo), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="6e225-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="6e225-524">Os tipos de retorno possíveis são qualquer EdmSimpleType, entidade tipo, tipo complexo, tipo de linha, ou tipo ref (ou uma coleção de um desses tipos).</span><span class="sxs-lookup"><span data-stu-id="6e225-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-525">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-525">Applicable Attributes</span></span>

<span data-ttu-id="6e225-526">A tabela a seguir descreve os atributos que podem ser aplicados para o **função** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="6e225-527">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-527">Attribute Name</span></span> | <span data-ttu-id="6e225-528">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-528">Is Required</span></span> | <span data-ttu-id="6e225-529">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="6e225-530">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-530">**Name**</span></span>       | <span data-ttu-id="6e225-531">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-531">Yes</span></span>         | <span data-ttu-id="6e225-532">O nome da função.</span><span class="sxs-lookup"><span data-stu-id="6e225-532">The name of the function.</span></span>          |
| <span data-ttu-id="6e225-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="6e225-533">**ReturnType**</span></span> | <span data-ttu-id="6e225-534">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-534">No</span></span>          | <span data-ttu-id="6e225-535">O tipo retornado pela função.</span><span class="sxs-lookup"><span data-stu-id="6e225-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-536">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **função** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="6e225-537">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-538">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-539">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-539">Example</span></span>

<span data-ttu-id="6e225-540">O exemplo a seguir usa uma **função** elemento para definir uma função que retorna o número de anos, desde que um instrutor foi contratado.</span><span class="sxs-lookup"><span data-stu-id="6e225-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="6e225-541">Elemento FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="6e225-542">O **FunctionImport** elemento na linguagem de definição de esquema conceitual (CSDL) representa uma função que está definido na fonte de dados, mas está disponível para objetos por meio do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="6e225-543">Por exemplo, um elemento de função no modelo de armazenamento pode ser usado para representar um procedimento armazenado em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="6e225-544">Um **FunctionImport** elemento no modelo conceitual representa a função correspondente em um aplicativo do Entity Framework e é mapeado para a função de modelo de armazenamento usando o elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="6e225-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="6e225-545">Quando a função é chamada no aplicativo, o procedimento armazenado correspondente é executado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="6e225-546">O **FunctionImport** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-547">Documentação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6e225-548">Parâmetro (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="6e225-549">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="6e225-550">ReturnType (FunctionImport) (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="6e225-551">Uma **parâmetro** elemento deve ser definido para cada parâmetro que a função aceita.</span><span class="sxs-lookup"><span data-stu-id="6e225-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="6e225-552">Retornar de um tipo para uma função deve ser especificado com qualquer um de **ReturnType** elemento (FunctionImport) ou o **ReturnType** atributo (veja abaixo), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="6e225-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="6e225-553">O valor do tipo de retorno deve ser uma coleção de EdmSimpleType, EntityType ou ComplexType.</span><span class="sxs-lookup"><span data-stu-id="6e225-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-554">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-554">Applicable Attributes</span></span>

<span data-ttu-id="6e225-555">A tabela a seguir descreve os atributos que podem ser aplicados para o **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="6e225-556">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-556">Attribute Name</span></span>   | <span data-ttu-id="6e225-557">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-557">Is Required</span></span> | <span data-ttu-id="6e225-558">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-559">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-559">**Name**</span></span>         | <span data-ttu-id="6e225-560">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-560">Yes</span></span>         | <span data-ttu-id="6e225-561">O nome da função importada.</span><span class="sxs-lookup"><span data-stu-id="6e225-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="6e225-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="6e225-562">**ReturnType**</span></span>   | <span data-ttu-id="6e225-563">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-563">No</span></span>          | <span data-ttu-id="6e225-564">O tipo que a função retorna.</span><span class="sxs-lookup"><span data-stu-id="6e225-564">The type that the function returns.</span></span> <span data-ttu-id="6e225-565">Não use esse atributo se a função não retorna um valor.</span><span class="sxs-lookup"><span data-stu-id="6e225-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="6e225-566">Caso contrário, o valor deve ser uma coleção de EDMSimpleType, EntityType ou ComplexType.</span><span class="sxs-lookup"><span data-stu-id="6e225-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="6e225-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="6e225-567">**EntitySet**</span></span>    | <span data-ttu-id="6e225-568">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-568">No</span></span>          | <span data-ttu-id="6e225-569">Se a função retorna uma coleção de entidade tipos, o valor da **EntitySet** devem ser o conjunto de entidades ao qual pertence a coleção.</span><span class="sxs-lookup"><span data-stu-id="6e225-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="6e225-570">Caso contrário, o **EntitySet** atributo não deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="6e225-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="6e225-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="6e225-571">**IsComposable**</span></span> | <span data-ttu-id="6e225-572">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-572">No</span></span>          | <span data-ttu-id="6e225-573">Se o valor for definido como true, a função é passível de composição (Table-valued Function) e pode ser usada em uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="6e225-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="6e225-574">O padrão é **false**.</span><span class="sxs-lookup"><span data-stu-id="6e225-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="6e225-575">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="6e225-576">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-577">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-578">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-578">Example</span></span>

<span data-ttu-id="6e225-579">A exemplo a seguir mostra uma **FunctionImport** elemento que aceita um parâmetro e retorna uma coleção de tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="6e225-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="6e225-580">Elemento key (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-580">Key Element (CSDL)</span></span>

<span data-ttu-id="6e225-581">O **chave** elemento é um elemento filho do elemento EntityType e define um *chave de entidade* (uma propriedade ou um conjunto de propriedades que determinam a identidade de um tipo de entidade).</span><span class="sxs-lookup"><span data-stu-id="6e225-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="6e225-582">As propriedades que compõem uma chave de entidade são escolhidas em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="6e225-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="6e225-583">Os valores das propriedades de chave de entidade devem identificar exclusivamente uma instância do tipo de entidade dentro de uma entidade definida em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="6e225-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="6e225-584">As propriedades que compõem uma chave de entidade devem ser escolhidas para garantir a exclusividade de instâncias em um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6e225-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="6e225-585">O **chave** elemento define uma chave de entidade, fazendo referência a um ou mais das propriedades de um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="6e225-586">O **chave** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="6e225-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="6e225-587">PropertyRef (um ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6e225-588">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-589">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-589">Applicable Attributes</span></span>

<span data-ttu-id="6e225-590">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **chave** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="6e225-591">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-592">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6e225-593">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-593">Example</span></span>

<span data-ttu-id="6e225-594">O exemplo a seguir define um tipo de entidade nomeado **livro**.</span><span class="sxs-lookup"><span data-stu-id="6e225-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="6e225-595">A chave de entidade é definida fazendo referência a **ISBN** propriedade do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="6e225-596">O **ISBN** propriedade é uma boa opção para a chave de entidade porque um número padrão de livro (ISBN) internacional identifica um livro.</span><span class="sxs-lookup"><span data-stu-id="6e225-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="6e225-597">O exemplo a seguir mostra um tipo de entidade (**autor**) que tem uma chave de entidade que consiste em duas propriedades, **nome** e **endereço**.</span><span class="sxs-lookup"><span data-stu-id="6e225-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

<span data-ttu-id="6e225-598">Usando o **nome** e **endereço** da entidade para a chave é uma opção razoável, porque dois autores de mesmo nome serão improvável de viver no mesmo endereço.</span><span class="sxs-lookup"><span data-stu-id="6e225-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="6e225-599">No entanto, esta opção para uma chave de entidade não garante absolutamente chaves exclusivas de entidade em um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6e225-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="6e225-600">Adicionando uma propriedade, tal como **AuthorId**, que pode ser usado para identificar exclusivamente um autor seria recomendado nesse caso.</span><span class="sxs-lookup"><span data-stu-id="6e225-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="6e225-601">Elemento Member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-601">Member Element (CSDL)</span></span>

<span data-ttu-id="6e225-602">O **membro** elemento é um elemento filho do elemento EnumType e define um membro do tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="6e225-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-603">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-603">Applicable Attributes</span></span>

<span data-ttu-id="6e225-604">A tabela a seguir descreve os atributos que podem ser aplicados para o **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="6e225-605">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-605">Attribute Name</span></span> | <span data-ttu-id="6e225-606">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-606">Is Required</span></span> | <span data-ttu-id="6e225-607">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-608">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-608">**Name**</span></span>       | <span data-ttu-id="6e225-609">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-609">Yes</span></span>         | <span data-ttu-id="6e225-610">O nome do membro.</span><span class="sxs-lookup"><span data-stu-id="6e225-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="6e225-611">**Value**</span><span class="sxs-lookup"><span data-stu-id="6e225-611">**Value**</span></span>      | <span data-ttu-id="6e225-612">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-612">No</span></span>          | <span data-ttu-id="6e225-613">O valor do membro.</span><span class="sxs-lookup"><span data-stu-id="6e225-613">The value of the member.</span></span> <span data-ttu-id="6e225-614">Por padrão, o primeiro membro tem o valor 0 e o valor de cada enumerador seguinte é incrementado em 1.</span><span class="sxs-lookup"><span data-stu-id="6e225-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="6e225-615">Podem existir vários membros com os mesmos valores.</span><span class="sxs-lookup"><span data-stu-id="6e225-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-616">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="6e225-617">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-618">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-619">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-619">Example</span></span>

<span data-ttu-id="6e225-620">A exemplo a seguir mostra uma **EnumType** elemento com três **membro** elementos:</span><span class="sxs-lookup"><span data-stu-id="6e225-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="6e225-621">Elemento NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="6e225-622">Um **NavigationProperty** elemento define uma propriedade de navegação, que fornece uma referência a outra extremidade de uma associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="6e225-623">Ao contrário das propriedades definidas com o elemento de propriedade, as propriedades de navegação não definem a forma e características dos dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="6e225-624">Eles fornecem uma maneira de navegar de uma associação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="6e225-625">Observe que as propriedades de navegação são opcionais em ambos os tipos de entidade termina de uma associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="6e225-626">Se você definir uma propriedade de navegação em um tipo de entidade no final de uma associação, você não precisa definir uma propriedade de navegação no tipo de entidade no outro extremo de associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="6e225-627">O tipo de dados retornado por uma propriedade de navegação é determinado pela multiplicidade de extremidade remoto da associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="6e225-628">Por exemplo, suponha que uma propriedade de navegação **OrdersNavProp**, existe em um **Customer** tipo de entidade e navega de uma associação um-para-muitos entre **cliente** e  **Ordem**.</span><span class="sxs-lookup"><span data-stu-id="6e225-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="6e225-629">Como o fim remoto da associação para a propriedade de navegação tem multiplicidade de muitos (\*), seu tipo de dados é uma coleção (de **ordem**).</span><span class="sxs-lookup"><span data-stu-id="6e225-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="6e225-630">Da mesma forma, se uma propriedade de navegação **CustomerNavProp**, existe na **ordem** tipo de entidade, seu tipo de dados seria **cliente** , pois a multiplicidade de extremidade remoto é um (1).</span><span class="sxs-lookup"><span data-stu-id="6e225-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="6e225-631">Um **NavigationProperty** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-632">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-633">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-634">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-634">Applicable Attributes</span></span>

<span data-ttu-id="6e225-635">A tabela a seguir descreve os atributos que podem ser aplicados para o **NavigationProperty** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="6e225-636">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-636">Attribute Name</span></span>   | <span data-ttu-id="6e225-637">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-637">Is Required</span></span> | <span data-ttu-id="6e225-638">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-639">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-639">**Name**</span></span>         | <span data-ttu-id="6e225-640">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-640">Yes</span></span>         | <span data-ttu-id="6e225-641">O nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="6e225-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="6e225-642">**Relação**</span><span class="sxs-lookup"><span data-stu-id="6e225-642">**Relationship**</span></span> | <span data-ttu-id="6e225-643">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-643">Yes</span></span>         | <span data-ttu-id="6e225-644">O nome de uma associação que está dentro do escopo do modelo.</span><span class="sxs-lookup"><span data-stu-id="6e225-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="6e225-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="6e225-645">**ToRole**</span></span>       | <span data-ttu-id="6e225-646">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-646">Yes</span></span>         | <span data-ttu-id="6e225-647">O final da associação no qual a navegação é encerrada.</span><span class="sxs-lookup"><span data-stu-id="6e225-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="6e225-648">O valor da **ToRole** atributo deve ser igual ao valor de um dos **função** atributos definidos em uma das extremidades da associação (definidas no elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="6e225-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="6e225-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="6e225-649">**FromRole**</span></span>     | <span data-ttu-id="6e225-650">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-650">Yes</span></span>         | <span data-ttu-id="6e225-651">O final da associação da qual iniciar a navegação.</span><span class="sxs-lookup"><span data-stu-id="6e225-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="6e225-652">O valor da **FromRole** atributo deve ser igual ao valor de um dos **função** atributos definidos em uma das extremidades da associação (definidas no elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="6e225-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-653">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **NavigationProperty** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="6e225-654">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-655">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-656">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-656">Example</span></span>

<span data-ttu-id="6e225-657">O exemplo a seguir define um tipo de entidade (**livro**) com duas propriedades de navegação (**PublishedBy** e **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="6e225-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="6e225-658">Elemento OnDelete (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="6e225-659">O **OnDelete** elemento na linguagem de definição de esquema conceitual (CSDL) define o comportamento que está conectado com uma associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="6e225-660">Se o **ação** atributo é definido como **Cascade** relacionados em uma extremidade de uma associação, os tipos de entidade na outra extremidade da associação são excluídos quando o tipo de entidade no primeiro final é excluído.</span><span class="sxs-lookup"><span data-stu-id="6e225-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="6e225-661">Se a associação entre dois tipos de entidade é uma relação de chave primária de chave para o primário, um objeto dependente carregado é excluído quando o objeto de entidade na outra extremidade da associação é excluído, independentemente do **OnDelete** especificação.</span><span class="sxs-lookup"><span data-stu-id="6e225-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="6e225-662">O **OnDelete** elemento só afeta o comportamento de tempo de execução de um aplicativo; ele não afeta o comportamento da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="6e225-663">O comportamento definido na fonte de dados deve ser o mesmo que o comportamento definido no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6e225-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="6e225-664">Uma **OnDelete** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-665">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-666">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-667">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-667">Applicable Attributes</span></span>

<span data-ttu-id="6e225-668">A tabela a seguir descreve os atributos que podem ser aplicados para o **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="6e225-669">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-669">Attribute Name</span></span> | <span data-ttu-id="6e225-670">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-670">Is Required</span></span> | <span data-ttu-id="6e225-671">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-672">**Ação**</span><span class="sxs-lookup"><span data-stu-id="6e225-672">**Action**</span></span>     | <span data-ttu-id="6e225-673">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-673">Yes</span></span>         | <span data-ttu-id="6e225-674">**CASCADE** ou **None**.</span><span class="sxs-lookup"><span data-stu-id="6e225-674">**Cascade** or **None**.</span></span> <span data-ttu-id="6e225-675">Se **Cascade**, tipos de entidade dependente serão excluídos quando o tipo de entidade de segurança de entidade é excluído.</span><span class="sxs-lookup"><span data-stu-id="6e225-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="6e225-676">Se **None**, tipos de entidade dependente não serão excluídos quando o tipo de entidade de segurança de entidade é excluído.</span><span class="sxs-lookup"><span data-stu-id="6e225-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-677">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="6e225-678">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-679">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-680">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-680">Example</span></span>

<span data-ttu-id="6e225-681">A exemplo a seguir mostra uma **associação** elemento que define o **CustomerOrders** associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="6e225-682">O **OnDelete** elemento indica que todos os **pedidos** que estão relacionados a um determinado **cliente** e terem sido carregados para o ObjectContext serão excluídos quando o  **Cliente** é excluído.</span><span class="sxs-lookup"><span data-stu-id="6e225-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="6e225-683">Elemento Parameter (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="6e225-684">O **parâmetro** elemento na linguagem de definição de esquema conceitual (CSDL) pode ser um filho do elemento FunctionImport ou o elemento de função.</span><span class="sxs-lookup"><span data-stu-id="6e225-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="6e225-685">Elemento FunctionImport aplicativo</span><span class="sxs-lookup"><span data-stu-id="6e225-685">FunctionImport Element Application</span></span>

<span data-ttu-id="6e225-686">Um **parâmetro** elemento (como um filho de **FunctionImport** elemento) é usado para definir parâmetros de entrada e saídos para importações de função que são declarados no CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="6e225-687">O **parâmetro** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-688">Documentação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6e225-689">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="6e225-690">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-690">Applicable Attributes</span></span>

<span data-ttu-id="6e225-691">A tabela a seguir descreve os atributos que podem ser aplicados para o **parâmetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="6e225-692">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-692">Attribute Name</span></span> | <span data-ttu-id="6e225-693">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-693">Is Required</span></span> | <span data-ttu-id="6e225-694">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-695">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-695">**Name**</span></span>       | <span data-ttu-id="6e225-696">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-696">Yes</span></span>         | <span data-ttu-id="6e225-697">O nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e225-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6e225-698">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-698">**Type**</span></span>       | <span data-ttu-id="6e225-699">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-699">Yes</span></span>         | <span data-ttu-id="6e225-700">O tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e225-700">The parameter type.</span></span> <span data-ttu-id="6e225-701">O valor deve ser um **EDMSimpleType** ou um tipo complexo que está dentro do escopo do modelo.</span><span class="sxs-lookup"><span data-stu-id="6e225-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="6e225-702">**Modo**</span><span class="sxs-lookup"><span data-stu-id="6e225-702">**Mode**</span></span>       | <span data-ttu-id="6e225-703">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-703">No</span></span>          | <span data-ttu-id="6e225-704">**Na**, **Out**, ou **InOut** dependendo se o parâmetro for uma entrada, saída ou parâmetro de entrada/saída.</span><span class="sxs-lookup"><span data-stu-id="6e225-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="6e225-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-705">**MaxLength**</span></span>  | <span data-ttu-id="6e225-706">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-706">No</span></span>          | <span data-ttu-id="6e225-707">O comprimento máximo permitido do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e225-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="6e225-708">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="6e225-708">**Precision**</span></span>  | <span data-ttu-id="6e225-709">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-709">No</span></span>          | <span data-ttu-id="6e225-710">A precisão do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e225-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6e225-711">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="6e225-711">**Scale**</span></span>      | <span data-ttu-id="6e225-712">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-712">No</span></span>          | <span data-ttu-id="6e225-713">A escala do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e225-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="6e225-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6e225-714">**SRID**</span></span>       | <span data-ttu-id="6e225-715">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-715">No</span></span>          | <span data-ttu-id="6e225-716">Identificador de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="6e225-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6e225-717">Válido somente para parâmetros de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="6e225-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="6e225-718">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e225-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-719">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **parâmetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="6e225-720">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-721">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6e225-722">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-722">Example</span></span>

<span data-ttu-id="6e225-723">A exemplo a seguir mostra uma **FunctionImport** elemento com um **parâmetro** elemento filho.</span><span class="sxs-lookup"><span data-stu-id="6e225-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="6e225-724">A função aceita um parâmetro de entrada e retorna uma coleção de tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="6e225-725">Aplicativo de elemento de função</span><span class="sxs-lookup"><span data-stu-id="6e225-725">Function Element Application</span></span>

<span data-ttu-id="6e225-726">Um **parâmetro** elemento (como um filho de **função** elemento) define os parâmetros para as funções que são definidas ou declarados em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="6e225-727">O **parâmetro** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-728">Documentação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="6e225-729">CollectionType (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="6e225-730">ReferenceType (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="6e225-731">RowType (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-732">Somente um dos **CollectionType**, **ReferenceType**, ou **RowType** elementos podem ser um elemento filho de um **propriedade** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="6e225-733">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-734">Elementos de anotação devem aparecer após todos os outros elementos filho.</span><span class="sxs-lookup"><span data-stu-id="6e225-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="6e225-735">Elementos de anotação só são permitidos no CSDL v2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="6e225-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6e225-736">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-736">Applicable Attributes</span></span>

<span data-ttu-id="6e225-737">A tabela a seguir descreve os atributos que podem ser aplicados para o **parâmetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="6e225-738">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-738">Attribute Name</span></span>   | <span data-ttu-id="6e225-739">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-739">Is Required</span></span> | <span data-ttu-id="6e225-740">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-741">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-741">**Name**</span></span>         | <span data-ttu-id="6e225-742">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-742">Yes</span></span>         | <span data-ttu-id="6e225-743">O nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e225-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6e225-744">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-744">**Type**</span></span>         | <span data-ttu-id="6e225-745">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-745">No</span></span>          | <span data-ttu-id="6e225-746">O tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e225-746">The parameter type.</span></span> <span data-ttu-id="6e225-747">Um parâmetro pode ser qualquer um dos seguintes tipos de (ou coleções desses tipos):</span><span class="sxs-lookup"><span data-stu-id="6e225-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="6e225-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6e225-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="6e225-749">tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="6e225-749">entity type</span></span> <br/> <span data-ttu-id="6e225-750">tipo complexo</span><span class="sxs-lookup"><span data-stu-id="6e225-750">complex type</span></span> <br/> <span data-ttu-id="6e225-751">tipo de linha</span><span class="sxs-lookup"><span data-stu-id="6e225-751">row type</span></span> <br/> <span data-ttu-id="6e225-752">tipo de referência</span><span class="sxs-lookup"><span data-stu-id="6e225-752">reference type</span></span>                             |
| <span data-ttu-id="6e225-753">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="6e225-753">**Nullable**</span></span>     | <span data-ttu-id="6e225-754">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-754">No</span></span>          | <span data-ttu-id="6e225-755">**Verdadeiro** (o valor padrão) ou **falsos** dependendo se a propriedade pode ter um **nulo** valor.</span><span class="sxs-lookup"><span data-stu-id="6e225-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="6e225-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6e225-756">**DefaultValue**</span></span> | <span data-ttu-id="6e225-757">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-757">No</span></span>          | <span data-ttu-id="6e225-758">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6e225-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-759">**MaxLength**</span></span>    | <span data-ttu-id="6e225-760">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-760">No</span></span>          | <span data-ttu-id="6e225-761">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6e225-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-762">**FixedLength**</span></span>  | <span data-ttu-id="6e225-763">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-763">No</span></span>          | <span data-ttu-id="6e225-764">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="6e225-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6e225-765">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="6e225-765">**Precision**</span></span>    | <span data-ttu-id="6e225-766">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-766">No</span></span>          | <span data-ttu-id="6e225-767">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6e225-768">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="6e225-768">**Scale**</span></span>        | <span data-ttu-id="6e225-769">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-769">No</span></span>          | <span data-ttu-id="6e225-770">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6e225-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6e225-771">**SRID**</span></span>         | <span data-ttu-id="6e225-772">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-772">No</span></span>          | <span data-ttu-id="6e225-773">Identificador de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="6e225-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6e225-774">Válido somente para as propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="6e225-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6e225-775">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e225-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6e225-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6e225-776">**Unicode**</span></span>      | <span data-ttu-id="6e225-777">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-777">No</span></span>          | <span data-ttu-id="6e225-778">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="6e225-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6e225-779">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="6e225-779">**Collation**</span></span>    | <span data-ttu-id="6e225-780">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-780">No</span></span>          | <span data-ttu-id="6e225-781">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6e225-782">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **parâmetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="6e225-783">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-784">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6e225-785">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-785">Example</span></span>

<span data-ttu-id="6e225-786">A exemplo a seguir mostra uma **função** que usa um elemento **parâmetro** elemento filho para definir um parâmetro de função.</span><span class="sxs-lookup"><span data-stu-id="6e225-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="6e225-787">Elemento de entidade de segurança (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="6e225-788">O **Principal** na linguagem de definição de esquema conceitual (CSDL) é um elemento filho ao elemento ReferentialConstraint que define o final principal de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="6e225-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="6e225-789">Um **ReferentialConstraint** elemento define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="6e225-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6e225-790">Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados podem fazer referência a chave primária de outra tabela, uma propriedade (ou propriedades) de um tipo de entidade podem fazer referência à chave de entidade de outro tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6e225-791">O tipo de entidade que é referenciado é chamado de *final principal* da restrição.</span><span class="sxs-lookup"><span data-stu-id="6e225-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6e225-792">O tipo de entidade que faz referência a extremidade de entidade é chamado de *final dependente* da restrição.</span><span class="sxs-lookup"><span data-stu-id="6e225-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="6e225-793">**PropertyRef** elementos são usados para especificar quais teclas são referenciadas pelo final dependente.</span><span class="sxs-lookup"><span data-stu-id="6e225-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="6e225-794">O **Principal** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-795">PropertyRef (um ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6e225-796">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-797">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-797">Applicable Attributes</span></span>

<span data-ttu-id="6e225-798">A tabela a seguir descreve os atributos que podem ser aplicados para o **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="6e225-799">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-799">Attribute Name</span></span> | <span data-ttu-id="6e225-800">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-800">Is Required</span></span> | <span data-ttu-id="6e225-801">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="6e225-802">**Função**</span><span class="sxs-lookup"><span data-stu-id="6e225-802">**Role**</span></span>       | <span data-ttu-id="6e225-803">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-803">Yes</span></span>         | <span data-ttu-id="6e225-804">O nome do tipo de entidade no final da associação de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="6e225-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-805">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="6e225-806">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-807">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-808">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-808">Example</span></span>

<span data-ttu-id="6e225-809">A exemplo a seguir mostra uma **ReferentialConstraint** que faz parte da definição de elemento de **PublishedBy** associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="6e225-810">O **Id** propriedade da **Publisher** tipo de entidade constitui o final principal de restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="6e225-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="6e225-811">Elemento de propriedade (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-811">Property Element (CSDL)</span></span>

<span data-ttu-id="6e225-812">O **propriedade** elemento na linguagem de definição de esquema conceitual (CSDL) pode ser um filho do elemento EntityType, ComplexType elemento ou o elemento RowType.</span><span class="sxs-lookup"><span data-stu-id="6e225-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="6e225-813">EntityType e aplicativos de elemento ComplexType</span><span class="sxs-lookup"><span data-stu-id="6e225-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="6e225-814">**Propriedade** elementos (como filhos **EntityType** ou **ComplexType** elementos) definem a forma e características dos dados que contém uma instância do tipo de entidade ou a instância de tipo complexo .</span><span class="sxs-lookup"><span data-stu-id="6e225-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="6e225-815">Propriedades em um modelo conceitual são análogas às propriedades que são definidas em uma classe.</span><span class="sxs-lookup"><span data-stu-id="6e225-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="6e225-816">Da mesma forma que as propriedades em uma classe definem a forma da classe e transportam informações sobre objetos, as propriedades em um modelo conceitual definem a forma de um tipo de entidade e transportam informações sobre as instâncias dos tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="6e225-817">O **propriedade** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-818">Elemento Documentation (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6e225-819">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="6e225-820">Facetas podem ser aplicadas a um **propriedade** elemento: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **precisão**, **escala**, **Unicode**, **agrupamento**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="6e225-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="6e225-821">As facetas são atributos XML que fornecem informações sobre como os valores de propriedade são armazenados no armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-822">Facetas só podem ser aplicadas às propriedades do tipo **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="6e225-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6e225-823">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-823">Applicable Attributes</span></span>

<span data-ttu-id="6e225-824">A tabela a seguir descreve os atributos que podem ser aplicados para o **propriedade** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="6e225-825">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-825">Attribute Name</span></span>                                                         | <span data-ttu-id="6e225-826">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-826">Is Required</span></span> | <span data-ttu-id="6e225-827">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-828">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-828">**Name**</span></span>                                                               | <span data-ttu-id="6e225-829">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-829">Yes</span></span>         | <span data-ttu-id="6e225-830">O nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="6e225-831">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-831">**Type**</span></span>                                                               | <span data-ttu-id="6e225-832">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-832">Yes</span></span>         | <span data-ttu-id="6e225-833">O tipo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-833">The type of the property value.</span></span> <span data-ttu-id="6e225-834">O tipo de valor de propriedade deve ser um **EDMSimpleType** ou um tipo complexo (indicado por um nome totalmente qualificado) que está dentro do escopo do modelo.</span><span class="sxs-lookup"><span data-stu-id="6e225-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="6e225-835">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="6e225-835">**Nullable**</span></span>                                                           | <span data-ttu-id="6e225-836">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-836">No</span></span>          | <span data-ttu-id="6e225-837">**Verdadeiro** (o valor padrão) ou <strong>falso</strong> dependendo se a propriedade pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="6e225-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="6e225-838">> O v1 CSDL uma propriedade de tipo complexo deve ter `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6e225-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6e225-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6e225-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="6e225-840">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-840">No</span></span>          | <span data-ttu-id="6e225-841">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6e225-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="6e225-843">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-843">No</span></span>          | <span data-ttu-id="6e225-844">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6e225-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="6e225-846">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-846">No</span></span>          | <span data-ttu-id="6e225-847">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="6e225-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6e225-848">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="6e225-848">**Precision**</span></span>                                                          | <span data-ttu-id="6e225-849">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-849">No</span></span>          | <span data-ttu-id="6e225-850">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6e225-851">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="6e225-851">**Scale**</span></span>                                                              | <span data-ttu-id="6e225-852">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-852">No</span></span>          | <span data-ttu-id="6e225-853">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6e225-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6e225-854">**SRID**</span></span>                                                               | <span data-ttu-id="6e225-855">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-855">No</span></span>          | <span data-ttu-id="6e225-856">Identificador de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="6e225-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6e225-857">Válido somente para as propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="6e225-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6e225-858">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e225-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6e225-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6e225-859">**Unicode**</span></span>                                                            | <span data-ttu-id="6e225-860">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-860">No</span></span>          | <span data-ttu-id="6e225-861">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="6e225-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6e225-862">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="6e225-862">**Collation**</span></span>                                                          | <span data-ttu-id="6e225-863">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-863">No</span></span>          | <span data-ttu-id="6e225-864">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="6e225-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="6e225-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="6e225-866">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-866">No</span></span>          | <span data-ttu-id="6e225-867">**None** (o valor padrão) ou **fixo**.</span><span class="sxs-lookup"><span data-stu-id="6e225-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="6e225-868">Se o valor for definido como **fixo**, o valor da propriedade será usado na verificação de simultaneidade otimista.</span><span class="sxs-lookup"><span data-stu-id="6e225-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="6e225-869">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **propriedade** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="6e225-870">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-871">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6e225-872">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-872">Example</span></span>

<span data-ttu-id="6e225-873">A exemplo a seguir mostra uma **EntityType** elemento com três **propriedade** elementos:</span><span class="sxs-lookup"><span data-stu-id="6e225-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="6e225-874">A exemplo a seguir mostra uma **ComplexType** elemento com cinco **propriedade** elementos:</span><span class="sxs-lookup"><span data-stu-id="6e225-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="6e225-875">O elemento RowType aplicativo</span><span class="sxs-lookup"><span data-stu-id="6e225-875">RowType Element Application</span></span>

<span data-ttu-id="6e225-876">**Propriedade** elementos (como os filhos de um **RowType** elemento) definem a forma e características dos dados que podem ser passadas para ou retornados de uma função definida pelo modelo.</span><span class="sxs-lookup"><span data-stu-id="6e225-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="6e225-877">O **propriedade** elemento pode ter exatamente um dos seguintes elementos filhos:</span><span class="sxs-lookup"><span data-stu-id="6e225-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="6e225-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="6e225-878">CollectionType</span></span>
-   <span data-ttu-id="6e225-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="6e225-879">ReferenceType</span></span>
-   <span data-ttu-id="6e225-880">RowType</span><span class="sxs-lookup"><span data-stu-id="6e225-880">RowType</span></span>

<span data-ttu-id="6e225-881">O **propriedade** elemento pode ter qualquer número elementos de anotação de filho.</span><span class="sxs-lookup"><span data-stu-id="6e225-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-882">Elementos de anotação só são permitidos no CSDL v2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="6e225-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6e225-883">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-883">Applicable Attributes</span></span>

<span data-ttu-id="6e225-884">A tabela a seguir descreve os atributos que podem ser aplicados para o **propriedade** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="6e225-885">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-885">Attribute Name</span></span>                                                     | <span data-ttu-id="6e225-886">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-886">Is Required</span></span> | <span data-ttu-id="6e225-887">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-888">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-888">**Name**</span></span>                                                           | <span data-ttu-id="6e225-889">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-889">Yes</span></span>         | <span data-ttu-id="6e225-890">O nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="6e225-891">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-891">**Type**</span></span>                                                           | <span data-ttu-id="6e225-892">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-892">Yes</span></span>         | <span data-ttu-id="6e225-893">O tipo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6e225-894">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="6e225-894">**Nullable**</span></span>                                                       | <span data-ttu-id="6e225-895">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-895">No</span></span>          | <span data-ttu-id="6e225-896">**Verdadeiro** (o valor padrão) ou **falso** dependendo se a propriedade pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="6e225-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="6e225-897">> No CSDL v1 uma propriedade de tipo complexo deve ter `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6e225-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6e225-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6e225-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="6e225-899">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-899">No</span></span>          | <span data-ttu-id="6e225-900">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6e225-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="6e225-902">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-902">No</span></span>          | <span data-ttu-id="6e225-903">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6e225-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="6e225-905">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-905">No</span></span>          | <span data-ttu-id="6e225-906">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="6e225-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6e225-907">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="6e225-907">**Precision**</span></span>                                                      | <span data-ttu-id="6e225-908">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-908">No</span></span>          | <span data-ttu-id="6e225-909">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6e225-910">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="6e225-910">**Scale**</span></span>                                                          | <span data-ttu-id="6e225-911">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-911">No</span></span>          | <span data-ttu-id="6e225-912">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6e225-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6e225-913">**SRID**</span></span>                                                           | <span data-ttu-id="6e225-914">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-914">No</span></span>          | <span data-ttu-id="6e225-915">Identificador de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="6e225-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6e225-916">Válido somente para as propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="6e225-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6e225-917">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e225-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6e225-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6e225-918">**Unicode**</span></span>                                                        | <span data-ttu-id="6e225-919">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-919">No</span></span>          | <span data-ttu-id="6e225-920">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="6e225-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6e225-921">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="6e225-921">**Collation**</span></span>                                                      | <span data-ttu-id="6e225-922">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-922">No</span></span>          | <span data-ttu-id="6e225-923">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6e225-924">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **propriedade** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="6e225-925">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-926">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6e225-927">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-927">Example</span></span>

<span data-ttu-id="6e225-928">A exemplo a seguir mostra **propriedade** elementos usados para definir a forma do tipo de retorno de uma função definida pelo modelo.</span><span class="sxs-lookup"><span data-stu-id="6e225-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="6e225-929">Elemento PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="6e225-930">O **PropertyRef** elemento na linguagem de definição de esquema conceitual (CSDL) faz referência a uma propriedade de um tipo de entidade para indicar que a propriedade realizará uma das seguintes funções:</span><span class="sxs-lookup"><span data-stu-id="6e225-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="6e225-931">Parte da chave da entidade (uma propriedade ou um conjunto de propriedades que determinam a identidade de um tipo de entidade).</span><span class="sxs-lookup"><span data-stu-id="6e225-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="6e225-932">Um ou mais **PropertyRef** elementos podem ser usados para definir uma chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="6e225-933">O final dependente ou entidade de segurança de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="6e225-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="6e225-934">O **PropertyRef** elemento só pode ter elementos de anotação (zero ou mais) como elementos filho.</span><span class="sxs-lookup"><span data-stu-id="6e225-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-935">Elementos de anotação só são permitidos no CSDL v2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="6e225-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-936">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-936">Applicable Attributes</span></span>

<span data-ttu-id="6e225-937">A tabela a seguir descreve os atributos que podem ser aplicados para o **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="6e225-938">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-938">Attribute Name</span></span> | <span data-ttu-id="6e225-939">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-939">Is Required</span></span> | <span data-ttu-id="6e225-940">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="6e225-941">**Nome**</span><span class="sxs-lookup"><span data-stu-id="6e225-941">**Name**</span></span>       | <span data-ttu-id="6e225-942">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-942">Yes</span></span>         | <span data-ttu-id="6e225-943">O nome da propriedade referenciada.</span><span class="sxs-lookup"><span data-stu-id="6e225-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-944">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="6e225-945">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-946">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-947">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-947">Example</span></span>

<span data-ttu-id="6e225-948">O exemplo a seguir define um tipo de entidade (**livro**).</span><span class="sxs-lookup"><span data-stu-id="6e225-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="6e225-949">A chave de entidade é definida fazendo referência a **ISBN** propriedade do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="6e225-950">No exemplo a seguir, dois **PropertyRef** elementos são usados para indicar se duas propriedades (**Id** e **PublisherId**) são as extremidades de entidade e dependentes de um referencial restrição.</span><span class="sxs-lookup"><span data-stu-id="6e225-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="6e225-951">Elemento ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="6e225-952">O **ReferenceType** elemento na linguagem de definição de esquema conceitual (CSDL) especifica uma referência a um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="6e225-953">O **ReferenceType** elemento pode ser um filho dos elementos a seguir:</span><span class="sxs-lookup"><span data-stu-id="6e225-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="6e225-954">ReturnType (função)</span><span class="sxs-lookup"><span data-stu-id="6e225-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="6e225-955">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6e225-955">Parameter</span></span>
-   <span data-ttu-id="6e225-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="6e225-956">CollectionType</span></span>

<span data-ttu-id="6e225-957">O **ReferenceType** elemento é usado ao definir um tipo de parâmetro ou retornado para uma função.</span><span class="sxs-lookup"><span data-stu-id="6e225-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="6e225-958">Um **ReferenceType** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-959">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-960">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-961">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-961">Applicable Attributes</span></span>

<span data-ttu-id="6e225-962">A tabela a seguir descreve os atributos que podem ser aplicados para o **ReferenceType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="6e225-963">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-963">Attribute Name</span></span> | <span data-ttu-id="6e225-964">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-964">Is Required</span></span> | <span data-ttu-id="6e225-965">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="6e225-966">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-966">**Type**</span></span>       | <span data-ttu-id="6e225-967">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-967">Yes</span></span>         | <span data-ttu-id="6e225-968">O nome do tipo de entidade que está sendo referenciado.</span><span class="sxs-lookup"><span data-stu-id="6e225-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-969">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReferenceType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="6e225-970">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-971">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-972">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-972">Example</span></span>

<span data-ttu-id="6e225-973">A exemplo a seguir mostra a **ReferenceType** elemento usado como um filho de um **parâmetro** elemento em uma função definida modelo que aceita uma referência a um **pessoa** entidade tipo:</span><span class="sxs-lookup"><span data-stu-id="6e225-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="6e225-974">A exemplo a seguir mostra a **ReferenceType** elemento usado como um filho de um **ReturnType** elemento (função) em uma função definida modelo que retorna uma referência a um **pessoa**tipo de entidade:</span><span class="sxs-lookup"><span data-stu-id="6e225-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="6e225-975">Elemento ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="6e225-976">Um **ReferentialConstraint** elemento na linguagem de definição de esquema conceitual (CSDL) define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="6e225-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6e225-977">Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados podem fazer referência a chave primária de outra tabela, uma propriedade (ou propriedades) de um tipo de entidade podem fazer referência à chave de entidade de outro tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6e225-978">O tipo de entidade que é referenciado é chamado de *final principal* da restrição.</span><span class="sxs-lookup"><span data-stu-id="6e225-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6e225-979">O tipo de entidade que faz referência a extremidade de entidade é chamado de *final dependente* da restrição.</span><span class="sxs-lookup"><span data-stu-id="6e225-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="6e225-980">Se uma chave estrangeira que é exposta no tipo de uma entidade faz referência a uma propriedade em outro tipo de entidade, o **ReferentialConstraint** elemento define uma associação entre os dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="6e225-981">Porque o **ReferentialConstraint** elemento fornece informações sobre como dois tipos de entidade são relacionados, nenhum elemento AssociationSetMapping correspondente é necessário na linguagem de especificação de mapeamento (MSL).</span><span class="sxs-lookup"><span data-stu-id="6e225-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="6e225-982">Uma associação entre dois tipos de entidade que não têm chaves estrangeiras expostas deve ter um correspondente **AssociationSetMapping** elemento para mapear as informações de associação à fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="6e225-983">Se uma chave estrangeira não é exposta em um tipo de entidade, o **ReferentialConstraint** elemento só pode definir uma restrição de chave primária chave para o primário entre o tipo de entidade e o outro tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="6e225-984">Um **ReferentialConstraint** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-985">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-986">Entidade de segurança (exatamente um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="6e225-987">Dependente (exatamente um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="6e225-988">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-989">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-989">Applicable Attributes</span></span>

<span data-ttu-id="6e225-990">O **ReferentialConstraint** elemento pode ter qualquer número de atributos de anotação (atributos personalizados de XML).</span><span class="sxs-lookup"><span data-stu-id="6e225-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="6e225-991">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-992">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6e225-993">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-993">Example</span></span>

<span data-ttu-id="6e225-994">A exemplo a seguir mostra uma **ReferentialConstraint** elemento que está sendo usado como parte da definição da **PublishedBy** associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="6e225-995">Elemento ReturnType (função) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="6e225-996">O **ReturnType** elemento (função) na linguagem de definição de esquema conceitual (CSDL) Especifica o tipo de retorno para uma função que é definida em um elemento de função.</span><span class="sxs-lookup"><span data-stu-id="6e225-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="6e225-997">Uma função de tipo de retorno também pode ser especificado com um **ReturnType** atributo.</span><span class="sxs-lookup"><span data-stu-id="6e225-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="6e225-998">Retornar tipos podem ser qualquer **EdmSimpleType**, tipo de entidade, tipo complexo, tipo de linha, tipo ref ou uma coleção de um desses tipos.</span><span class="sxs-lookup"><span data-stu-id="6e225-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="6e225-999">O tipo de retorno de uma função pode ser especificado com o **tipo** atributo da **ReturnType** elemento (função), ou com um dos seguintes elementos filhos:</span><span class="sxs-lookup"><span data-stu-id="6e225-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="6e225-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="6e225-1000">CollectionType</span></span>
-   <span data-ttu-id="6e225-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="6e225-1001">ReferenceType</span></span>
-   <span data-ttu-id="6e225-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="6e225-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-1003">Um modelo não validará se você especificar uma função retornar um tipo com ambos os **tipo** atributo da **ReturnType** elemento (função) e um dos elementos filho.</span><span class="sxs-lookup"><span data-stu-id="6e225-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-1004">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-1004">Applicable Attributes</span></span>

<span data-ttu-id="6e225-1005">A tabela a seguir descreve os atributos que podem ser aplicados para o **ReturnType** elemento (função).</span><span class="sxs-lookup"><span data-stu-id="6e225-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="6e225-1006">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-1006">Attribute Name</span></span> | <span data-ttu-id="6e225-1007">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-1007">Is Required</span></span> | <span data-ttu-id="6e225-1008">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="6e225-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="6e225-1009">**ReturnType**</span></span> | <span data-ttu-id="6e225-1010">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1010">No</span></span>          | <span data-ttu-id="6e225-1011">O tipo retornado pela função.</span><span class="sxs-lookup"><span data-stu-id="6e225-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-1012">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReturnType** elemento (função).</span><span class="sxs-lookup"><span data-stu-id="6e225-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="6e225-1013">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-1014">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-1015">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1015">Example</span></span>

<span data-ttu-id="6e225-1016">O exemplo a seguir usa uma **função** elemento para definir uma função que retorna o número de anos que foi um livro na impressão.</span><span class="sxs-lookup"><span data-stu-id="6e225-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="6e225-1017">Observe que o tipo de retorno for especificado, o **tipo** atributo de um **ReturnType** elemento (função).</span><span class="sxs-lookup"><span data-stu-id="6e225-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="6e225-1018">ReturnType (FunctionImport) elemento (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="6e225-1019">O **ReturnType** (FunctionImport) elemento na linguagem de definição de esquema conceitual (CSDL) Especifica o tipo de retorno para uma função que é definido em um elemento FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="6e225-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="6e225-1020">Uma função de tipo de retorno também pode ser especificado com um **ReturnType** atributo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="6e225-1021">Retornar tipos podem ser qualquer coleção de tipo de entidade, tipo complexo, ou **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="6e225-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="6e225-1022">O tipo de retorno de uma função é especificado com o **tipo** atributo da **ReturnType** elemento (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="6e225-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-1023">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-1023">Applicable Attributes</span></span>

<span data-ttu-id="6e225-1024">A tabela a seguir descreve os atributos que podem ser aplicados para o **ReturnType** elemento (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="6e225-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="6e225-1025">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-1025">Attribute Name</span></span> | <span data-ttu-id="6e225-1026">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-1026">Is Required</span></span> | <span data-ttu-id="6e225-1027">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-1028">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-1028">**Type**</span></span>       | <span data-ttu-id="6e225-1029">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1029">No</span></span>          | <span data-ttu-id="6e225-1030">O tipo que a função retorna.</span><span class="sxs-lookup"><span data-stu-id="6e225-1030">The type that the function returns.</span></span> <span data-ttu-id="6e225-1031">O valor deve ser uma coleção de EDMSimpleType, EntityType ou ComplexType.</span><span class="sxs-lookup"><span data-stu-id="6e225-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="6e225-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="6e225-1032">**EntitySet**</span></span>  | <span data-ttu-id="6e225-1033">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1033">No</span></span>          | <span data-ttu-id="6e225-1034">Se a função retorna uma coleção de entidade tipos, o valor da **EntitySet** devem ser o conjunto de entidades ao qual pertence a coleção.</span><span class="sxs-lookup"><span data-stu-id="6e225-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="6e225-1035">Caso contrário, o **EntitySet** atributo não deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="6e225-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-1036">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReturnType** elemento (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="6e225-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="6e225-1037">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-1038">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-1039">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1039">Example</span></span>

<span data-ttu-id="6e225-1040">O exemplo a seguir usa uma **FunctionImport** que retorna livros e editores.</span><span class="sxs-lookup"><span data-stu-id="6e225-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="6e225-1041">Observe que a função retorna dois conjuntos de resultados e, portanto, duas **ReturnType** elementos (FunctionImport) são especificados.</span><span class="sxs-lookup"><span data-stu-id="6e225-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="6e225-1042">Elemento RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="6e225-1043">Um **RowType** elemento na linguagem de definição de esquema conceitual (CSDL) define uma estrutura sem nome como um parâmetro ou tipo de retorno para uma função definida no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="6e225-1044">Um **RowType** elemento pode ser o filho dos elementos a seguir:</span><span class="sxs-lookup"><span data-stu-id="6e225-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="6e225-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="6e225-1045">CollectionType</span></span>
-   <span data-ttu-id="6e225-1046">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6e225-1046">Parameter</span></span>
-   <span data-ttu-id="6e225-1047">ReturnType (função)</span><span class="sxs-lookup"><span data-stu-id="6e225-1047">ReturnType (Function)</span></span>

<span data-ttu-id="6e225-1048">Um **RowType** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-1049">Propriedade (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="6e225-1049">Property (one or more)</span></span>
-   <span data-ttu-id="6e225-1050">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="6e225-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-1051">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-1051">Applicable Attributes</span></span>

<span data-ttu-id="6e225-1052">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **RowType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="6e225-1053">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-1054">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6e225-1055">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1055">Example</span></span>

<span data-ttu-id="6e225-1056">O exemplo a seguir mostra uma função definida modelo que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de linhas (conforme especificado na **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="6e225-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a><span data-ttu-id="6e225-1057">Elemento de esquema (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="6e225-1058">O **esquema** é o elemento raiz de uma definição de modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="6e225-1059">Ele contém definições para os contêineres que compõem um modelo conceitual, funções e objetos.</span><span class="sxs-lookup"><span data-stu-id="6e225-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="6e225-1060">O **esquema** elemento pode conter zero ou mais dos seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="6e225-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="6e225-1061">Usando o</span><span class="sxs-lookup"><span data-stu-id="6e225-1061">Using</span></span>
-   <span data-ttu-id="6e225-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="6e225-1062">EntityContainer</span></span>
-   <span data-ttu-id="6e225-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="6e225-1063">EntityType</span></span>
-   <span data-ttu-id="6e225-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="6e225-1064">EnumType</span></span>
-   <span data-ttu-id="6e225-1065">Associação</span><span class="sxs-lookup"><span data-stu-id="6e225-1065">Association</span></span>
-   <span data-ttu-id="6e225-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="6e225-1066">ComplexType</span></span>
-   <span data-ttu-id="6e225-1067">Função</span><span class="sxs-lookup"><span data-stu-id="6e225-1067">Function</span></span>

<span data-ttu-id="6e225-1068">Um **esquema** elemento pode conter zero ou mais elementos de anotação.</span><span class="sxs-lookup"><span data-stu-id="6e225-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-1069">O **função** elemento e os elementos de anotação só são permitidos no CSDL v2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="6e225-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="6e225-1070">O **esquema** elemento usa o **Namespace** atributo para definir o namespace para o tipo de entidade, tipo complexo e objetos de associação em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="6e225-1071">Dentro de um namespace, não possui dois objetos podem ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="6e225-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="6e225-1072">Namespaces pode abranger vários **esquema** elementos e vários arquivos. CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="6e225-1073">Um namespace de modelo conceitual é diferente do namespace de XML o **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="6e225-1074">Um namespace de modelo conceitual (conforme definido pela **Namespace** atributo) é um contêiner lógico para tipos de entidade, tipos complexos e tipos de associação.</span><span class="sxs-lookup"><span data-stu-id="6e225-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="6e225-1075">O namespace de XML (indicado pelo **xmlns** atributo) de uma **esquema** elemento é o namespace padrão para os elementos filho e atributos dos **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="6e225-1076">Namespaces XML do formulário http://schemas.microsoft.com/ado/YYYY/MM/edm (em que aaaa e MM representam um ano e mês, respectivamente) são reservados para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="6e225-1077">Atributos e elementos personalizados não podem ser nos namespaces que têm esse formulário.</span><span class="sxs-lookup"><span data-stu-id="6e225-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-1078">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-1078">Applicable Attributes</span></span>

<span data-ttu-id="6e225-1079">A tabela a seguir descreve os atributos podem ser aplicados para o **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="6e225-1080">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-1080">Attribute Name</span></span> | <span data-ttu-id="6e225-1081">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-1081">Is Required</span></span> | <span data-ttu-id="6e225-1082">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="6e225-1083">**Namespace**</span></span>  | <span data-ttu-id="6e225-1084">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1084">Yes</span></span>         | <span data-ttu-id="6e225-1085">O namespace do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="6e225-1086">O valor de **Namespace** atributo é usado para formar o nome totalmente qualificado de um tipo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="6e225-1087">Por exemplo, se um **EntityType** denominado *Customer* está no namespace Simple.Example.Model e, em seguida, o nome totalmente qualificado do **EntityType** é SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="6e225-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="6e225-1088">As seguintes cadeias de caracteres não podem ser usadas como o valor para o **Namespace** atributo: **sistema**, **transitório**, ou **Edm**.</span><span class="sxs-lookup"><span data-stu-id="6e225-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="6e225-1089">O valor para o **Namespace** atributo não pode ser o mesmo que o valor para o **Namespace** atributo no elemento do esquema de SSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="6e225-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="6e225-1090">**Alias**</span></span>      | <span data-ttu-id="6e225-1091">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1091">No</span></span>          | <span data-ttu-id="6e225-1092">Um identificador usado no lugar do nome de namespace.</span><span class="sxs-lookup"><span data-stu-id="6e225-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="6e225-1093">Por exemplo, se um **EntityType** denominado *Customer* está no namespace Simple.Example.Model e o valor da **Alias** atributo é *modelo*, em seguida, você pode usar Model.Customer como o nome totalmente qualificado do **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="6e225-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="6e225-1094">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="6e225-1095">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-1096">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-1097">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1097">Example</span></span>

<span data-ttu-id="6e225-1098">A exemplo a seguir mostra uma **esquema** elemento que contém uma **EntityContainer** elemento, dois **EntityType** elementos e um **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="6e225-1099">Elemento TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="6e225-1100">O **TypeRef** elemento na linguagem de definição de esquema conceitual (CSDL) fornece uma referência a um tipo nomeado existente.</span><span class="sxs-lookup"><span data-stu-id="6e225-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="6e225-1101">O **TypeRef** elemento pode ser um filho do elemento CollectionType, que é usado para especificar que uma função tem uma coleção como um tipo de parâmetro ou retornado.</span><span class="sxs-lookup"><span data-stu-id="6e225-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="6e225-1102">Um **TypeRef** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="6e225-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6e225-1103">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="6e225-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6e225-1104">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="6e225-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-1105">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-1105">Applicable Attributes</span></span>

<span data-ttu-id="6e225-1106">A tabela a seguir descreve os atributos que podem ser aplicados para o **TypeRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="6e225-1107">Observe que o **DefaultValue**, **MaxLength**, **FixedLength**, **precisão**, **escala**,  **Unicode**, e **agrupamento** atributos só são aplicáveis ao **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="6e225-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="6e225-1108">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="6e225-1109">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-1109">Is Required</span></span> | <span data-ttu-id="6e225-1110">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-1111">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="6e225-1111">**Type**</span></span>                                                           | <span data-ttu-id="6e225-1112">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1112">No</span></span>          | <span data-ttu-id="6e225-1113">O nome do tipo que está sendo referenciado.</span><span class="sxs-lookup"><span data-stu-id="6e225-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="6e225-1114">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="6e225-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="6e225-1115">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1115">No</span></span>          | <span data-ttu-id="6e225-1116">**Verdadeiro** (o valor padrão) ou **falso** dependendo se a propriedade pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="6e225-1117">> No CSDL v1 uma propriedade de tipo complexo deve ter `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6e225-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6e225-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6e225-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="6e225-1119">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1119">No</span></span>          | <span data-ttu-id="6e225-1120">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6e225-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="6e225-1122">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1122">No</span></span>          | <span data-ttu-id="6e225-1123">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6e225-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="6e225-1125">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1125">No</span></span>          | <span data-ttu-id="6e225-1126">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6e225-1127">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="6e225-1127">**Precision**</span></span>                                                      | <span data-ttu-id="6e225-1128">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1128">No</span></span>          | <span data-ttu-id="6e225-1129">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6e225-1130">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="6e225-1130">**Scale**</span></span>                                                          | <span data-ttu-id="6e225-1131">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1131">No</span></span>          | <span data-ttu-id="6e225-1132">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6e225-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6e225-1133">**SRID**</span></span>                                                           | <span data-ttu-id="6e225-1134">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1134">No</span></span>          | <span data-ttu-id="6e225-1135">Identificador de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="6e225-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6e225-1136">Válido somente para as propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="6e225-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6e225-1137">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e225-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6e225-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6e225-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="6e225-1139">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1139">No</span></span>          | <span data-ttu-id="6e225-1140">**Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="6e225-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6e225-1141">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="6e225-1141">**Collation**</span></span>                                                      | <span data-ttu-id="6e225-1142">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1142">No</span></span>          | <span data-ttu-id="6e225-1143">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6e225-1144">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6e225-1145">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-1146">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-1147">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1147">Example</span></span>

<span data-ttu-id="6e225-1148">O exemplo a seguir mostra uma função definida modelo que usa o **TypeRef** elemento (como um filho de um **CollectionType** elemento) para especificar que a função aceita uma coleção de  **Departamento** tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="6e225-1149">Usando o elemento (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="6e225-1150">O **Using** elemento na linguagem de definição de esquema conceitual (CSDL) importa o conteúdo de um modelo conceitual que existe em um namespace diferente.</span><span class="sxs-lookup"><span data-stu-id="6e225-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="6e225-1151">Definindo o valor da **Namespace** atributo, você pode consultar os tipos de entidade, tipos complexos e tipos de associação que são definidos em outro modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="6e225-1152">Mais de um **Using** elemento pode ser um filho de um **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-1153">O **Using** elemento no CSDL não funcionará exatamente como um **usando** instrução em uma linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="6e225-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="6e225-1154">Importando um namespace com um **usando** instrução em uma linguagem de programação, você não afetam os objetos no namespace original.</span><span class="sxs-lookup"><span data-stu-id="6e225-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="6e225-1155">Em CSDL, um namespace importado pode conter um tipo de entidade que é derivado de um tipo de entidade no namespace original.</span><span class="sxs-lookup"><span data-stu-id="6e225-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="6e225-1156">Isso pode afetar os conjuntos de entidades declarados no namespace original.</span><span class="sxs-lookup"><span data-stu-id="6e225-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="6e225-1157">O **Using** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="6e225-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="6e225-1158">Documentação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6e225-1159">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="6e225-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6e225-1160">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-1160">Applicable Attributes</span></span>

<span data-ttu-id="6e225-1161">A tabela a seguir descreve os atributos podem ser aplicados para o **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="6e225-1162">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="6e225-1162">Attribute Name</span></span> | <span data-ttu-id="6e225-1163">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="6e225-1163">Is Required</span></span> | <span data-ttu-id="6e225-1164">Valor</span><span class="sxs-lookup"><span data-stu-id="6e225-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6e225-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="6e225-1165">**Namespace**</span></span>  | <span data-ttu-id="6e225-1166">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1166">Yes</span></span>         | <span data-ttu-id="6e225-1167">O nome do namespace importado.</span><span class="sxs-lookup"><span data-stu-id="6e225-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="6e225-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="6e225-1168">**Alias**</span></span>      | <span data-ttu-id="6e225-1169">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1169">Yes</span></span>         | <span data-ttu-id="6e225-1170">Um identificador usado no lugar do nome de namespace.</span><span class="sxs-lookup"><span data-stu-id="6e225-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="6e225-1171">Embora esse atributo é necessário, ele não é necessário que ele ser usado no lugar do nome do namespace para qualificar nomes de objeto.</span><span class="sxs-lookup"><span data-stu-id="6e225-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6e225-1172">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="6e225-1173">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6e225-1174">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6e225-1175">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1175">Example</span></span>

<span data-ttu-id="6e225-1176">O exemplo a seguir demonstra a **Using** elemento que está sendo usado para importar um namespace que é definida em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="6e225-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="6e225-1177">Observe que o namespace para o **esquema** elemento mostrado é `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="6e225-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="6e225-1178">O `Address` propriedade no `Publisher` **EntityType** é um tipo complexo que é definido na `ExtendedBooksModel` namespace (importados com o **Using** elemento).</span><span class="sxs-lookup"><span data-stu-id="6e225-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="6e225-1179">Atributos de anotação (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="6e225-1180">Atributos de anotação na linguagem de definição de esquema conceitual (CSDL) são os atributos personalizados de XML no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="6e225-1181">Além de ter a estrutura XML válida, o seguinte deve ser verdadeiro para atributos de anotação:</span><span class="sxs-lookup"><span data-stu-id="6e225-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="6e225-1182">Atributos de anotação não devem estar em qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="6e225-1183">Mais de um atributo de anotação pode ser aplicado a um determinado elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="6e225-1184">Os nomes totalmente qualificados de todos os atributos dois anotação não pode ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="6e225-1185">Atributos de anotação podem ser usados para fornecer metadados adicionais sobre os elementos em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="6e225-1186">Os metadados contidos em elementos de anotação podem ser acessado no tempo de execução usando classes no namespace Metadata.</span><span class="sxs-lookup"><span data-stu-id="6e225-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="6e225-1187">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1187">Example</span></span>

<span data-ttu-id="6e225-1188">A exemplo a seguir mostra uma **EntityType** elemento com um atributo de anotação (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="6e225-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="6e225-1189">O exemplo também mostra um elemento de anotação aplicado ao elemento de tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="6e225-1190">O código a seguir recupera os metadados no atributo de anotação e grava-o no console:</span><span class="sxs-lookup"><span data-stu-id="6e225-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="6e225-1191">O código acima pressupõe que o `School.csdl` arquivo está no diretório de saída do projeto e que você tenha adicionado o seguinte `Imports` e `Using` instruções ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="6e225-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="6e225-1192">Elementos de anotação (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="6e225-1193">Elementos de anotação na linguagem de definição de esquema conceitual (CSDL) são os elementos XML personalizados no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="6e225-1194">Além de ter a estrutura XML válida, o seguinte deve ser verdadeiro para elementos de anotação:</span><span class="sxs-lookup"><span data-stu-id="6e225-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="6e225-1195">Elementos de anotação não devem estar em qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="6e225-1196">Mais de um elemento de anotação pode ser um filho de um determinado elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="6e225-1197">Os nomes totalmente qualificados de quaisquer elementos de dois de anotação não pode ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="6e225-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="6e225-1198">Elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="6e225-1199">Elementos de anotação podem ser usados para fornecer metadados adicionais sobre os elementos em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="6e225-1200">Começando com o .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessado no tempo de execução usando classes no namespace Metadata.</span><span class="sxs-lookup"><span data-stu-id="6e225-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="6e225-1201">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1201">Example</span></span>

<span data-ttu-id="6e225-1202">A exemplo a seguir mostra uma **EntityType** elemento com um elemento de anotação (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="6e225-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="6e225-1203">O exemplo também mostra um atributo de anotação aplicado ao elemento de tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="6e225-1204">O código a seguir recupera os metadados no elemento de anotação e grava-o no console:</span><span class="sxs-lookup"><span data-stu-id="6e225-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="6e225-1205">O código acima presume que o arquivo de School está no diretório de saída do projeto e que você tenha adicionado o seguinte `Imports` e `Using` instruções ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="6e225-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="6e225-1206">Tipos de modelo conceituais (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="6e225-1207">Linguagem de definição de esquema conceitual (CSDL) dá suporte a um conjunto de tipos de dados primitivo abstrato, chamados **EDMSimpleTypes**, que definem as propriedades em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="6e225-1208">**EDMSimpleTypes** são proxies para os tipos de dados primitivos que têm suporte no ambiente de hospedagem ou armazenamento.</span><span class="sxs-lookup"><span data-stu-id="6e225-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="6e225-1209">A tabela a seguir lista os tipos de dados primitivos que são compatíveis com a CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="6e225-1210">A tabela também lista as facetas que podem ser aplicadas a cada **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="6e225-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="6e225-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="6e225-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="6e225-1212">Descrição</span><span class="sxs-lookup"><span data-stu-id="6e225-1212">Description</span></span>                                                | <span data-ttu-id="6e225-1213">Facetas aplicáveis</span><span class="sxs-lookup"><span data-stu-id="6e225-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="6e225-1214">**EDM**</span><span class="sxs-lookup"><span data-stu-id="6e225-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="6e225-1215">Contém dados binários.</span><span class="sxs-lookup"><span data-stu-id="6e225-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="6e225-1216">MaxLength, FixedLength, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="6e225-1217">**EDM**</span><span class="sxs-lookup"><span data-stu-id="6e225-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="6e225-1218">Contém o valor **verdadeira** ou **falso**.</span><span class="sxs-lookup"><span data-stu-id="6e225-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="6e225-1219">Anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="6e225-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="6e225-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="6e225-1221">Contém um valor inteiro de 8 bits sem sinal.</span><span class="sxs-lookup"><span data-stu-id="6e225-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="6e225-1222">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1223">**EDM**</span><span class="sxs-lookup"><span data-stu-id="6e225-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="6e225-1224">Representa uma data e hora.</span><span class="sxs-lookup"><span data-stu-id="6e225-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="6e225-1225">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1226">**EDM**</span><span class="sxs-lookup"><span data-stu-id="6e225-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="6e225-1227">Contém uma data e hora como um deslocamento em minutos GMT.</span><span class="sxs-lookup"><span data-stu-id="6e225-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="6e225-1228">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="6e225-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="6e225-1230">Contém um valor numérico com precisão e escala fixa.</span><span class="sxs-lookup"><span data-stu-id="6e225-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="6e225-1231">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1232">**EDM. Double**</span><span class="sxs-lookup"><span data-stu-id="6e225-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="6e225-1233">Contém um número com precisão de 15 dígitos de ponto flutuante</span><span class="sxs-lookup"><span data-stu-id="6e225-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="6e225-1234">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="6e225-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="6e225-1236">Contém um número com precisão de 7 dígitos de ponto flutuante.</span><span class="sxs-lookup"><span data-stu-id="6e225-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="6e225-1237">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="6e225-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="6e225-1239">Contém um identificador exclusivo de 16 bytes.</span><span class="sxs-lookup"><span data-stu-id="6e225-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="6e225-1240">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="6e225-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="6e225-1242">Contém um valor inteiro com sinal de 16 bits.</span><span class="sxs-lookup"><span data-stu-id="6e225-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="6e225-1243">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="6e225-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="6e225-1245">Contém um valor inteiro com sinal de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="6e225-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="6e225-1246">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="6e225-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="6e225-1248">Contém um valor inteiro com sinal de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="6e225-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="6e225-1249">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="6e225-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="6e225-1251">Contém um valor inteiro de 8 bits com sinal.</span><span class="sxs-lookup"><span data-stu-id="6e225-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="6e225-1252">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1253">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="6e225-1253">**Edm.String**</span></span>                   | <span data-ttu-id="6e225-1254">Contém dados de caractere.</span><span class="sxs-lookup"><span data-stu-id="6e225-1254">Contains character data.</span></span>                                   | <span data-ttu-id="6e225-1255">Unicode, FixedLength, MaxLength, agrupamento, precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="6e225-1256">**EDM**</span><span class="sxs-lookup"><span data-stu-id="6e225-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="6e225-1257">Contém uma hora.</span><span class="sxs-lookup"><span data-stu-id="6e225-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="6e225-1258">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="6e225-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6e225-1259">**Geography**</span><span class="sxs-lookup"><span data-stu-id="6e225-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="6e225-1260">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1261">**EDM. geographypoint**</span><span class="sxs-lookup"><span data-stu-id="6e225-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="6e225-1262">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="6e225-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="6e225-1264">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1265">**Geographypolygon**</span><span class="sxs-lookup"><span data-stu-id="6e225-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="6e225-1266">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="6e225-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="6e225-1268">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="6e225-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="6e225-1270">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="6e225-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="6e225-1272">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="6e225-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="6e225-1274">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1275">**EDM**</span><span class="sxs-lookup"><span data-stu-id="6e225-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="6e225-1276">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="6e225-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="6e225-1278">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="6e225-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="6e225-1280">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="6e225-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="6e225-1282">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="6e225-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="6e225-1284">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="6e225-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="6e225-1286">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="6e225-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="6e225-1288">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6e225-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="6e225-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="6e225-1290">Permite valor nulo, padrão, o SRID</span><span class="sxs-lookup"><span data-stu-id="6e225-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="6e225-1291">Facetas (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6e225-1291">Facets (CSDL)</span></span>

<span data-ttu-id="6e225-1292">Facetas na linguagem de definição de esquema conceitual (CSDL) representam as restrições nas propriedades dos tipos de entidade e tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="6e225-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="6e225-1293">As facetas são exibidos como atributos XML em elementos CSDL a seguir:</span><span class="sxs-lookup"><span data-stu-id="6e225-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="6e225-1294">Propriedade</span><span class="sxs-lookup"><span data-stu-id="6e225-1294">Property</span></span>
-   <span data-ttu-id="6e225-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="6e225-1295">TypeRef</span></span>
-   <span data-ttu-id="6e225-1296">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6e225-1296">Parameter</span></span>

<span data-ttu-id="6e225-1297">A tabela a seguir descreve as facetas que têm suporte no CSDL.</span><span class="sxs-lookup"><span data-stu-id="6e225-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="6e225-1298">Todas as facetas são opcionais.</span><span class="sxs-lookup"><span data-stu-id="6e225-1298">All facets are optional.</span></span> <span data-ttu-id="6e225-1299">Algumas facetas listadas abaixo são usadas pelo Entity Framework ao gerar um banco de dados de um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="6e225-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="6e225-1300">Para obter informações sobre tipos de dados em um modelo conceitual, consulte tipos de modelo conceituais (CSDL).</span><span class="sxs-lookup"><span data-stu-id="6e225-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="6e225-1301">Aspecto</span><span class="sxs-lookup"><span data-stu-id="6e225-1301">Facet</span></span>               | <span data-ttu-id="6e225-1302">Descrição</span><span class="sxs-lookup"><span data-stu-id="6e225-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="6e225-1303">Aplica-se a</span><span class="sxs-lookup"><span data-stu-id="6e225-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="6e225-1304">Usado para a geração de banco de dados</span><span class="sxs-lookup"><span data-stu-id="6e225-1304">Used for the database generation</span></span> | <span data-ttu-id="6e225-1305">Usado pelo tempo de execução</span><span class="sxs-lookup"><span data-stu-id="6e225-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="6e225-1306">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="6e225-1306">**Collation**</span></span>       | <span data-ttu-id="6e225-1307">Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="6e225-1308">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="6e225-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6e225-1309">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1309">Yes</span></span>                              | <span data-ttu-id="6e225-1310">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1310">No</span></span>                  |
| <span data-ttu-id="6e225-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="6e225-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="6e225-1312">Indica que o valor da propriedade deve ser usado para verificação de simultaneidade otimista.</span><span class="sxs-lookup"><span data-stu-id="6e225-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="6e225-1313">Todos os **EDMSimpleType** propriedades</span><span class="sxs-lookup"><span data-stu-id="6e225-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6e225-1314">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1314">No</span></span>                               | <span data-ttu-id="6e225-1315">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1315">Yes</span></span>                 |
| <span data-ttu-id="6e225-1316">**Padrão**</span><span class="sxs-lookup"><span data-stu-id="6e225-1316">**Default**</span></span>         | <span data-ttu-id="6e225-1317">Especifica o valor de propriedade padrão se nenhum valor é fornecido em cima de instanciação.</span><span class="sxs-lookup"><span data-stu-id="6e225-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="6e225-1318">Todos os **EDMSimpleType** propriedades</span><span class="sxs-lookup"><span data-stu-id="6e225-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6e225-1319">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1319">Yes</span></span>                              | <span data-ttu-id="6e225-1320">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1320">Yes</span></span>                 |
| <span data-ttu-id="6e225-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-1321">**FixedLength**</span></span>     | <span data-ttu-id="6e225-1322">Especifica se o comprimento do valor da propriedade pode variar.</span><span class="sxs-lookup"><span data-stu-id="6e225-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="6e225-1323">**EDM**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="6e225-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6e225-1324">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1324">Yes</span></span>                              | <span data-ttu-id="6e225-1325">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1325">No</span></span>                  |
| <span data-ttu-id="6e225-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6e225-1326">**MaxLength**</span></span>       | <span data-ttu-id="6e225-1327">Especifica o comprimento máximo de valor de propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="6e225-1328">**EDM**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="6e225-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6e225-1329">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1329">Yes</span></span>                              | <span data-ttu-id="6e225-1330">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1330">No</span></span>                  |
| <span data-ttu-id="6e225-1331">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="6e225-1331">**Nullable**</span></span>        | <span data-ttu-id="6e225-1332">Especifica se a propriedade pode ter um **nulo** valor.</span><span class="sxs-lookup"><span data-stu-id="6e225-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="6e225-1333">Todos os **EDMSimpleType** propriedades</span><span class="sxs-lookup"><span data-stu-id="6e225-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6e225-1334">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1334">Yes</span></span>                              | <span data-ttu-id="6e225-1335">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1335">Yes</span></span>                 |
| <span data-ttu-id="6e225-1336">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="6e225-1336">**Precision**</span></span>       | <span data-ttu-id="6e225-1337">Para propriedades do tipo **Decimal**, especifica o número de dígitos de um valor da propriedade pode ter.</span><span class="sxs-lookup"><span data-stu-id="6e225-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="6e225-1338">Para propriedades do tipo **tempo**, **DateTime**, e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="6e225-1339">**EDM**, **EDM**, **Edm.Decimal**, **EDM**</span><span class="sxs-lookup"><span data-stu-id="6e225-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="6e225-1340">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1340">Yes</span></span>                              | <span data-ttu-id="6e225-1341">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1341">No</span></span>                  |
| <span data-ttu-id="6e225-1342">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="6e225-1342">**Scale**</span></span>           | <span data-ttu-id="6e225-1343">Especifica o número de dígitos à direita do ponto decimal para o valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6e225-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="6e225-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="6e225-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="6e225-1345">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1345">Yes</span></span>                              | <span data-ttu-id="6e225-1346">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1346">No</span></span>                  |
| <span data-ttu-id="6e225-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6e225-1347">**SRID**</span></span>            | <span data-ttu-id="6e225-1348">Especifica a ID de sistema de referência espacial sistema.</span><span class="sxs-lookup"><span data-stu-id="6e225-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="6e225-1349">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e225-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="6e225-1350">**Geography, EDM. geographypoint, Edm.GeographyLineString, geographypolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, EDM, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="6e225-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="6e225-1351">Não</span><span class="sxs-lookup"><span data-stu-id="6e225-1351">No</span></span>                               | <span data-ttu-id="6e225-1352">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1352">Yes</span></span>                 |
| <span data-ttu-id="6e225-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6e225-1353">**Unicode**</span></span>         | <span data-ttu-id="6e225-1354">Indica se o valor da propriedade é armazenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="6e225-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="6e225-1355">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="6e225-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6e225-1356">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1356">Yes</span></span>                              | <span data-ttu-id="6e225-1357">Sim</span><span class="sxs-lookup"><span data-stu-id="6e225-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="6e225-1358">Ao gerar um banco de dados de um modelo conceitual, o Assistente de banco de dados gerar reconhecerão o valor da **StoreGeneratedPattern** o atributo em uma **propriedade** elemento se ele está a seguir namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="6e225-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="6e225-1359">São os valores com suporte para o atributo **Identity** e **computado**.</span><span class="sxs-lookup"><span data-stu-id="6e225-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="6e225-1360">Um valor de **identidade** produzirá uma coluna de banco de dados com um valor de identidade que é gerado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="6e225-1361">Um valor de **computado** produzirá uma coluna com um valor que é computada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6e225-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="6e225-1362">Exemplo</span><span class="sxs-lookup"><span data-stu-id="6e225-1362">Example</span></span>

<span data-ttu-id="6e225-1363">O exemplo a seguir mostra as facetas aplicadas às propriedades de um tipo de entidade:</span><span class="sxs-lookup"><span data-stu-id="6e225-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
