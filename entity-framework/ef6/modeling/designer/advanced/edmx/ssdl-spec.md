---
title: Especificação de SSDL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
caps.latest.revision: 3
ms.openlocfilehash: a9977c80d9a9401afdcad2284a705bcb28790fb8
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39119933"
---
# <a name="ssdl-specification"></a><span data-ttu-id="26b51-102">Especificação de SSDL</span><span class="sxs-lookup"><span data-stu-id="26b51-102">SSDL Specification</span></span>
<span data-ttu-id="26b51-103">Linguagem de definição de esquema de Store (SSDL) é uma linguagem baseada em XML que descreve o modelo de armazenamento de um aplicativo do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="26b51-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="26b51-104">Em um aplicativo do Entity Framework, os metadados de modelo de armazenamento é carregado de um arquivo. SSDL (escrito em SSDL) em uma instância da System.Data.Metadata.Edm.StoreItemCollection em são acessados por meio de métodos no Classe System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="26b51-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="26b51-105">Entity Framework usa metadados do modelo de armazenamento para converter consultas no modelo conceitual para comandos específicos do repositório.</span><span class="sxs-lookup"><span data-stu-id="26b51-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="26b51-106">O Entity Framework Designer (Designer de EF) armazena informações de modelo de armazenamento em um arquivo. edmx em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="26b51-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="26b51-107">No momento da compilação o Entity Designer usa as informações em um arquivo. edmx para criar o arquivo. SSDL que é necessário pelo Entity Framework em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="26b51-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="26b51-108">Versões de SSDL são diferenciadas por namespaces XML.</span><span class="sxs-lookup"><span data-stu-id="26b51-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="26b51-109">Versão SSDL</span><span class="sxs-lookup"><span data-stu-id="26b51-109">SSDL Version</span></span> | <span data-ttu-id="26b51-110">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="26b51-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="26b51-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="26b51-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="26b51-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="26b51-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="26b51-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="26b51-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="26b51-114">Elemento de associação (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-114">Association Element (SSDL)</span></span>

