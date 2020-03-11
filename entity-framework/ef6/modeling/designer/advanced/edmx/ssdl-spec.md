---
title: Especificação de SSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418721"
---
# <a name="ssdl-specification"></a><span data-ttu-id="16dfd-102">Especificação de SSDL</span><span class="sxs-lookup"><span data-stu-id="16dfd-102">SSDL Specification</span></span>
<span data-ttu-id="16dfd-103">O SSDL (Store Schema Definition Language) é uma linguagem baseada em XML que descreve o modelo de armazenamento de um aplicativo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dfd-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="16dfd-104">Em um aplicativo Entity Framework, os metadados do modelo de armazenamento são carregados de um arquivo. SSDL (gravado em SSDL) em uma instância do System. Data. Metadata. Edm. StoreItemCollection e é acessível usando métodos no Classe System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="16dfd-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="16dfd-105">Entity Framework usa metadados do modelo de armazenamento para converter consultas em relação ao modelo conceitual para armazenar comandos específicos do armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="16dfd-106">O Entity Framework Designer (EF designer) armazena informações do modelo de armazenamento em um arquivo. edmx em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="16dfd-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="16dfd-107">No momento da compilação, o Entity Designer usa informações em um arquivo. edmx para criar o arquivo. SSDL necessário para Entity Framework em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="16dfd-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="16dfd-108">As versões de SSDL são diferenciadas por namespaces XML.</span><span class="sxs-lookup"><span data-stu-id="16dfd-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="16dfd-109">Versão do SSDL</span><span class="sxs-lookup"><span data-stu-id="16dfd-109">SSDL Version</span></span> | <span data-ttu-id="16dfd-110">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="16dfd-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="16dfd-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="16dfd-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="16dfd-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="16dfd-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="16dfd-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="16dfd-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="16dfd-114">Elemento Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-114">Association Element (SSDL)</span></span>

