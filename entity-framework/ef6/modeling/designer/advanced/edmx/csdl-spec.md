---
title: Especificação de CSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418773"
---
# <a name="csdl-specification"></a><span data-ttu-id="b9df4-102">Especificação de CSDL</span><span class="sxs-lookup"><span data-stu-id="b9df4-102">CSDL Specification</span></span>
<span data-ttu-id="b9df4-103">O CSDL (linguagem de definição de esquema conceitual) é uma linguagem baseada em XML que descreve as entidades, as relações e as funções que compõem um modelo conceitual de um aplicativo controlado por dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="b9df4-104">Esse modelo conceitual pode ser usado pelo Entity Framework ou WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="b9df4-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="b9df4-105">Os metadados que são descritos com CSDL são usados pelo Entity Framework para mapear entidades e relações que são definidas em um modelo conceitual para uma fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="b9df4-106">Para obter mais informações, consulte Especificação de [SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) e [especificação MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="b9df4-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="b9df4-107">CSDL é a implementação do Entity Framework do Modelo de Dados de Entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="b9df4-108">Em um aplicativo Entity Framework, os metadados do modelo conceitual são carregados de um arquivo. CSDL (escrito em CSDL) em uma instância do System. Data. Metadata. Edm. EdmItemCollection e é acessível usando métodos no Classe System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="b9df4-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="b9df4-109">Entity Framework usa metadados de modelo conceitual para converter consultas em relação ao modelo conceitual para comandos específicos da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="b9df4-110">O designer do EF armazena informações do modelo conceitual em um arquivo. edmx em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="b9df4-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="b9df4-111">No momento da compilação, o designer do EF usa informações em um arquivo. edmx para criar o arquivo. CSDL necessário pelo Entity Framework em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="b9df4-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="b9df4-112">As versões de CSDL são diferenciadas por namespaces XML.</span><span class="sxs-lookup"><span data-stu-id="b9df4-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="b9df4-113">Versão de CSDL</span><span class="sxs-lookup"><span data-stu-id="b9df4-113">CSDL Version</span></span> | <span data-ttu-id="b9df4-114">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="b9df4-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="b9df4-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="b9df4-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="b9df4-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="b9df4-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="b9df4-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="b9df4-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="b9df4-118">Elemento Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-118">Association Element (CSDL)</span></span>

<span data-ttu-id="b9df4-119">Um elemento **Association** define uma relação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="b9df4-120">Uma associação deve especificar os tipos de entidade que estão envolvidos na relação e o número possível de tipos de entidade em cada extremidade da relação, que é conhecida como multiplicidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="b9df4-121">A multiplicidade de uma extremidade de associação pode ter um valor de um (1), zero ou um (0.. 1) ou muitos (\*).</span><span class="sxs-lookup"><span data-stu-id="b9df4-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="b9df4-122">Essas informações são especificadas em dois elementos finais filho.</span><span class="sxs-lookup"><span data-stu-id="b9df4-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="b9df4-123">Instâncias de tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras, se forem expostas em um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="b9df4-124">Em um aplicativo, uma instância de uma associação representa uma associação específica entre instâncias de tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="b9df4-125">As instâncias de associação são agrupadas logicamente em um conjunto de associações.</span><span class="sxs-lookup"><span data-stu-id="b9df4-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="b9df4-126">Um elemento **Association** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-127">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-128">Final (exatamente 2 elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="b9df4-129">ReferentialConstraint (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-130">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-131">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-131">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-132">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="b9df4-133">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-133">Attribute Name</span></span> | <span data-ttu-id="b9df4-134">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-134">Is Required</span></span> | <span data-ttu-id="b9df4-135">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="b9df4-136">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-136">**Name**</span></span>       | <span data-ttu-id="b9df4-137">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-137">Yes</span></span>         | <span data-ttu-id="b9df4-138">O nome da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-139">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="b9df4-140">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-141">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-142">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-142">Example</span></span>

<span data-ttu-id="b9df4-143">O exemplo a seguir mostra um elemento **Association** que define a associação **CustomerOrders** quando chaves estrangeiras não foram expostas nos tipos de entidade **Customer** e **Order** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="b9df4-144">Os valores de **multiplicidade** para cada **extremidade** da Associação indicam que muitos **pedidos** podem ser associados a um **cliente**, mas somente um **cliente** pode ser associado a um **pedido**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="b9df4-145">Além disso, o elemento **OnDelete** indica que todos os **pedidos** relacionados a um **cliente** específico e que foram carregados no ObjectContext serão excluídos se o **cliente** for excluído.</span><span class="sxs-lookup"><span data-stu-id="b9df4-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="b9df4-146">O exemplo a seguir mostra um elemento **Association** que define a associação **CustomerOrders** quando chaves estrangeiras foram expostas nos tipos de entidade **Customer** e **Order** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="b9df4-147">Com as chaves estrangeiras expostas, a relação entre as entidades é gerenciada com um elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="b9df4-148">Um elemento AssociationSetMapping correspondente não é necessário para mapear essa associação para a fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="b9df4-149">Elemento AssociationSet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="b9df4-150">O elemento **AssociationSet** na CSDL (linguagem de definição de esquema conceitual) é um contêiner lógico para instâncias de associação do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="b9df4-151">Um conjunto de associação fornece uma definição para Agrupamento de instâncias de associação para que elas possam ser mapeadas para uma fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="b9df4-152">O elemento **AssociationSet** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-153">Documentação (zero ou um elemento permitido)</span><span class="sxs-lookup"><span data-stu-id="b9df4-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="b9df4-154">End (exatamente dois elementos necessários)</span><span class="sxs-lookup"><span data-stu-id="b9df4-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="b9df4-155">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="b9df4-156">O atributo **Association** especifica o tipo de associação que um conjunto de associação contém.</span><span class="sxs-lookup"><span data-stu-id="b9df4-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="b9df4-157">Os conjuntos de entidades que compõem as extremidades de um conjunto de associação são especificados com exatamente dois elementos **end** filho.</span><span class="sxs-lookup"><span data-stu-id="b9df4-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-158">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-158">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-159">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="b9df4-160">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-160">Attribute Name</span></span>  | <span data-ttu-id="b9df4-161">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-161">Is Required</span></span> | <span data-ttu-id="b9df4-162">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-163">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-163">**Name**</span></span>        | <span data-ttu-id="b9df4-164">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-164">Yes</span></span>         | <span data-ttu-id="b9df4-165">O nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="b9df4-165">The name of the entity set.</span></span> <span data-ttu-id="b9df4-166">O valor do atributo **Name** não pode ser o mesmo que o valor do atributo **Association** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="b9df4-167">**Associação**</span><span class="sxs-lookup"><span data-stu-id="b9df4-167">**Association**</span></span> | <span data-ttu-id="b9df4-168">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-168">Yes</span></span>         | <span data-ttu-id="b9df4-169">O nome totalmente qualificado da associação que o conjunto de associação contém instâncias do.</span><span class="sxs-lookup"><span data-stu-id="b9df4-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="b9df4-170">A associação deve estar no mesmo namespace que o conjunto de associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-171">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="b9df4-172">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-173">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-174">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-174">Example</span></span>

<span data-ttu-id="b9df4-175">O exemplo a seguir mostra um elemento **EntityContainer** com dois elementos **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="b9df4-176">Elemento CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="b9df4-177">O elemento **CollectionType** na linguagem de definição de esquema conceitual (CSDL) especifica que um parâmetro de função ou tipo de retorno de função é uma coleção.</span><span class="sxs-lookup"><span data-stu-id="b9df4-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="b9df4-178">O elemento **CollectionType** pode ser filho do elemento Parameter ou do elemento ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="b9df4-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="b9df4-179">O tipo de coleção pode ser especificado usando o atributo **Type** ou um dos seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="b9df4-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="b9df4-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-180">**CollectionType**</span></span>
-   <span data-ttu-id="b9df4-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="b9df4-181">ReferenceType</span></span>
-   <span data-ttu-id="b9df4-182">RowType</span><span class="sxs-lookup"><span data-stu-id="b9df4-182">RowType</span></span>
-   <span data-ttu-id="b9df4-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="b9df4-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-184">Um modelo não será validado se o tipo de uma coleção for especificado com o atributo **Type** e um elemento filho.</span><span class="sxs-lookup"><span data-stu-id="b9df4-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-185">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-185">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-186">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="b9df4-187">Observe que os atributos **DefaultValue**, **MaxLength**, **cadeia**, **Precision**, **Scale**, **Unicode**e **Collation** são aplicáveis somente às coleções de **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="b9df4-188">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-188">Attribute Name</span></span>                                                          | <span data-ttu-id="b9df4-189">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-189">Is Required</span></span> | <span data-ttu-id="b9df4-190">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-191">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-191">**Type**</span></span>                                                                | <span data-ttu-id="b9df4-192">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-192">No</span></span>          | <span data-ttu-id="b9df4-193">O tipo do conjunto.</span><span class="sxs-lookup"><span data-stu-id="b9df4-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="b9df4-194">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-194">**Nullable**</span></span>                                                            | <span data-ttu-id="b9df4-195">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-195">No</span></span>          | <span data-ttu-id="b9df4-196">**True** (o valor padrão) ou **False**, dependendo se a propriedade pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="b9df4-197">> No CSDL v1, uma propriedade de tipo complexo deve ter `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="b9df4-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="b9df4-198">**ValorPadrão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="b9df4-199">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-199">No</span></span>          | <span data-ttu-id="b9df4-200">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="b9df4-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="b9df4-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="b9df4-202">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-202">No</span></span>          | <span data-ttu-id="b9df4-203">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="b9df4-204">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="b9df4-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="b9df4-205">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-205">No</span></span>          | <span data-ttu-id="b9df4-206">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="b9df4-207">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-207">**Precision**</span></span>                                                           | <span data-ttu-id="b9df4-208">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-208">No</span></span>          | <span data-ttu-id="b9df4-209">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="b9df4-210">**Escala**</span><span class="sxs-lookup"><span data-stu-id="b9df4-210">**Scale**</span></span>                                                               | <span data-ttu-id="b9df4-211">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-211">No</span></span>          | <span data-ttu-id="b9df4-212">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="b9df4-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="b9df4-213">**SRID**</span></span>                                                                | <span data-ttu-id="b9df4-214">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-214">No</span></span>          | <span data-ttu-id="b9df4-215">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="b9df4-216">Válido somente para propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="b9df4-217">   Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="b9df4-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="b9df4-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="b9df4-218">**Unicode**</span></span>                                                             | <span data-ttu-id="b9df4-219">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-219">No</span></span>          | <span data-ttu-id="b9df4-220">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="b9df4-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="b9df4-221">**Ordenação**</span><span class="sxs-lookup"><span data-stu-id="b9df4-221">**Collation**</span></span>                                                           | <span data-ttu-id="b9df4-222">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-222">No</span></span>          | <span data-ttu-id="b9df4-223">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="b9df4-224">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="b9df4-225">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-226">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-227">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-227">Example</span></span>

<span data-ttu-id="b9df4-228">O exemplo a seguir mostra uma função definida pelo modelo que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de tipos de entidade **Person** (conforme especificado com o atributo **ElementType** ).</span><span class="sxs-lookup"><span data-stu-id="b9df4-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="b9df4-229">O exemplo a seguir mostra uma função definida pelo modelo que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas (conforme especificado no elemento **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="b9df4-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="b9df4-230">O exemplo a seguir mostra uma função definida pelo modelo que usa o elemento **CollectionType** para especificar que a função aceita como um parâmetro uma coleção de tipos de entidade **Department** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="b9df4-231">Elemento complexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="b9df4-232">Um elemento **complexType** define uma estrutura de dados composta por propriedades **EdmSimpleType** ou outros tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="b9df4-233">  Um tipo complexo pode ser uma propriedade de um tipo de entidade ou outro tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="b9df4-234">Um tipo complexo é semelhante a um tipo de entidade no que um tipo complexo define dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="b9df4-235">No entanto, há algumas diferenças entre tipos complexos e tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="b9df4-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="b9df4-236">Tipos complexos não têm identidades (ou chaves) e, portanto, não podem existir de forma independente.</span><span class="sxs-lookup"><span data-stu-id="b9df4-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="b9df4-237">Tipos complexos só podem existir como propriedades de tipos de entidade ou outros tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="b9df4-238">Os tipos complexos não podem participar em associações.</span><span class="sxs-lookup"><span data-stu-id="b9df4-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="b9df4-239">Nenhuma extremidade de uma associação pode ser um tipo complexo e, portanto, as propriedades de navegação não podem ser definidas para tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="b9df4-240">Uma propriedade de tipo complexo não pode ter um valor nulo, embora as propriedades escalares de um tipo complexo possam ser definidas como NULL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="b9df4-241">Um elemento **complexType** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-242">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-243">Propriedade (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="b9df4-244">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="b9df4-245">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **complexType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="b9df4-246">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="b9df4-247">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-247">Is Required</span></span> | <span data-ttu-id="b9df4-248">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-249">Nome</span><span class="sxs-lookup"><span data-stu-id="b9df4-249">Name</span></span>                                                                                                           | <span data-ttu-id="b9df4-250">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-250">Yes</span></span>         | <span data-ttu-id="b9df4-251">O nome do tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-251">The name of the complex type.</span></span> <span data-ttu-id="b9df4-252">O nome de um tipo complexo não pode ser igual ao nome de outro tipo complexo, tipo de entidade ou associação que está dentro do escopo do modelo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="b9df4-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="b9df4-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="b9df4-254">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-254">No</span></span>          | <span data-ttu-id="b9df4-255">O nome de outro tipo complexo que é o tipo base do tipo complexo que está sendo definido.</span><span class="sxs-lookup"><span data-stu-id="b9df4-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="b9df4-256">> Este atributo não é aplicável em CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="b9df4-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="b9df4-257">A herança para tipos complexos não tem suporte nessa versão.</span><span class="sxs-lookup"><span data-stu-id="b9df4-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="b9df4-258">Resumo</span><span class="sxs-lookup"><span data-stu-id="b9df4-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="b9df4-259">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-259">No</span></span>          | <span data-ttu-id="b9df4-260">**True** ou **false** (o valor padrão) dependendo se o tipo complexo é um tipo abstrato.</span><span class="sxs-lookup"><span data-stu-id="b9df4-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="b9df4-261">> Este atributo não é aplicável em CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="b9df4-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="b9df4-262">Tipos complexos nessa versão não podem ser tipos abstratos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="b9df4-263">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **complexType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="b9df4-264">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-265">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-266">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-266">Example</span></span>