<span data-ttu-id="26b51-115">Uma **associação** elemento na linguagem de definição de esquema de armazenamento (SSDL) especifica colunas que participam da tabela em uma restrição de chave estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="26b51-116">Dois elementos de final de filho obrigatório especificam tabelas nas extremidades da associação e a multiplicidade em cada extremidade.</span><span class="sxs-lookup"><span data-stu-id="26b51-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="26b51-117">Um elemento ReferentialConstraint opcional especifica as entidade e dependentes extremidades de associação, bem como as colunas participantes.</span><span class="sxs-lookup"><span data-stu-id="26b51-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="26b51-118">Se nenhum **ReferentialConstraint** elemento estiver presente, um elemento AssociationSetMapping deve ser usado para especificar os mapeamentos de coluna para a associação.</span><span class="sxs-lookup"><span data-stu-id="26b51-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="26b51-119">O **associação** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-120">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="26b51-121">End (exatamente dois)</span><span class="sxs-lookup"><span data-stu-id="26b51-121">End (exactly two)</span></span>
-   <span data-ttu-id="26b51-122">ReferentialConstraint (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="26b51-123">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-124">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-124">Applicable Attributes</span></span>

<span data-ttu-id="26b51-125">A tabela a seguir descreve os atributos que podem ser aplicados para o **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="26b51-126">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-126">Attribute Name</span></span> | <span data-ttu-id="26b51-127">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-127">Is Required</span></span> | <span data-ttu-id="26b51-128">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-129">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-129">**Name**</span></span>       | <span data-ttu-id="26b51-130">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-130">Yes</span></span>         | <span data-ttu-id="26b51-131">O nome da restrição de chave estrangeira correspondente no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-132">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="26b51-133">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-134">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-135">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-135">Example</span></span>

<span data-ttu-id="26b51-136">A exemplo a seguir mostra uma **associação** elemento que usa um **ReferentialConstraint** elemento para especificar as colunas que participam do **FK\_CustomerOrders**  restrição de chave estrangeira:</span><span class="sxs-lookup"><span data-stu-id="26b51-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="26b51-137">Elemento AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="26b51-138">O **AssociationSet** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma restrição de chave estrangeira entre duas tabelas no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="26b51-139">As colunas da tabela que participam da restrição de chave estrangeira são especificadas em um elemento de associação.</span><span class="sxs-lookup"><span data-stu-id="26b51-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="26b51-140">O **associação** elemento que corresponde a um determinado **AssociationSet** especificado no elemento a **associação** atributo do **AssociationSet**  elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="26b51-141">Conjuntos de associações de SSDL são mapeados para conjuntos de associações de CSDL por um elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="26b51-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="26b51-142">No entanto, se definir a associação de CSDL para um determinado association da CSDL é definida por meio de um elemento ReferentialConstraint, nenhum correspondente **AssociationSetMapping** elemento é necessário.</span><span class="sxs-lookup"><span data-stu-id="26b51-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="26b51-143">Nesse caso, se um **AssociationSetMapping** elemento estiver presente, ele define os mapeamentos de serão substituídos pela **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="26b51-144">O **AssociationSet** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-145">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="26b51-146">End (zero ou dois)</span><span class="sxs-lookup"><span data-stu-id="26b51-146">End (zero or two)</span></span>
-   <span data-ttu-id="26b51-147">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-148">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-148">Applicable Attributes</span></span>

<span data-ttu-id="26b51-149">A tabela a seguir descreve os atributos que podem ser aplicados para o **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="26b51-150">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-150">Attribute Name</span></span>  | <span data-ttu-id="26b51-151">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-151">Is Required</span></span> | <span data-ttu-id="26b51-152">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-153">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-153">**Name**</span></span>        | <span data-ttu-id="26b51-154">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-154">Yes</span></span>         | <span data-ttu-id="26b51-155">O nome da restrição de chave estrangeira que a associação definida representa.</span><span class="sxs-lookup"><span data-stu-id="26b51-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="26b51-156">**Associação**</span><span class="sxs-lookup"><span data-stu-id="26b51-156">**Association**</span></span> | <span data-ttu-id="26b51-157">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-157">Yes</span></span>         | <span data-ttu-id="26b51-158">O nome da associação que define as colunas que participam na restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-159">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="26b51-160">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-161">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-162">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-162">Example</span></span>

<span data-ttu-id="26b51-163">A exemplo a seguir mostra uma **AssociationSet** elemento que representa o `FK_CustomerOrders` restrição de chave estrangeira no banco de dados subjacente:</span><span class="sxs-lookup"><span data-stu-id="26b51-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="26b51-164">Elemento CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="26b51-165">O **CollectionType** elemento na linguagem de definição de esquema de armazenamento (SSDL) Especifica que o tipo de retorno da função é uma coleção.</span><span class="sxs-lookup"><span data-stu-id="26b51-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="26b51-166">O **CollectionType** elemento é um filho do elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="26b51-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="26b51-167">O tipo de coleção é especificado usando o elemento RowType filho:</span><span class="sxs-lookup"><span data-stu-id="26b51-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="26b51-168">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="26b51-169">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-170">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-171">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-171">Example</span></span>

<span data-ttu-id="26b51-172">O exemplo a seguir mostra uma função que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de linhas.</span><span class="sxs-lookup"><span data-stu-id="26b51-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="26b51-173">Elemento CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="26b51-174">O **CommandText** elemento na linguagem de definição de esquema de armazenamento (SSDL) é um filho do elemento de função que permite que você defina uma instrução SQL que é executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="26b51-175">O **CommandText** elemento permite que você adicione funcionalidade semelhante a um procedimento armazenado no banco de dados, mas definir a **CommandText** elemento no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="26b51-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="26b51-176">O **CommandText** elemento não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="26b51-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="26b51-177">O corpo dos **CommandText** elemento deve ser uma instrução SQL válida para o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="26b51-178">Atributos não são aplicáveis para o **CommandText** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-179">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-179">Example</span></span>

<span data-ttu-id="26b51-180">A exemplo a seguir mostra uma **função** elemento com um filho **CommandText** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="26b51-181">Expor a **UpdateProductInOrder** funcionará como um método no ObjectContext, importá-los para o modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="26b51-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="26b51-182">Elemento DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="26b51-183">O **DefiningQuery** elemento na linguagem de definição de esquema de armazenamento (SSDL) permite que você execute uma instrução SQL diretamente no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="26b51-184">O **DefiningQuery** elemento normalmente é usado como um modo de exibição de banco de dados, mas o modo de exibição é definido no modelo de armazenamento, em vez do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="26b51-185">O modo de exibição definido em uma **DefiningQuery** elemento pode ser mapeado para um tipo de entidade no modelo conceitual por meio de um elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="26b51-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="26b51-186">Esses mapeamentos são somente leitura.</span><span class="sxs-lookup"><span data-stu-id="26b51-186">These mappings are read-only.</span></span>  

<span data-ttu-id="26b51-187">A sintaxe SSDL a seguir mostra a declaração de um **EntitySet** seguido de **DefiningQuery** elemento que contém uma consulta usada para recuperar o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="26b51-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="26b51-188">Você pode usar procedimentos armazenados no Entity Framework para habilitar cenários de leitura / gravação sobre modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="26b51-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="26b51-189">Você pode usar uma exibição da fonte de dados ou uma exibição de Entity SQL que a tabela base para recuperação de dados e para processamento por procedimentos armazenados de alterações.</span><span class="sxs-lookup"><span data-stu-id="26b51-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="26b51-190">Você pode usar o **DefiningQuery** elemento para o Microsoft SQL Server Compact 3.5 de destino.</span><span class="sxs-lookup"><span data-stu-id="26b51-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="26b51-191">Embora o SQL Server Compact 3.5 não oferece suporte a procedimentos armazenados, você pode implementar uma funcionalidade semelhante a **DefiningQuery** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="26b51-192">É outro lugar onde ele pode ser útil na criação de procedimentos armazenados para superar uma incompatibilidade entre os tipos de dados usados na linguagem de programação e aqueles da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="26b51-193">Você poderia escrever um **DefiningQuery** que leva a um determinado conjunto de parâmetros e, em seguida, chama um procedimento armazenado com um conjunto diferente de parâmetros, por exemplo, um procedimento armazenado que exclui dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="26b51-194">Elemento dependente (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="26b51-195">O **dependentes** na linguagem de definição de esquema de armazenamento (SSDL) é um elemento filho ao elemento ReferentialConstraint que define o final dependente de uma restrição de chave estrangeira (também chamado de uma restrição referencial).</span><span class="sxs-lookup"><span data-stu-id="26b51-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="26b51-196">O **dependentes** elemento Especifica a coluna (ou colunas) em uma tabela que fazem referência a uma coluna de chave primária (ou colunas).</span><span class="sxs-lookup"><span data-stu-id="26b51-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="26b51-197">**PropertyRef** elementos especificam quais colunas são referenciadas.</span><span class="sxs-lookup"><span data-stu-id="26b51-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="26b51-198">O Principal elemento Especifica as colunas de chave primária referenciadas pelas colunas especificadas na **dependentes** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="26b51-199">O **dependentes** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-200">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="26b51-201">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-202">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-202">Applicable Attributes</span></span>

<span data-ttu-id="26b51-203">A tabela a seguir descreve os atributos que podem ser aplicados para o **dependentes** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="26b51-204">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-204">Attribute Name</span></span> | <span data-ttu-id="26b51-205">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-205">Is Required</span></span> | <span data-ttu-id="26b51-206">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-207">**Função**</span><span class="sxs-lookup"><span data-stu-id="26b51-207">**Role**</span></span>       | <span data-ttu-id="26b51-208">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-208">Yes</span></span>         | <span data-ttu-id="26b51-209">O mesmo valor que o **função** (se usado) de atributo do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna de referência.</span><span class="sxs-lookup"><span data-stu-id="26b51-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-210">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **dependentes** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="26b51-211">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="26b51-212">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-213">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-213">Example</span></span>

<span data-ttu-id="26b51-214">O exemplo a seguir mostra um elemento de associação que usa uma **ReferentialConstraint** elemento para especificar as colunas que participam do **FK\_CustomerOrders** chave estrangeira restrição.</span><span class="sxs-lookup"><span data-stu-id="26b51-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="26b51-215">O **dependentes** elemento Especifica a **CustomerId** coluna do **ordem** tabela como o final dependente da restrição.</span><span class="sxs-lookup"><span data-stu-id="26b51-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="26b51-216">Elemento Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="26b51-217">O **documentação** elemento na linguagem de definição de esquema de armazenamento (SSDL) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai.</span><span class="sxs-lookup"><span data-stu-id="26b51-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="26b51-218">O **documentação** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-219">**Resumo**: uma breve descrição do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="26b51-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="26b51-220">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="26b51-220">(zero or one element)</span></span>
-   <span data-ttu-id="26b51-221">**LongDescription**: uma descrição abrangente do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="26b51-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="26b51-222">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="26b51-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-223">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-223">Applicable Attributes</span></span>

<span data-ttu-id="26b51-224">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **documentação** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="26b51-225">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="26b51-226">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-227">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-227">Example</span></span>

<span data-ttu-id="26b51-228">A exemplo a seguir mostra a **documentação** como um elemento filho de um elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="26b51-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="26b51-229">Elemento final (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-229">End Element (SSDL)</span></span>

<span data-ttu-id="26b51-230">O **final** elemento na linguagem de definição de esquema de armazenamento (SSDL) Especifica a tabela e o número de linhas em uma extremidade de uma restrição de chave estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="26b51-231">O **final** elemento pode ser um filho do elemento de associação ou o elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="26b51-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="26b51-232">Em cada caso, os possíveis elementos filho e atributos aplicáveis são diferentes.</span><span class="sxs-lookup"><span data-stu-id="26b51-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="26b51-233">Elemento final como um filho do elemento de associação</span><span class="sxs-lookup"><span data-stu-id="26b51-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="26b51-234">Uma **final** elemento (como um filho a **associação** elemento) Especifica a tabela e o número de linhas no final de uma restrição de chave estrangeira com a **tipo** e **Multiplicidade** atributos, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="26b51-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="26b51-235">Extremidades de uma restrição de chave estrangeira são definidas como parte de uma associação de SSDL; uma associação de SSDL deve ter exatamente duas extremidades.</span><span class="sxs-lookup"><span data-stu-id="26b51-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="26b51-236">Uma **final** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-237">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="26b51-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="26b51-238">OnDelete (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="26b51-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="26b51-239">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="26b51-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="26b51-240">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-240">Applicable Attributes</span></span>

<span data-ttu-id="26b51-241">A tabela a seguir descreve os atributos que podem ser aplicados para o **final** elemento quando ele for o filho de um **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="26b51-242">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-242">Attribute Name</span></span>   | <span data-ttu-id="26b51-243">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-243">Is Required</span></span> | <span data-ttu-id="26b51-244">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-245">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="26b51-245">**Type**</span></span>         | <span data-ttu-id="26b51-246">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-246">Yes</span></span>         | <span data-ttu-id="26b51-247">O nome totalmente qualificado do conjunto de entidades SSDL é no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="26b51-248">**Função**</span><span class="sxs-lookup"><span data-stu-id="26b51-248">**Role**</span></span>         | <span data-ttu-id="26b51-249">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-249">No</span></span>          | <span data-ttu-id="26b51-250">O valor de **função** atributo no elemento Principal ou dependente do elemento ReferentialConstraint correspondente (se usado).</span><span class="sxs-lookup"><span data-stu-id="26b51-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="26b51-251">**Multiplicidade**</span><span class="sxs-lookup"><span data-stu-id="26b51-251">**Multiplicity**</span></span> | <span data-ttu-id="26b51-252">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-252">Yes</span></span>         | <span data-ttu-id="26b51-253">**1**, **entre 0 e 1**, ou **\*** dependendo do número de linhas que podem estar no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="26b51-254">**1** indica que exatamente uma linha existe no final de restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="26b51-255">**entre 0 e 1** indica que existe de zero ou uma linha no final de restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="26b51-256">**\*** indica que zero, um ou mais linhas existem no final de restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-257">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **final** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="26b51-258">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="26b51-259">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="26b51-260">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-260">Example</span></span>

<span data-ttu-id="26b51-261">A exemplo a seguir mostra uma **associação** elemento que define o **FK\_CustomerOrders** restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="26b51-262">O **multiplicidade** valores especificados em cada **final** elemento indicam que muitas linhas no **pedidos** tabela pode ser associada uma linha no **clientes**  tabela, mas apenas uma linha em de **clientes** tabela pode ser associada uma linha no **pedidos** tabela.</span><span class="sxs-lookup"><span data-stu-id="26b51-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="26b51-263">Além disso, o **OnDelete** elemento indica que todas as linhas a **pedidos** tabelas que fazem referência a uma linha específica no **clientes** tabela será excluída se a linha na o **clientes** tabela for excluída.</span><span class="sxs-lookup"><span data-stu-id="26b51-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="26b51-264">Elemento final como um filho do elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="26b51-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="26b51-265">O **final** elemento (como um filho de **AssociationSet** elemento) especifica uma tabela em uma extremidade de uma restrição de chave estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="26b51-266">Uma **final** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-267">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="26b51-268">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="26b51-269">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-269">Applicable Attributes</span></span>

<span data-ttu-id="26b51-270">A tabela a seguir descreve os atributos que podem ser aplicados para o **final** elemento quando ele for o filho de um **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="26b51-271">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-271">Attribute Name</span></span> | <span data-ttu-id="26b51-272">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-272">Is Required</span></span> | <span data-ttu-id="26b51-273">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="26b51-274">**EntitySet**</span></span>  | <span data-ttu-id="26b51-275">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-275">Yes</span></span>         | <span data-ttu-id="26b51-276">O nome do conjunto de entidades SSDL é no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="26b51-277">**Função**</span><span class="sxs-lookup"><span data-stu-id="26b51-277">**Role**</span></span>       | <span data-ttu-id="26b51-278">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-278">No</span></span>          | <span data-ttu-id="26b51-279">O valor de um dos **função** atributos especificados em um **final** elemento do elemento de associação correspondente.</span><span class="sxs-lookup"><span data-stu-id="26b51-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-280">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **final** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="26b51-281">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="26b51-282">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="26b51-283">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-283">Example</span></span>

<span data-ttu-id="26b51-284">A exemplo a seguir mostra uma **EntityContainer** elemento com um **AssociationSet** elemento com dois **final** elementos:</span><span class="sxs-lookup"><span data-stu-id="26b51-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="26b51-285">Elemento EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="26b51-286">Uma **EntityContainer** elemento na linguagem de definição de esquema de armazenamento (SSDL) descreve a estrutura da fonte de dados subjacente em um aplicativo do Entity Framework: conjuntos de entidades SSDL (definidos em elementos EntitySet) representam tabelas um banco de dados, tipos de entidade SSDL (definidos em elementos EntityType) representam linhas em uma tabela e conjuntos de associações (definidos em elementos de AssociationSet) representam as restrições de chave estrangeira em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="26b51-287">Um contêiner de entidade do modelo de armazenamento é mapeado para um contêiner de entidade do modelo conceitual através de elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="26b51-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="26b51-288">Uma **EntityContainer** elemento pode ter zero ou mais elementos de documentação.</span><span class="sxs-lookup"><span data-stu-id="26b51-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="26b51-289">Se um **documentação** elemento estiver presente, ela deve preceder todos os outros elementos filho.</span><span class="sxs-lookup"><span data-stu-id="26b51-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="26b51-290">Uma **EntityContainer** elemento pode ter zero ou mais dos seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="26b51-291">EntitySet</span></span>
-   <span data-ttu-id="26b51-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="26b51-292">AssociationSet</span></span>
-   <span data-ttu-id="26b51-293">Elementos de anotação</span><span class="sxs-lookup"><span data-stu-id="26b51-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-294">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-294">Applicable Attributes</span></span>

<span data-ttu-id="26b51-295">A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="26b51-296">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-296">Attribute Name</span></span> | <span data-ttu-id="26b51-297">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-297">Is Required</span></span> | <span data-ttu-id="26b51-298">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="26b51-299">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-299">**Name**</span></span>       | <span data-ttu-id="26b51-300">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-300">Yes</span></span>         | <span data-ttu-id="26b51-301">O nome do contêiner de entidade.</span><span class="sxs-lookup"><span data-stu-id="26b51-301">The name of the entity container.</span></span> <span data-ttu-id="26b51-302">Esse nome não pode conter pontos (.).</span><span class="sxs-lookup"><span data-stu-id="26b51-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-303">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="26b51-304">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-305">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-306">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-306">Example</span></span>

<span data-ttu-id="26b51-307">A exemplo a seguir mostra uma **EntityContainer** elemento que define dois conjuntos de entidades e um conjunto de associações.</span><span class="sxs-lookup"><span data-stu-id="26b51-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="26b51-308">Observe que os nomes de tipo de associação e o tipo de entidade são qualificados pelo nome de namespace do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="26b51-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="26b51-309">Elemento EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="26b51-310">Uma **EntitySet** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma tabela ou exibição no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="26b51-311">Um elemento EntityType em SSDL representa uma linha na tabela ou exibição.</span><span class="sxs-lookup"><span data-stu-id="26b51-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="26b51-312">O **EntityType** atributo de uma **EntitySet** elemento Especifica o tipo de entidade específico SSDL que representa as linhas em um conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="26b51-313">O mapeamento entre um conjunto de entidades do CSDL e um conjunto de entidades SSDL é especificado em um elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="26b51-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="26b51-314">O **EntitySet** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-315">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="26b51-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="26b51-316">DefiningQuery (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="26b51-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="26b51-317">Elementos de anotação</span><span class="sxs-lookup"><span data-stu-id="26b51-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-318">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-318">Applicable Attributes</span></span>

<span data-ttu-id="26b51-319">A tabela a seguir descreve os atributos que podem ser aplicados para o **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="26b51-320">Alguns atributos (não listados aqui) podem ser qualificados com o **armazenar** alias.</span><span class="sxs-lookup"><span data-stu-id="26b51-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="26b51-321">Esses atributos são usados pelo Assistente de modelo de atualização ao atualizar um modelo.</span><span class="sxs-lookup"><span data-stu-id="26b51-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="26b51-322">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-322">Attribute Name</span></span> | <span data-ttu-id="26b51-323">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-323">Is Required</span></span> | <span data-ttu-id="26b51-324">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-325">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-325">**Name**</span></span>       | <span data-ttu-id="26b51-326">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-326">Yes</span></span>         | <span data-ttu-id="26b51-327">O nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="26b51-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="26b51-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="26b51-328">**EntityType**</span></span> | <span data-ttu-id="26b51-329">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-329">Yes</span></span>         | <span data-ttu-id="26b51-330">O nome totalmente qualificado do tipo de entidade para a qual o conjunto de entidades contém instâncias.</span><span class="sxs-lookup"><span data-stu-id="26b51-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="26b51-331">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="26b51-331">**Schema**</span></span>     | <span data-ttu-id="26b51-332">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-332">No</span></span>          | <span data-ttu-id="26b51-333">O esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="26b51-334">**Tabela**</span><span class="sxs-lookup"><span data-stu-id="26b51-334">**Table**</span></span>      | <span data-ttu-id="26b51-335">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-335">No</span></span>          | <span data-ttu-id="26b51-336">A tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="26b51-337">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="26b51-338">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-339">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-340">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-340">Example</span></span>

<span data-ttu-id="26b51-341">A exemplo a seguir mostra uma **EntityContainer** elemento que tem dois **EntitySet** elementos e um **AssociationSet** elemento:</span><span class="sxs-lookup"><span data-stu-id="26b51-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="26b51-342">Elemento EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="26b51-343">Uma **EntityType** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma linha em uma tabela ou exibição de banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="26b51-344">Um elemento de EntitySet em SSDL representa a tabela ou exibição na qual as linhas ocorrerem.</span><span class="sxs-lookup"><span data-stu-id="26b51-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="26b51-345">O **EntityType** atributo de uma **EntitySet** elemento Especifica o tipo de entidade específico SSDL que representa as linhas em um conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="26b51-346">O mapeamento entre um tipo de entidade SSDL e um tipo de entidade CSDL é especificado em um elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="26b51-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="26b51-347">O **EntityType** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-348">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="26b51-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="26b51-349">Chave (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="26b51-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="26b51-350">Elementos de anotação</span><span class="sxs-lookup"><span data-stu-id="26b51-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-351">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-351">Applicable Attributes</span></span>

<span data-ttu-id="26b51-352">A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="26b51-353">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-353">Attribute Name</span></span> | <span data-ttu-id="26b51-354">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-354">Is Required</span></span> | <span data-ttu-id="26b51-355">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-356">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-356">**Name**</span></span>       | <span data-ttu-id="26b51-357">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-357">Yes</span></span>         | <span data-ttu-id="26b51-358">O nome do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="26b51-358">The name of the entity type.</span></span> <span data-ttu-id="26b51-359">Esse valor geralmente é igual ao nome da tabela na qual o tipo de entidade representa uma linha.</span><span class="sxs-lookup"><span data-stu-id="26b51-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="26b51-360">Esse valor não pode conter nenhum pontos (.).</span><span class="sxs-lookup"><span data-stu-id="26b51-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-361">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="26b51-362">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-363">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-364">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-364">Example</span></span>

<span data-ttu-id="26b51-365">A exemplo a seguir mostra uma **EntityType** elemento com duas propriedades:</span><span class="sxs-lookup"><span data-stu-id="26b51-365">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="26b51-366">Elemento Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-366">Function Element (SSDL)</span></span>

<span data-ttu-id="26b51-367">O **função** elemento na linguagem de definição de esquema de armazenamento (SSDL) especifica um procedimento armazenado que existe no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="26b51-368">O **função** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-369">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="26b51-370">Parâmetro (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="26b51-371">CommandText (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="26b51-372">ReturnType (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="26b51-373">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="26b51-374">Retornar de um tipo para uma função deve ser especificado com qualquer um de **ReturnType** elemento ou o **ReturnType** atributo (veja abaixo), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="26b51-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="26b51-375">Os procedimentos armazenados que são especificados no modelo de armazenamento podem ser importados para o modelo conceitual de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="26b51-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="26b51-376">Para obter mais informações, consulte [consultando com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="26b51-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="26b51-377">O **função** elemento também pode ser usado para definir funções personalizadas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="26b51-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-378">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-378">Applicable Attributes</span></span>

<span data-ttu-id="26b51-379">A tabela a seguir descreve os atributos que podem ser aplicados para o **função** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="26b51-380">Alguns atributos (não listados aqui) podem ser qualificados com o **armazenar** alias.</span><span class="sxs-lookup"><span data-stu-id="26b51-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="26b51-381">Esses atributos são usados pelo Assistente de modelo de atualização ao atualizar um modelo.</span><span class="sxs-lookup"><span data-stu-id="26b51-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="26b51-382">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-382">Attribute Name</span></span>             | <span data-ttu-id="26b51-383">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-383">Is Required</span></span> | <span data-ttu-id="26b51-384">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-385">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-385">**Name**</span></span>                   | <span data-ttu-id="26b51-386">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-386">Yes</span></span>         | <span data-ttu-id="26b51-387">O nome do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="26b51-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="26b51-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="26b51-388">**ReturnType**</span></span>             | <span data-ttu-id="26b51-389">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-389">No</span></span>          | <span data-ttu-id="26b51-390">O tipo de retorno do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="26b51-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="26b51-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="26b51-391">**Aggregate**</span></span>              | <span data-ttu-id="26b51-392">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-392">No</span></span>          | <span data-ttu-id="26b51-393">**Verdadeiro** se o procedimento armazenado retorna um valor de agregação; caso contrário **falso**.</span><span class="sxs-lookup"><span data-stu-id="26b51-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="26b51-394">**Interno**</span><span class="sxs-lookup"><span data-stu-id="26b51-394">**BuiltIn**</span></span>                | <span data-ttu-id="26b51-395">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-395">No</span></span>          | <span data-ttu-id="26b51-396">**Verdadeiro** se a função for uma interna<sup>1</sup> função; caso contrário **False**.</span><span class="sxs-lookup"><span data-stu-id="26b51-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="26b51-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="26b51-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="26b51-398">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-398">No</span></span>          | <span data-ttu-id="26b51-399">O nome do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="26b51-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="26b51-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="26b51-400">**NiladicFunction**</span></span>        | <span data-ttu-id="26b51-401">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-401">No</span></span>          | <span data-ttu-id="26b51-402">**Verdadeiro** se a função for um niladic<sup>2</sup> função; **Falsos** caso contrário.</span><span class="sxs-lookup"><span data-stu-id="26b51-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="26b51-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="26b51-403">**IsComposable**</span></span>           | <span data-ttu-id="26b51-404">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-404">No</span></span>          | <span data-ttu-id="26b51-405">**Verdadeiro** se a função for um Combinável<sup>3</sup> função; **Falsos** caso contrário.</span><span class="sxs-lookup"><span data-stu-id="26b51-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="26b51-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="26b51-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="26b51-407">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-407">No</span></span>          | <span data-ttu-id="26b51-408">A enumeração que define a semântica de tipo usada para resolver as sobrecargas de função.</span><span class="sxs-lookup"><span data-stu-id="26b51-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="26b51-409">A enumeração é definida no manifesto do provedor por definição de função.</span><span class="sxs-lookup"><span data-stu-id="26b51-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="26b51-410">O valor padrão é **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="26b51-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="26b51-411">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="26b51-411">**Schema**</span></span>                 | <span data-ttu-id="26b51-412">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-412">No</span></span>          | <span data-ttu-id="26b51-413">O nome do esquema no qual o procedimento armazenado é definido.</span><span class="sxs-lookup"><span data-stu-id="26b51-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="26b51-414"><sup>1</sup> uma função interna é uma função que é definida no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="26b51-415">Para obter informações sobre as funções que são definidos no modelo de armazenamento, consulte o elemento CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="26b51-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="26b51-416"><sup>2</sup> uma função niladic é uma função que não aceita parâmetros e, quando chamado, não requer parênteses.</span><span class="sxs-lookup"><span data-stu-id="26b51-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="26b51-417"><sup>3</sup> duas funções são combináveis se a saída de uma função pode ser a entrada para a outra função.</span><span class="sxs-lookup"><span data-stu-id="26b51-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="26b51-418">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **função** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="26b51-419">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-420">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-421">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-421">Example</span></span>

<span data-ttu-id="26b51-422">A exemplo a seguir mostra uma **função** elemento que corresponde do **UpdateOrderQuantity** procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="26b51-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="26b51-423">O procedimento armazenado aceita dois parâmetros e não retorna um valor.</span><span class="sxs-lookup"><span data-stu-id="26b51-423">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="26b51-424">Elemento key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-424">Key Element (SSDL)</span></span>

<span data-ttu-id="26b51-425">O **chave** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa a chave primária de uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="26b51-426">**Chave** é um elemento filho de um elemento EntityType, que representa uma linha em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="26b51-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="26b51-427">A chave primária é definida na **chave** elemento fazendo referência a um ou mais elementos de propriedade são definidos na **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="26b51-428">O **chave** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-429">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="26b51-430">Elementos de anotação</span><span class="sxs-lookup"><span data-stu-id="26b51-430">Annotation elements</span></span>

<span data-ttu-id="26b51-431">Atributos não são aplicáveis para o **chave** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-432">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-432">Example</span></span>

<span data-ttu-id="26b51-433">A exemplo a seguir mostra uma **EntityType** elemento com uma chave que faz referência a uma propriedade:</span><span class="sxs-lookup"><span data-stu-id="26b51-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="26b51-434">Elemento OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="26b51-435">O **OnDelete** elemento na linguagem de definição de esquema de armazenamento (SSDL) reflete o comportamento do banco de dados quando uma linha que participa de uma restrição de chave estrangeira é excluída.</span><span class="sxs-lookup"><span data-stu-id="26b51-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="26b51-436">Se a ação for definida como **Cascade**, em seguida, as linhas que fazem referência a uma linha que está sendo excluída também serão excluídas.</span><span class="sxs-lookup"><span data-stu-id="26b51-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="26b51-437">Se a ação for definida como **None**, em seguida, as linhas que fazem referência a uma linha que está sendo excluída também não são excluídas.</span><span class="sxs-lookup"><span data-stu-id="26b51-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="26b51-438">Uma **OnDelete** é um elemento filho de um elemento final.</span><span class="sxs-lookup"><span data-stu-id="26b51-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="26b51-439">Uma **OnDelete** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-440">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="26b51-441">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-442">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-442">Applicable Attributes</span></span>

<span data-ttu-id="26b51-443">A tabela a seguir descreve os atributos que podem ser aplicados para o **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="26b51-444">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-444">Attribute Name</span></span> | <span data-ttu-id="26b51-445">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-445">Is Required</span></span> | <span data-ttu-id="26b51-446">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-447">**Ação**</span><span class="sxs-lookup"><span data-stu-id="26b51-447">**Action**</span></span>     | <span data-ttu-id="26b51-448">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-448">Yes</span></span>         | <span data-ttu-id="26b51-449">**CASCADE** ou **None**.</span><span class="sxs-lookup"><span data-stu-id="26b51-449">**Cascade** or **None**.</span></span> <span data-ttu-id="26b51-450">(O valor **Restricted** é válida, mas tem o mesmo comportamento **None**.)</span><span class="sxs-lookup"><span data-stu-id="26b51-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-451">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="26b51-452">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-453">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-454">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-454">Example</span></span>

<span data-ttu-id="26b51-455">A exemplo a seguir mostra uma **associação** elemento que define o **FK\_CustomerOrders** restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="26b51-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="26b51-456">O **OnDelete** elemento indica que todas as linhas a **pedidos** tabelas que fazem referência a uma linha específica no **clientes** tabela será excluída se na linha do **Os clientes** tabela for excluída.</span><span class="sxs-lookup"><span data-stu-id="26b51-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="26b51-457">Elemento Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="26b51-458">O **parâmetro** elemento na linguagem de definição de esquema de armazenamento (SSDL) é um filho do elemento de função que especifica os parâmetros para um procedimento armazenado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="26b51-459">O **parâmetro** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-460">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="26b51-461">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-462">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-462">Applicable Attributes</span></span>

<span data-ttu-id="26b51-463">A tabela a seguir descreve os atributos que podem ser aplicados para o **parâmetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="26b51-464">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-464">Attribute Name</span></span> | <span data-ttu-id="26b51-465">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-465">Is Required</span></span> | <span data-ttu-id="26b51-466">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-467">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-467">**Name**</span></span>       | <span data-ttu-id="26b51-468">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-468">Yes</span></span>         | <span data-ttu-id="26b51-469">O nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="26b51-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="26b51-470">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="26b51-470">**Type**</span></span>       | <span data-ttu-id="26b51-471">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-471">Yes</span></span>         | <span data-ttu-id="26b51-472">O tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="26b51-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="26b51-473">**Modo**</span><span class="sxs-lookup"><span data-stu-id="26b51-473">**Mode**</span></span>       | <span data-ttu-id="26b51-474">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-474">No</span></span>          | <span data-ttu-id="26b51-475">**Na**, **Out**, ou **InOut** dependendo se o parâmetro for uma entrada, saída ou parâmetro de entrada/saída.</span><span class="sxs-lookup"><span data-stu-id="26b51-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="26b51-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="26b51-476">**MaxLength**</span></span>  | <span data-ttu-id="26b51-477">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-477">No</span></span>          | <span data-ttu-id="26b51-478">O comprimento máximo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="26b51-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="26b51-479">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="26b51-479">**Precision**</span></span>  | <span data-ttu-id="26b51-480">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-480">No</span></span>          | <span data-ttu-id="26b51-481">A precisão do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="26b51-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="26b51-482">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="26b51-482">**Scale**</span></span>      | <span data-ttu-id="26b51-483">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-483">No</span></span>          | <span data-ttu-id="26b51-484">A escala do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="26b51-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="26b51-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="26b51-485">**SRID**</span></span>       | <span data-ttu-id="26b51-486">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-486">No</span></span>          | <span data-ttu-id="26b51-487">Identificador de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="26b51-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="26b51-488">Válido somente para parâmetros de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="26b51-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="26b51-489">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="26b51-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-490">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **parâmetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="26b51-491">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-492">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-493">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-493">Example</span></span>

<span data-ttu-id="26b51-494">A exemplo a seguir mostra uma **função** elemento que tem dois **parâmetro** elementos que especificam parâmetros de entrada:</span><span class="sxs-lookup"><span data-stu-id="26b51-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="26b51-495">Elemento de entidade de segurança (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="26b51-496">O **Principal** na linguagem de definição de esquema de armazenamento (SSDL) é um elemento filho ao elemento ReferentialConstraint que define o final principal de uma restrição de chave estrangeira (também chamado de uma restrição referencial).</span><span class="sxs-lookup"><span data-stu-id="26b51-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="26b51-497">O **Principal** elemento Especifica a coluna de chave primária (ou colunas) em uma tabela que é referenciada por outra coluna (ou colunas).</span><span class="sxs-lookup"><span data-stu-id="26b51-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="26b51-498">**PropertyRef** elementos especificam quais colunas são referenciadas.</span><span class="sxs-lookup"><span data-stu-id="26b51-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="26b51-499">O elemento dependente Especifica colunas que fazem referência a colunas de chave primária que são especificadas na **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="26b51-500">O **Principal** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="26b51-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="26b51-501">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="26b51-502">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-503">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-503">Applicable Attributes</span></span>

<span data-ttu-id="26b51-504">A tabela a seguir descreve os atributos que podem ser aplicados para o **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="26b51-505">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-505">Attribute Name</span></span> | <span data-ttu-id="26b51-506">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-506">Is Required</span></span> | <span data-ttu-id="26b51-507">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-508">**Função**</span><span class="sxs-lookup"><span data-stu-id="26b51-508">**Role**</span></span>       | <span data-ttu-id="26b51-509">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-509">Yes</span></span>         | <span data-ttu-id="26b51-510">O mesmo valor que o **função** (se usado) de atributo do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna referenciada.</span><span class="sxs-lookup"><span data-stu-id="26b51-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-511">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="26b51-512">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="26b51-513">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-514">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-514">Example</span></span>

<span data-ttu-id="26b51-515">O exemplo a seguir mostra um elemento de associação que usa uma **ReferentialConstraint** elemento para especificar as colunas que participam do **FK\_CustomerOrders** chave estrangeira restrição.</span><span class="sxs-lookup"><span data-stu-id="26b51-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="26b51-516">O **Principal** elemento Especifica a **CustomerId** coluna do **cliente** tabela como o final principal da restrição.</span><span class="sxs-lookup"><span data-stu-id="26b51-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="26b51-517">Elemento de propriedade (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-517">Property Element (SSDL)</span></span>

<span data-ttu-id="26b51-518">O **propriedade** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma coluna em uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="26b51-519">**Propriedade** elementos são filhos de elementos de EntityType, que representam as linhas em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="26b51-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="26b51-520">Cada **propriedade** elemento definido em um **EntityType** elemento representa uma coluna.</span><span class="sxs-lookup"><span data-stu-id="26b51-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="26b51-521">Um **propriedade** elemento não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="26b51-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-522">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-522">Applicable Attributes</span></span>

<span data-ttu-id="26b51-523">A tabela a seguir descreve os atributos que podem ser aplicados para o **propriedade** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="26b51-524">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-524">Attribute Name</span></span>            | <span data-ttu-id="26b51-525">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-525">Is Required</span></span> | <span data-ttu-id="26b51-526">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-527">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-527">**Name**</span></span>                  | <span data-ttu-id="26b51-528">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-528">Yes</span></span>         | <span data-ttu-id="26b51-529">O nome da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="26b51-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="26b51-530">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="26b51-530">**Type**</span></span>                  | <span data-ttu-id="26b51-531">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-531">Yes</span></span>         | <span data-ttu-id="26b51-532">O tipo da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="26b51-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="26b51-533">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="26b51-533">**Nullable**</span></span>              | <span data-ttu-id="26b51-534">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-534">No</span></span>          | <span data-ttu-id="26b51-535">**Verdadeiro** (o valor padrão) ou **falso** dependendo se a coluna correspondente pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="26b51-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="26b51-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="26b51-536">**DefaultValue**</span></span>          | <span data-ttu-id="26b51-537">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-537">No</span></span>          | <span data-ttu-id="26b51-538">O valor padrão da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="26b51-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="26b51-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="26b51-539">**MaxLength**</span></span>             | <span data-ttu-id="26b51-540">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-540">No</span></span>          | <span data-ttu-id="26b51-541">O comprimento máximo da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="26b51-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="26b51-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="26b51-542">**FixedLength**</span></span>           | <span data-ttu-id="26b51-543">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-543">No</span></span>          | <span data-ttu-id="26b51-544">**Verdadeiro** ou **falso** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="26b51-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="26b51-545">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="26b51-545">**Precision**</span></span>             | <span data-ttu-id="26b51-546">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-546">No</span></span>          | <span data-ttu-id="26b51-547">A precisão da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="26b51-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="26b51-548">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="26b51-548">**Scale**</span></span>                 | <span data-ttu-id="26b51-549">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-549">No</span></span>          | <span data-ttu-id="26b51-550">A escala da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="26b51-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="26b51-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="26b51-551">**Unicode**</span></span>               | <span data-ttu-id="26b51-552">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-552">No</span></span>          | <span data-ttu-id="26b51-553">**Verdadeiro** ou **falso** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="26b51-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="26b51-554">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="26b51-554">**Collation**</span></span>             | <span data-ttu-id="26b51-555">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-555">No</span></span>          | <span data-ttu-id="26b51-556">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="26b51-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="26b51-557">**SRID**</span></span>                  | <span data-ttu-id="26b51-558">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-558">No</span></span>          | <span data-ttu-id="26b51-559">Identificador de referência espacial do sistema.</span><span class="sxs-lookup"><span data-stu-id="26b51-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="26b51-560">Válido somente para as propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="26b51-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="26b51-561">Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="26b51-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="26b51-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="26b51-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="26b51-563">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-563">No</span></span>          | <span data-ttu-id="26b51-564">**None**, **Identity** (se o valor da coluna correspondente é uma identidade que é gerada no banco de dados), ou **calculado** (se o valor da coluna correspondente é calculado no banco de dados).</span><span class="sxs-lookup"><span data-stu-id="26b51-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="26b51-565">Não válido para RowType propriedades.</span><span class="sxs-lookup"><span data-stu-id="26b51-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-566">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **propriedade** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="26b51-567">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-568">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-569">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-569">Example</span></span>

<span data-ttu-id="26b51-570">A exemplo a seguir mostra uma **EntityType** elemento com dois filhos **propriedade** elementos:</span><span class="sxs-lookup"><span data-stu-id="26b51-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="26b51-571">Elemento PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="26b51-572">O **PropertyRef** elemento na linguagem de definição de esquema de armazenamento (SSDL) faz referência a uma propriedade definida em um elemento EntityType para indicar que a propriedade realizará uma das seguintes funções:</span><span class="sxs-lookup"><span data-stu-id="26b51-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="26b51-573">Fazer parte da chave primária da tabela que o **EntityType** representa.</span><span class="sxs-lookup"><span data-stu-id="26b51-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="26b51-574">Um ou mais **PropertyRef** elementos podem ser usados para definir uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="26b51-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="26b51-575">Para obter mais informações, consulte o elemento de chave.</span><span class="sxs-lookup"><span data-stu-id="26b51-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="26b51-576">Ser a parte final dependente ou entidade de segurança de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="26b51-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="26b51-577">Para obter mais informações, consulte o elemento ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="26b51-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="26b51-578">O **PropertyRef** elemento só pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="26b51-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="26b51-579">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="26b51-580">Elementos de anotação</span><span class="sxs-lookup"><span data-stu-id="26b51-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-581">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-581">Applicable Attributes</span></span>

<span data-ttu-id="26b51-582">A tabela a seguir descreve os atributos que podem ser aplicados para o **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="26b51-583">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-583">Attribute Name</span></span> | <span data-ttu-id="26b51-584">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-584">Is Required</span></span> | <span data-ttu-id="26b51-585">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="26b51-586">**Nome**</span><span class="sxs-lookup"><span data-stu-id="26b51-586">**Name**</span></span>       | <span data-ttu-id="26b51-587">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-587">Yes</span></span>         | <span data-ttu-id="26b51-588">O nome da propriedade referenciada.</span><span class="sxs-lookup"><span data-stu-id="26b51-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="26b51-589">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="26b51-590">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="26b51-591">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-592">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-592">Example</span></span>

<span data-ttu-id="26b51-593">A exemplo a seguir mostra uma **PropertyRef** elemento usado para definir uma chave primária, fazendo referência a uma propriedade que é definida em um **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="26b51-594">Elemento ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="26b51-595">O **ReferentialConstraint** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma restrição de chave estrangeira (também chamada de uma restrição de integridade referencial) no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="26b51-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="26b51-596">As extremidades de entidade e dependentes da restrição são especificadas pelos elementos filhos entidade e dependente, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="26b51-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="26b51-597">Colunas que integram as extremidades de entidade e dependentes são referenciadas com elementos PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="26b51-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="26b51-598">O **ReferentialConstraint** é um elemento filho opcional do elemento de associação.</span><span class="sxs-lookup"><span data-stu-id="26b51-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="26b51-599">Se um **ReferentialConstraint** elemento não será usado para mapear a restrição de chave estrangeira que é especificada na **associação** elemento, um AssociationSetMapping elemento deve ser usado para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="26b51-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="26b51-600">O **ReferentialConstraint** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="26b51-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="26b51-601">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="26b51-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="26b51-602">Entidade de segurança (exatamente uma)</span><span class="sxs-lookup"><span data-stu-id="26b51-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="26b51-603">Dependente (exatamente uma)</span><span class="sxs-lookup"><span data-stu-id="26b51-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="26b51-604">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-605">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-605">Applicable Attributes</span></span>

<span data-ttu-id="26b51-606">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="26b51-607">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-608">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-609">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-609">Example</span></span>

<span data-ttu-id="26b51-610">A exemplo a seguir mostra uma **associação** elemento que usa um **ReferentialConstraint** elemento para especificar as colunas que participam do **FK\_CustomerOrders**  restrição de chave estrangeira:</span><span class="sxs-lookup"><span data-stu-id="26b51-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="26b51-611">Elemento ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="26b51-612">O **ReturnType** elemento na linguagem de definição de esquema de armazenamento (SSDL) Especifica o tipo de retorno para uma função que é definido em um **função** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="26b51-613">Uma função de tipo de retorno também pode ser especificado com um **ReturnType** atributo.</span><span class="sxs-lookup"><span data-stu-id="26b51-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="26b51-614">O tipo de retorno de uma função é especificado com o **tipo** atributo ou o **ReturnType** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="26b51-615">O **ReturnType** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="26b51-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="26b51-616">CollectionType (um)</span><span class="sxs-lookup"><span data-stu-id="26b51-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="26b51-617">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReturnType** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="26b51-618">No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="26b51-619">Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-620">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-620">Example</span></span>

<span data-ttu-id="26b51-621">O exemplo a seguir usa uma **função** que retorna uma coleção de linhas.</span><span class="sxs-lookup"><span data-stu-id="26b51-621">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="26b51-622">Elemento RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="26b51-623">Um **RowType** elemento na linguagem de definição de esquema de armazenamento (SSDL) define uma estrutura sem nome como um retorno de tipo para uma função definida no repositório.</span><span class="sxs-lookup"><span data-stu-id="26b51-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="26b51-624">Um **RowType** é o elemento filho do **CollectionType** elemento:</span><span class="sxs-lookup"><span data-stu-id="26b51-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="26b51-625">Um **RowType** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="26b51-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="26b51-626">Propriedade (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="26b51-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="26b51-627">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-627">Example</span></span>

<span data-ttu-id="26b51-628">O exemplo a seguir mostra uma função de armazenamento que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de linhas (conforme especificado na **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="26b51-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="26b51-629">Elemento de esquema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="26b51-630">O **esquema** na linguagem de definição de esquema de armazenamento (SSDL) é o elemento raiz de uma definição de modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="26b51-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="26b51-631">Ele contém definições para os contêineres que compõem um modelo de armazenamento, funções e objetos.</span><span class="sxs-lookup"><span data-stu-id="26b51-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="26b51-632">O **esquema** elemento pode conter zero ou mais dos seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="26b51-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="26b51-633">Associação</span><span class="sxs-lookup"><span data-stu-id="26b51-633">Association</span></span>
-   <span data-ttu-id="26b51-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="26b51-634">EntityType</span></span>
-   <span data-ttu-id="26b51-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="26b51-635">EntityContainer</span></span>
-   <span data-ttu-id="26b51-636">Função</span><span class="sxs-lookup"><span data-stu-id="26b51-636">Function</span></span>

<span data-ttu-id="26b51-637">O **esquema** elemento usa o **Namespace** atributo para definir o namespace para os objetos de associação e o tipo de entidade em um modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="26b51-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="26b51-638">Dentro de um namespace, não possui dois objetos podem ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="26b51-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="26b51-639">Um namespace de modelo de armazenamento é diferente do namespace de XML o **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="26b51-640">Um namespace de modelo de armazenamento (conforme definido pela **Namespace** atributo) é um contêiner lógico para tipos de entidade e tipos de associação.</span><span class="sxs-lookup"><span data-stu-id="26b51-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="26b51-641">O namespace de XML (indicado pelo **xmlns** atributo) de uma **esquema** elemento é o namespace padrão para os elementos filho e atributos dos **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="26b51-642">Namespaces XML do formulário http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (em que aaaa e MM representam um ano e mês, respectivamente) são reservados para SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="26b51-643">Atributos e elementos personalizados não podem ser nos namespaces que têm esse formulário.</span><span class="sxs-lookup"><span data-stu-id="26b51-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="26b51-644">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="26b51-644">Applicable Attributes</span></span>

<span data-ttu-id="26b51-645">A tabela a seguir descreve os atributos podem ser aplicados para o **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="26b51-646">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="26b51-646">Attribute Name</span></span>            | <span data-ttu-id="26b51-647">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="26b51-647">Is Required</span></span> | <span data-ttu-id="26b51-648">Valor</span><span class="sxs-lookup"><span data-stu-id="26b51-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="26b51-649">**Namespace**</span></span>             | <span data-ttu-id="26b51-650">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-650">Yes</span></span>         | <span data-ttu-id="26b51-651">O namespace do modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="26b51-651">The namespace of the storage model.</span></span> <span data-ttu-id="26b51-652">O valor de **Namespace** atributo é usado para formar o nome totalmente qualificado de um tipo.</span><span class="sxs-lookup"><span data-stu-id="26b51-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="26b51-653">Por exemplo, se um **EntityType** denominado *Customer* está no namespace ExampleModel.Store e, em seguida, o nome totalmente qualificado do **EntityType** é ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="26b51-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="26b51-654">As seguintes cadeias de caracteres não podem ser usadas como o valor para o **Namespace** atributo: **sistema**, **transitório**, ou **Edm**.</span><span class="sxs-lookup"><span data-stu-id="26b51-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="26b51-655">O valor para o **Namespace** atributo não pode ser o mesmo que o valor para o **Namespace** atributo no elemento do esquema de CSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="26b51-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="26b51-656">**Alias**</span></span>                 | <span data-ttu-id="26b51-657">Não</span><span class="sxs-lookup"><span data-stu-id="26b51-657">No</span></span>          | <span data-ttu-id="26b51-658">Um identificador usado no lugar do nome de namespace.</span><span class="sxs-lookup"><span data-stu-id="26b51-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="26b51-659">Por exemplo, se um **EntityType** denominado *Customer* está no namespace ExampleModel.Store e o valor da **Alias** atributo é *StorageModel*, em seguida, você pode usar StorageModel.Customer como o nome totalmente qualificado do **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="26b51-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="26b51-660">**Provedor**</span><span class="sxs-lookup"><span data-stu-id="26b51-660">**Provider**</span></span>              | <span data-ttu-id="26b51-661">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-661">Yes</span></span>         | <span data-ttu-id="26b51-662">O provedor de dados.</span><span class="sxs-lookup"><span data-stu-id="26b51-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="26b51-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="26b51-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="26b51-664">Sim</span><span class="sxs-lookup"><span data-stu-id="26b51-664">Yes</span></span>         | <span data-ttu-id="26b51-665">Um token que indica ao provedor que manifesto do provedor para retornar.</span><span class="sxs-lookup"><span data-stu-id="26b51-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="26b51-666">Nenhum formato para o token é definido.</span><span class="sxs-lookup"><span data-stu-id="26b51-666">No format for the token is defined.</span></span> <span data-ttu-id="26b51-667">Valores para o token são definidos pelo provedor.</span><span class="sxs-lookup"><span data-stu-id="26b51-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="26b51-668">Para obter informações sobre tokens de manifesto de provedor do SQL Server, consulte SqlClient para Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="26b51-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="26b51-669">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-669">Example</span></span>

<span data-ttu-id="26b51-670">A exemplo a seguir mostra uma **esquema** elemento que contém uma **EntityContainer** elemento, dois **EntityType** elementos e um **associação** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="26b51-671">Atributos de anotação</span><span class="sxs-lookup"><span data-stu-id="26b51-671">Annotation Attributes</span></span>

<span data-ttu-id="26b51-672">Atributos de anotação na linguagem de definição de esquema de armazenamento (SSDL) são atributos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre os elementos no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="26b51-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="26b51-673">Além de ter a estrutura XML válida, as seguintes restrições se aplicam aos atributos de anotação:</span><span class="sxs-lookup"><span data-stu-id="26b51-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="26b51-674">Atributos de anotação não devem estar em qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="26b51-675">Os nomes totalmente qualificados de todos os atributos dois anotação não pode ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="26b51-676">Mais de um atributo de anotação pode ser aplicado a um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="26b51-677">Os metadados contidos em elementos de anotação podem ser acessado no tempo de execução usando classes no namespace Metadata.</span><span class="sxs-lookup"><span data-stu-id="26b51-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-678">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-678">Example</span></span>

<span data-ttu-id="26b51-679">O exemplo a seguir mostra um elemento EntityType que tem um atributo de anotação aplicado para o **OrderId** propriedade.</span><span class="sxs-lookup"><span data-stu-id="26b51-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="26b51-680">O exemplo também mostram um elemento de anotação adicionado para o **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="26b51-680">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="26b51-681">Elementos de anotação (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="26b51-682">Elementos de anotação de linguagem de definição de esquema de armazenamento (SSDL) são elementos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre o modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="26b51-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="26b51-683">Além de ter a estrutura XML válida, as seguintes restrições se aplicam a elementos de anotação:</span><span class="sxs-lookup"><span data-stu-id="26b51-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="26b51-684">Elementos de anotação não devem estar em qualquer namespace XML que é reservado para o SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="26b51-685">Os nomes totalmente qualificados de quaisquer elementos de dois de anotação não pode ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="26b51-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="26b51-686">Elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="26b51-687">Mais de um elemento de anotação pode ser um filho de um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="26b51-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="26b51-688">Começando com o .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessado no tempo de execução usando classes no namespace Metadata.</span><span class="sxs-lookup"><span data-stu-id="26b51-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="26b51-689">Exemplo</span><span class="sxs-lookup"><span data-stu-id="26b51-689">Example</span></span>

<span data-ttu-id="26b51-690">O exemplo a seguir mostra um elemento EntityType que tem um elemento de anotação (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="26b51-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="26b51-691">O exemplo também mostra um atributo de anotação aplicado para o **OrderId** propriedade.</span><span class="sxs-lookup"><span data-stu-id="26b51-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="26b51-692">Facetas (SSDL)</span><span class="sxs-lookup"><span data-stu-id="26b51-692">Facets (SSDL)</span></span>

<span data-ttu-id="26b51-693">Facetas na linguagem de definição de esquema de armazenamento (SSDL) representam as restrições nos tipos de coluna são especificadas em elementos de propriedade.</span><span class="sxs-lookup"><span data-stu-id="26b51-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="26b51-694">As facetas são implementadas como atributos XML em **propriedade** elementos.</span><span class="sxs-lookup"><span data-stu-id="26b51-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="26b51-695">A tabela a seguir descreve as facetas que são suportadas em SSDL:</span><span class="sxs-lookup"><span data-stu-id="26b51-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="26b51-696">Aspecto</span><span class="sxs-lookup"><span data-stu-id="26b51-696">Facet</span></span>           | <span data-ttu-id="26b51-697">Descrição</span><span class="sxs-lookup"><span data-stu-id="26b51-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26b51-698">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="26b51-698">**Collation**</span></span>   | <span data-ttu-id="26b51-699">Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="26b51-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="26b51-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="26b51-700">**FixedLength**</span></span> | <span data-ttu-id="26b51-701">Especifica se o comprimento do valor da coluna pode variar.</span><span class="sxs-lookup"><span data-stu-id="26b51-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="26b51-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="26b51-702">**MaxLength**</span></span>   | <span data-ttu-id="26b51-703">Especifica o comprimento máximo do valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="26b51-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="26b51-704">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="26b51-704">**Precision**</span></span>   | <span data-ttu-id="26b51-705">Para propriedades do tipo **Decimal**, especifica o número de dígitos de um valor da propriedade pode ter.</span><span class="sxs-lookup"><span data-stu-id="26b51-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="26b51-706">Para propriedades do tipo **tempo**, **DateTime**, e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="26b51-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="26b51-707">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="26b51-707">**Scale**</span></span>       | <span data-ttu-id="26b51-708">Especifica o número de dígitos à direita do ponto decimal para o valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="26b51-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="26b51-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="26b51-709">**Unicode**</span></span>     | <span data-ttu-id="26b51-710">Indica se o valor da coluna é armazenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="26b51-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