<span data-ttu-id="16dfd-115">Um elemento de **Associação** na linguagem de definição de esquema de repositório (SSDL) especifica colunas de tabela que participam de uma restrição de chave estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="16dfd-116">Dois elementos end filho obrigatórios especificam tabelas nas extremidades da associação e a multiplicidade em cada extremidade.</span><span class="sxs-lookup"><span data-stu-id="16dfd-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="16dfd-117">Um elemento ReferentialConstraint opcional especifica as extremidades principais e dependentes da associação, bem como as colunas participantes.</span><span class="sxs-lookup"><span data-stu-id="16dfd-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="16dfd-118">Se nenhum elemento **ReferentialConstraint** estiver presente, um elemento AssociationSetMapping deverá ser usado para especificar os mapeamentos de coluna para a associação.</span><span class="sxs-lookup"><span data-stu-id="16dfd-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="16dfd-119">O elemento **Association** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-120">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="16dfd-121">Término (exatamente duas)</span><span class="sxs-lookup"><span data-stu-id="16dfd-121">End (exactly two)</span></span>
-   <span data-ttu-id="16dfd-122">ReferentialConstraint (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="16dfd-123">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-124">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-124">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-125">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="16dfd-126">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-126">Attribute Name</span></span> | <span data-ttu-id="16dfd-127">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-127">Is Required</span></span> | <span data-ttu-id="16dfd-128">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-129">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-129">**Name**</span></span>       | <span data-ttu-id="16dfd-130">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-130">Yes</span></span>         | <span data-ttu-id="16dfd-131">O nome da restrição de chave estrangeira correspondente no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-132">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="16dfd-133">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-134">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-135">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-135">Example</span></span>

<span data-ttu-id="16dfd-136">O exemplo a seguir mostra um elemento **Association** que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira de **CustomerOrders de CE\_** :</span><span class="sxs-lookup"><span data-stu-id="16dfd-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="16dfd-137">Elemento AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="16dfd-138">O elemento **AssociationSet** na linguagem de definição de esquema de repositório (SSDL) representa uma restrição de chave estrangeira entre duas tabelas no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="16dfd-139">As colunas da tabela que participam da restrição FOREIGN KEY são especificadas em um elemento Association.</span><span class="sxs-lookup"><span data-stu-id="16dfd-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="16dfd-140">O elemento **Association** que corresponde a um determinado elemento **AssociationSet** é especificado no atributo **Association** do elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="16dfd-141">Os conjuntos de associação SSDL são mapeados para conjuntos de associação CSDL por um elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="16dfd-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="16dfd-142">No entanto, se a associação de CSDL para um determinado conjunto de associação de CSDL for definida usando um elemento ReferentialConstraint, nenhum elemento **AssociationSetMapping** correspondente será necessário.</span><span class="sxs-lookup"><span data-stu-id="16dfd-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="16dfd-143">Nesse caso, se um elemento **AssociationSetMapping** estiver presente, os mapeamentos que ele definir serão substituídos pelo elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="16dfd-144">O elemento **AssociationSet** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-145">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="16dfd-146">End (zero ou dois)</span><span class="sxs-lookup"><span data-stu-id="16dfd-146">End (zero or two)</span></span>
-   <span data-ttu-id="16dfd-147">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-148">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-148">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-149">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="16dfd-150">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-150">Attribute Name</span></span>  | <span data-ttu-id="16dfd-151">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-151">Is Required</span></span> | <span data-ttu-id="16dfd-152">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-153">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-153">**Name**</span></span>        | <span data-ttu-id="16dfd-154">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-154">Yes</span></span>         | <span data-ttu-id="16dfd-155">O nome da restrição de chave estrangeira que o conjunto de associação representa.</span><span class="sxs-lookup"><span data-stu-id="16dfd-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="16dfd-156">**Associação**</span><span class="sxs-lookup"><span data-stu-id="16dfd-156">**Association**</span></span> | <span data-ttu-id="16dfd-157">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-157">Yes</span></span>         | <span data-ttu-id="16dfd-158">O nome da associação que define as colunas que participam da restrição FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="16dfd-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-159">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="16dfd-160">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-161">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-162">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-162">Example</span></span>

<span data-ttu-id="16dfd-163">O exemplo a seguir mostra um elemento **AssociationSet** que representa a restrição de chave estrangeira `FK_CustomerOrders` no banco de dados subjacente:</span><span class="sxs-lookup"><span data-stu-id="16dfd-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="16dfd-164">Elemento CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="16dfd-165">O elemento **CollectionType** em repositório de linguagem de definição de esquema (SSDL) especifica que o tipo de retorno de uma função é uma coleção.</span><span class="sxs-lookup"><span data-stu-id="16dfd-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="16dfd-166">O elemento **CollectionType** é um filho do elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="16dfd-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="16dfd-167">O tipo de coleção é especificado usando o elemento filho RowType:</span><span class="sxs-lookup"><span data-stu-id="16dfd-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="16dfd-168">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="16dfd-169">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-170">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-171">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-171">Example</span></span>

<span data-ttu-id="16dfd-172">O exemplo a seguir mostra uma função que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas.</span><span class="sxs-lookup"><span data-stu-id="16dfd-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="16dfd-173">Elemento CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="16dfd-174">O elemento **CommandText** na Store (linguagem de definição de esquema) do repositório é um filho do elemento Function que permite que você defina uma instrução SQL que é executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="16dfd-175">O elemento **CommandText** permite adicionar funcionalidade semelhante a um procedimento armazenado no banco de dados, mas você define o elemento **CommandText** no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="16dfd-176">O elemento **CommandText** não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="16dfd-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="16dfd-177">O corpo do elemento **CommandText** deve ser uma instrução SQL válida para o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="16dfd-178">Nenhum atributo é aplicável ao elemento **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-179">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-179">Example</span></span>

<span data-ttu-id="16dfd-180">O exemplo a seguir mostra um elemento **Function** com um elemento de **CommandText** filho.</span><span class="sxs-lookup"><span data-stu-id="16dfd-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="16dfd-181">Expor a função **UpdateProductInOrder** como um método no ObjectContext importando-o para o modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="16dfd-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="16dfd-182">Elemento DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="16dfd-183">O elemento **DefiningQuery** na linguagem de definição de esquema de repositório (SSDL) permite executar uma instrução SQL diretamente no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="16dfd-184">O elemento **DefiningQuery** é comumente usado como uma exibição de banco de dados, mas a exibição é definida no modelo de armazenamento em vez do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="16dfd-185">O modo de exibição definido em um elemento **DefiningQuery** pode ser mapeado para um tipo de entidade no modelo conceitual por meio de um elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="16dfd-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="16dfd-186">Esses mapeamentos são somente leitura.</span><span class="sxs-lookup"><span data-stu-id="16dfd-186">These mappings are read-only.</span></span>  

<span data-ttu-id="16dfd-187">A sintaxe SSDL a seguir mostra a declaração de um **EntitySet** seguido pelo elemento **DefiningQuery** que contém uma consulta usada para recuperar a exibição.</span><span class="sxs-lookup"><span data-stu-id="16dfd-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="16dfd-188">Você pode usar procedimentos armazenados no Entity Framework para habilitar cenários de leitura/gravação em exibições.</span><span class="sxs-lookup"><span data-stu-id="16dfd-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="16dfd-189"> Você pode usar uma exibição da fonte de dados ou uma exibição Entity SQL como a tabela base para recuperar dados e para o processamento de alterações por procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="16dfd-190">Você pode usar o elemento **DefiningQuery** para direcionar Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="16dfd-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="16dfd-191">Embora SQL Server Compact 3,5 não ofereça suporte a procedimentos armazenados, você pode implementar uma funcionalidade semelhante com o elemento **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="16dfd-192">Outro lugar onde pode ser útil é a criação de procedimentos armazenados para superar uma incompatibilidade entre os tipos de dados usados na linguagem de programação e os da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="16dfd-193">Você poderia escrever um **DefiningQuery** que usa um determinado conjunto de parâmetros e, em seguida, chama um procedimento armazenado com um conjunto diferente de parâmetros, por exemplo, um procedimento armazenado que exclui dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="16dfd-194">Elemento dependente (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="16dfd-195">O elemento **dependente** na linguagem de definição de esquema de repositório (SSDL) é um elemento filho para o elemento ReferentialConstraint que define a extremidade dependente de uma restrição FOREIGN KEY (também chamada de restrição referencial).</span><span class="sxs-lookup"><span data-stu-id="16dfd-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="16dfd-196">O elemento **dependente** especifica a coluna (ou colunas) em uma tabela que faz referência a uma coluna de chave primária (ou colunas).</span><span class="sxs-lookup"><span data-stu-id="16dfd-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="16dfd-197">Os elementos **PropertyRef** especificam quais colunas são referenciadas.</span><span class="sxs-lookup"><span data-stu-id="16dfd-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="16dfd-198">O elemento principal especifica as colunas de chave primária que são referenciadas por colunas que são especificadas no elemento **dependente** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="16dfd-199">O elemento **dependente** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-200">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="16dfd-201">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-202">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-202">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-203">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **dependente** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="16dfd-204">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-204">Attribute Name</span></span> | <span data-ttu-id="16dfd-205">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-205">Is Required</span></span> | <span data-ttu-id="16dfd-206">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-207">**Função**</span><span class="sxs-lookup"><span data-stu-id="16dfd-207">**Role**</span></span>       | <span data-ttu-id="16dfd-208">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-208">Yes</span></span>         | <span data-ttu-id="16dfd-209">O mesmo valor que o atributo de **função** (se usado) do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna de referência.</span><span class="sxs-lookup"><span data-stu-id="16dfd-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-210">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **dependente** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="16dfd-211">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="16dfd-212">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-213">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-213">Example</span></span>

<span data-ttu-id="16dfd-214">O exemplo a seguir mostra um elemento Association que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira de **CustomerOrders de CE\_** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="16dfd-215">O elemento **dependente** especifica a coluna **CustomerID** da tabela **Order** como a extremidade dependente da restrição.</span><span class="sxs-lookup"><span data-stu-id="16dfd-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="16dfd-216">Elemento Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="16dfd-217">O elemento **Documentation** na Store (linguagem de definição de esquema de repositório) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai.</span><span class="sxs-lookup"><span data-stu-id="16dfd-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="16dfd-218">O elemento de **documentação** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-219">**Resumo**: uma breve descrição do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="16dfd-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="16dfd-220">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="16dfd-220">(zero or one element)</span></span>
-   <span data-ttu-id="16dfd-221">**LongDescription**: uma descrição extensiva do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="16dfd-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="16dfd-222">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="16dfd-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-223">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-223">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-224">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **documentação** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="16dfd-225">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="16dfd-226">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-227">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-227">Example</span></span>

<span data-ttu-id="16dfd-228">O exemplo a seguir mostra o elemento de **documentação** como um elemento filho de um elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="16dfd-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="16dfd-229">Elemento final (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-229">End Element (SSDL)</span></span>

<span data-ttu-id="16dfd-230">O elemento **final** na Store de linguagem de definição de esquema (SSDL) especifica a tabela e o número de linhas em uma extremidade de uma restrição de chave estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="16dfd-231">O elemento **end** pode ser filho do elemento Association ou do elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="16dfd-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="16dfd-232">Em cada caso, os possíveis elementos filho e os atributos aplicáveis são diferentes.</span><span class="sxs-lookup"><span data-stu-id="16dfd-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="16dfd-233">Elemento end como um filho do elemento Association</span><span class="sxs-lookup"><span data-stu-id="16dfd-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="16dfd-234">Um elemento **final** (como um filho do elemento **Association** ) especifica a tabela e o número de linhas no final de uma restrição FOREIGN KEY com os atributos **Type** e **multiplicidade** , respectivamente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="16dfd-235">As extremidades de uma restrição FOREIGN KEY são definidas como parte de uma associação SSDL; uma associação SSDL deve ter exatamente duas extremidades.</span><span class="sxs-lookup"><span data-stu-id="16dfd-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="16dfd-236">Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-237">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="16dfd-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="16dfd-238">OnDelete (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="16dfd-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="16dfd-239">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="16dfd-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-240">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-240">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-241">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="16dfd-242">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-242">Attribute Name</span></span>   | <span data-ttu-id="16dfd-243">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-243">Is Required</span></span> | <span data-ttu-id="16dfd-244">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-245">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="16dfd-245">**Type**</span></span>         | <span data-ttu-id="16dfd-246">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-246">Yes</span></span>         | <span data-ttu-id="16dfd-247">O nome totalmente qualificado do conjunto de entidades SSDL que está no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16dfd-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="16dfd-248">**Função**</span><span class="sxs-lookup"><span data-stu-id="16dfd-248">**Role**</span></span>         | <span data-ttu-id="16dfd-249">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-249">No</span></span>          | <span data-ttu-id="16dfd-250">O valor do atributo **role** no elemento principal ou dependente do elemento ReferentialConstraint correspondente (se usado).</span><span class="sxs-lookup"><span data-stu-id="16dfd-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="16dfd-251">**Multiplicidade**</span><span class="sxs-lookup"><span data-stu-id="16dfd-251">**Multiplicity**</span></span> | <span data-ttu-id="16dfd-252">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-252">Yes</span></span>         | <span data-ttu-id="16dfd-253">**1**, **0.. 1**ou **\*** dependendo do número de linhas que podem estar no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16dfd-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="16dfd-254">**1** indica que exatamente uma linha existe no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16dfd-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="16dfd-255">**0.. 1** indica que zero ou uma linha existe no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16dfd-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="16dfd-256">**\*** indica que zero, uma ou mais linhas existem no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16dfd-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-257">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="16dfd-258">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="16dfd-259">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="16dfd-260">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-260">Example</span></span>

<span data-ttu-id="16dfd-261">O exemplo a seguir mostra um elemento **Association** que define a restrição de chave estrangeira **\_CustomerOrders de FK** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="16dfd-262">Os valores de **multiplicidade** especificados em cada elemento de **extremidade** indicam que muitas linhas na tabela **Orders** podem ser associadas a uma linha na tabela **Customers** , mas apenas uma linha na tabela **Customers** pode ser associada a uma linha na tabela **Orders** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="16dfd-263">Além disso, o elemento **OnDelete** indica que todas as linhas na tabela **Orders** que fazem referência a uma linha específica na tabela **Customers** serão excluídas se a linha na tabela **Customers** for excluída.</span><span class="sxs-lookup"><span data-stu-id="16dfd-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="16dfd-264">Elemento end como um filho do elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="16dfd-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="16dfd-265">O elemento **end** (como um filho do elemento **AssociationSet** ) especifica uma tabela em uma extremidade de uma restrição FOREIGN KEY no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="16dfd-266">Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-267">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="16dfd-268">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-269">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-269">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-270">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="16dfd-271">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-271">Attribute Name</span></span> | <span data-ttu-id="16dfd-272">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-272">Is Required</span></span> | <span data-ttu-id="16dfd-273">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="16dfd-274">**EntitySet**</span></span>  | <span data-ttu-id="16dfd-275">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-275">Yes</span></span>         | <span data-ttu-id="16dfd-276">O nome do conjunto de entidades SSDL que está no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="16dfd-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="16dfd-277">**Função**</span><span class="sxs-lookup"><span data-stu-id="16dfd-277">**Role**</span></span>       | <span data-ttu-id="16dfd-278">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-278">No</span></span>          | <span data-ttu-id="16dfd-279">O valor de um dos atributos de **função** especificados em um elemento **end** do elemento Association correspondente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-280">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="16dfd-281">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="16dfd-282">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="16dfd-283">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-283">Example</span></span>

<span data-ttu-id="16dfd-284">O exemplo a seguir mostra um elemento **EntityContainer** com um elemento **AssociationSet** com dois elementos **end** :</span><span class="sxs-lookup"><span data-stu-id="16dfd-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="16dfd-285">Elemento EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="16dfd-286">Um elemento **EntityContainer** na Store (linguagem de definição de esquema) de repositório descreve a estrutura da fonte de dados subjacente em um aplicativo Entity Framework: conjuntos de entidades SSDL (definidos em elementos EntitySet) representam tabelas em um banco de dado, tipos de entidade SSDL (definidos em elementos EntityType) representam linhas em uma tabela e os conjuntos de associação (definidos em elementos AssociationSet) representam restrições Foreign Key em um banco de</span><span class="sxs-lookup"><span data-stu-id="16dfd-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="16dfd-287">Um contêiner de entidade de modelo de armazenamento é mapeado para um contêiner de entidade de modelo conceitual por meio do elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="16dfd-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="16dfd-288">Um elemento **EntityContainer** pode ter zero ou um elementos de documentação.</span><span class="sxs-lookup"><span data-stu-id="16dfd-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="16dfd-289">Se um elemento de **documentação** estiver presente, ele deverá preceder todos os outros elementos filho.</span><span class="sxs-lookup"><span data-stu-id="16dfd-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="16dfd-290">Um elemento **EntityContainer** pode ter zero ou mais dos seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="16dfd-291">EntitySet</span></span>
-   <span data-ttu-id="16dfd-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="16dfd-292">AssociationSet</span></span>
-   <span data-ttu-id="16dfd-293">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="16dfd-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-294">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-294">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-295">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="16dfd-296">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-296">Attribute Name</span></span> | <span data-ttu-id="16dfd-297">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-297">Is Required</span></span> | <span data-ttu-id="16dfd-298">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-299">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-299">**Name**</span></span>       | <span data-ttu-id="16dfd-300">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-300">Yes</span></span>         | <span data-ttu-id="16dfd-301">O nome do contêiner de entidade.</span><span class="sxs-lookup"><span data-stu-id="16dfd-301">The name of the entity container.</span></span> <span data-ttu-id="16dfd-302">Este nome não pode conter pontos (.).</span><span class="sxs-lookup"><span data-stu-id="16dfd-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-303">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="16dfd-304">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-305">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-306">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-306">Example</span></span>

<span data-ttu-id="16dfd-307">O exemplo a seguir mostra um elemento **EntityContainer** que define dois conjuntos de entidades e um conjunto de associação.</span><span class="sxs-lookup"><span data-stu-id="16dfd-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="16dfd-308">Observe que o tipo de entidade e os nomes de tipo de associação são qualificados pelo nome do namespace do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="16dfd-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="16dfd-309">Elemento EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="16dfd-310">Um elemento **EntitySet** na linguagem de definição de esquema de repositório (SSDL) representa uma tabela ou exibição no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="16dfd-311">Um elemento EntityType em SSDL representa uma linha na tabela ou exibição.</span><span class="sxs-lookup"><span data-stu-id="16dfd-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="16dfd-312">O atributo **EntityType** de um elemento **EntitySet** especifica o tipo de entidade SSDL específico que representa linhas em um conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="16dfd-313">O mapeamento entre um conjunto de entidades CSDL e um conjunto de entidades SSDL é especificado em um elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="16dfd-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="16dfd-314">O elemento **EntitySet** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-315">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="16dfd-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="16dfd-316">DefiningQuery (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="16dfd-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="16dfd-317">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="16dfd-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-318">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-318">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-319">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="16dfd-320">Alguns atributos (não listados aqui) podem ser qualificados com o alias da **loja** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="16dfd-321">Esses atributos são usados pelo assistente de modelo de atualização ao atualizar um modelo.</span><span class="sxs-lookup"><span data-stu-id="16dfd-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="16dfd-322">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-322">Attribute Name</span></span> | <span data-ttu-id="16dfd-323">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-323">Is Required</span></span> | <span data-ttu-id="16dfd-324">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-325">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-325">**Name**</span></span>       | <span data-ttu-id="16dfd-326">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-326">Yes</span></span>         | <span data-ttu-id="16dfd-327">O nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="16dfd-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="16dfd-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="16dfd-328">**EntityType**</span></span> | <span data-ttu-id="16dfd-329">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-329">Yes</span></span>         | <span data-ttu-id="16dfd-330">O nome totalmente qualificado do tipo de entidade para o qual o conjunto de entidades contém instâncias.</span><span class="sxs-lookup"><span data-stu-id="16dfd-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="16dfd-331">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="16dfd-331">**Schema**</span></span>     | <span data-ttu-id="16dfd-332">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-332">No</span></span>          | <span data-ttu-id="16dfd-333">O esquema do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="16dfd-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="16dfd-334">**Table**</span></span>      | <span data-ttu-id="16dfd-335">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-335">No</span></span>          | <span data-ttu-id="16dfd-336">A tabela do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="16dfd-337">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="16dfd-338">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-339">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-340">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-340">Example</span></span>

<span data-ttu-id="16dfd-341">O exemplo a seguir mostra um elemento **EntityContainer** que tem dois elementos **EntitySet** e um elemento **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="16dfd-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="16dfd-342">Elemento EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="16dfd-343">Um elemento **EntityType** na linguagem de definição de esquema de repositório (SSDL) representa uma linha em uma tabela ou exibição do banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="16dfd-344">Um elemento EntitySet em SSDL representa a tabela ou exibição na qual as linhas ocorrem.</span><span class="sxs-lookup"><span data-stu-id="16dfd-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="16dfd-345">O atributo **EntityType** de um elemento **EntitySet** especifica o tipo de entidade SSDL específico que representa linhas em um conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="16dfd-346">O mapeamento entre um tipo de entidade SSDL e um tipo de entidade CSDL é especificado em um elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="16dfd-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="16dfd-347">O elemento **EntityType** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-348">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="16dfd-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="16dfd-349">Chave (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="16dfd-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="16dfd-350">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="16dfd-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-351">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-351">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-352">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="16dfd-353">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-353">Attribute Name</span></span> | <span data-ttu-id="16dfd-354">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-354">Is Required</span></span> | <span data-ttu-id="16dfd-355">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-356">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-356">**Name**</span></span>       | <span data-ttu-id="16dfd-357">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-357">Yes</span></span>         | <span data-ttu-id="16dfd-358">O nome do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="16dfd-358">The name of the entity type.</span></span> <span data-ttu-id="16dfd-359">Esse valor é geralmente o mesmo que o nome da tabela na qual o tipo de entidade representa uma linha.</span><span class="sxs-lookup"><span data-stu-id="16dfd-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="16dfd-360">Esse valor não pode conter pontos (.).</span><span class="sxs-lookup"><span data-stu-id="16dfd-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-361">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="16dfd-362">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-363">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-364">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-364">Example</span></span>

<span data-ttu-id="16dfd-365">O exemplo a seguir mostra um elemento **EntityType** com duas propriedades:</span><span class="sxs-lookup"><span data-stu-id="16dfd-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="16dfd-366">Elemento Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-366">Function Element (SSDL)</span></span>

<span data-ttu-id="16dfd-367">O elemento **Function** na linguagem de definição de esquema de repositório (SSDL) especifica um procedimento armazenado que existe no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="16dfd-368">O elemento **Function** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-369">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="16dfd-370">Parâmetro (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="16dfd-371">CommandText (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="16dfd-372">ReturnType (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="16dfd-373">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="16dfd-374">Um tipo de retorno para uma função deve ser especificado com o elemento **ReturnType** ou o atributo **ReturnType** (veja abaixo), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="16dfd-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="16dfd-375">Os procedimentos armazenados que são especificados no modelo de armazenamento podem ser importados para o modelo conceitual de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16dfd-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="16dfd-376">Para obter mais informações, consulte [consultando com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="16dfd-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="16dfd-377">O elemento **Function** também pode ser usado para definir funções personalizadas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-378">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-378">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-379">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="16dfd-380">Alguns atributos (não listados aqui) podem ser qualificados com o alias da **loja** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="16dfd-381">Esses atributos são usados pelo assistente de modelo de atualização ao atualizar um modelo.</span><span class="sxs-lookup"><span data-stu-id="16dfd-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="16dfd-382">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-382">Attribute Name</span></span>             | <span data-ttu-id="16dfd-383">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-383">Is Required</span></span> | <span data-ttu-id="16dfd-384">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-385">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-385">**Name**</span></span>                   | <span data-ttu-id="16dfd-386">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-386">Yes</span></span>         | <span data-ttu-id="16dfd-387">O nome do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="16dfd-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="16dfd-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="16dfd-388">**ReturnType**</span></span>             | <span data-ttu-id="16dfd-389">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-389">No</span></span>          | <span data-ttu-id="16dfd-390">O tipo de retorno do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="16dfd-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="16dfd-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="16dfd-391">**Aggregate**</span></span>              | <span data-ttu-id="16dfd-392">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-392">No</span></span>          | <span data-ttu-id="16dfd-393">**True** se o procedimento armazenado retornar um valor de agregação; caso contrário, **false**.</span><span class="sxs-lookup"><span data-stu-id="16dfd-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="16dfd-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="16dfd-394">**BuiltIn**</span></span>                | <span data-ttu-id="16dfd-395">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-395">No</span></span>          | <span data-ttu-id="16dfd-396">**True** se a função for uma função interna<sup>1</sup> ; caso contrário, **false**.</span><span class="sxs-lookup"><span data-stu-id="16dfd-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="16dfd-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="16dfd-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="16dfd-398">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-398">No</span></span>          | <span data-ttu-id="16dfd-399">O nome do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="16dfd-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="16dfd-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="16dfd-400">**NiladicFunction**</span></span>        | <span data-ttu-id="16dfd-401">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-401">No</span></span>          | <span data-ttu-id="16dfd-402">**True** se a função for uma função niladic<sup>2</sup> ; Caso contrário, **false** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="16dfd-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="16dfd-403">**IsComposable**</span></span>           | <span data-ttu-id="16dfd-404">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-404">No</span></span>          | <span data-ttu-id="16dfd-405">**True** se a função for uma função combinável<sup>3</sup> ; Caso contrário, **false** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="16dfd-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="16dfd-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="16dfd-407">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-407">No</span></span>          | <span data-ttu-id="16dfd-408">A enumeração que define a semântica de tipo usada para resolver sobrecargas de função.</span><span class="sxs-lookup"><span data-stu-id="16dfd-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="16dfd-409">A enumeração é definida no manifesto do provedor por definição de função.</span><span class="sxs-lookup"><span data-stu-id="16dfd-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="16dfd-410">O valor padrão é **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="16dfd-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="16dfd-411">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="16dfd-411">**Schema**</span></span>                 | <span data-ttu-id="16dfd-412">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-412">No</span></span>          | <span data-ttu-id="16dfd-413">O nome do esquema no qual o procedimento armazenado é definido.</span><span class="sxs-lookup"><span data-stu-id="16dfd-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="16dfd-414"><sup>1</sup> uma função interna é uma função que é definida no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="16dfd-415">Para obter informações sobre as funções definidas no modelo de armazenamento, consulte elemento CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="16dfd-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="16dfd-416"><sup>2</sup> uma função niladic é uma função que não aceita parâmetros e, quando chamado, não requer parênteses.</span><span class="sxs-lookup"><span data-stu-id="16dfd-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="16dfd-417"><sup>3</sup> duas funções são combináveis se a saída de uma função pode ser a entrada para a outra função.</span><span class="sxs-lookup"><span data-stu-id="16dfd-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="16dfd-418">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="16dfd-419">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-420">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-421">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-421">Example</span></span>

<span data-ttu-id="16dfd-422">O exemplo a seguir mostra um elemento **Function** que corresponde ao procedimento armazenado **UpdateOrderQuantity** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="16dfd-423">O procedimento armazenado aceita dois parâmetros e não retorna um valor.</span><span class="sxs-lookup"><span data-stu-id="16dfd-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="16dfd-424">Elemento Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-424">Key Element (SSDL)</span></span>

<span data-ttu-id="16dfd-425">O elemento de **chave** na linguagem de definição de esquema de repositório (SSDL) representa a chave primária de uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="16dfd-426">**Key** é um elemento filho de um elemento EntityType, que representa uma linha em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="16dfd-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="16dfd-427">A chave primária é definida no elemento de **chave** referenciando um ou mais elementos de propriedade que são definidos no elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="16dfd-428">O elemento **Key** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-429">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="16dfd-430">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="16dfd-430">Annotation elements</span></span>

<span data-ttu-id="16dfd-431">Nenhum atributo é aplicável ao elemento de **chave** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-432">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-432">Example</span></span>

<span data-ttu-id="16dfd-433">O exemplo a seguir mostra um elemento **EntityType** com uma chave que faz referência a uma propriedade:</span><span class="sxs-lookup"><span data-stu-id="16dfd-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="16dfd-434">Elemento ondelery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="16dfd-435">O elemento **OnDelete** na Store (linguagem de definição de esquema) do repositório reflete o comportamento do banco de dados quando uma linha que participa de uma restrição de chave estrangeira é excluída.</span><span class="sxs-lookup"><span data-stu-id="16dfd-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="16dfd-436">Se a ação for definida como **Cascade**, as linhas que fazem referência a uma linha que está sendo excluída também serão excluídas.</span><span class="sxs-lookup"><span data-stu-id="16dfd-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="16dfd-437">Se a ação for definida como **nenhuma**, as linhas que fazem referência a uma linha que está sendo excluída também não serão excluídas.</span><span class="sxs-lookup"><span data-stu-id="16dfd-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="16dfd-438">Um elemento **OnDelete** é um elemento filho de um elemento end.</span><span class="sxs-lookup"><span data-stu-id="16dfd-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="16dfd-439">Um elemento **OnDelete** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-440">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="16dfd-441">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-442">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-442">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-443">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="16dfd-444">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-444">Attribute Name</span></span> | <span data-ttu-id="16dfd-445">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-445">Is Required</span></span> | <span data-ttu-id="16dfd-446">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-447">**Ação**</span><span class="sxs-lookup"><span data-stu-id="16dfd-447">**Action**</span></span>     | <span data-ttu-id="16dfd-448">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-448">Yes</span></span>         | <span data-ttu-id="16dfd-449">**Cascade** ou **None**.</span><span class="sxs-lookup"><span data-stu-id="16dfd-449">**Cascade** or **None**.</span></span> <span data-ttu-id="16dfd-450">(O valor **restrito** é válido, mas tem o mesmo comportamento que **nenhum**.)</span><span class="sxs-lookup"><span data-stu-id="16dfd-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-451">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="16dfd-452">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-453">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-454">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-454">Example</span></span>

<span data-ttu-id="16dfd-455">O exemplo a seguir mostra um elemento **Association** que define a restrição de chave estrangeira **\_CustomerOrders de FK** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="16dfd-456">O elemento **OnDelete** indica que todas as linhas na tabela **Orders** que fazem referência a uma linha específica na tabela **Customers** serão excluídas se a linha na tabela **Customers** for excluída.</span><span class="sxs-lookup"><span data-stu-id="16dfd-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="16dfd-457">Elemento Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="16dfd-458">O elemento **Parameter** na linguagem de definição de esquema de repositório (SSDL) é um filho do elemento Function que especifica parâmetros para um procedimento armazenado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="16dfd-459">O elemento **Parameter** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-460">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="16dfd-461">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-462">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-462">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-463">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="16dfd-464">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-464">Attribute Name</span></span> | <span data-ttu-id="16dfd-465">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-465">Is Required</span></span> | <span data-ttu-id="16dfd-466">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-467">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-467">**Name**</span></span>       | <span data-ttu-id="16dfd-468">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-468">Yes</span></span>         | <span data-ttu-id="16dfd-469">O nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="16dfd-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="16dfd-470">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="16dfd-470">**Type**</span></span>       | <span data-ttu-id="16dfd-471">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-471">Yes</span></span>         | <span data-ttu-id="16dfd-472">O tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="16dfd-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="16dfd-473">**Modo**</span><span class="sxs-lookup"><span data-stu-id="16dfd-473">**Mode**</span></span>       | <span data-ttu-id="16dfd-474">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-474">No</span></span>          | <span data-ttu-id="16dfd-475">**In**, **out**ou **Inout** , dependendo se o parâmetro é um parâmetro de entrada, saída ou entrada/saída.</span><span class="sxs-lookup"><span data-stu-id="16dfd-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="16dfd-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="16dfd-476">**MaxLength**</span></span>  | <span data-ttu-id="16dfd-477">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-477">No</span></span>          | <span data-ttu-id="16dfd-478">O comprimento máximo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="16dfd-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="16dfd-479">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="16dfd-479">**Precision**</span></span>  | <span data-ttu-id="16dfd-480">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-480">No</span></span>          | <span data-ttu-id="16dfd-481">A precisão do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="16dfd-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="16dfd-482">**Escala**</span><span class="sxs-lookup"><span data-stu-id="16dfd-482">**Scale**</span></span>      | <span data-ttu-id="16dfd-483">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-483">No</span></span>          | <span data-ttu-id="16dfd-484">A escala do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="16dfd-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="16dfd-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="16dfd-485">**SRID**</span></span>       | <span data-ttu-id="16dfd-486">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-486">No</span></span>          | <span data-ttu-id="16dfd-487">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="16dfd-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="16dfd-488">Válido somente para parâmetros de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="16dfd-489">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dfd-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-490">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="16dfd-491">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-492">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-493">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-493">Example</span></span>

<span data-ttu-id="16dfd-494">O exemplo a seguir mostra um elemento **Function** que tem dois elementos de **parâmetro** que especificam parâmetros de entrada:</span><span class="sxs-lookup"><span data-stu-id="16dfd-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="16dfd-495">Elemento principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="16dfd-496">O elemento **principal** no SSDL (Store Schema Definition Language) é um elemento filho do elemento ReferentialConstraint que define a extremidade principal de uma restrição FOREIGN KEY (também chamada de restrição referencial).</span><span class="sxs-lookup"><span data-stu-id="16dfd-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="16dfd-497">O elemento **principal** especifica a coluna de chave primária (ou colunas) em uma tabela que é referenciada por outra coluna (ou colunas).</span><span class="sxs-lookup"><span data-stu-id="16dfd-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="16dfd-498">Os elementos **PropertyRef** especificam quais colunas são referenciadas.</span><span class="sxs-lookup"><span data-stu-id="16dfd-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="16dfd-499">O elemento dependente especifica colunas que referenciam as colunas de chave primária que são especificadas no elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="16dfd-500">O elemento **principal** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="16dfd-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="16dfd-501">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="16dfd-502">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-503">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-503">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-504">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="16dfd-505">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-505">Attribute Name</span></span> | <span data-ttu-id="16dfd-506">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-506">Is Required</span></span> | <span data-ttu-id="16dfd-507">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-508">**Função**</span><span class="sxs-lookup"><span data-stu-id="16dfd-508">**Role**</span></span>       | <span data-ttu-id="16dfd-509">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-509">Yes</span></span>         | <span data-ttu-id="16dfd-510">O mesmo valor que o atributo de **função** (se usado) do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna referenciada.</span><span class="sxs-lookup"><span data-stu-id="16dfd-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-511">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="16dfd-512">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="16dfd-513">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-514">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-514">Example</span></span>

<span data-ttu-id="16dfd-515">O exemplo a seguir mostra um elemento Association que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira de **CustomerOrders de CE\_** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="16dfd-516">O elemento **principal** especifica a coluna **CustomerID** da tabela **Customer** como a extremidade principal da restrição.</span><span class="sxs-lookup"><span data-stu-id="16dfd-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="16dfd-517">Elemento Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-517">Property Element (SSDL)</span></span>

<span data-ttu-id="16dfd-518">O elemento de **Propriedade** na linguagem de definição de esquema de repositório (SSDL) representa uma coluna em uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="16dfd-519">Elementos de **Propriedade** são filhos de elementos EntityType, que representam linhas em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="16dfd-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="16dfd-520">Cada elemento de **Propriedade** definido em um elemento **EntityType** representa uma coluna.</span><span class="sxs-lookup"><span data-stu-id="16dfd-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="16dfd-521">Um elemento de **Propriedade** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="16dfd-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-522">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-522">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-523">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="16dfd-524">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-524">Attribute Name</span></span>            | <span data-ttu-id="16dfd-525">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-525">Is Required</span></span> | <span data-ttu-id="16dfd-526">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-527">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-527">**Name**</span></span>                  | <span data-ttu-id="16dfd-528">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-528">Yes</span></span>         | <span data-ttu-id="16dfd-529">O nome da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="16dfd-530">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="16dfd-530">**Type**</span></span>                  | <span data-ttu-id="16dfd-531">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-531">Yes</span></span>         | <span data-ttu-id="16dfd-532">O tipo da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="16dfd-533">**Permite valor nulo**</span><span class="sxs-lookup"><span data-stu-id="16dfd-533">**Nullable**</span></span>              | <span data-ttu-id="16dfd-534">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-534">No</span></span>          | <span data-ttu-id="16dfd-535">**True** (o valor padrão) ou **false** , dependendo se a coluna correspondente pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="16dfd-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="16dfd-536">**ValorPadrão**</span><span class="sxs-lookup"><span data-stu-id="16dfd-536">**DefaultValue**</span></span>          | <span data-ttu-id="16dfd-537">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-537">No</span></span>          | <span data-ttu-id="16dfd-538">O valor padrão da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="16dfd-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="16dfd-539">**MaxLength**</span></span>             | <span data-ttu-id="16dfd-540">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-540">No</span></span>          | <span data-ttu-id="16dfd-541">O comprimento máximo da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="16dfd-542">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="16dfd-542">**FixedLength**</span></span>           | <span data-ttu-id="16dfd-543">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-543">No</span></span>          | <span data-ttu-id="16dfd-544">**True** ou **false** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="16dfd-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="16dfd-545">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="16dfd-545">**Precision**</span></span>             | <span data-ttu-id="16dfd-546">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-546">No</span></span>          | <span data-ttu-id="16dfd-547">A precisão da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="16dfd-548">**Escala**</span><span class="sxs-lookup"><span data-stu-id="16dfd-548">**Scale**</span></span>                 | <span data-ttu-id="16dfd-549">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-549">No</span></span>          | <span data-ttu-id="16dfd-550">A escala da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="16dfd-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="16dfd-551">**Unicode**</span></span>               | <span data-ttu-id="16dfd-552">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-552">No</span></span>          | <span data-ttu-id="16dfd-553">**True** ou **false** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="16dfd-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="16dfd-554">**Ordenação**</span><span class="sxs-lookup"><span data-stu-id="16dfd-554">**Collation**</span></span>             | <span data-ttu-id="16dfd-555">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-555">No</span></span>          | <span data-ttu-id="16dfd-556">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="16dfd-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="16dfd-557">**SRID**</span></span>                  | <span data-ttu-id="16dfd-558">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-558">No</span></span>          | <span data-ttu-id="16dfd-559">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="16dfd-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="16dfd-560">Válido somente para propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="16dfd-561">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dfd-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="16dfd-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="16dfd-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="16dfd-563">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-563">No</span></span>          | <span data-ttu-id="16dfd-564">**None**, **Identity** (se o valor da coluna correspondente for uma identidade gerada no banco de dados) ou **computado** (se o valor da coluna correspondente for computado no banco de dados).</span><span class="sxs-lookup"><span data-stu-id="16dfd-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="16dfd-565">Não é válido para propriedades RowType.</span><span class="sxs-lookup"><span data-stu-id="16dfd-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-566">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="16dfd-567">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-568">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-569">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-569">Example</span></span>

<span data-ttu-id="16dfd-570">O exemplo a seguir mostra um elemento **EntityType** com dois elementos de **Propriedade** filho:</span><span class="sxs-lookup"><span data-stu-id="16dfd-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="16dfd-571">Elemento PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="16dfd-572">O elemento **PropertyRef** na linguagem de definição de esquema de repositório (SSDL) faz referência a uma propriedade definida em um elemento EntityType para indicar que a propriedade executará uma das seguintes funções:</span><span class="sxs-lookup"><span data-stu-id="16dfd-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="16dfd-573">Faça parte da chave primária da tabela que o **EntityType** representa.</span><span class="sxs-lookup"><span data-stu-id="16dfd-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="16dfd-574">Um ou mais elementos **PropertyRef** podem ser usados para definir uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="16dfd-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="16dfd-575">Para obter mais informações, consulte Key Element.</span><span class="sxs-lookup"><span data-stu-id="16dfd-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="16dfd-576">Ser a extremidade dependente ou de entidade de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="16dfd-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="16dfd-577">Para obter mais informações, consulte elemento ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="16dfd-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="16dfd-578">O elemento **PropertyRef** só pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="16dfd-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="16dfd-579">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="16dfd-580">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="16dfd-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-581">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-581">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-582">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="16dfd-583">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-583">Attribute Name</span></span> | <span data-ttu-id="16dfd-584">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-584">Is Required</span></span> | <span data-ttu-id="16dfd-585">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="16dfd-586">**Nome**</span><span class="sxs-lookup"><span data-stu-id="16dfd-586">**Name**</span></span>       | <span data-ttu-id="16dfd-587">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-587">Yes</span></span>         | <span data-ttu-id="16dfd-588">O nome da propriedade referenciada.</span><span class="sxs-lookup"><span data-stu-id="16dfd-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="16dfd-589">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="16dfd-590">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="16dfd-591">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-592">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-592">Example</span></span>

<span data-ttu-id="16dfd-593">O exemplo a seguir mostra um elemento **PropertyRef** usado para definir uma chave primária referenciando uma propriedade que é definida em um elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="16dfd-594">Elemento ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="16dfd-595">O elemento **ReferentialConstraint** na linguagem de definição de esquema de repositório (SSDL) representa uma restrição FOREIGN KEY (também chamada de restrição de integridade referencial) no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="16dfd-596">As extremidades principais e dependentes da restrição são especificadas pelos elementos filho principal e dependente, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="16dfd-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="16dfd-597">As colunas que participam das extremidades principal e dependente são referenciadas com elementos PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="16dfd-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="16dfd-598">O elemento **ReferentialConstraint** é um elemento filho opcional do elemento Association.</span><span class="sxs-lookup"><span data-stu-id="16dfd-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="16dfd-599">Se um elemento **ReferentialConstraint** não for usado para mapear a restrição FOREIGN KEY especificada no elemento **Association** , um elemento AssociationSetMapping deverá ser usado para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="16dfd-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="16dfd-600">O elemento **ReferentialConstraint** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="16dfd-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="16dfd-601">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="16dfd-602">Entidade de segurança (exatamente uma)</span><span class="sxs-lookup"><span data-stu-id="16dfd-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="16dfd-603">Dependente (exatamente um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="16dfd-604">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-605">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-605">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-606">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="16dfd-607">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-608">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-609">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-609">Example</span></span>

<span data-ttu-id="16dfd-610">O exemplo a seguir mostra um elemento **Association** que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira de **CustomerOrders de CE\_** :</span><span class="sxs-lookup"><span data-stu-id="16dfd-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="16dfd-611">Elemento ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="16dfd-612">O elemento **ReturnType** na linguagem de definição de esquema de repositório (SSDL) especifica o tipo de retorno para uma função que é definida em um elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="16dfd-613">Um tipo de retorno de função também pode ser especificado com um atributo **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="16dfd-614">O tipo de retorno de uma função é especificado com o atributo **Type** ou o elemento **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="16dfd-615">O elemento **ReturnType** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="16dfd-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="16dfd-616">CollectionType (um)</span><span class="sxs-lookup"><span data-stu-id="16dfd-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="16dfd-617">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="16dfd-618">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="16dfd-619">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-620">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-620">Example</span></span>

<span data-ttu-id="16dfd-621">O exemplo a seguir usa uma **função** que retorna uma coleção de linhas.</span><span class="sxs-lookup"><span data-stu-id="16dfd-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="16dfd-622">Elemento RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="16dfd-623">Um elemento **RowType** na linguagem de definição de esquema de repositório (SSDL) define uma estrutura sem nome como um tipo de retorno para uma função definida no repositório.</span><span class="sxs-lookup"><span data-stu-id="16dfd-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="16dfd-624">Um elemento **RowType** é o elemento filho do elemento **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="16dfd-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="16dfd-625">Um elemento **RowType** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="16dfd-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="16dfd-626">Propriedade (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="16dfd-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="16dfd-627">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-627">Example</span></span>

<span data-ttu-id="16dfd-628">O exemplo a seguir mostra uma função de repositório que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas (conforme especificado no elemento **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="16dfd-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="16dfd-629">Elemento de esquema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="16dfd-630">O elemento de **esquema** na linguagem de definição de esquema de repositório (SSDL) é o elemento raiz de uma definição de modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="16dfd-631">Ele contém definições para os objetos, funções e contêineres que compõem um modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="16dfd-632">O elemento **Schema** pode conter zero ou mais dos seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="16dfd-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="16dfd-633">Associação</span><span class="sxs-lookup"><span data-stu-id="16dfd-633">Association</span></span>
-   <span data-ttu-id="16dfd-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="16dfd-634">EntityType</span></span>
-   <span data-ttu-id="16dfd-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="16dfd-635">EntityContainer</span></span>
-   <span data-ttu-id="16dfd-636">Função</span><span class="sxs-lookup"><span data-stu-id="16dfd-636">Function</span></span>

<span data-ttu-id="16dfd-637">O elemento **Schema** usa o atributo **namespace** para definir o namespace para o tipo de entidade e objetos de associação em um modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="16dfd-638">Em um namespace, dois objetos não podem ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="16dfd-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="16dfd-639">Um namespace do modelo de armazenamento é diferente do namespace XML do elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="16dfd-640">Um namespace de modelo de armazenamento (conforme definido pelo atributo **namespace** ) é um contêiner lógico para tipos de entidade e tipos de associação.</span><span class="sxs-lookup"><span data-stu-id="16dfd-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="16dfd-641">O namespace XML (indicado pelo atributo **xmlns** ) de um elemento **Schema** é o namespace padrão para elementos filho e atributos do elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="16dfd-642">Os namespaces XML do formulário https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (em que AAAA e MM representam um ano e um mês respectivamente) são reservados para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="16dfd-643">Atributos e elementos personalizados não podem estar em namespaces que tenham esse formulário.</span><span class="sxs-lookup"><span data-stu-id="16dfd-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="16dfd-644">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="16dfd-644">Applicable Attributes</span></span>

<span data-ttu-id="16dfd-645">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="16dfd-646">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="16dfd-646">Attribute Name</span></span>            | <span data-ttu-id="16dfd-647">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="16dfd-647">Is Required</span></span> | <span data-ttu-id="16dfd-648">Valor</span><span class="sxs-lookup"><span data-stu-id="16dfd-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="16dfd-649">**Namespace**</span></span>             | <span data-ttu-id="16dfd-650">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-650">Yes</span></span>         | <span data-ttu-id="16dfd-651">O namespace do modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-651">The namespace of the storage model.</span></span> <span data-ttu-id="16dfd-652">O valor do atributo **namespace** é usado para formar o nome totalmente qualificado de um tipo.</span><span class="sxs-lookup"><span data-stu-id="16dfd-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="16dfd-653">Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace ExampleModel. Store, o nome totalmente qualificado do **EntityType** será ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="16dfd-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="16dfd-654">As cadeias de caracteres a seguir não podem ser usadas como o valor do atributo de **namespace** : **System**, **transitório**ou **EDM**.</span><span class="sxs-lookup"><span data-stu-id="16dfd-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="16dfd-655">O valor do atributo **namespace** não pode ser o mesmo que o valor do atributo **namespace** no elemento de esquema CSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="16dfd-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="16dfd-656">**Alias**</span></span>                 | <span data-ttu-id="16dfd-657">Não</span><span class="sxs-lookup"><span data-stu-id="16dfd-657">No</span></span>          | <span data-ttu-id="16dfd-658">Um identificador usado no lugar do nome do namespace.</span><span class="sxs-lookup"><span data-stu-id="16dfd-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="16dfd-659">Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace ExampleModel. Store e o valor do atributo **alias** for *StorageModel*, você poderá usar StorageModel. Customer como o nome totalmente qualificado do **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="16dfd-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="16dfd-660">**Provedor**</span><span class="sxs-lookup"><span data-stu-id="16dfd-660">**Provider**</span></span>              | <span data-ttu-id="16dfd-661">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-661">Yes</span></span>         | <span data-ttu-id="16dfd-662">O provedor de dados.</span><span class="sxs-lookup"><span data-stu-id="16dfd-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="16dfd-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="16dfd-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="16dfd-664">Sim</span><span class="sxs-lookup"><span data-stu-id="16dfd-664">Yes</span></span>         | <span data-ttu-id="16dfd-665">Um token que indica ao provedor que o manifesto do provedor deve retornar.</span><span class="sxs-lookup"><span data-stu-id="16dfd-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="16dfd-666">Nenhum formato para o token está definido.</span><span class="sxs-lookup"><span data-stu-id="16dfd-666">No format for the token is defined.</span></span> <span data-ttu-id="16dfd-667">Os valores para o token são definidos pelo provedor.</span><span class="sxs-lookup"><span data-stu-id="16dfd-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="16dfd-668">Para obter informações sobre SQL Server tokens de manifesto do provedor, consulte SqlClient para Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dfd-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="16dfd-669">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-669">Example</span></span>

<span data-ttu-id="16dfd-670">O exemplo a seguir mostra um elemento de **esquema** que contém um elemento **EntityContainer** , dois elementos **EntityType** e um elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

## <a name="annotation-attributes"></a><span data-ttu-id="16dfd-671">Atributos de anotação</span><span class="sxs-lookup"><span data-stu-id="16dfd-671">Annotation Attributes</span></span>

<span data-ttu-id="16dfd-672">Os atributos de anotação no SSDL (Store Schema Definition Language) são atributos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre os elementos no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="16dfd-673">Além de ter uma estrutura XML válida, as seguintes restrições se aplicam aos atributos de anotação:</span><span class="sxs-lookup"><span data-stu-id="16dfd-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="16dfd-674">Os atributos de anotação não devem estar em nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="16dfd-675">Os nomes totalmente qualificados de quaisquer dois atributos de anotação não devem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="16dfd-676">Mais de um atributo Annotation pode ser aplicado a um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="16dfd-677">Os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="16dfd-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-678">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-678">Example</span></span>

<span data-ttu-id="16dfd-679">O exemplo a seguir mostra um elemento EntityType que tem um atributo Annotation aplicado à propriedade **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="16dfd-680">O exemplo também mostra um elemento Annotation adicionado ao elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="16dfd-681">Elementos de anotação (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="16dfd-682">Os elementos de anotação no SSDL (Store Schema Definition Language) são elementos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre o modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="16dfd-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="16dfd-683">Além de ter uma estrutura XML válida, as seguintes restrições se aplicam aos elementos de anotação:</span><span class="sxs-lookup"><span data-stu-id="16dfd-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="16dfd-684">Os elementos de anotação não devem estar em nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="16dfd-685">Os nomes totalmente qualificados de quaisquer dois elementos de anotação não devem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="16dfd-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="16dfd-686">Os elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="16dfd-687">Mais de um elemento Annotation pode ser um filho de um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="16dfd-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="16dfd-688">A partir do .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="16dfd-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="16dfd-689">Exemplo</span><span class="sxs-lookup"><span data-stu-id="16dfd-689">Example</span></span>

<span data-ttu-id="16dfd-690">O exemplo a seguir mostra um elemento EntityType que tem um elemento Annotation (**customelement**).</span><span class="sxs-lookup"><span data-stu-id="16dfd-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="16dfd-691">O exemplo também mostra um atributo Annotation aplicado à propriedade **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="16dfd-692">Facetas (SSDL)</span><span class="sxs-lookup"><span data-stu-id="16dfd-692">Facets (SSDL)</span></span>

<span data-ttu-id="16dfd-693">As facetas na linguagem de definição de esquema de repositório (SSDL) representam restrições em tipos de coluna que são especificados em elementos de propriedade.</span><span class="sxs-lookup"><span data-stu-id="16dfd-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="16dfd-694">As facetas são implementadas como atributos XML em elementos de **Propriedade** .</span><span class="sxs-lookup"><span data-stu-id="16dfd-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="16dfd-695">A tabela a seguir descreve as facetas com suporte no SSDL:</span><span class="sxs-lookup"><span data-stu-id="16dfd-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="16dfd-696">Faceta</span><span class="sxs-lookup"><span data-stu-id="16dfd-696">Facet</span></span>           | <span data-ttu-id="16dfd-697">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="16dfd-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dfd-698">**Ordenação**</span><span class="sxs-lookup"><span data-stu-id="16dfd-698">**Collation**</span></span>   | <span data-ttu-id="16dfd-699">Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="16dfd-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="16dfd-700">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="16dfd-700">**FixedLength**</span></span> | <span data-ttu-id="16dfd-701">Especifica se o comprimento do valor da coluna pode variar.</span><span class="sxs-lookup"><span data-stu-id="16dfd-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="16dfd-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="16dfd-702">**MaxLength**</span></span>   | <span data-ttu-id="16dfd-703">Especifica o comprimento máximo do valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="16dfd-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="16dfd-704">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="16dfd-704">**Precision**</span></span>   | <span data-ttu-id="16dfd-705">Para propriedades do tipo **decimal**, especifica o número de dígitos que um valor de propriedade pode ter.</span><span class="sxs-lookup"><span data-stu-id="16dfd-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="16dfd-706">Para propriedades do tipo **time**, **DateTime**e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="16dfd-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="16dfd-707">**Escala**</span><span class="sxs-lookup"><span data-stu-id="16dfd-707">**Scale**</span></span>       | <span data-ttu-id="16dfd-708">Especifica o número de dígitos à direita do ponto decimal para o valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="16dfd-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="16dfd-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="16dfd-709">**Unicode**</span></span>     | <span data-ttu-id="16dfd-710">Indica se o valor da coluna é armazenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="16dfd-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