<span data-ttu-id="b9df4-267">O exemplo a seguir mostra um tipo complexo, **endereço**, com as propriedades **EdmSimpleType** **StreetAddress**, **City**, **EstadoOuProvíncia**, **Country**e **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="b9df4-268">Para definir o **endereço** de tipo complexo (acima) como uma propriedade de um tipo de entidade, você deve declarar o tipo de propriedade na definição de tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="b9df4-269">O exemplo a seguir mostra a propriedade **Address** como um tipo complexo em um tipo de entidade (**Editor**):</span><span class="sxs-lookup"><span data-stu-id="b9df4-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="b9df4-270">Elemento de definição (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="b9df4-271">O elemento de **definição** da linguagem de definição de esquema conceitual (CSDL) contém uma Entity SQL expressão que define uma função no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="b9df4-272">Para fins de validação, um elemento de **definição** pode conter conteúdo arbitrário.</span><span class="sxs-lookup"><span data-stu-id="b9df4-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="b9df4-273">No entanto, Entity Framework gerará uma exceção em tempo de execução se um elemento de **definição** não contiver um Entity SQL válido.</span><span class="sxs-lookup"><span data-stu-id="b9df4-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-274">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-274">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-275">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **definição** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="b9df4-276">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-277">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="b9df4-278">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-278">Example</span></span>

<span data-ttu-id="b9df4-279">O exemplo a seguir usa um elemento de **definição** para definir uma função que retorna o número de anos desde que um livro foi publicado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="b9df4-280">O conteúdo do elemento de **definição** é escrito em Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="b9df4-281">Elemento dependent (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="b9df4-282">O elemento **dependente** na linguagem de definição de esquema conceitual (CSDL) é um elemento filho para o elemento ReferentialConstraint e define a extremidade dependente de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="b9df4-283">Um elemento **ReferentialConstraint** define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="b9df4-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="b9df4-284">Da mesma forma que uma coluna (ou colunas) de uma tabela de base de dados podem fazer referência a chave primária de outra tabela, propriedade (ou propriedades) de tipo de entidade podem fazer referência chave de entidade de outro tipo de objeto.</span><span class="sxs-lookup"><span data-stu-id="b9df4-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="b9df4-285">O tipo de entidade referenciado é chamado de *extremidade principal* da restrição.</span><span class="sxs-lookup"><span data-stu-id="b9df4-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="b9df4-286">O tipo de entidade que faz referência à extremidade principal é chamado de *extremidade dependente* da restrição.</span><span class="sxs-lookup"><span data-stu-id="b9df4-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="b9df4-287">Os elementos **PropertyRef** são usados para especificar quais chaves fazem referência à extremidade principal.</span><span class="sxs-lookup"><span data-stu-id="b9df4-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="b9df4-288">O elemento **dependente** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-289">PropertyRef (um ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="b9df4-290">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-291">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-291">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-292">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **dependente** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="b9df4-293">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-293">Attribute Name</span></span> | <span data-ttu-id="b9df4-294">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-294">Is Required</span></span> | <span data-ttu-id="b9df4-295">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="b9df4-296">**Função**</span><span class="sxs-lookup"><span data-stu-id="b9df4-296">**Role**</span></span>       | <span data-ttu-id="b9df4-297">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-297">Yes</span></span>         | <span data-ttu-id="b9df4-298">O nome do tipo de entidade na extremidade dependente da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-299">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **dependente** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="b9df4-300">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-301">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-302">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-302">Example</span></span>

<span data-ttu-id="b9df4-303">O exemplo a seguir mostra um elemento **ReferentialConstraint** que está sendo usado como parte da definição da Associação **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="b9df4-304">A propriedade **PublisherID** do tipo de entidade **Book** compõe a extremidade dependente da restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="b9df4-305">Elemento Documentation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="b9df4-306">O elemento **Documentation** na CSDL (linguagem de definição de esquema conceitual) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai.</span><span class="sxs-lookup"><span data-stu-id="b9df4-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="b9df4-307">Em um arquivo. edmx, quando o elemento de **documentação** é um filho de um elemento que aparece como um objeto na superfície de design do designer do EF (como uma entidade, associação ou Propriedade), o conteúdo do elemento de **documentação** aparecerá na janela **Propriedades** do Visual Studio para o objeto.</span><span class="sxs-lookup"><span data-stu-id="b9df4-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="b9df4-308">O elemento de **documentação** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-309">**Resumo**: uma breve descrição do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="b9df4-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="b9df4-310">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-310">(zero or one element)</span></span>
-   <span data-ttu-id="b9df4-311">**LongDescription**: uma descrição extensiva do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="b9df4-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="b9df4-312">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-312">(zero or one element)</span></span>
-   <span data-ttu-id="b9df4-313">Elementos de anotação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-313">Annotation elements.</span></span> <span data-ttu-id="b9df4-314">(zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-315">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-315">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-316">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **documentação** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="b9df4-317">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-318">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="b9df4-319">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-319">Example</span></span>

<span data-ttu-id="b9df4-320">O exemplo a seguir mostra o elemento de **documentação** como um elemento filho de um elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="b9df4-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="b9df4-321">Se o trecho de código a seguir estivesse no conteúdo CSDL de um arquivo. edmx, o conteúdo dos elementos **Summary** e **longDescription** apareceria na janela **Propriedades** do Visual Studio quando você clicar no tipo de entidade `Customer`.</span><span class="sxs-lookup"><span data-stu-id="b9df4-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="b9df4-322">Elemento end (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-322">End Element (CSDL)</span></span>

<span data-ttu-id="b9df4-323">O elemento **end** na linguagem de definição de esquema conceitual (CSDL) pode ser filho do elemento Association ou do elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="b9df4-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="b9df4-324">Em cada caso, a função do elemento **final** é diferente e os atributos aplicáveis são diferentes.</span><span class="sxs-lookup"><span data-stu-id="b9df4-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="b9df4-325">Elemento end como um filho do elemento Association</span><span class="sxs-lookup"><span data-stu-id="b9df4-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="b9df4-326">Um elemento **final** (como um filho do elemento **Association** ) identifica o tipo de entidade em uma extremidade de uma associação e o número de instâncias de tipo de entidade que podem existir no final de uma associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="b9df4-327">Termina de associação são definidas como parte de uma associação; uma associação deve ter exatamente duas termina de associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="b9df4-328">As instâncias do tipo de entidade no final de uma associação podem ser acessadas através de propriedades de navegação ou de chaves estrangeiras se são expostos em um tipo de objeto.</span><span class="sxs-lookup"><span data-stu-id="b9df4-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="b9df4-329">Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-330">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-331">OnDelete (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-332">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-333">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-333">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-334">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="b9df4-335">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-335">Attribute Name</span></span>   | <span data-ttu-id="b9df4-336">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-336">Is Required</span></span> | <span data-ttu-id="b9df4-337">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-338">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-338">**Type**</span></span>         | <span data-ttu-id="b9df4-339">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-339">Yes</span></span>         | <span data-ttu-id="b9df4-340">O nome do tipo de entidade em uma extremidade da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="b9df4-341">**Função**</span><span class="sxs-lookup"><span data-stu-id="b9df4-341">**Role**</span></span>         | <span data-ttu-id="b9df4-342">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-342">No</span></span>          | <span data-ttu-id="b9df4-343">Um nome para o final da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-343">A name for the association end.</span></span> <span data-ttu-id="b9df4-344">Se nenhum nome for fornecido, o nome do tipo de entidade na extremidade da Associação será usado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="b9df4-345">**Multiplicidade**</span><span class="sxs-lookup"><span data-stu-id="b9df4-345">**Multiplicity**</span></span> | <span data-ttu-id="b9df4-346">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-346">Yes</span></span>         | <span data-ttu-id="b9df4-347">**1**, **0.. 1**ou **\*** dependendo do número de instâncias de tipo de entidade que podem estar no final da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="b9df4-348">**1** indica que exatamente uma instância de tipo de entidade existe na extremidade da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="b9df4-349">**0.. 1** indica que nenhuma instância de tipo de entidade existe na extremidade da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="b9df4-350">**\*** indica que nenhuma instância de tipo de entidade existe na extremidade da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-351">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="b9df4-352">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-353">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="b9df4-354">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-354">Example</span></span>

<span data-ttu-id="b9df4-355">O exemplo a seguir mostra um elemento **Association** que define a associação **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="b9df4-356">Os valores de **multiplicidade** para cada **extremidade** da Associação indicam que muitos **pedidos** podem ser associados a um **cliente**, mas somente um **cliente** pode ser associado a um **pedido**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="b9df4-357">Além disso, o elemento **OnDelete** indica que todos os **pedidos** relacionados a um **cliente** específico e que foram carregados no ObjectContext serão excluídos se o **cliente** for excluído.</span><span class="sxs-lookup"><span data-stu-id="b9df4-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="b9df4-358">Elemento end como um filho do elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="b9df4-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="b9df4-359">O elemento **end** especifica uma extremidade de um conjunto de associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="b9df4-360">O elemento **AssociationSet** deve conter dois elementos **end** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="b9df4-361">As informações contidas em um elemento **final** são usadas no mapeamento de uma associação definida para uma fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="b9df4-362">Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-363">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-364">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-365">Os elementos de anotação devem aparecer após todos os outros elementos filho.</span><span class="sxs-lookup"><span data-stu-id="b9df4-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="b9df4-366">Os elementos de anotação são permitidos somente no CSDL V2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="b9df4-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-367">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-367">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-368">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="b9df4-369">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-369">Attribute Name</span></span> | <span data-ttu-id="b9df4-370">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-370">Is Required</span></span> | <span data-ttu-id="b9df4-371">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="b9df4-372">**EntitySet**</span></span>  | <span data-ttu-id="b9df4-373">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-373">Yes</span></span>         | <span data-ttu-id="b9df4-374">O nome do elemento **EntitySet** que define uma extremidade do elemento de **AssociationSet** pai.</span><span class="sxs-lookup"><span data-stu-id="b9df4-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="b9df4-375">O elemento **EntitySet** deve ser definido no mesmo contêiner de entidade que o elemento de **AssociationSet** pai.</span><span class="sxs-lookup"><span data-stu-id="b9df4-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="b9df4-376">**Função**</span><span class="sxs-lookup"><span data-stu-id="b9df4-376">**Role**</span></span>       | <span data-ttu-id="b9df4-377">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-377">No</span></span>          | <span data-ttu-id="b9df4-378">O nome da extremidade do conjunto de associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-378">The name of the association set end.</span></span> <span data-ttu-id="b9df4-379">Se o atributo **role** não for usado, o nome do conjunto de associação end será o nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="b9df4-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="b9df4-380">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="b9df4-381">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-382">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="b9df4-383">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-383">Example</span></span>

<span data-ttu-id="b9df4-384">O exemplo a seguir mostra um elemento **EntityContainer** com dois elementos **AssociationSet** , cada um com dois elementos **end** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="b9df4-385">Elemento EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="b9df4-386">O elemento **EntityContainer** na linguagem de definição de esquema conceitual (CSDL) é um contêiner lógico para conjuntos de entidades, conjuntos de associação e importações de função.</span><span class="sxs-lookup"><span data-stu-id="b9df4-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="b9df4-387">Um contêiner de entidade de modelo conceitual é mapeado para um contêiner de entidade de modelo de armazenamento por meio do elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="b9df4-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="b9df4-388">Um contêiner de entidade do modelo de armazenamento descreve a estrutura do banco de dados: os conjuntos de entidades descrevem tabelas, os conjuntos de associação descrevem as restrições de chave estrangeira e as importações de função descrevem procedimentos armazenados em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="b9df4-389">Um elemento **EntityContainer** pode ter zero ou um elementos de documentação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="b9df4-390">Se um elemento de **documentação** estiver presente, ele deverá preceder todos os elementos **EntitySet**, **AssociationSet**e **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="b9df4-391">Um elemento **EntityContainer** pode ter zero ou mais dos seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="b9df4-392">EntitySet</span></span>
-   <span data-ttu-id="b9df4-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="b9df4-393">AssociationSet</span></span>
-   <span data-ttu-id="b9df4-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="b9df4-394">FunctionImport</span></span>
-   <span data-ttu-id="b9df4-395">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="b9df4-395">Annotation elements</span></span>

<span data-ttu-id="b9df4-396">Você pode estender um elemento **EntityContainer** para incluir o conteúdo de outro **EntityContainer** que está dentro do mesmo namespace.</span><span class="sxs-lookup"><span data-stu-id="b9df4-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="b9df4-397">Para incluir o conteúdo de outro **EntityContainer**, no elemento **EntityContainer** de referência, defina o valor do atributo **extends** como o nome do elemento **EntityContainer** que você deseja incluir.</span><span class="sxs-lookup"><span data-stu-id="b9df4-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="b9df4-398">Todos os elementos filho do elemento **EntityContainer** incluído serão tratados como elementos filho do elemento **EntityContainer** de referência.</span><span class="sxs-lookup"><span data-stu-id="b9df4-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-399">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-399">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-400">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="b9df4-401">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-401">Attribute Name</span></span> | <span data-ttu-id="b9df4-402">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-402">Is Required</span></span> | <span data-ttu-id="b9df4-403">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="b9df4-404">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-404">**Name**</span></span>       | <span data-ttu-id="b9df4-405">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-405">Yes</span></span>         | <span data-ttu-id="b9df4-406">O nome do contêiner de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="b9df4-407">**Estende**</span><span class="sxs-lookup"><span data-stu-id="b9df4-407">**Extends**</span></span>    | <span data-ttu-id="b9df4-408">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-408">No</span></span>          | <span data-ttu-id="b9df4-409">O nome de outro contêiner de entidade no mesmo namespace.</span><span class="sxs-lookup"><span data-stu-id="b9df4-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-410">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="b9df4-411">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-412">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-413">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-413">Example</span></span>

<span data-ttu-id="b9df4-414">O exemplo a seguir mostra um elemento **EntityContainer** que define três conjuntos de entidades e dois conjuntos de associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="b9df4-415">Elemento EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="b9df4-416">O elemento **EntitySet** na linguagem de definição de esquema conceitual é um contêiner lógico para instâncias de um tipo de entidade e instâncias de qualquer tipo que é derivado desse tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="b9df4-417">A relação entre um tipo de entidade e um conjunto de entidades é análoga à relação entre uma linha e uma tabela em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="b9df4-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="b9df4-418">Como uma linha, um tipo de entidade define um conjunto de dados relacionados e, como uma tabela, um conjunto de entidades contém instâncias dessa definição.</span><span class="sxs-lookup"><span data-stu-id="b9df4-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="b9df4-419">Um conjunto de entidades fornece um constructo para agrupar instâncias de tipo de entidade para que elas possam ser mapeadas para estruturas de dados relacionadas em uma fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="b9df4-420">Mais de um conjunto de entidades para um determinado tipo de entidade pode ser definido.</span><span class="sxs-lookup"><span data-stu-id="b9df4-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-421">O designer do EF não oferece suporte a modelos conceituais que contêm vários conjuntos de entidades por tipo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="b9df4-422">O elemento **EntitySet** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-423">Elemento Documentation (zero ou um elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="b9df4-424">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-425">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-425">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-426">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="b9df4-427">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-427">Attribute Name</span></span> | <span data-ttu-id="b9df4-428">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-428">Is Required</span></span> | <span data-ttu-id="b9df4-429">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-430">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-430">**Name**</span></span>       | <span data-ttu-id="b9df4-431">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-431">Yes</span></span>         | <span data-ttu-id="b9df4-432">O nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="b9df4-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="b9df4-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-433">**EntityType**</span></span> | <span data-ttu-id="b9df4-434">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-434">Yes</span></span>         | <span data-ttu-id="b9df4-435">O nome totalmente qualificado do tipo de entidade para o qual o conjunto de entidades contém instâncias.</span><span class="sxs-lookup"><span data-stu-id="b9df4-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-436">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="b9df4-437">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-438">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-439">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-439">Example</span></span>

<span data-ttu-id="b9df4-440">O exemplo a seguir mostra um elemento **EntityContainer** com três elementos **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="b9df4-441">É possível definir vários conjuntos de entidades por tipo (MEST).</span><span class="sxs-lookup"><span data-stu-id="b9df4-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="b9df4-442">O exemplo a seguir define um contêiner de entidade com dois conjuntos de entidades para o tipo de entidade de **livro** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="b9df4-443">Elemento EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="b9df4-444">O elemento **EntityType** representa a estrutura de um conceito de nível superior, como um cliente ou uma ordem, em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="b9df4-445">Um tipo de entidade é um modelo para instâncias de tipos de entidade em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="b9df4-446">Cada modelo contém as informações a seguir:</span><span class="sxs-lookup"><span data-stu-id="b9df4-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="b9df4-447">Um nome exclusivo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-447">A unique name.</span></span> <span data-ttu-id="b9df4-448">(Obrigatório.)</span><span class="sxs-lookup"><span data-stu-id="b9df4-448">(Required.)</span></span>
-   <span data-ttu-id="b9df4-449">Uma chave de entidade que é definida por uma ou mais propriedades.</span><span class="sxs-lookup"><span data-stu-id="b9df4-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="b9df4-450">(Obrigatório.)</span><span class="sxs-lookup"><span data-stu-id="b9df4-450">(Required.)</span></span>
-   <span data-ttu-id="b9df4-451">Propriedades para conter dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-451">Properties for containing data.</span></span> <span data-ttu-id="b9df4-452">(Opcional).</span><span class="sxs-lookup"><span data-stu-id="b9df4-452">(Optional.)</span></span>
-   <span data-ttu-id="b9df4-453">Propriedades de navegação que permitem a navegação de uma extremidade de uma associação para a outra extremidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="b9df4-454">(Opcional).</span><span class="sxs-lookup"><span data-stu-id="b9df4-454">(Optional.)</span></span>

<span data-ttu-id="b9df4-455">Em um aplicativo, uma instância de um tipo de entidade representa um objeto específico (como um cliente ou uma ordem específica.)</span><span class="sxs-lookup"><span data-stu-id="b9df4-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="b9df4-456">Cada instância de um tipo de entidade deve ter chave de entidade exclusivo dentro de conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="b9df4-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="b9df4-457">Duas instâncias do tipo de entidade são consideradas iguais somente se são do mesmo tipo e os valores das chaves de entidade são os mesmos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="b9df4-458">Um elemento **EntityType** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-459">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-460">Chave (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-461">Propriedade (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="b9df4-462">NavigationProperty (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="b9df4-463">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-464">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-464">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-465">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="b9df4-466">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="b9df4-467">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-467">Is Required</span></span> | <span data-ttu-id="b9df4-468">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-469">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="b9df4-470">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-470">Yes</span></span>         | <span data-ttu-id="b9df4-471">O nome do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="b9df4-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="b9df4-473">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-473">No</span></span>          | <span data-ttu-id="b9df4-474">O nome de outro tipo de entidade que é o tipo básico do tipo de entidade que está sendo definido.</span><span class="sxs-lookup"><span data-stu-id="b9df4-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="b9df4-475">**Resume**</span><span class="sxs-lookup"><span data-stu-id="b9df4-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="b9df4-476">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-476">No</span></span>          | <span data-ttu-id="b9df4-477">**True** ou **false**, dependendo se o tipo de entidade é um tipo abstrato.</span><span class="sxs-lookup"><span data-stu-id="b9df4-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="b9df4-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="b9df4-479">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-479">No</span></span>          | <span data-ttu-id="b9df4-480">**True** ou **false** dependendo se o tipo de entidade é um tipo de entidade aberto.</span><span class="sxs-lookup"><span data-stu-id="b9df4-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="b9df4-481">> O atributo **OpenType** só é aplicável a tipos de entidade que são definidos em modelos conceituais que são usados com os serviços de dados do ADO.net.</span><span class="sxs-lookup"><span data-stu-id="b9df4-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="b9df4-482">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="b9df4-483">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-484">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-485">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-485">Example</span></span>

<span data-ttu-id="b9df4-486">O exemplo a seguir mostra um elemento **EntityType** com três elementos **Property** e dois elementos **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="b9df4-487">Elemento EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="b9df4-488">O elemento **enumType** representa um tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="b9df4-489">Um elemento **enumType** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-490">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-491">Membro (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="b9df4-492">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-493">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-493">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-494">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **enumType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="b9df4-495">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-495">Attribute Name</span></span>     | <span data-ttu-id="b9df4-496">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-496">Is Required</span></span> | <span data-ttu-id="b9df4-497">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-498">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-498">**Name**</span></span>           | <span data-ttu-id="b9df4-499">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-499">Yes</span></span>         | <span data-ttu-id="b9df4-500">O nome do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="b9df4-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="b9df4-501">**IsFlags**</span></span>        | <span data-ttu-id="b9df4-502">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-502">No</span></span>          | <span data-ttu-id="b9df4-503">**True** ou **false**, dependendo se o tipo de enumeração pode ser usado como um conjunto de sinalizadores.</span><span class="sxs-lookup"><span data-stu-id="b9df4-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="b9df4-504">O valor padrão é **false.** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="b9df4-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-505">**UnderlyingType**</span></span> | <span data-ttu-id="b9df4-506">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-506">No</span></span>          | <span data-ttu-id="b9df4-507">**EDM. byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** ou **EDM. SByte** que definem o intervalo de valores do tipo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="b9df4-508">  O tipo subjacente padrão de elementos de enumeração é **EDM. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-509">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **enumType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="b9df4-510">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-511">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-512">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-512">Example</span></span>

<span data-ttu-id="b9df4-513">O exemplo a seguir mostra um elemento **enumType** com três elementos de **membro** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="b9df4-514">Elemento Function (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-514">Function Element (CSDL)</span></span>

<span data-ttu-id="b9df4-515">O elemento **Function** na linguagem de definição de esquema conceitual (CSDL) é usado para definir ou declarar funções no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="b9df4-516">Uma função é definida usando um elemento de definição.</span><span class="sxs-lookup"><span data-stu-id="b9df4-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="b9df4-517">Um elemento **Function** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-518">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-519">Parâmetro (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="b9df4-520">Definindo a definição (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-521">ReturnType (função) (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-522">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="b9df4-523">Um tipo de retorno para uma função deve ser especificado com o elemento **ReturnType** (função) ou o atributo **ReturnType** (veja abaixo), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="b9df4-524">Os tipos de retorno possíveis são qualquer EdmSimpleType, tipo de entidade, tipo complexo, tipo de linha ou tipo de referência (ou uma coleção de um desses tipos).</span><span class="sxs-lookup"><span data-stu-id="b9df4-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-525">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-525">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-526">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="b9df4-527">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-527">Attribute Name</span></span> | <span data-ttu-id="b9df4-528">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-528">Is Required</span></span> | <span data-ttu-id="b9df4-529">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="b9df4-530">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-530">**Name**</span></span>       | <span data-ttu-id="b9df4-531">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-531">Yes</span></span>         | <span data-ttu-id="b9df4-532">O nome da função.</span><span class="sxs-lookup"><span data-stu-id="b9df4-532">The name of the function.</span></span>          |
| <span data-ttu-id="b9df4-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-533">**ReturnType**</span></span> | <span data-ttu-id="b9df4-534">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-534">No</span></span>          | <span data-ttu-id="b9df4-535">O tipo retornado pela função.</span><span class="sxs-lookup"><span data-stu-id="b9df4-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-536">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="b9df4-537">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-538">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-539">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-539">Example</span></span>

<span data-ttu-id="b9df4-540">O exemplo a seguir usa um elemento **Function** para definir uma função que retorna o número de anos desde que um instrutor foi contratado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="b9df4-541">Elemento FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="b9df4-542">O elemento **FunctionImport** na linguagem de definição de esquema conceitual (CSDL) representa uma função que é definida na fonte de dados, mas disponível para objetos por meio do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="b9df4-543">Por exemplo, um elemento Function no modelo de armazenamento pode ser usado para representar um procedimento armazenado em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="b9df4-544">Um elemento **FunctionImport** no modelo conceitual representa a função correspondente em um aplicativo Entity Framework e é mapeado para a função de modelo de armazenamento usando o elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="b9df4-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="b9df4-545">Quando a função é chamada no aplicativo, o procedimento armazenado correspondente é executado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="b9df4-546">O elemento **FunctionImport** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-547">Documentação (zero ou um elemento permitido)</span><span class="sxs-lookup"><span data-stu-id="b9df4-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="b9df4-548">Parâmetro (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="b9df4-549">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="b9df4-550">ReturnType (FunctionImport) (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="b9df4-551">Um elemento de **parâmetro** deve ser definido para cada parâmetro que a função aceita.</span><span class="sxs-lookup"><span data-stu-id="b9df4-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="b9df4-552">Um tipo de retorno para uma função deve ser especificado com o elemento **ReturnType** (FunctionImport) ou o atributo **ReturnType** (veja abaixo), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="b9df4-553">O valor do tipo de retorno deve ser uma coleção de EdmSimpleType, EntityType ou complexType.</span><span class="sxs-lookup"><span data-stu-id="b9df4-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-554">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-554">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-555">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="b9df4-556">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-556">Attribute Name</span></span>   | <span data-ttu-id="b9df4-557">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-557">Is Required</span></span> | <span data-ttu-id="b9df4-558">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-559">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-559">**Name**</span></span>         | <span data-ttu-id="b9df4-560">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-560">Yes</span></span>         | <span data-ttu-id="b9df4-561">O nome da função importada.</span><span class="sxs-lookup"><span data-stu-id="b9df4-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="b9df4-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-562">**ReturnType**</span></span>   | <span data-ttu-id="b9df4-563">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-563">No</span></span>          | <span data-ttu-id="b9df4-564">O tipo que a função retorna.</span><span class="sxs-lookup"><span data-stu-id="b9df4-564">The type that the function returns.</span></span> <span data-ttu-id="b9df4-565">Não use esse atributo se a função não retorna um valor.</span><span class="sxs-lookup"><span data-stu-id="b9df4-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="b9df4-566">Caso contrário, o valor deve ser uma coleção de complexType, EntityType ou EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="b9df4-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="b9df4-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="b9df4-567">**EntitySet**</span></span>    | <span data-ttu-id="b9df4-568">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-568">No</span></span>          | <span data-ttu-id="b9df4-569">Se a função retornar uma coleção de tipos de entidade, o valor do **EntitySet** deverá ser o conjunto de entidades ao qual a coleção pertence.</span><span class="sxs-lookup"><span data-stu-id="b9df4-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="b9df4-570">Caso contrário, o atributo **EntitySet** não deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="b9df4-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="b9df4-571">**IsComposable**</span></span> | <span data-ttu-id="b9df4-572">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-572">No</span></span>          | <span data-ttu-id="b9df4-573">Se o valor for definido como true, a função será combinável (função com valor de tabela) e poderá ser usada em uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="b9df4-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="b9df4-574">  O padrão é **false**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="b9df4-575">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="b9df4-576">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-577">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-578">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-578">Example</span></span>

<span data-ttu-id="b9df4-579">O exemplo a seguir mostra um elemento **FunctionImport** que aceita um parâmetro e retorna uma coleção de tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="b9df4-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="b9df4-580">Elemento Key (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-580">Key Element (CSDL)</span></span>

<span data-ttu-id="b9df4-581">O elemento **Key** é um elemento filho do elemento EntityType e define uma *chave de entidade* (uma propriedade ou um conjunto de propriedades de um tipo de entidade que determinam a identidade).</span><span class="sxs-lookup"><span data-stu-id="b9df4-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="b9df4-582">As propriedades que compõem uma chave de entidade são escolhidas em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="b9df4-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="b9df4-583">Os valores das propriedades de chave de entidade devem exclusivamente identificar uma instância do tipo de entidade dentro de conjunto de entidades em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="b9df4-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="b9df4-584">As propriedades que compõem uma chave de entidade devem ser escolhidas para garantir a exclusividade de instâncias em um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="b9df4-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="b9df4-585">O elemento **Key** define uma chave de entidade referenciando uma ou mais das propriedades de um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="b9df4-586">O elemento **Key** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="b9df4-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="b9df4-587">PropertyRef (um ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="b9df4-588">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-589">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-589">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-590">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **chave** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="b9df4-591">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-592">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="b9df4-593">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-593">Example</span></span>

<span data-ttu-id="b9df4-594">O exemplo a seguir define um tipo de entidade chamado **Book**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="b9df4-595">A chave de entidade é definida referenciando a propriedade **ISBN** do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="b9df4-596">A propriedade **ISBN** é uma boa opção para a chave de entidade porque um ISBN (número de livro padrão internacional) identifica exclusivamente um livro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="b9df4-597">O exemplo a seguir mostra um tipo de entidade (**autor**) que tem uma chave de entidade que consiste em duas propriedades, **nome** e **endereço**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="b9df4-598">Usar o **nome** e o **endereço** para a chave de entidade é uma opção razoável, pois dois autores com o mesmo nome provavelmente não residirão no mesmo endereço.</span><span class="sxs-lookup"><span data-stu-id="b9df4-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="b9df4-599">No entanto, esta opção para uma chave de entidade não garante absolutamente chaves exclusivas de entidade em um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="b9df4-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="b9df4-600">A adição de uma propriedade, como **AuthorId**, que poderia ser usada para identificar exclusivamente um autor seria recomendável nesse caso.</span><span class="sxs-lookup"><span data-stu-id="b9df4-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="b9df4-601">Elemento member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-601">Member Element (CSDL)</span></span>

<span data-ttu-id="b9df4-602">O elemento **Member** é um elemento filho do elemento enumType e define um membro do tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-603">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-603">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-604">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="b9df4-605">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-605">Attribute Name</span></span> | <span data-ttu-id="b9df4-606">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-606">Is Required</span></span> | <span data-ttu-id="b9df4-607">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-608">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-608">**Name**</span></span>       | <span data-ttu-id="b9df4-609">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-609">Yes</span></span>         | <span data-ttu-id="b9df4-610">O nome do membro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="b9df4-611">**Valor**</span><span class="sxs-lookup"><span data-stu-id="b9df4-611">**Value**</span></span>      | <span data-ttu-id="b9df4-612">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-612">No</span></span>          | <span data-ttu-id="b9df4-613">O valor do membro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-613">The value of the member.</span></span> <span data-ttu-id="b9df4-614">Por padrão, o primeiro membro tem o valor 0 e o valor de cada enumerador sucessivo é incrementado em 1.</span><span class="sxs-lookup"><span data-stu-id="b9df4-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="b9df4-615">Vários membros com os mesmos valores podem existir.</span><span class="sxs-lookup"><span data-stu-id="b9df4-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-616">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="b9df4-617">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-618">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-619">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-619">Example</span></span>

<span data-ttu-id="b9df4-620">O exemplo a seguir mostra um elemento **enumType** com três elementos de **membro** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="b9df4-621">Elemento NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="b9df4-622">Um elemento **NavigationProperty** define uma propriedade de navegação, que fornece uma referência à outra extremidade de uma associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="b9df4-623">Ao contrário das propriedades definidas com o elemento Property, as propriedades de navegação não definem a forma e as características dos dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="b9df4-624">Eles fornecem uma maneira de navegar por uma associação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="b9df4-625">Observe que as propriedades de navegação são opcionais em ambos os tipos de entidade termina de uma associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="b9df4-626">Se você definir uma propriedade de navegação em um tipo de entidade no final de uma associação, você não precisa definir uma propriedade de navegação no tipo de entidade no outro extremo de associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="b9df4-627">O tipo de dados retornado por uma propriedade de navegação é determinado pela multiplicidade de sua extremidade de associação remota.</span><span class="sxs-lookup"><span data-stu-id="b9df4-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="b9df4-628">Por exemplo, suponha que uma propriedade de navegação, **OrdersNavProp**, exista em um tipo de entidade **Customer** e navegue por uma associação um-para-muitos entre **Customer** e **Order**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="b9df4-629">Como a extremidade de associação remota para a propriedade de navegação tem muitas multiplicidades (\*), seu tipo de dados é uma coleção (de **ordem**).</span><span class="sxs-lookup"><span data-stu-id="b9df4-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="b9df4-630">Da mesma forma, se uma propriedade de navegação, **CustomerNavProp**, existir no tipo de entidade **Order** , seu tipo de dados será **Customer** , pois a multiplicidade da extremidade remota é um (1).</span><span class="sxs-lookup"><span data-stu-id="b9df4-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="b9df4-631">Um elemento **NavigationProperty** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-632">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-633">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-634">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-634">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-635">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="b9df4-636">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-636">Attribute Name</span></span>   | <span data-ttu-id="b9df4-637">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-637">Is Required</span></span> | <span data-ttu-id="b9df4-638">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-639">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-639">**Name**</span></span>         | <span data-ttu-id="b9df4-640">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-640">Yes</span></span>         | <span data-ttu-id="b9df4-641">O nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="b9df4-642">**Inter-relações**</span><span class="sxs-lookup"><span data-stu-id="b9df4-642">**Relationship**</span></span> | <span data-ttu-id="b9df4-643">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-643">Yes</span></span>         | <span data-ttu-id="b9df4-644">O nome de uma associação que está dentro do escopo do modelo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="b9df4-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="b9df4-645">**ToRole**</span></span>       | <span data-ttu-id="b9df4-646">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-646">Yes</span></span>         | <span data-ttu-id="b9df4-647">O fim da associação na qual a navegação termina.</span><span class="sxs-lookup"><span data-stu-id="b9df4-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="b9df4-648">O valor do atributo **ToRole** deve ser o mesmo que o valor de um dos atributos **role** definidos em uma das extremidades de associação (definido no elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="b9df4-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="b9df4-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="b9df4-649">**FromRole**</span></span>     | <span data-ttu-id="b9df4-650">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-650">Yes</span></span>         | <span data-ttu-id="b9df4-651">O fim da associação da qual a navegação começa.</span><span class="sxs-lookup"><span data-stu-id="b9df4-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="b9df4-652">O valor do atributo **FromRole** deve ser o mesmo que o valor de um dos atributos de **função** definidos em uma das extremidades de associação (definido no elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="b9df4-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-653">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="b9df4-654">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-655">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-656">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-656">Example</span></span>

<span data-ttu-id="b9df4-657">O exemplo a seguir define um tipo de entidade (**Book**) com duas propriedades de navegação (**PublishedBy** e **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="b9df4-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="b9df4-658">Elemento OnDelete (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="b9df4-659">O elemento **OnDelete** na linguagem de definição de esquema conceitual (CSDL) define o comportamento que está conectado a uma associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="b9df4-660">Se o atributo **Action** for definido como **Cascade** em uma extremidade de uma associação, os tipos de entidade relacionados na outra extremidade da Associação serão excluídos quando o tipo de entidade na primeira extremidade for excluído.</span><span class="sxs-lookup"><span data-stu-id="b9df4-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="b9df4-661">Se a associação entre dois tipos de entidade for uma relação de chave primária de chave para primária, um objeto dependente carregado será excluído quando o objeto principal na outra extremidade da associação for excluído, independentemente da especificação **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="b9df4-662">O elemento **OnDelete** afeta apenas o comportamento de tempo de execução de um aplicativo; Ele não afeta o comportamento na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="b9df4-663">O comportamento definido na fonte de dados deve ser igual ao comportamento definido no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="b9df4-664">Um elemento **OnDelete** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-665">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-666">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-667">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-667">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-668">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="b9df4-669">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-669">Attribute Name</span></span> | <span data-ttu-id="b9df4-670">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-670">Is Required</span></span> | <span data-ttu-id="b9df4-671">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-672">**Ação**</span><span class="sxs-lookup"><span data-stu-id="b9df4-672">**Action**</span></span>     | <span data-ttu-id="b9df4-673">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-673">Yes</span></span>         | <span data-ttu-id="b9df4-674">**Cascade** ou **None**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-674">**Cascade** or **None**.</span></span> <span data-ttu-id="b9df4-675">Se os tipos de entidade **em cascata**, dependentes serão excluídos quando o tipo de entidade principal for excluído.</span><span class="sxs-lookup"><span data-stu-id="b9df4-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="b9df4-676">Se **nenhum**, os tipos de entidade dependentes não serão excluídos quando o tipo de entidade principal for excluído.</span><span class="sxs-lookup"><span data-stu-id="b9df4-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-677">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="b9df4-678">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-679">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-680">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-680">Example</span></span>

<span data-ttu-id="b9df4-681">O exemplo a seguir mostra um elemento **Association** que define a associação **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="b9df4-682">O elemento **OnDelete** indica que todos os **pedidos** relacionados a um **cliente** específico e que foram carregados no ObjectContext serão excluídos quando o **cliente** for excluído.</span><span class="sxs-lookup"><span data-stu-id="b9df4-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="b9df4-683">Elemento Parameter (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="b9df4-684">O elemento **Parameter** na linguagem de definição de esquema conceitual (CSDL) pode ser um filho do elemento FunctionImport ou do elemento Function.</span><span class="sxs-lookup"><span data-stu-id="b9df4-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="b9df4-685">Aplicativo de elemento FunctionImport</span><span class="sxs-lookup"><span data-stu-id="b9df4-685">FunctionImport Element Application</span></span>

<span data-ttu-id="b9df4-686">Um elemento **Parameter** (como um filho do elemento **FunctionImport** ) é usado para definir parâmetros de entrada e saída para importações de função que são declaradas em CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="b9df4-687">O elemento **Parameter** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-688">Documentação (zero ou um elemento permitido)</span><span class="sxs-lookup"><span data-stu-id="b9df4-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="b9df4-689">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-690">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-690">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-691">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="b9df4-692">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-692">Attribute Name</span></span> | <span data-ttu-id="b9df4-693">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-693">Is Required</span></span> | <span data-ttu-id="b9df4-694">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-695">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-695">**Name**</span></span>       | <span data-ttu-id="b9df4-696">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-696">Yes</span></span>         | <span data-ttu-id="b9df4-697">O nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="b9df4-698">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-698">**Type**</span></span>       | <span data-ttu-id="b9df4-699">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-699">Yes</span></span>         | <span data-ttu-id="b9df4-700">O tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-700">The parameter type.</span></span> <span data-ttu-id="b9df4-701">O valor deve ser um **EDMSimpleType** ou um tipo complexo que está dentro do escopo do modelo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="b9df4-702">**Modo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-702">**Mode**</span></span>       | <span data-ttu-id="b9df4-703">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-703">No</span></span>          | <span data-ttu-id="b9df4-704">**In**, **out**ou **Inout** , dependendo se o parâmetro é um parâmetro de entrada, saída ou entrada/saída.</span><span class="sxs-lookup"><span data-stu-id="b9df4-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="b9df4-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="b9df4-705">**MaxLength**</span></span>  | <span data-ttu-id="b9df4-706">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-706">No</span></span>          | <span data-ttu-id="b9df4-707">O comprimento máximo permitido do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="b9df4-708">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-708">**Precision**</span></span>  | <span data-ttu-id="b9df4-709">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-709">No</span></span>          | <span data-ttu-id="b9df4-710">A precisão do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="b9df4-711">**Escala**</span><span class="sxs-lookup"><span data-stu-id="b9df4-711">**Scale**</span></span>      | <span data-ttu-id="b9df4-712">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-712">No</span></span>          | <span data-ttu-id="b9df4-713">A escala do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="b9df4-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="b9df4-714">**SRID**</span></span>       | <span data-ttu-id="b9df4-715">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-715">No</span></span>          | <span data-ttu-id="b9df4-716">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="b9df4-717">Válido somente para parâmetros de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="b9df4-718">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9df4-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-719">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="b9df4-720">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-721">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="b9df4-722">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-722">Example</span></span>

<span data-ttu-id="b9df4-723">O exemplo a seguir mostra um elemento **FunctionImport** com um elemento filho de **parâmetro** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="b9df4-724">A função aceita um parâmetro de entrada e retorna uma coleção de tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="b9df4-725">Aplicativo de elemento de função</span><span class="sxs-lookup"><span data-stu-id="b9df4-725">Function Element Application</span></span>

<span data-ttu-id="b9df4-726">Um elemento **Parameter** (como um filho do elemento **Function** ) define parâmetros para funções que são definidas ou declaradas em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="b9df4-727">O elemento **Parameter** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-728">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="b9df4-729">CollectionType (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="b9df4-730">ReferenceType (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="b9df4-731">RowType (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-732">Somente um dos elementos **CollectionType**, **ReferenceType**ou **RowType** pode ser um elemento filho de um elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="b9df4-733">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-734">Os elementos de anotação devem aparecer após todos os outros elementos filho.</span><span class="sxs-lookup"><span data-stu-id="b9df4-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="b9df4-735">Os elementos de anotação são permitidos somente no CSDL V2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="b9df4-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-736">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-736">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-737">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="b9df4-738">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-738">Attribute Name</span></span>   | <span data-ttu-id="b9df4-739">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-739">Is Required</span></span> | <span data-ttu-id="b9df4-740">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-741">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-741">**Name**</span></span>         | <span data-ttu-id="b9df4-742">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-742">Yes</span></span>         | <span data-ttu-id="b9df4-743">O nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="b9df4-744">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-744">**Type**</span></span>         | <span data-ttu-id="b9df4-745">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-745">No</span></span>          | <span data-ttu-id="b9df4-746">O tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b9df4-746">The parameter type.</span></span> <span data-ttu-id="b9df4-747">Um parâmetro pode ser qualquer um dos seguintes tipos (ou coleções desses tipos):</span><span class="sxs-lookup"><span data-stu-id="b9df4-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="b9df4-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="b9df4-749">tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="b9df4-749">entity type</span></span> <br/> <span data-ttu-id="b9df4-750">tipo complexo</span><span class="sxs-lookup"><span data-stu-id="b9df4-750">complex type</span></span> <br/> <span data-ttu-id="b9df4-751">Tipo de linha</span><span class="sxs-lookup"><span data-stu-id="b9df4-751">row type</span></span> <br/> <span data-ttu-id="b9df4-752">tipo de referência</span><span class="sxs-lookup"><span data-stu-id="b9df4-752">reference type</span></span>                             |
| <span data-ttu-id="b9df4-753">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-753">**Nullable**</span></span>     | <span data-ttu-id="b9df4-754">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-754">No</span></span>          | <span data-ttu-id="b9df4-755">**True** (o valor padrão) ou **false** , dependendo se a propriedade pode ter um valor **nulo** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="b9df4-756">**ValorPadrão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-756">**DefaultValue**</span></span> | <span data-ttu-id="b9df4-757">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-757">No</span></span>          | <span data-ttu-id="b9df4-758">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="b9df4-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="b9df4-759">**MaxLength**</span></span>    | <span data-ttu-id="b9df4-760">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-760">No</span></span>          | <span data-ttu-id="b9df4-761">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="b9df4-762">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="b9df4-762">**FixedLength**</span></span>  | <span data-ttu-id="b9df4-763">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-763">No</span></span>          | <span data-ttu-id="b9df4-764">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="b9df4-765">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-765">**Precision**</span></span>    | <span data-ttu-id="b9df4-766">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-766">No</span></span>          | <span data-ttu-id="b9df4-767">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="b9df4-768">**Escala**</span><span class="sxs-lookup"><span data-stu-id="b9df4-768">**Scale**</span></span>        | <span data-ttu-id="b9df4-769">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-769">No</span></span>          | <span data-ttu-id="b9df4-770">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="b9df4-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="b9df4-771">**SRID**</span></span>         | <span data-ttu-id="b9df4-772">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-772">No</span></span>          | <span data-ttu-id="b9df4-773">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="b9df4-774">Válido somente para propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="b9df4-775">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9df4-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="b9df4-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="b9df4-776">**Unicode**</span></span>      | <span data-ttu-id="b9df4-777">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-777">No</span></span>          | <span data-ttu-id="b9df4-778">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="b9df4-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="b9df4-779">**Ordenação**</span><span class="sxs-lookup"><span data-stu-id="b9df4-779">**Collation**</span></span>    | <span data-ttu-id="b9df4-780">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-780">No</span></span>          | <span data-ttu-id="b9df4-781">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="b9df4-782">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="b9df4-783">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-784">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="b9df4-785">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-785">Example</span></span>

<span data-ttu-id="b9df4-786">O exemplo a seguir mostra um elemento **Function** que usa um elemento filho de **parâmetro** para definir um parâmetro de função.</span><span class="sxs-lookup"><span data-stu-id="b9df4-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="b9df4-787">Elemento principal (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="b9df4-788">O elemento **principal** na CSDL (linguagem de definição de esquema conceitual) é um elemento filho para o elemento ReferentialConstraint que define a extremidade principal de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="b9df4-789">Um elemento **ReferentialConstraint** define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="b9df4-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="b9df4-790">Da mesma forma que uma coluna (ou colunas) de uma tabela de base de dados podem fazer referência a chave primária de outra tabela, propriedade (ou propriedades) de tipo de entidade podem fazer referência chave de entidade de outro tipo de objeto.</span><span class="sxs-lookup"><span data-stu-id="b9df4-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="b9df4-791">O tipo de entidade referenciado é chamado de *extremidade principal* da restrição.</span><span class="sxs-lookup"><span data-stu-id="b9df4-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="b9df4-792">O tipo de entidade que faz referência à extremidade principal é chamado de *extremidade dependente* da restrição.</span><span class="sxs-lookup"><span data-stu-id="b9df4-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="b9df4-793">Os elementos **PropertyRef** são usados para especificar quais chaves são referenciadas pela extremidade dependente.</span><span class="sxs-lookup"><span data-stu-id="b9df4-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="b9df4-794">O elemento **principal** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-795">PropertyRef (um ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="b9df4-796">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-797">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-797">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-798">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="b9df4-799">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-799">Attribute Name</span></span> | <span data-ttu-id="b9df4-800">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-800">Is Required</span></span> | <span data-ttu-id="b9df4-801">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="b9df4-802">**Função**</span><span class="sxs-lookup"><span data-stu-id="b9df4-802">**Role**</span></span>       | <span data-ttu-id="b9df4-803">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-803">Yes</span></span>         | <span data-ttu-id="b9df4-804">O nome do tipo de entidade na extremidade principal da associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-805">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="b9df4-806">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-807">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-808">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-808">Example</span></span>

<span data-ttu-id="b9df4-809">O exemplo a seguir mostra um elemento **ReferentialConstraint** que faz parte da definição da Associação **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="b9df4-810">A propriedade **ID** do tipo de entidade **Editor** compõe a extremidade principal da restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="b9df4-811">Elemento Property (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-811">Property Element (CSDL)</span></span>

<span data-ttu-id="b9df4-812">O elemento **Property** na linguagem de definição de esquema conceitual (CSDL) pode ser filho do elemento EntityType, do elemento complexType ou do elemento RowType.</span><span class="sxs-lookup"><span data-stu-id="b9df4-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="b9df4-813">Aplicativos de elemento EntityType e complexType</span><span class="sxs-lookup"><span data-stu-id="b9df4-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="b9df4-814">Os elementos de **Propriedade** (como filhos de **EntityType** ou elementos **complexType** ) definem a forma e as características dos dados que uma instância de tipo de entidade ou instância de tipo complexo conterá.</span><span class="sxs-lookup"><span data-stu-id="b9df4-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="b9df4-815">As propriedades em um modelo conceitual são análogas às propriedades definidas em uma classe.</span><span class="sxs-lookup"><span data-stu-id="b9df4-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="b9df4-816">Da mesma forma que as propriedades em uma classe definem a forma da classe e transportam informações sobre objetos, as propriedades em um modelo conceitual definem a forma de um tipo de entidade e transportam informações sobre as instâncias dos tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="b9df4-817">O elemento **Property** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-818">Elemento Documentation (zero ou um elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="b9df4-819">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="b9df4-820">As facetas a seguir podem ser aplicadas a um elemento **Property** : **Nullable**, **DefaultValue**, **MaxLength**, **cadeia**, **Precision**, **Dimension**, **Unicode**, **Collation**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="b9df4-821">Facetas são atributos XML que fornecem informações sobre como os valores de propriedade são armazenados no armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-822">Facetas só podem ser aplicadas a propriedades do tipo **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-823">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-823">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-824">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="b9df4-825">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-825">Attribute Name</span></span>                                                         | <span data-ttu-id="b9df4-826">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-826">Is Required</span></span> | <span data-ttu-id="b9df4-827">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-828">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-828">**Name**</span></span>                                                               | <span data-ttu-id="b9df4-829">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-829">Yes</span></span>         | <span data-ttu-id="b9df4-830">O nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="b9df4-831">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-831">**Type**</span></span>                                                               | <span data-ttu-id="b9df4-832">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-832">Yes</span></span>         | <span data-ttu-id="b9df4-833">O tipo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-833">The type of the property value.</span></span> <span data-ttu-id="b9df4-834">O tipo de valor de propriedade deve ser um **EDMSimpleType** ou um tipo complexo (indicado por um nome totalmente qualificado) que está dentro do escopo do modelo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="b9df4-835">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-835">**Nullable**</span></span>                                                           | <span data-ttu-id="b9df4-836">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-836">No</span></span>          | <span data-ttu-id="b9df4-837">**True** (o valor padrão) ou <strong>False</strong>, dependendo se a propriedade pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="b9df4-838">> No CSDL v1, uma propriedade de tipo complexo deve ter `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="b9df4-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="b9df4-839">**ValorPadrão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="b9df4-840">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-840">No</span></span>          | <span data-ttu-id="b9df4-841">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="b9df4-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="b9df4-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="b9df4-843">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-843">No</span></span>          | <span data-ttu-id="b9df4-844">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="b9df4-845">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="b9df4-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="b9df4-846">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-846">No</span></span>          | <span data-ttu-id="b9df4-847">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="b9df4-848">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-848">**Precision**</span></span>                                                          | <span data-ttu-id="b9df4-849">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-849">No</span></span>          | <span data-ttu-id="b9df4-850">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="b9df4-851">**Escala**</span><span class="sxs-lookup"><span data-stu-id="b9df4-851">**Scale**</span></span>                                                              | <span data-ttu-id="b9df4-852">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-852">No</span></span>          | <span data-ttu-id="b9df4-853">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="b9df4-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="b9df4-854">**SRID**</span></span>                                                               | <span data-ttu-id="b9df4-855">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-855">No</span></span>          | <span data-ttu-id="b9df4-856">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="b9df4-857">Válido somente para propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="b9df4-858">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9df4-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="b9df4-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="b9df4-859">**Unicode**</span></span>                                                            | <span data-ttu-id="b9df4-860">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-860">No</span></span>          | <span data-ttu-id="b9df4-861">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="b9df4-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="b9df4-862">**Ordenação**</span><span class="sxs-lookup"><span data-stu-id="b9df4-862">**Collation**</span></span>                                                          | <span data-ttu-id="b9df4-863">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-863">No</span></span>          | <span data-ttu-id="b9df4-864">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="b9df4-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="b9df4-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="b9df4-866">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-866">No</span></span>          | <span data-ttu-id="b9df4-867">**Nenhum** (o valor padrão) ou **Fixo**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="b9df4-868">Se o valor for definido como **Fixo**, o valor da propriedade será usado na verificação de simultaneidade otimista.</span><span class="sxs-lookup"><span data-stu-id="b9df4-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="b9df4-869">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="b9df4-870">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-871">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="b9df4-872">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-872">Example</span></span>

<span data-ttu-id="b9df4-873">O exemplo a seguir mostra um elemento **EntityType** com três elementos **Property** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="b9df4-874">O exemplo a seguir mostra um elemento **complexType** com cinco elementos **Property** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="b9df4-875">Aplicativo de elemento RowType</span><span class="sxs-lookup"><span data-stu-id="b9df4-875">RowType Element Application</span></span>

<span data-ttu-id="b9df4-876">Elementos de **Propriedade** (como os filhos de um elemento **RowType** ) definem a forma e as características dos dados que podem ser passados para ou retornados de uma função definida pelo modelo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="b9df4-877">O elemento **Property** pode ter exatamente um dos seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="b9df4-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="b9df4-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="b9df4-878">CollectionType</span></span>
-   <span data-ttu-id="b9df4-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="b9df4-879">ReferenceType</span></span>
-   <span data-ttu-id="b9df4-880">RowType</span><span class="sxs-lookup"><span data-stu-id="b9df4-880">RowType</span></span>

<span data-ttu-id="b9df4-881">O elemento **Property** pode ter qualquer elemento de anotação filho de número.</span><span class="sxs-lookup"><span data-stu-id="b9df4-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-882">Os elementos de anotação são permitidos somente no CSDL V2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="b9df4-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-883">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-883">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-884">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="b9df4-885">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-885">Attribute Name</span></span>                                                     | <span data-ttu-id="b9df4-886">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-886">Is Required</span></span> | <span data-ttu-id="b9df4-887">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-888">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-888">**Name**</span></span>                                                           | <span data-ttu-id="b9df4-889">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-889">Yes</span></span>         | <span data-ttu-id="b9df4-890">O nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="b9df4-891">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-891">**Type**</span></span>                                                           | <span data-ttu-id="b9df4-892">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-892">Yes</span></span>         | <span data-ttu-id="b9df4-893">O tipo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="b9df4-894">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-894">**Nullable**</span></span>                                                       | <span data-ttu-id="b9df4-895">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-895">No</span></span>          | <span data-ttu-id="b9df4-896">**True** (o valor padrão) ou **False**, dependendo se a propriedade pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="b9df4-897">> Em CSDL v1, uma propriedade de tipo complexo deve ter `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="b9df4-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="b9df4-898">**ValorPadrão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="b9df4-899">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-899">No</span></span>          | <span data-ttu-id="b9df4-900">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="b9df4-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="b9df4-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="b9df4-902">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-902">No</span></span>          | <span data-ttu-id="b9df4-903">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="b9df4-904">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="b9df4-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="b9df4-905">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-905">No</span></span>          | <span data-ttu-id="b9df4-906">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="b9df4-907">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-907">**Precision**</span></span>                                                      | <span data-ttu-id="b9df4-908">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-908">No</span></span>          | <span data-ttu-id="b9df4-909">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="b9df4-910">**Escala**</span><span class="sxs-lookup"><span data-stu-id="b9df4-910">**Scale**</span></span>                                                          | <span data-ttu-id="b9df4-911">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-911">No</span></span>          | <span data-ttu-id="b9df4-912">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="b9df4-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="b9df4-913">**SRID**</span></span>                                                           | <span data-ttu-id="b9df4-914">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-914">No</span></span>          | <span data-ttu-id="b9df4-915">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="b9df4-916">Válido somente para propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="b9df4-917">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9df4-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="b9df4-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="b9df4-918">**Unicode**</span></span>                                                        | <span data-ttu-id="b9df4-919">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-919">No</span></span>          | <span data-ttu-id="b9df4-920">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="b9df4-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="b9df4-921">**Ordenação**</span><span class="sxs-lookup"><span data-stu-id="b9df4-921">**Collation**</span></span>                                                      | <span data-ttu-id="b9df4-922">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-922">No</span></span>          | <span data-ttu-id="b9df4-923">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="b9df4-924">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="b9df4-925">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-926">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="b9df4-927">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-927">Example</span></span>

<span data-ttu-id="b9df4-928">O exemplo a seguir mostra os elementos de **Propriedade** usados para definir a forma do tipo de retorno de uma função definida pelo modelo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="b9df4-929">Elemento PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="b9df4-930">O elemento **PropertyRef** na linguagem de definição de esquema conceitual (CSDL) faz referência a uma propriedade de um tipo de entidade para indicar que a propriedade executará uma das seguintes funções:</span><span class="sxs-lookup"><span data-stu-id="b9df4-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="b9df4-931">Parte da chave da entidade (uma propriedade ou um conjunto de propriedades de um tipo de entidade que determinam a identidade).</span><span class="sxs-lookup"><span data-stu-id="b9df4-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="b9df4-932">Um ou mais elementos **PropertyRef** podem ser usados para definir uma chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="b9df4-933">A extremidade dependente ou principal de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="b9df4-934">O elemento **PropertyRef** só pode ter elementos Annotation (zero ou mais) como elementos filho.</span><span class="sxs-lookup"><span data-stu-id="b9df4-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-935">Os elementos de anotação são permitidos somente no CSDL V2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="b9df4-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-936">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-936">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-937">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="b9df4-938">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-938">Attribute Name</span></span> | <span data-ttu-id="b9df4-939">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-939">Is Required</span></span> | <span data-ttu-id="b9df4-940">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="b9df4-941">**Nome**</span><span class="sxs-lookup"><span data-stu-id="b9df4-941">**Name**</span></span>       | <span data-ttu-id="b9df4-942">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-942">Yes</span></span>         | <span data-ttu-id="b9df4-943">O nome da propriedade referenciada.</span><span class="sxs-lookup"><span data-stu-id="b9df4-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-944">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="b9df4-945">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-946">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-947">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-947">Example</span></span>

<span data-ttu-id="b9df4-948">O exemplo a seguir define um tipo de entidade (**livro**).</span><span class="sxs-lookup"><span data-stu-id="b9df4-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="b9df4-949">A chave de entidade é definida referenciando a propriedade **ISBN** do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="b9df4-950">No exemplo a seguir, dois elementos **PropertyRef** são usados para indicar que duas propriedades (**ID** e **PublisherID**) são as extremidades principal e dependente de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="b9df4-951">Elemento ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="b9df4-952">O elemento **ReferenceType** na CSDL (linguagem de definição de esquema conceitual) especifica uma referência a um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="b9df4-953">O elemento **ReferenceType** pode ser um filho dos seguintes elementos:</span><span class="sxs-lookup"><span data-stu-id="b9df4-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="b9df4-954">ReturnType (função)</span><span class="sxs-lookup"><span data-stu-id="b9df4-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="b9df4-955">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="b9df4-955">Parameter</span></span>
-   <span data-ttu-id="b9df4-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="b9df4-956">CollectionType</span></span>

<span data-ttu-id="b9df4-957">O elemento **ReferenceType** é usado ao definir um parâmetro ou tipo de retorno para uma função.</span><span class="sxs-lookup"><span data-stu-id="b9df4-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="b9df4-958">Um elemento **ReferenceType** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-959">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-960">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-961">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-961">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-962">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="b9df4-963">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-963">Attribute Name</span></span> | <span data-ttu-id="b9df4-964">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-964">Is Required</span></span> | <span data-ttu-id="b9df4-965">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="b9df4-966">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-966">**Type**</span></span>       | <span data-ttu-id="b9df4-967">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-967">Yes</span></span>         | <span data-ttu-id="b9df4-968">O nome do tipo de entidade que está sendo referenciado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-969">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="b9df4-970">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-971">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-972">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-972">Example</span></span>

<span data-ttu-id="b9df4-973">O exemplo a seguir mostra o elemento **ReferenceType** usado como um filho de um elemento **Parameter** em uma função definida pelo modelo que aceita uma referência a um tipo de entidade **Person** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="b9df4-974">O exemplo a seguir mostra o elemento **ReferenceType** usado como um filho de um elemento **ReturnType** (Function) em uma função definida pelo modelo que retorna uma referência a um tipo de entidade **Person** :</span><span class="sxs-lookup"><span data-stu-id="b9df4-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="b9df4-975">Elemento ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="b9df4-976">Um elemento **ReferentialConstraint** na CSDL (linguagem de definição de esquema conceitual) define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="b9df4-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="b9df4-977">Da mesma forma que uma coluna (ou colunas) de uma tabela de base de dados podem fazer referência a chave primária de outra tabela, propriedade (ou propriedades) de tipo de entidade podem fazer referência chave de entidade de outro tipo de objeto.</span><span class="sxs-lookup"><span data-stu-id="b9df4-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="b9df4-978">O tipo de entidade referenciado é chamado de *extremidade principal* da restrição.</span><span class="sxs-lookup"><span data-stu-id="b9df4-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="b9df4-979">O tipo de entidade que faz referência à extremidade principal é chamado de *extremidade dependente* da restrição.</span><span class="sxs-lookup"><span data-stu-id="b9df4-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="b9df4-980">Se uma chave estrangeira exposta em um tipo de entidade referenciar uma propriedade em outro tipo de entidade, o elemento **ReferentialConstraint** definirá uma associação entre os dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="b9df4-981">Como o elemento **ReferentialConstraint** fornece informações sobre como dois tipos de entidade estão relacionados, nenhum elemento AssociationSetMapping correspondente é necessário na MSL (linguagem de especificação de mapeamento).</span><span class="sxs-lookup"><span data-stu-id="b9df4-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="b9df4-982">Uma associação entre dois tipos de entidade que não têm chaves estrangeiras expostas deve ter um elemento **AssociationSetMapping** correspondente para mapear informações de associação para a fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="b9df4-983">Se uma chave estrangeira não for exposta em um tipo de entidade, o elemento **ReferentialConstraint** só poderá definir uma restrição PRIMARY KEY para Primary Key entre o tipo de entidade e outro tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="b9df4-984">Um elemento **ReferentialConstraint** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-985">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-986">Principal (exatamente um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="b9df4-987">Dependente (exatamente um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="b9df4-988">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-989">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-989">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-990">O elemento **ReferentialConstraint** pode ter qualquer número de atributos de anotação (atributos XML personalizados).</span><span class="sxs-lookup"><span data-stu-id="b9df4-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="b9df4-991">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-992">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="b9df4-993">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-993">Example</span></span>

<span data-ttu-id="b9df4-994">O exemplo a seguir mostra um elemento **ReferentialConstraint** que está sendo usado como parte da definição da Associação **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="b9df4-995">Elemento ReturnType (Function) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="b9df4-996">O elemento **ReturnType** (Function) na linguagem de definição de esquema conceitual (CSDL) especifica o tipo de retorno para uma função que é definida em um elemento Function.</span><span class="sxs-lookup"><span data-stu-id="b9df4-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="b9df4-997">Um tipo de retorno de função também pode ser especificado com um atributo **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="b9df4-998">Os tipos de retorno podem ser qualquer **EdmSimpleType**, tipo de entidade, tipo complexo, tipo de linha, tipo de referência ou uma coleção de um desses tipos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="b9df4-999">O tipo de retorno de uma função pode ser especificado com o atributo **Type** do elemento **ReturnType** (Function) ou com um dos seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="b9df4-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="b9df4-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="b9df4-1000">CollectionType</span></span>
-   <span data-ttu-id="b9df4-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="b9df4-1001">ReferenceType</span></span>
-   <span data-ttu-id="b9df4-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="b9df4-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-1003">Um modelo não será validado se você especificar um tipo de retorno de função com o atributo **Type** do elemento **ReturnType** (função) e um dos elementos filho.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-1004">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-1004">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-1005">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **ReturnType** (função).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="b9df4-1006">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1006">Attribute Name</span></span> | <span data-ttu-id="b9df4-1007">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-1007">Is Required</span></span> | <span data-ttu-id="b9df4-1008">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="b9df4-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1009">**ReturnType**</span></span> | <span data-ttu-id="b9df4-1010">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1010">No</span></span>          | <span data-ttu-id="b9df4-1011">O tipo retornado pela função.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-1012">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReturnType** (função).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="b9df4-1013">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-1014">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-1015">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1015">Example</span></span>

<span data-ttu-id="b9df4-1016">O exemplo a seguir usa um elemento **Function** para definir uma função que retorna o número de anos em que um livro foi impresso.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="b9df4-1017">Observe que o tipo de retorno é especificado pelo atributo **Type** de um elemento **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="b9df4-1018">Elemento ReturnType (FunctionImport) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="b9df4-1019">O elemento **ReturnType** (FunctionImport) na linguagem de definição de esquema conceitual (CSDL) especifica o tipo de retorno para uma função que é definida em um elemento FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="b9df4-1020">Um tipo de retorno de função também pode ser especificado com um atributo **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="b9df4-1021">Tipos de retorno podem ser qualquer coleção de tipo de entidade, tipo complexo ou **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="b9df4-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="b9df4-1022">O tipo de retorno de uma função é especificado com o atributo **Type** do elemento **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-1023">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-1023">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-1024">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="b9df4-1025">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1025">Attribute Name</span></span> | <span data-ttu-id="b9df4-1026">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-1026">Is Required</span></span> | <span data-ttu-id="b9df4-1027">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-1028">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1028">**Type**</span></span>       | <span data-ttu-id="b9df4-1029">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1029">No</span></span>          | <span data-ttu-id="b9df4-1030">O tipo que a função retorna.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1030">The type that the function returns.</span></span> <span data-ttu-id="b9df4-1031">O valor deve ser uma coleção de complexType, EntityType ou EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="b9df4-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1032">**EntitySet**</span></span>  | <span data-ttu-id="b9df4-1033">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1033">No</span></span>          | <span data-ttu-id="b9df4-1034">Se a função retornar uma coleção de tipos de entidade, o valor do **EntitySet** deverá ser o conjunto de entidades ao qual a coleção pertence.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="b9df4-1035">Caso contrário, o atributo **EntitySet** não deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-1036">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="b9df4-1037">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-1038">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-1039">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1039">Example</span></span>

<span data-ttu-id="b9df4-1040">O exemplo a seguir usa uma **FunctionImport** que retorna livros e publicadores.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="b9df4-1041">Observe que a função retorna dois conjuntos de resultados e, portanto, dois elementos **ReturnType** (FunctionImport) são especificados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="b9df4-1042">Elemento RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="b9df4-1043">Um elemento **RowType** na CSDL (linguagem de definição de esquema conceitual) define uma estrutura sem nome como um parâmetro ou tipo de retorno para uma função definida no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="b9df4-1044">Um elemento **RowType** pode ser o filho dos seguintes elementos:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="b9df4-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="b9df4-1045">CollectionType</span></span>
-   <span data-ttu-id="b9df4-1046">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="b9df4-1046">Parameter</span></span>
-   <span data-ttu-id="b9df4-1047">ReturnType (função)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1047">ReturnType (Function)</span></span>

<span data-ttu-id="b9df4-1048">Um elemento **RowType** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-1049">Propriedade (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1049">Property (one or more)</span></span>
-   <span data-ttu-id="b9df4-1050">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-1051">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-1051">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-1052">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **RowType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="b9df4-1053">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-1054">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="b9df4-1055">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1055">Example</span></span>

<span data-ttu-id="b9df4-1056">O exemplo a seguir mostra uma função definida pelo modelo que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas (conforme especificado no elemento **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="b9df4-1057">Elemento Schema (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="b9df4-1058">O elemento **Schema** é o elemento raiz de uma definição de modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="b9df4-1059">Ele contém definições para os objetos, funções e contêineres que compõem um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="b9df4-1060">O elemento **Schema** pode conter zero ou mais dos seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="b9df4-1061">Usar</span><span class="sxs-lookup"><span data-stu-id="b9df4-1061">Using</span></span>
-   <span data-ttu-id="b9df4-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="b9df4-1062">EntityContainer</span></span>
-   <span data-ttu-id="b9df4-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="b9df4-1063">EntityType</span></span>
-   <span data-ttu-id="b9df4-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="b9df4-1064">EnumType</span></span>
-   <span data-ttu-id="b9df4-1065">Associação</span><span class="sxs-lookup"><span data-stu-id="b9df4-1065">Association</span></span>
-   <span data-ttu-id="b9df4-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="b9df4-1066">ComplexType</span></span>
-   <span data-ttu-id="b9df4-1067">Função</span><span class="sxs-lookup"><span data-stu-id="b9df4-1067">Function</span></span>

<span data-ttu-id="b9df4-1068">Um elemento de **esquema** pode conter zero ou um dos elementos de anotação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-1069">Os elementos de **função** e de anotação são permitidos somente no CSDL V2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="b9df4-1070">O elemento **Schema** usa o atributo **namespace** para definir o namespace para o tipo de entidade, tipo complexo e objetos de associação em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="b9df4-1071">Em um namespace, dois objetos não podem ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="b9df4-1072">Os namespaces podem abranger vários elementos de **esquema** e vários arquivos. CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="b9df4-1073">Um namespace de modelo conceitual é diferente do namespace XML do elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="b9df4-1074">Um namespace de modelo conceitual (conforme definido pelo atributo **namespace** ) é um contêiner lógico para tipos de entidade, tipos complexos e tipos de associação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="b9df4-1075">O namespace XML (indicado pelo atributo **xmlns** ) de um elemento **Schema** é o namespace padrão para elementos filho e atributos do elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="b9df4-1076">Os namespaces XML do formulário https://schemas.microsoft.com/ado/YYYY/MM/edm (em que AAAA e MM representam um ano e um mês respectivamente) são reservados para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="b9df4-1077">Atributos e elementos personalizados não podem estar em namespaces que tenham esse formulário.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-1078">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-1078">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-1079">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="b9df4-1080">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1080">Attribute Name</span></span> | <span data-ttu-id="b9df4-1081">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-1081">Is Required</span></span> | <span data-ttu-id="b9df4-1082">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1083">**Namespace**</span></span>  | <span data-ttu-id="b9df4-1084">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1084">Yes</span></span>         | <span data-ttu-id="b9df4-1085">O namespace do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="b9df4-1086">O valor do atributo **namespace** é usado para formar o nome totalmente qualificado de um tipo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="b9df4-1087">Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace simples. example. Model, o nome totalmente qualificado do **EntityType** será SimpleExampleModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="b9df4-1088">As cadeias de caracteres a seguir não podem ser usadas como o valor do atributo de **namespace** : **System**, **transitório**ou **EDM**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="b9df4-1089">O valor do atributo **namespace** não pode ser o mesmo que o valor do atributo **namespace** no elemento de esquema SSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="b9df4-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1090">**Alias**</span></span>      | <span data-ttu-id="b9df4-1091">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1091">No</span></span>          | <span data-ttu-id="b9df4-1092">Um identificador usado no lugar do nome do namespace.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="b9df4-1093">Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace simples. example. Model e o valor do atributo **alias** for *Model*, você poderá usar Model. Customer como o nome totalmente qualificado do **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="b9df4-1094">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **esquema** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="b9df4-1095">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-1096">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-1097">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1097">Example</span></span>

<span data-ttu-id="b9df4-1098">O exemplo a seguir mostra um elemento de **esquema** que contém um elemento **EntityContainer** , dois elementos **EntityType** e um elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="b9df4-1099">Elemento TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="b9df4-1100">O elemento **TypeRef** na linguagem de definição de esquema conceitual (CSDL) fornece uma referência a um tipo nomeado existente.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="b9df4-1101">O elemento **TypeRef** pode ser um filho do elemento CollectionType, que é usado para especificar que uma função tem uma coleção como um tipo de parâmetro ou de retorno.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="b9df4-1102">Um elemento **TypeRef** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="b9df4-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b9df4-1103">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="b9df4-1104">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-1105">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-1105">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-1106">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="b9df4-1107">Observe que os atributos **DefaultValue**, **MaxLength**, **cadeia**, **Precision**, **Scale**, **Unicode**e **Collation** são aplicáveis somente ao **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="b9df4-1108">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="b9df4-1109">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-1109">Is Required</span></span> | <span data-ttu-id="b9df4-1110">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-1111">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1111">**Type**</span></span>                                                           | <span data-ttu-id="b9df4-1112">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1112">No</span></span>          | <span data-ttu-id="b9df4-1113">O nome do tipo que está sendo referenciado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="b9df4-1114">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="b9df4-1115">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1115">No</span></span>          | <span data-ttu-id="b9df4-1116">**True** (o valor padrão) ou **False**, dependendo se a propriedade pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="b9df4-1117">> Em CSDL v1, uma propriedade de tipo complexo deve ter `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="b9df4-1118">**ValorPadrão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="b9df4-1119">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1119">No</span></span>          | <span data-ttu-id="b9df4-1120">O valor padrão da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="b9df4-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="b9df4-1122">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1122">No</span></span>          | <span data-ttu-id="b9df4-1123">O comprimento máximo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="b9df4-1124">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="b9df4-1125">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1125">No</span></span>          | <span data-ttu-id="b9df4-1126">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="b9df4-1127">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1127">**Precision**</span></span>                                                      | <span data-ttu-id="b9df4-1128">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1128">No</span></span>          | <span data-ttu-id="b9df4-1129">A precisão do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="b9df4-1130">**Escala**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1130">**Scale**</span></span>                                                          | <span data-ttu-id="b9df4-1131">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1131">No</span></span>          | <span data-ttu-id="b9df4-1132">A escala do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="b9df4-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1133">**SRID**</span></span>                                                           | <span data-ttu-id="b9df4-1134">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1134">No</span></span>          | <span data-ttu-id="b9df4-1135">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="b9df4-1136">Válido somente para propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="b9df4-1137">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="b9df4-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="b9df4-1139">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1139">No</span></span>          | <span data-ttu-id="b9df4-1140">**True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="b9df4-1141">**Ordenação**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1141">**Collation**</span></span>                                                      | <span data-ttu-id="b9df4-1142">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1142">No</span></span>          | <span data-ttu-id="b9df4-1143">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="b9df4-1144">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="b9df4-1145">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-1146">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-1147">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1147">Example</span></span>

<span data-ttu-id="b9df4-1148">O exemplo a seguir mostra uma função definida pelo modelo que usa o elemento **TypeRef** (como um filho de um elemento **CollectionType** ) para especificar que a função aceita uma coleção de tipos de entidade **Department** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="b9df4-1149">Elemento using (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="b9df4-1150">O elemento **using** na linguagem de definição de esquema conceitual (CSDL) importa o conteúdo de um modelo conceitual que existe em um namespace diferente.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="b9df4-1151">Ao definir o valor do atributo **namespace** , você pode se referir a tipos de entidade, tipos complexos e tipos de associação que são definidos em outro modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="b9df4-1152">Mais de um elemento **using** pode ser um filho de um elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-1153">O elemento **using** em CSDL não funciona exatamente como uma instrução **using** em uma linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="b9df4-1154">Ao importar um namespace com uma instrução **using** em uma linguagem de programação, você não afeta os objetos no namespace original.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="b9df4-1155">No CSDL, um namespace importado pode conter um tipo de entidade derivado de um tipo de entidade no namespace original.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="b9df4-1156">Isso pode afetar os conjuntos de entidades declarados no namespace original.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="b9df4-1157">O elemento **using** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="b9df4-1158">Documentação (zero ou um elemento permitido)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="b9df4-1159">Elementos de anotação (zero ou mais elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b9df4-1160">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-1160">Applicable Attributes</span></span>

<span data-ttu-id="b9df4-1161">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="b9df4-1162">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1162">Attribute Name</span></span> | <span data-ttu-id="b9df4-1163">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="b9df4-1163">Is Required</span></span> | <span data-ttu-id="b9df4-1164">Valor</span><span class="sxs-lookup"><span data-stu-id="b9df4-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1165">**Namespace**</span></span>  | <span data-ttu-id="b9df4-1166">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1166">Yes</span></span>         | <span data-ttu-id="b9df4-1167">O nome do namespace importado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="b9df4-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1168">**Alias**</span></span>      | <span data-ttu-id="b9df4-1169">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1169">Yes</span></span>         | <span data-ttu-id="b9df4-1170">Um identificador usado no lugar do nome do namespace.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="b9df4-1171">Embora esse atributo seja necessário, não é necessário que ele seja usado no lugar do nome do namespace para qualificar nomes de objeto.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="b9df4-1172">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="b9df4-1173">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="b9df4-1174">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="b9df4-1175">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1175">Example</span></span>

<span data-ttu-id="b9df4-1176">O exemplo a seguir demonstra o elemento **using** que está sendo usado para importar um namespace que é definido em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="b9df4-1177">Observe que o namespace para o elemento de **esquema** mostrado é `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="b9df4-1178">A propriedade `Address` no `Publisher`**EntityType** é um tipo complexo que é definido no namespace `ExtendedBooksModel` (importado com o elemento **using** ).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="b9df4-1179">Atributos de anotação (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="b9df4-1180">Os atributos de anotação em CSDL (linguagem de definição de esquema conceitual) são atributos XML personalizados no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="b9df4-1181">Além de ter uma estrutura XML válida, o seguinte deve ser verdadeiro dos atributos de anotação:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="b9df4-1182">Os atributos de anotação não devem estar em nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="b9df4-1183">Mais de um atributo Annotation pode ser aplicado a um determinado elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="b9df4-1184">Os nomes totalmente qualificados de quaisquer dois atributos de anotação não devem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="b9df4-1185">Os atributos de anotação podem ser usados para fornecer metadados extras sobre os elementos em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="b9df4-1186">Os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="b9df4-1187">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1187">Example</span></span>

<span data-ttu-id="b9df4-1188">O exemplo a seguir mostra um elemento **EntityType** com um atributo de anotação (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="b9df4-1189">O exemplo também mostra um elemento Annotation aplicado ao elemento tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="b9df4-1190">O código a seguir recupera os metadados no atributo Annotation e grava-os no console:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="b9df4-1191">O código acima pressupõe que o arquivo de `School.csdl` está no diretório de saída do projeto e que você adicionou as instruções `Imports` e `Using` a seguir ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="b9df4-1192">Elementos de anotação (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="b9df4-1193">Os elementos Annotation na linguagem de definição de esquema conceitual (CSDL) são elementos XML personalizados no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="b9df4-1194">Além de ter uma estrutura XML válida, o seguinte deve ser verdadeiro para elementos de anotação:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="b9df4-1195">Os elementos de anotação não devem estar em nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="b9df4-1196">Mais de um elemento Annotation pode ser um filho de um determinado elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="b9df4-1197">Os nomes totalmente qualificados de quaisquer dois elementos de anotação não devem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="b9df4-1198">Os elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="b9df4-1199">Os elementos de anotação podem ser usados para fornecer metadados extras sobre os elementos em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="b9df4-1200">A partir do .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="b9df4-1201">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1201">Example</span></span>

<span data-ttu-id="b9df4-1202">O exemplo a seguir mostra um elemento **EntityType** com um elemento Annotation (**customelement**).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="b9df4-1203">O exemplo também mostra um atributo Annotation aplicado ao elemento tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="b9df4-1204">O código a seguir recupera os metadados no elemento Annotation e grava-os no console:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="b9df4-1205">O código acima pressupõe que o arquivo School. CSDL esteja no diretório de saída do projeto e que você tenha adicionado as instruções `Imports` e `Using` a seguir ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="b9df4-1206">CSDL (tipos de modelo conceitual)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="b9df4-1207">O CSDL (linguagem de definição de esquema conceitual) dá suporte a um conjunto de tipos de dados primitivos abstratos, chamados **EDMSimpleTypes**, que definem propriedades em um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="b9df4-1208">**EDMSimpleTypes** são proxies para tipos de dados primitivos que têm suporte no ambiente de armazenamento ou de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="b9df4-1209">A tabela a seguir lista os tipos de dados primitivos que são suportados pelo CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="b9df4-1210">A tabela também lista as facetas que podem ser aplicadas a cada **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="b9df4-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="b9df4-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="b9df4-1212">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="b9df4-1212">Description</span></span>                                                | <span data-ttu-id="b9df4-1213">Facetas aplicáveis</span><span class="sxs-lookup"><span data-stu-id="b9df4-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="b9df4-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="b9df4-1215">Contém dados binários.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="b9df4-1216">MaxLength, FixedLength, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="b9df4-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="b9df4-1218">Contém o valor **true** ou **false**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="b9df4-1219">Anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="b9df4-1220">**EDM. byte**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="b9df4-1221">Contém um valor inteiro de 8 bits sem sinal.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="b9df4-1222">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="b9df4-1224">Representa uma data e hora.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="b9df4-1225">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="b9df4-1227">Contém uma data e hora como um deslocamento em minutos GMT.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="b9df4-1228">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1229">**EDM. decimal**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="b9df4-1230">Contém um valor numérico com precisão e escala fixa.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="b9df4-1231">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="b9df4-1233">Contém um número de ponto flutuante com precisão de 15 dígitos</span><span class="sxs-lookup"><span data-stu-id="b9df4-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="b9df4-1234">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="b9df4-1236">Contém um número de ponto flutuante com precisão de 7 dígitos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="b9df4-1237">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="b9df4-1239">Contém um identificador exclusivo de 16 bytes.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="b9df4-1240">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="b9df4-1242">Contém um valor inteiro de 16 bits assinado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="b9df4-1243">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="b9df4-1245">Contém um valor inteiro de 32 bits assinado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="b9df4-1246">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="b9df4-1248">Contém um valor inteiro de 64 bits assinado.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="b9df4-1249">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="b9df4-1251">Contém um valor inteiro de 8 bits com sinal.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="b9df4-1252">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1253">**Edm.String**</span></span>                   | <span data-ttu-id="b9df4-1254">Contém dados de caractere.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1254">Contains character data.</span></span>                                   | <span data-ttu-id="b9df4-1255">Unicode, FixedLength, MaxLength, ordenação, precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="b9df4-1256">**EDM. time**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="b9df4-1257">Contém uma hora.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="b9df4-1258">Precisão, anulável, opção</span><span class="sxs-lookup"><span data-stu-id="b9df4-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="b9df4-1259">**EDM. geography**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="b9df4-1260">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="b9df4-1262">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="b9df4-1264">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1265">**EDM. geographyPolygon que**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="b9df4-1266">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1267">**EDM. geographyMultiPoint que**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="b9df4-1268">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="b9df4-1270">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1271">**EDM. geographyMultiPolygon que**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="b9df4-1272">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1273">**EDM. geographcollection**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="b9df4-1274">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="b9df4-1276">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1277">**EDM. geometryPoint que**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="b9df4-1278">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1279">**EDM. geometryLineString que**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="b9df4-1280">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1281">**EDM. geometryPolygon que**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="b9df4-1282">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1283">**EDM. geometryMultiPoint que**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="b9df4-1284">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="b9df4-1286">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1287">**EDM. geometryMultiPolygon que**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="b9df4-1288">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="b9df4-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="b9df4-1290">Permite valor nulo, padrão, SRID</span><span class="sxs-lookup"><span data-stu-id="b9df4-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="b9df4-1291">Facetas (CSDL)</span><span class="sxs-lookup"><span data-stu-id="b9df4-1291">Facets (CSDL)</span></span>

<span data-ttu-id="b9df4-1292">Facetas em CSDL (linguagem de definição de esquema conceitual) representam restrições em Propriedades de tipos de entidade e tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="b9df4-1293">As facetas aparecem como atributos XML nos seguintes elementos CSDL:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="b9df4-1294">Propriedade</span><span class="sxs-lookup"><span data-stu-id="b9df4-1294">Property</span></span>
-   <span data-ttu-id="b9df4-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="b9df4-1295">TypeRef</span></span>
-   <span data-ttu-id="b9df4-1296">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="b9df4-1296">Parameter</span></span>

<span data-ttu-id="b9df4-1297">A tabela a seguir descreve as facetas com suporte em CSDL.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="b9df4-1298">Todas as facetas são opcionais.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1298">All facets are optional.</span></span> <span data-ttu-id="b9df4-1299">Algumas facetas listadas abaixo são usadas pelo Entity Framework ao gerar um banco de dados de um modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="b9df4-1300">Para obter informações sobre tipos de dados em um modelo conceitual, consulte tipos de modelo conceituais (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="b9df4-1301">Faceta</span><span class="sxs-lookup"><span data-stu-id="b9df4-1301">Facet</span></span>               | <span data-ttu-id="b9df4-1302">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="b9df4-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="b9df4-1303">Aplica-se a</span><span class="sxs-lookup"><span data-stu-id="b9df4-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="b9df4-1304">Usado para a geração de banco de dados</span><span class="sxs-lookup"><span data-stu-id="b9df4-1304">Used for the database generation</span></span> | <span data-ttu-id="b9df4-1305">Usado pelo tempo de execução</span><span class="sxs-lookup"><span data-stu-id="b9df4-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="b9df4-1306">**Ordenação**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1306">**Collation**</span></span>       | <span data-ttu-id="b9df4-1307">Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="b9df4-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="b9df4-1309">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1309">Yes</span></span>                              | <span data-ttu-id="b9df4-1310">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1310">No</span></span>                  |
| <span data-ttu-id="b9df4-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="b9df4-1312">Indica que o valor da propriedade deve ser usado para verificação de simultaneidade otimista.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="b9df4-1313">Todas as propriedades **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="b9df4-1314">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1314">No</span></span>                               | <span data-ttu-id="b9df4-1315">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1315">Yes</span></span>                 |
| <span data-ttu-id="b9df4-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1316">**Default**</span></span>         | <span data-ttu-id="b9df4-1317">Especifica o valor de propriedade padrão se nenhum valor é fornecido em cima de instanciação.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="b9df4-1318">Todas as propriedades **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="b9df4-1319">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1319">Yes</span></span>                              | <span data-ttu-id="b9df4-1320">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1320">Yes</span></span>                 |
| <span data-ttu-id="b9df4-1321">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1321">**FixedLength**</span></span>     | <span data-ttu-id="b9df4-1322">Especifica se o comprimento do valor da propriedade pode variar.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="b9df4-1323">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="b9df4-1324">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1324">Yes</span></span>                              | <span data-ttu-id="b9df4-1325">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1325">No</span></span>                  |
| <span data-ttu-id="b9df4-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1326">**MaxLength**</span></span>       | <span data-ttu-id="b9df4-1327">Especifica o comprimento máximo de valor de propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="b9df4-1328">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="b9df4-1329">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1329">Yes</span></span>                              | <span data-ttu-id="b9df4-1330">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1330">No</span></span>                  |
| <span data-ttu-id="b9df4-1331">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1331">**Nullable**</span></span>        | <span data-ttu-id="b9df4-1332">Especifica se a propriedade pode ter um valor **nulo** .</span><span class="sxs-lookup"><span data-stu-id="b9df4-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="b9df4-1333">Todas as propriedades **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="b9df4-1334">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1334">Yes</span></span>                              | <span data-ttu-id="b9df4-1335">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1335">Yes</span></span>                 |
| <span data-ttu-id="b9df4-1336">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1336">**Precision**</span></span>       | <span data-ttu-id="b9df4-1337">Para propriedades do tipo **decimal**, especifica o número de dígitos que um valor de propriedade pode ter.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="b9df4-1338">Para propriedades do tipo **time**, **DateTime**e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="b9df4-1339">**EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. decimal**, **EDM. time**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="b9df4-1340">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1340">Yes</span></span>                              | <span data-ttu-id="b9df4-1341">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1341">No</span></span>                  |
| <span data-ttu-id="b9df4-1342">**Escala**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1342">**Scale**</span></span>           | <span data-ttu-id="b9df4-1343">Especifica o número de dígitos à direita do ponto decimal para o valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="b9df4-1344">**EDM. decimal**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="b9df4-1345">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1345">Yes</span></span>                              | <span data-ttu-id="b9df4-1346">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1346">No</span></span>                  |
| <span data-ttu-id="b9df4-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1347">**SRID**</span></span>            | <span data-ttu-id="b9df4-1348">Especifica a ID do sistema de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="b9df4-1349">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9df4-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="b9df4-1350">**EDM. geography, EDM. GeographyPoint, EDM. GeographyLineString, EDM. geographyPolygon que, EDM. geographyMultiPoint que, EDM. GeographyMultiLineString, EDM. geographyMultiPolygon que, EDM. geographcollection, EDM. Geometry, EDM. geometryPoint que, EDM. geometryLineString que, EDM. geometryPolygon que, EDM. geometryMultiPoint que, EDM. GeometryMultiLineString, EDM. geometryMultiPolygon que, EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="b9df4-1351">Não</span><span class="sxs-lookup"><span data-stu-id="b9df4-1351">No</span></span>                               | <span data-ttu-id="b9df4-1352">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1352">Yes</span></span>                 |
| <span data-ttu-id="b9df4-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1353">**Unicode**</span></span>         | <span data-ttu-id="b9df4-1354">Indica se o valor da propriedade é armazenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="b9df4-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="b9df4-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="b9df4-1356">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1356">Yes</span></span>                              | <span data-ttu-id="b9df4-1357">Sim</span><span class="sxs-lookup"><span data-stu-id="b9df4-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="b9df4-1358">Ao gerar um banco de dados de um modelo conceitual, o assistente para gerar banco de dados reconhecerá o valor do atributo **StoreGeneratedPattern** em um elemento de **Propriedade** se ele estiver no seguinte namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="b9df4-1359">Os valores com suporte para o atributo são **identidade** e **computados**.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="b9df4-1360">Um valor de **Identity** produzirá uma coluna de banco de dados com um valor de identidade que é gerado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="b9df4-1361">Um valor de **computado** produzirá uma coluna com um valor que é computado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b9df4-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="b9df4-1362">Exemplo</span><span class="sxs-lookup"><span data-stu-id="b9df4-1362">Example</span></span>

<span data-ttu-id="b9df4-1363">O exemplo a seguir mostra as facetas aplicadas às propriedades de um tipo de entidade:</span><span class="sxs-lookup"><span data-stu-id="b9df4-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
