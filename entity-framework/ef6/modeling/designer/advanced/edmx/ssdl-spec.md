---
title: Especificação de SSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182544"
---
# <a name="ssdl-specification"></a><span data-ttu-id="93f64-102">Especificação de SSDL</span><span class="sxs-lookup"><span data-stu-id="93f64-102">SSDL Specification</span></span>
<span data-ttu-id="93f64-103">O SSDL (Store Schema Definition Language) é uma linguagem baseada em XML que descreve o modelo de armazenamento de um aplicativo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="93f64-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="93f64-104">Em um aplicativo Entity Framework, os metadados do modelo de armazenamento são carregados de um arquivo. SSDL (gravado em SSDL) em uma instância do System. Data. Metadata. Edm. StoreItemCollection e é acessível usando métodos no Classe System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="93f64-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="93f64-105">Entity Framework usa metadados do modelo de armazenamento para converter consultas em relação ao modelo conceitual para armazenar comandos específicos do armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="93f64-106">O Entity Framework Designer (EF designer) armazena informações do modelo de armazenamento em um arquivo. edmx em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="93f64-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="93f64-107">No momento da compilação, o Entity Designer usa informações em um arquivo. edmx para criar o arquivo. SSDL necessário para Entity Framework em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="93f64-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="93f64-108">As versões de SSDL são diferenciadas por namespaces XML.</span><span class="sxs-lookup"><span data-stu-id="93f64-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="93f64-109">Versão do SSDL</span><span class="sxs-lookup"><span data-stu-id="93f64-109">SSDL Version</span></span> | <span data-ttu-id="93f64-110">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="93f64-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="93f64-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="93f64-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="93f64-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="93f64-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="93f64-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="93f64-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="93f64-114">Elemento Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-114">Association Element (SSDL)</span></span>

<span data-ttu-id="93f64-115">Um elemento de **Associação** na linguagem de definição de esquema de repositório (SSDL) especifica colunas de tabela que participam de uma restrição de chave estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="93f64-116">Dois elementos end filho obrigatórios especificam tabelas nas extremidades da associação e a multiplicidade em cada extremidade.</span><span class="sxs-lookup"><span data-stu-id="93f64-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="93f64-117">Um elemento ReferentialConstraint opcional especifica as extremidades principais e dependentes da associação, bem como as colunas participantes.</span><span class="sxs-lookup"><span data-stu-id="93f64-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="93f64-118">Se nenhum elemento **ReferentialConstraint** estiver presente, um elemento AssociationSetMapping deverá ser usado para especificar os mapeamentos de coluna para a associação.</span><span class="sxs-lookup"><span data-stu-id="93f64-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="93f64-119">O elemento **Association** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-120">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="93f64-121">Término (exatamente duas)</span><span class="sxs-lookup"><span data-stu-id="93f64-121">End (exactly two)</span></span>
-   <span data-ttu-id="93f64-122">ReferentialConstraint (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="93f64-123">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-124">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-124">Applicable Attributes</span></span>

<span data-ttu-id="93f64-125">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="93f64-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="93f64-126">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-126">Attribute Name</span></span> | <span data-ttu-id="93f64-127">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-127">Is Required</span></span> | <span data-ttu-id="93f64-128">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-129">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-129">**Name**</span></span>       | <span data-ttu-id="93f64-130">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-130">Yes</span></span>         | <span data-ttu-id="93f64-131">O nome da restrição de chave estrangeira correspondente no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-132">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="93f64-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="93f64-133">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-134">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-135">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-135">Example</span></span>

<span data-ttu-id="93f64-136">O exemplo a seguir mostra um elemento **Association** que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira de **CustomerOrders de CE\_** :</span><span class="sxs-lookup"><span data-stu-id="93f64-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="93f64-137">Elemento AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="93f64-138">O elemento **AssociationSet** na linguagem de definição de esquema de repositório (SSDL) representa uma restrição de chave estrangeira entre duas tabelas no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="93f64-139">As colunas da tabela que participam da restrição FOREIGN KEY são especificadas em um elemento Association.</span><span class="sxs-lookup"><span data-stu-id="93f64-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="93f64-140">O elemento **Association** que corresponde a um determinado elemento **AssociationSet** é especificado no atributo **Association** do elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="93f64-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="93f64-141">Os conjuntos de associação SSDL são mapeados para conjuntos de associação CSDL por um elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="93f64-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="93f64-142">No entanto, se a associação de CSDL para um determinado conjunto de associação de CSDL for definida usando um elemento ReferentialConstraint, nenhum elemento **AssociationSetMapping** correspondente será necessário.</span><span class="sxs-lookup"><span data-stu-id="93f64-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="93f64-143">Nesse caso, se um elemento **AssociationSetMapping** estiver presente, os mapeamentos que ele definir serão substituídos pelo elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="93f64-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="93f64-144">O elemento **AssociationSet** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-145">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="93f64-146">End (zero ou dois)</span><span class="sxs-lookup"><span data-stu-id="93f64-146">End (zero or two)</span></span>
-   <span data-ttu-id="93f64-147">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-148">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-148">Applicable Attributes</span></span>

<span data-ttu-id="93f64-149">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="93f64-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="93f64-150">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-150">Attribute Name</span></span>  | <span data-ttu-id="93f64-151">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-151">Is Required</span></span> | <span data-ttu-id="93f64-152">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-153">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-153">**Name**</span></span>        | <span data-ttu-id="93f64-154">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-154">Yes</span></span>         | <span data-ttu-id="93f64-155">O nome da restrição de chave estrangeira que o conjunto de associação representa.</span><span class="sxs-lookup"><span data-stu-id="93f64-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="93f64-156">**Associação**</span><span class="sxs-lookup"><span data-stu-id="93f64-156">**Association**</span></span> | <span data-ttu-id="93f64-157">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-157">Yes</span></span>         | <span data-ttu-id="93f64-158">O nome da associação que define as colunas que participam da restrição FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="93f64-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-159">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="93f64-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="93f64-160">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-161">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-162">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-162">Example</span></span>

<span data-ttu-id="93f64-163">O exemplo a seguir mostra um elemento **AssociationSet** que representa a restrição de chave estrangeira `FK_CustomerOrders` no banco de dados subjacente:</span><span class="sxs-lookup"><span data-stu-id="93f64-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="93f64-164">Elemento CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="93f64-165">O elemento **CollectionType** em repositório de linguagem de definição de esquema (SSDL) especifica que o tipo de retorno de uma função é uma coleção.</span><span class="sxs-lookup"><span data-stu-id="93f64-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="93f64-166">O elemento **CollectionType** é um filho do elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="93f64-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="93f64-167">O tipo de coleção é especificado usando o elemento filho RowType:</span><span class="sxs-lookup"><span data-stu-id="93f64-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="93f64-168">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="93f64-169">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-170">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-171">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-171">Example</span></span>

<span data-ttu-id="93f64-172">O exemplo a seguir mostra uma função que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas.</span><span class="sxs-lookup"><span data-stu-id="93f64-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="93f64-173">Elemento CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="93f64-174">O elemento **CommandText** na Store (linguagem de definição de esquema) do repositório é um filho do elemento Function que permite que você defina uma instrução SQL que é executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="93f64-175">O elemento **CommandText** permite adicionar funcionalidade semelhante a um procedimento armazenado no banco de dados, mas você define o elemento **CommandText** no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="93f64-176">O elemento **CommandText** não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="93f64-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="93f64-177">O corpo do elemento **CommandText** deve ser uma instrução SQL válida para o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="93f64-178">Nenhum atributo é aplicável ao elemento **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="93f64-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-179">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-179">Example</span></span>

<span data-ttu-id="93f64-180">O exemplo a seguir mostra um elemento **Function** com um elemento de **CommandText** filho.</span><span class="sxs-lookup"><span data-stu-id="93f64-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="93f64-181">Expor a função **UpdateProductInOrder** como um método no ObjectContext importando-o para o modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="93f64-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="93f64-182">Elemento DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="93f64-183">O elemento **DefiningQuery** na linguagem de definição de esquema de repositório (SSDL) permite executar uma instrução SQL diretamente no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="93f64-184">O elemento **DefiningQuery** é comumente usado como uma exibição de banco de dados, mas a exibição é definida no modelo de armazenamento em vez do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="93f64-185">O modo de exibição definido em um elemento **DefiningQuery** pode ser mapeado para um tipo de entidade no modelo conceitual por meio de um elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="93f64-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="93f64-186">Esses mapeamentos são somente leitura.</span><span class="sxs-lookup"><span data-stu-id="93f64-186">These mappings are read-only.</span></span>  

<span data-ttu-id="93f64-187">A sintaxe SSDL a seguir mostra a declaração de um **EntitySet** seguido pelo elemento **DefiningQuery** que contém uma consulta usada para recuperar a exibição.</span><span class="sxs-lookup"><span data-stu-id="93f64-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="93f64-188">Você pode usar procedimentos armazenados no Entity Framework para habilitar cenários de leitura/gravação em exibições.</span><span class="sxs-lookup"><span data-stu-id="93f64-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="93f64-189"> Você pode usar uma exibição da fonte de dados ou uma exibição Entity SQL como a tabela base para recuperar dados e para o processamento de alterações por procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="93f64-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="93f64-190">Você pode usar o elemento **DefiningQuery** para direcionar Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="93f64-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="93f64-191">Embora SQL Server Compact 3,5 não ofereça suporte a procedimentos armazenados, você pode implementar uma funcionalidade semelhante com o elemento **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="93f64-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="93f64-192">Outro lugar onde pode ser útil é a criação de procedimentos armazenados para superar uma incompatibilidade entre os tipos de dados usados na linguagem de programação e os da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="93f64-193">Você poderia escrever um **DefiningQuery** que usa um determinado conjunto de parâmetros e, em seguida, chama um procedimento armazenado com um conjunto diferente de parâmetros, por exemplo, um procedimento armazenado que exclui dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="93f64-194">Elemento dependente (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="93f64-195">O elemento **dependente** na linguagem de definição de esquema de repositório (SSDL) é um elemento filho para o elemento ReferentialConstraint que define a extremidade dependente de uma restrição FOREIGN KEY (também chamada de restrição referencial).</span><span class="sxs-lookup"><span data-stu-id="93f64-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="93f64-196">O elemento **dependente** especifica a coluna (ou colunas) em uma tabela que faz referência a uma coluna de chave primária (ou colunas).</span><span class="sxs-lookup"><span data-stu-id="93f64-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="93f64-197">Os elementos **PropertyRef** especificam quais colunas são referenciadas.</span><span class="sxs-lookup"><span data-stu-id="93f64-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="93f64-198">O elemento principal especifica as colunas de chave primária que são referenciadas por colunas que são especificadas no elemento **dependente** .</span><span class="sxs-lookup"><span data-stu-id="93f64-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="93f64-199">O elemento **dependente** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-200">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="93f64-201">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-202">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-202">Applicable Attributes</span></span>

<span data-ttu-id="93f64-203">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **dependente** .</span><span class="sxs-lookup"><span data-stu-id="93f64-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="93f64-204">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-204">Attribute Name</span></span> | <span data-ttu-id="93f64-205">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-205">Is Required</span></span> | <span data-ttu-id="93f64-206">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-207">**Função**</span><span class="sxs-lookup"><span data-stu-id="93f64-207">**Role**</span></span>       | <span data-ttu-id="93f64-208">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-208">Yes</span></span>         | <span data-ttu-id="93f64-209">O mesmo valor que o atributo de **função** (se usado) do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna de referência.</span><span class="sxs-lookup"><span data-stu-id="93f64-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-210">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **dependente** .</span><span class="sxs-lookup"><span data-stu-id="93f64-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="93f64-211">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="93f64-212">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-213">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-213">Example</span></span>

<span data-ttu-id="93f64-214">O exemplo a seguir mostra um elemento Association que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira de **CustomerOrders de CE\_** .</span><span class="sxs-lookup"><span data-stu-id="93f64-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="93f64-215">O elemento **dependente** especifica a coluna **CustomerID** da tabela **Order** como a extremidade dependente da restrição.</span><span class="sxs-lookup"><span data-stu-id="93f64-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="93f64-216">Elemento Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="93f64-217">O elemento **Documentation** na Store (linguagem de definição de esquema de repositório) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai.</span><span class="sxs-lookup"><span data-stu-id="93f64-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="93f64-218">O elemento de **documentação** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-219">**Resumo**: uma breve descrição do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="93f64-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="93f64-220">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="93f64-220">(zero or one element)</span></span>
-   <span data-ttu-id="93f64-221">**LongDescription**: uma descrição extensiva do elemento pai.</span><span class="sxs-lookup"><span data-stu-id="93f64-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="93f64-222">(zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="93f64-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-223">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-223">Applicable Attributes</span></span>

<span data-ttu-id="93f64-224">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **documentação** .</span><span class="sxs-lookup"><span data-stu-id="93f64-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="93f64-225">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="93f64-226">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-227">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-227">Example</span></span>

<span data-ttu-id="93f64-228">O exemplo a seguir mostra o elemento de **documentação** como um elemento filho de um elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="93f64-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="93f64-229">Elemento final (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-229">End Element (SSDL)</span></span>

<span data-ttu-id="93f64-230">O elemento **final** na Store de linguagem de definição de esquema (SSDL) especifica a tabela e o número de linhas em uma extremidade de uma restrição de chave estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="93f64-231">O elemento **end** pode ser filho do elemento Association ou do elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="93f64-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="93f64-232">Em cada caso, os possíveis elementos filho e os atributos aplicáveis são diferentes.</span><span class="sxs-lookup"><span data-stu-id="93f64-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="93f64-233">Elemento end como um filho do elemento Association</span><span class="sxs-lookup"><span data-stu-id="93f64-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="93f64-234">Um elemento **final** (como um filho do elemento **Association** ) especifica a tabela e o número de linhas no final de uma restrição FOREIGN KEY com os atributos **Type** e **multiplicidade** , respectivamente.</span><span class="sxs-lookup"><span data-stu-id="93f64-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="93f64-235">As extremidades de uma restrição FOREIGN KEY são definidas como parte de uma associação SSDL; uma associação SSDL deve ter exatamente duas extremidades.</span><span class="sxs-lookup"><span data-stu-id="93f64-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="93f64-236">Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-237">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="93f64-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="93f64-238">OnDelete (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="93f64-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="93f64-239">Elementos de anotação (zero ou mais elementos)</span><span class="sxs-lookup"><span data-stu-id="93f64-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="93f64-240">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-240">Applicable Attributes</span></span>

<span data-ttu-id="93f64-241">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="93f64-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="93f64-242">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-242">Attribute Name</span></span>   | <span data-ttu-id="93f64-243">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-243">Is Required</span></span> | <span data-ttu-id="93f64-244">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-245">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="93f64-245">**Type**</span></span>         | <span data-ttu-id="93f64-246">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-246">Yes</span></span>         | <span data-ttu-id="93f64-247">O nome totalmente qualificado do conjunto de entidades SSDL que está no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="93f64-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="93f64-248">**Função**</span><span class="sxs-lookup"><span data-stu-id="93f64-248">**Role**</span></span>         | <span data-ttu-id="93f64-249">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-249">No</span></span>          | <span data-ttu-id="93f64-250">O valor do atributo **role** no elemento principal ou dependente do elemento ReferentialConstraint correspondente (se usado).</span><span class="sxs-lookup"><span data-stu-id="93f64-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="93f64-251">**Multiplicidade**</span><span class="sxs-lookup"><span data-stu-id="93f64-251">**Multiplicity**</span></span> | <span data-ttu-id="93f64-252">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-252">Yes</span></span>         | <span data-ttu-id="93f64-253">**1**, **0.. 1**ou **\*** dependendo do número de linhas que podem estar no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="93f64-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="93f64-254">**1** indica que exatamente uma linha existe no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="93f64-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="93f64-255">**0.. 1** indica que zero ou uma linha existe no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="93f64-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="93f64-256">**\*** indica que zero, uma ou mais linhas existem no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="93f64-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-257">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="93f64-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="93f64-258">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="93f64-259">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="93f64-260">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-260">Example</span></span>

<span data-ttu-id="93f64-261">O exemplo a seguir mostra um elemento **Association** que define a restrição de chave estrangeira **\_CustomerOrders de FK** .</span><span class="sxs-lookup"><span data-stu-id="93f64-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="93f64-262">Os valores de **multiplicidade** especificados em cada elemento de **extremidade** indicam que muitas linhas na tabela **Orders** podem ser associadas a uma linha na tabela **Customers** , mas apenas uma linha na tabela **Customers** pode ser associada a uma linha na tabela **Orders** .</span><span class="sxs-lookup"><span data-stu-id="93f64-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="93f64-263">Além disso, o elemento **OnDelete** indica que todas as linhas na tabela **Orders** que fazem referência a uma linha específica na tabela **Customers** serão excluídas se a linha na tabela **Customers** for excluída.</span><span class="sxs-lookup"><span data-stu-id="93f64-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="93f64-264">Elemento end como um filho do elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="93f64-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="93f64-265">O elemento **end** (como um filho do elemento **AssociationSet** ) especifica uma tabela em uma extremidade de uma restrição FOREIGN KEY no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="93f64-266">Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-267">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="93f64-268">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="93f64-269">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-269">Applicable Attributes</span></span>

<span data-ttu-id="93f64-270">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="93f64-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="93f64-271">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-271">Attribute Name</span></span> | <span data-ttu-id="93f64-272">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-272">Is Required</span></span> | <span data-ttu-id="93f64-273">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="93f64-274">**EntitySet**</span></span>  | <span data-ttu-id="93f64-275">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-275">Yes</span></span>         | <span data-ttu-id="93f64-276">O nome do conjunto de entidades SSDL que está no final da restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="93f64-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="93f64-277">**Função**</span><span class="sxs-lookup"><span data-stu-id="93f64-277">**Role**</span></span>       | <span data-ttu-id="93f64-278">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-278">No</span></span>          | <span data-ttu-id="93f64-279">O valor de um dos atributos de **função** especificados em um elemento **end** do elemento Association correspondente.</span><span class="sxs-lookup"><span data-stu-id="93f64-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-280">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="93f64-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="93f64-281">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="93f64-282">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="93f64-283">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-283">Example</span></span>

<span data-ttu-id="93f64-284">O exemplo a seguir mostra um elemento **EntityContainer** com um elemento **AssociationSet** com dois elementos **end** :</span><span class="sxs-lookup"><span data-stu-id="93f64-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="93f64-285">Elemento EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="93f64-286">Um elemento **EntityContainer** na Store (linguagem de definição de esquema) de repositório descreve a estrutura da fonte de dados subjacente em um aplicativo Entity Framework: conjuntos de entidades SSDL (definidos em elementos EntitySet) representam tabelas em um banco de dado, tipos de entidade SSDL (definidos em elementos EntityType) representam linhas em uma tabela e os conjuntos de associação (definidos em elementos AssociationSet) representam restrições Foreign Key em um banco de</span><span class="sxs-lookup"><span data-stu-id="93f64-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="93f64-287">Um contêiner de entidade de modelo de armazenamento é mapeado para um contêiner de entidade de modelo conceitual por meio do elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="93f64-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="93f64-288">Um elemento **EntityContainer** pode ter zero ou um elementos de documentação.</span><span class="sxs-lookup"><span data-stu-id="93f64-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="93f64-289">Se um elemento de **documentação** estiver presente, ele deverá preceder todos os outros elementos filho.</span><span class="sxs-lookup"><span data-stu-id="93f64-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="93f64-290">Um elemento **EntityContainer** pode ter zero ou mais dos seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="93f64-291">EntitySet</span></span>
-   <span data-ttu-id="93f64-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="93f64-292">AssociationSet</span></span>
-   <span data-ttu-id="93f64-293">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="93f64-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-294">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-294">Applicable Attributes</span></span>

<span data-ttu-id="93f64-295">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="93f64-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="93f64-296">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-296">Attribute Name</span></span> | <span data-ttu-id="93f64-297">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-297">Is Required</span></span> | <span data-ttu-id="93f64-298">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="93f64-299">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-299">**Name**</span></span>       | <span data-ttu-id="93f64-300">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-300">Yes</span></span>         | <span data-ttu-id="93f64-301">O nome do contêiner de entidade.</span><span class="sxs-lookup"><span data-stu-id="93f64-301">The name of the entity container.</span></span> <span data-ttu-id="93f64-302">Este nome não pode conter pontos (.).</span><span class="sxs-lookup"><span data-stu-id="93f64-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-303">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="93f64-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="93f64-304">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-305">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-306">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-306">Example</span></span>

<span data-ttu-id="93f64-307">O exemplo a seguir mostra um elemento **EntityContainer** que define dois conjuntos de entidades e um conjunto de associação.</span><span class="sxs-lookup"><span data-stu-id="93f64-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="93f64-308">Observe que o tipo de entidade e os nomes de tipo de associação são qualificados pelo nome do namespace do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="93f64-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="93f64-309">Elemento EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="93f64-310">Um elemento **EntitySet** na linguagem de definição de esquema de repositório (SSDL) representa uma tabela ou exibição no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="93f64-311">Um elemento EntityType em SSDL representa uma linha na tabela ou exibição.</span><span class="sxs-lookup"><span data-stu-id="93f64-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="93f64-312">O atributo **EntityType** de um elemento **EntitySet** especifica o tipo de entidade SSDL específico que representa linhas em um conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="93f64-313">O mapeamento entre um conjunto de entidades CSDL e um conjunto de entidades SSDL é especificado em um elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="93f64-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="93f64-314">O elemento **EntitySet** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-315">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="93f64-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="93f64-316">DefiningQuery (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="93f64-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="93f64-317">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="93f64-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-318">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-318">Applicable Attributes</span></span>

<span data-ttu-id="93f64-319">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="93f64-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="93f64-320">Alguns atributos (não listados aqui) podem ser qualificados com o alias da **loja** .</span><span class="sxs-lookup"><span data-stu-id="93f64-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="93f64-321">Esses atributos são usados pelo assistente de modelo de atualização ao atualizar um modelo.</span><span class="sxs-lookup"><span data-stu-id="93f64-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="93f64-322">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-322">Attribute Name</span></span> | <span data-ttu-id="93f64-323">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-323">Is Required</span></span> | <span data-ttu-id="93f64-324">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-325">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-325">**Name**</span></span>       | <span data-ttu-id="93f64-326">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-326">Yes</span></span>         | <span data-ttu-id="93f64-327">O nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="93f64-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="93f64-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="93f64-328">**EntityType**</span></span> | <span data-ttu-id="93f64-329">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-329">Yes</span></span>         | <span data-ttu-id="93f64-330">O nome totalmente qualificado do tipo de entidade para o qual o conjunto de entidades contém instâncias.</span><span class="sxs-lookup"><span data-stu-id="93f64-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="93f64-331">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="93f64-331">**Schema**</span></span>     | <span data-ttu-id="93f64-332">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-332">No</span></span>          | <span data-ttu-id="93f64-333">O esquema do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="93f64-334">**Tabela**</span><span class="sxs-lookup"><span data-stu-id="93f64-334">**Table**</span></span>      | <span data-ttu-id="93f64-335">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-335">No</span></span>          | <span data-ttu-id="93f64-336">A tabela do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="93f64-337">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="93f64-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="93f64-338">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-339">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-340">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-340">Example</span></span>

<span data-ttu-id="93f64-341">O exemplo a seguir mostra um elemento **EntityContainer** que tem dois elementos **EntitySet** e um elemento **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="93f64-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="93f64-342">Elemento EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="93f64-343">Um elemento **EntityType** na linguagem de definição de esquema de repositório (SSDL) representa uma linha em uma tabela ou exibição do banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="93f64-344">Um elemento EntitySet em SSDL representa a tabela ou exibição na qual as linhas ocorrem.</span><span class="sxs-lookup"><span data-stu-id="93f64-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="93f64-345">O atributo **EntityType** de um elemento **EntitySet** especifica o tipo de entidade SSDL específico que representa linhas em um conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="93f64-346">O mapeamento entre um tipo de entidade SSDL e um tipo de entidade CSDL é especificado em um elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="93f64-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="93f64-347">O elemento **EntityType** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-348">Documentação (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="93f64-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="93f64-349">Chave (zero ou um elemento)</span><span class="sxs-lookup"><span data-stu-id="93f64-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="93f64-350">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="93f64-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-351">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-351">Applicable Attributes</span></span>

<span data-ttu-id="93f64-352">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="93f64-353">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-353">Attribute Name</span></span> | <span data-ttu-id="93f64-354">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-354">Is Required</span></span> | <span data-ttu-id="93f64-355">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-356">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-356">**Name**</span></span>       | <span data-ttu-id="93f64-357">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-357">Yes</span></span>         | <span data-ttu-id="93f64-358">O nome do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="93f64-358">The name of the entity type.</span></span> <span data-ttu-id="93f64-359">Esse valor é geralmente o mesmo que o nome da tabela na qual o tipo de entidade representa uma linha.</span><span class="sxs-lookup"><span data-stu-id="93f64-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="93f64-360">Esse valor não pode conter pontos (.).</span><span class="sxs-lookup"><span data-stu-id="93f64-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-361">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="93f64-362">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-363">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-364">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-364">Example</span></span>

<span data-ttu-id="93f64-365">O exemplo a seguir mostra um elemento **EntityType** com duas propriedades:</span><span class="sxs-lookup"><span data-stu-id="93f64-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="93f64-366">Elemento Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-366">Function Element (SSDL)</span></span>

<span data-ttu-id="93f64-367">O elemento **Function** na linguagem de definição de esquema de repositório (SSDL) especifica um procedimento armazenado que existe no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="93f64-368">O elemento **Function** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-369">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="93f64-370">Parâmetro (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="93f64-371">CommandText (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="93f64-372">ReturnType (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="93f64-373">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="93f64-374">Um tipo de retorno para uma função deve ser especificado com o elemento **ReturnType** ou o atributo **ReturnType** (veja abaixo), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="93f64-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="93f64-375">Os procedimentos armazenados que são especificados no modelo de armazenamento podem ser importados para o modelo conceitual de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="93f64-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="93f64-376">Para obter mais informações, consulte [consultando com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="93f64-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="93f64-377">O elemento **Function** também pode ser usado para definir funções personalizadas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-378">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-378">Applicable Attributes</span></span>

<span data-ttu-id="93f64-379">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="93f64-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="93f64-380">Alguns atributos (não listados aqui) podem ser qualificados com o alias da **loja** .</span><span class="sxs-lookup"><span data-stu-id="93f64-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="93f64-381">Esses atributos são usados pelo assistente de modelo de atualização ao atualizar um modelo.</span><span class="sxs-lookup"><span data-stu-id="93f64-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="93f64-382">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-382">Attribute Name</span></span>             | <span data-ttu-id="93f64-383">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-383">Is Required</span></span> | <span data-ttu-id="93f64-384">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-385">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-385">**Name**</span></span>                   | <span data-ttu-id="93f64-386">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-386">Yes</span></span>         | <span data-ttu-id="93f64-387">O nome do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="93f64-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="93f64-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="93f64-388">**ReturnType**</span></span>             | <span data-ttu-id="93f64-389">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-389">No</span></span>          | <span data-ttu-id="93f64-390">O tipo de retorno do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="93f64-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="93f64-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="93f64-391">**Aggregate**</span></span>              | <span data-ttu-id="93f64-392">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-392">No</span></span>          | <span data-ttu-id="93f64-393">**True** se o procedimento armazenado retornar um valor de agregação; caso contrário, **false**.</span><span class="sxs-lookup"><span data-stu-id="93f64-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="93f64-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="93f64-394">**BuiltIn**</span></span>                | <span data-ttu-id="93f64-395">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-395">No</span></span>          | <span data-ttu-id="93f64-396">**True** se a função for uma função interna<sup>1</sup> ; caso contrário, **false**.</span><span class="sxs-lookup"><span data-stu-id="93f64-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="93f64-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="93f64-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="93f64-398">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-398">No</span></span>          | <span data-ttu-id="93f64-399">O nome do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="93f64-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="93f64-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="93f64-400">**NiladicFunction**</span></span>        | <span data-ttu-id="93f64-401">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-401">No</span></span>          | <span data-ttu-id="93f64-402">**True** se a função for uma função niladic<sup>2</sup> ; Caso contrário, **false** .</span><span class="sxs-lookup"><span data-stu-id="93f64-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="93f64-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="93f64-403">**IsComposable**</span></span>           | <span data-ttu-id="93f64-404">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-404">No</span></span>          | <span data-ttu-id="93f64-405">**True** se a função for uma função combinável<sup>3</sup> ; Caso contrário, **false** .</span><span class="sxs-lookup"><span data-stu-id="93f64-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="93f64-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="93f64-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="93f64-407">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-407">No</span></span>          | <span data-ttu-id="93f64-408">A enumeração que define a semântica de tipo usada para resolver sobrecargas de função.</span><span class="sxs-lookup"><span data-stu-id="93f64-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="93f64-409">A enumeração é definida no manifesto do provedor por definição de função.</span><span class="sxs-lookup"><span data-stu-id="93f64-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="93f64-410">O valor padrão é **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="93f64-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="93f64-411">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="93f64-411">**Schema**</span></span>                 | <span data-ttu-id="93f64-412">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-412">No</span></span>          | <span data-ttu-id="93f64-413">O nome do esquema no qual o procedimento armazenado é definido.</span><span class="sxs-lookup"><span data-stu-id="93f64-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="93f64-414"><sup>1</sup> uma função interna é uma função que é definida no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="93f64-415">Para obter informações sobre as funções definidas no modelo de armazenamento, consulte elemento CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="93f64-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="93f64-416"><sup>2</sup> uma função niladic é uma função que não aceita parâmetros e, quando chamado, não requer parênteses.</span><span class="sxs-lookup"><span data-stu-id="93f64-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="93f64-417"><sup>3</sup> duas funções são combináveis se a saída de uma função pode ser a entrada para a outra função.</span><span class="sxs-lookup"><span data-stu-id="93f64-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="93f64-418">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="93f64-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="93f64-419">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-420">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-421">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-421">Example</span></span>

<span data-ttu-id="93f64-422">O exemplo a seguir mostra um elemento **Function** que corresponde ao procedimento armazenado **UpdateOrderQuantity** .</span><span class="sxs-lookup"><span data-stu-id="93f64-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="93f64-423">O procedimento armazenado aceita dois parâmetros e não retorna um valor.</span><span class="sxs-lookup"><span data-stu-id="93f64-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="93f64-424">Elemento Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-424">Key Element (SSDL)</span></span>

<span data-ttu-id="93f64-425">O elemento de **chave** na linguagem de definição de esquema de repositório (SSDL) representa a chave primária de uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="93f64-426">**Key** é um elemento filho de um elemento EntityType, que representa uma linha em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="93f64-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="93f64-427">A chave primária é definida no elemento de **chave** referenciando um ou mais elementos de propriedade que são definidos no elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="93f64-428">O elemento **Key** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-429">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="93f64-430">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="93f64-430">Annotation elements</span></span>

<span data-ttu-id="93f64-431">Nenhum atributo é aplicável ao elemento de **chave** .</span><span class="sxs-lookup"><span data-stu-id="93f64-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-432">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-432">Example</span></span>

<span data-ttu-id="93f64-433">O exemplo a seguir mostra um elemento **EntityType** com uma chave que faz referência a uma propriedade:</span><span class="sxs-lookup"><span data-stu-id="93f64-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="93f64-434">Elemento ondelery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="93f64-435">O elemento **OnDelete** na Store (linguagem de definição de esquema) do repositório reflete o comportamento do banco de dados quando uma linha que participa de uma restrição de chave estrangeira é excluída.</span><span class="sxs-lookup"><span data-stu-id="93f64-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="93f64-436">Se a ação for definida como **Cascade**, as linhas que fazem referência a uma linha que está sendo excluída também serão excluídas.</span><span class="sxs-lookup"><span data-stu-id="93f64-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="93f64-437">Se a ação for definida como **nenhuma**, as linhas que fazem referência a uma linha que está sendo excluída também não serão excluídas.</span><span class="sxs-lookup"><span data-stu-id="93f64-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="93f64-438">Um elemento **OnDelete** é um elemento filho de um elemento end.</span><span class="sxs-lookup"><span data-stu-id="93f64-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="93f64-439">Um elemento **OnDelete** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-440">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="93f64-441">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-442">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-442">Applicable Attributes</span></span>

<span data-ttu-id="93f64-443">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="93f64-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="93f64-444">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-444">Attribute Name</span></span> | <span data-ttu-id="93f64-445">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-445">Is Required</span></span> | <span data-ttu-id="93f64-446">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-447">**Ação**</span><span class="sxs-lookup"><span data-stu-id="93f64-447">**Action**</span></span>     | <span data-ttu-id="93f64-448">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-448">Yes</span></span>         | <span data-ttu-id="93f64-449">**Cascade** ou **None**.</span><span class="sxs-lookup"><span data-stu-id="93f64-449">**Cascade** or **None**.</span></span> <span data-ttu-id="93f64-450">(O valor **restrito** é válido, mas tem o mesmo comportamento que **nenhum**.)</span><span class="sxs-lookup"><span data-stu-id="93f64-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-451">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="93f64-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="93f64-452">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-453">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-454">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-454">Example</span></span>

<span data-ttu-id="93f64-455">O exemplo a seguir mostra um elemento **Association** que define a restrição de chave estrangeira **\_CustomerOrders de FK** .</span><span class="sxs-lookup"><span data-stu-id="93f64-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="93f64-456">O elemento **OnDelete** indica que todas as linhas na tabela **Orders** que fazem referência a uma linha específica na tabela **Customers** serão excluídas se a linha na tabela **Customers** for excluída.</span><span class="sxs-lookup"><span data-stu-id="93f64-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="93f64-457">Elemento Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="93f64-458">O elemento **Parameter** na linguagem de definição de esquema de repositório (SSDL) é um filho do elemento Function que especifica parâmetros para um procedimento armazenado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="93f64-459">O elemento **Parameter** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-460">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="93f64-461">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-462">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-462">Applicable Attributes</span></span>

<span data-ttu-id="93f64-463">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="93f64-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="93f64-464">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-464">Attribute Name</span></span> | <span data-ttu-id="93f64-465">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-465">Is Required</span></span> | <span data-ttu-id="93f64-466">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-467">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-467">**Name**</span></span>       | <span data-ttu-id="93f64-468">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-468">Yes</span></span>         | <span data-ttu-id="93f64-469">O nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="93f64-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="93f64-470">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="93f64-470">**Type**</span></span>       | <span data-ttu-id="93f64-471">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-471">Yes</span></span>         | <span data-ttu-id="93f64-472">O tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="93f64-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="93f64-473">**Modo**</span><span class="sxs-lookup"><span data-stu-id="93f64-473">**Mode**</span></span>       | <span data-ttu-id="93f64-474">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-474">No</span></span>          | <span data-ttu-id="93f64-475">**In**, **out**ou **Inout** , dependendo se o parâmetro é um parâmetro de entrada, saída ou entrada/saída.</span><span class="sxs-lookup"><span data-stu-id="93f64-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="93f64-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="93f64-476">**MaxLength**</span></span>  | <span data-ttu-id="93f64-477">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-477">No</span></span>          | <span data-ttu-id="93f64-478">O comprimento máximo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="93f64-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="93f64-479">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="93f64-479">**Precision**</span></span>  | <span data-ttu-id="93f64-480">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-480">No</span></span>          | <span data-ttu-id="93f64-481">A precisão do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="93f64-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="93f64-482">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="93f64-482">**Scale**</span></span>      | <span data-ttu-id="93f64-483">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-483">No</span></span>          | <span data-ttu-id="93f64-484">A escala do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="93f64-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="93f64-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="93f64-485">**SRID**</span></span>       | <span data-ttu-id="93f64-486">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-486">No</span></span>          | <span data-ttu-id="93f64-487">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="93f64-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="93f64-488">Válido somente para parâmetros de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="93f64-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="93f64-489">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="93f64-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-490">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="93f64-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="93f64-491">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-492">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-493">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-493">Example</span></span>

<span data-ttu-id="93f64-494">O exemplo a seguir mostra um elemento **Function** que tem dois elementos de **parâmetro** que especificam parâmetros de entrada:</span><span class="sxs-lookup"><span data-stu-id="93f64-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="93f64-495">Elemento principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="93f64-496">O elemento **principal** no SSDL (Store Schema Definition Language) é um elemento filho do elemento ReferentialConstraint que define a extremidade principal de uma restrição FOREIGN KEY (também chamada de restrição referencial).</span><span class="sxs-lookup"><span data-stu-id="93f64-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="93f64-497">O elemento **principal** especifica a coluna de chave primária (ou colunas) em uma tabela que é referenciada por outra coluna (ou colunas).</span><span class="sxs-lookup"><span data-stu-id="93f64-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="93f64-498">Os elementos **PropertyRef** especificam quais colunas são referenciadas.</span><span class="sxs-lookup"><span data-stu-id="93f64-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="93f64-499">O elemento dependente especifica colunas que referenciam as colunas de chave primária que são especificadas no elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="93f64-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="93f64-500">O elemento **principal** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="93f64-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="93f64-501">PropertyRef (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="93f64-502">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-503">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-503">Applicable Attributes</span></span>

<span data-ttu-id="93f64-504">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="93f64-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="93f64-505">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-505">Attribute Name</span></span> | <span data-ttu-id="93f64-506">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-506">Is Required</span></span> | <span data-ttu-id="93f64-507">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-508">**Função**</span><span class="sxs-lookup"><span data-stu-id="93f64-508">**Role**</span></span>       | <span data-ttu-id="93f64-509">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-509">Yes</span></span>         | <span data-ttu-id="93f64-510">O mesmo valor que o atributo de **função** (se usado) do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna referenciada.</span><span class="sxs-lookup"><span data-stu-id="93f64-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-511">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="93f64-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="93f64-512">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="93f64-513">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-514">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-514">Example</span></span>

<span data-ttu-id="93f64-515">O exemplo a seguir mostra um elemento Association que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira de **CustomerOrders de CE\_** .</span><span class="sxs-lookup"><span data-stu-id="93f64-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="93f64-516">O elemento **principal** especifica a coluna **CustomerID** da tabela **Customer** como a extremidade principal da restrição.</span><span class="sxs-lookup"><span data-stu-id="93f64-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="93f64-517">Elemento Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-517">Property Element (SSDL)</span></span>

<span data-ttu-id="93f64-518">O elemento de **Propriedade** na linguagem de definição de esquema de repositório (SSDL) representa uma coluna em uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="93f64-519">Elementos de **Propriedade** são filhos de elementos EntityType, que representam linhas em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="93f64-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="93f64-520">Cada elemento de **Propriedade** definido em um elemento **EntityType** representa uma coluna.</span><span class="sxs-lookup"><span data-stu-id="93f64-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="93f64-521">Um elemento de **Propriedade** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="93f64-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-522">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-522">Applicable Attributes</span></span>

<span data-ttu-id="93f64-523">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="93f64-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="93f64-524">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-524">Attribute Name</span></span>            | <span data-ttu-id="93f64-525">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-525">Is Required</span></span> | <span data-ttu-id="93f64-526">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-527">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-527">**Name**</span></span>                  | <span data-ttu-id="93f64-528">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-528">Yes</span></span>         | <span data-ttu-id="93f64-529">O nome da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="93f64-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="93f64-530">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="93f64-530">**Type**</span></span>                  | <span data-ttu-id="93f64-531">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-531">Yes</span></span>         | <span data-ttu-id="93f64-532">O tipo da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="93f64-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="93f64-533">**Anula**</span><span class="sxs-lookup"><span data-stu-id="93f64-533">**Nullable**</span></span>              | <span data-ttu-id="93f64-534">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-534">No</span></span>          | <span data-ttu-id="93f64-535">**True** (o valor padrão) ou **false** , dependendo se a coluna correspondente pode ter um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="93f64-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="93f64-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="93f64-536">**DefaultValue**</span></span>          | <span data-ttu-id="93f64-537">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-537">No</span></span>          | <span data-ttu-id="93f64-538">O valor padrão da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="93f64-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="93f64-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="93f64-539">**MaxLength**</span></span>             | <span data-ttu-id="93f64-540">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-540">No</span></span>          | <span data-ttu-id="93f64-541">O comprimento máximo da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="93f64-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="93f64-542">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="93f64-542">**FixedLength**</span></span>           | <span data-ttu-id="93f64-543">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-543">No</span></span>          | <span data-ttu-id="93f64-544">**True** ou **false** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres de comprimento fixo.</span><span class="sxs-lookup"><span data-stu-id="93f64-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="93f64-545">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="93f64-545">**Precision**</span></span>             | <span data-ttu-id="93f64-546">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-546">No</span></span>          | <span data-ttu-id="93f64-547">A precisão da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="93f64-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="93f64-548">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="93f64-548">**Scale**</span></span>                 | <span data-ttu-id="93f64-549">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-549">No</span></span>          | <span data-ttu-id="93f64-550">A escala da coluna correspondente.</span><span class="sxs-lookup"><span data-stu-id="93f64-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="93f64-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="93f64-551">**Unicode**</span></span>               | <span data-ttu-id="93f64-552">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-552">No</span></span>          | <span data-ttu-id="93f64-553">**True** ou **false** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="93f64-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="93f64-554">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="93f64-554">**Collation**</span></span>             | <span data-ttu-id="93f64-555">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-555">No</span></span>          | <span data-ttu-id="93f64-556">Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="93f64-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="93f64-557">**SRID**</span></span>                  | <span data-ttu-id="93f64-558">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-558">No</span></span>          | <span data-ttu-id="93f64-559">Identificador de referência de sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="93f64-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="93f64-560">Válido somente para propriedades de tipos espaciais.</span><span class="sxs-lookup"><span data-stu-id="93f64-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="93f64-561">Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="93f64-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="93f64-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="93f64-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="93f64-563">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-563">No</span></span>          | <span data-ttu-id="93f64-564">**None**, **Identity** (se o valor da coluna correspondente for uma identidade gerada no banco de dados) ou **computado** (se o valor da coluna correspondente for computado no banco de dados).</span><span class="sxs-lookup"><span data-stu-id="93f64-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="93f64-565">Não é válido para propriedades RowType.</span><span class="sxs-lookup"><span data-stu-id="93f64-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-566">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="93f64-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="93f64-567">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-568">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-569">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-569">Example</span></span>

<span data-ttu-id="93f64-570">O exemplo a seguir mostra um elemento **EntityType** com dois elementos de **Propriedade** filho:</span><span class="sxs-lookup"><span data-stu-id="93f64-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="93f64-571">Elemento PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="93f64-572">O elemento **PropertyRef** na linguagem de definição de esquema de repositório (SSDL) faz referência a uma propriedade definida em um elemento EntityType para indicar que a propriedade executará uma das seguintes funções:</span><span class="sxs-lookup"><span data-stu-id="93f64-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="93f64-573">Faça parte da chave primária da tabela que o **EntityType** representa.</span><span class="sxs-lookup"><span data-stu-id="93f64-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="93f64-574">Um ou mais elementos **PropertyRef** podem ser usados para definir uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="93f64-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="93f64-575">Para obter mais informações, consulte Key Element.</span><span class="sxs-lookup"><span data-stu-id="93f64-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="93f64-576">Ser a extremidade dependente ou de entidade de uma restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="93f64-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="93f64-577">Para obter mais informações, consulte elemento ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="93f64-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="93f64-578">O elemento **PropertyRef** só pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="93f64-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="93f64-579">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="93f64-580">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="93f64-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-581">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-581">Applicable Attributes</span></span>

<span data-ttu-id="93f64-582">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="93f64-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="93f64-583">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-583">Attribute Name</span></span> | <span data-ttu-id="93f64-584">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-584">Is Required</span></span> | <span data-ttu-id="93f64-585">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="93f64-586">**Nome**</span><span class="sxs-lookup"><span data-stu-id="93f64-586">**Name**</span></span>       | <span data-ttu-id="93f64-587">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-587">Yes</span></span>         | <span data-ttu-id="93f64-588">O nome da propriedade referenciada.</span><span class="sxs-lookup"><span data-stu-id="93f64-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="93f64-589">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="93f64-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="93f64-590">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="93f64-591">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-592">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-592">Example</span></span>

<span data-ttu-id="93f64-593">O exemplo a seguir mostra um elemento **PropertyRef** usado para definir uma chave primária referenciando uma propriedade que é definida em um elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="93f64-594">Elemento ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="93f64-595">O elemento **ReferentialConstraint** na linguagem de definição de esquema de repositório (SSDL) representa uma restrição FOREIGN KEY (também chamada de restrição de integridade referencial) no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="93f64-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="93f64-596">As extremidades principais e dependentes da restrição são especificadas pelos elementos filho principal e dependente, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="93f64-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="93f64-597">As colunas que participam das extremidades principal e dependente são referenciadas com elementos PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="93f64-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="93f64-598">O elemento **ReferentialConstraint** é um elemento filho opcional do elemento Association.</span><span class="sxs-lookup"><span data-stu-id="93f64-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="93f64-599">Se um elemento **ReferentialConstraint** não for usado para mapear a restrição FOREIGN KEY especificada no elemento **Association** , um elemento AssociationSetMapping deverá ser usado para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="93f64-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="93f64-600">O elemento **ReferentialConstraint** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="93f64-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="93f64-601">Documentação (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="93f64-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="93f64-602">Entidade de segurança (exatamente uma)</span><span class="sxs-lookup"><span data-stu-id="93f64-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="93f64-603">Dependente (exatamente um)</span><span class="sxs-lookup"><span data-stu-id="93f64-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="93f64-604">Elementos de anotação (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-605">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-605">Applicable Attributes</span></span>

<span data-ttu-id="93f64-606">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="93f64-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="93f64-607">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-608">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-609">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-609">Example</span></span>

<span data-ttu-id="93f64-610">O exemplo a seguir mostra um elemento **Association** que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira de **CustomerOrders de CE\_** :</span><span class="sxs-lookup"><span data-stu-id="93f64-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="93f64-611">Elemento ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="93f64-612">O elemento **ReturnType** na linguagem de definição de esquema de repositório (SSDL) especifica o tipo de retorno para uma função que é definida em um elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="93f64-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="93f64-613">Um tipo de retorno de função também pode ser especificado com um atributo **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="93f64-614">O tipo de retorno de uma função é especificado com o atributo **Type** ou o elemento **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="93f64-615">O elemento **ReturnType** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="93f64-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="93f64-616">CollectionType (um)</span><span class="sxs-lookup"><span data-stu-id="93f64-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="93f64-617">Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="93f64-618">No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="93f64-619">Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-620">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-620">Example</span></span>

<span data-ttu-id="93f64-621">O exemplo a seguir usa uma **função** que retorna uma coleção de linhas.</span><span class="sxs-lookup"><span data-stu-id="93f64-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="93f64-622">Elemento RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="93f64-623">Um elemento **RowType** na linguagem de definição de esquema de repositório (SSDL) define uma estrutura sem nome como um tipo de retorno para uma função definida no repositório.</span><span class="sxs-lookup"><span data-stu-id="93f64-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="93f64-624">Um elemento **RowType** é o elemento filho do elemento **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="93f64-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="93f64-625">Um elemento **RowType** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="93f64-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="93f64-626">Propriedade (um ou mais)</span><span class="sxs-lookup"><span data-stu-id="93f64-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="93f64-627">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-627">Example</span></span>

<span data-ttu-id="93f64-628">O exemplo a seguir mostra uma função de repositório que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas (conforme especificado no elemento **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="93f64-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="93f64-629">Elemento de esquema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="93f64-630">O elemento de **esquema** na linguagem de definição de esquema de repositório (SSDL) é o elemento raiz de uma definição de modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="93f64-631">Ele contém definições para os objetos, funções e contêineres que compõem um modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="93f64-632">O elemento **Schema** pode conter zero ou mais dos seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="93f64-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="93f64-633">Associação</span><span class="sxs-lookup"><span data-stu-id="93f64-633">Association</span></span>
-   <span data-ttu-id="93f64-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="93f64-634">EntityType</span></span>
-   <span data-ttu-id="93f64-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="93f64-635">EntityContainer</span></span>
-   <span data-ttu-id="93f64-636">Função</span><span class="sxs-lookup"><span data-stu-id="93f64-636">Function</span></span>

<span data-ttu-id="93f64-637">O elemento **Schema** usa o atributo **namespace** para definir o namespace para o tipo de entidade e objetos de associação em um modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="93f64-638">Em um namespace, dois objetos não podem ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="93f64-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="93f64-639">Um namespace do modelo de armazenamento é diferente do namespace XML do elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="93f64-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="93f64-640">Um namespace de modelo de armazenamento (conforme definido pelo atributo **namespace** ) é um contêiner lógico para tipos de entidade e tipos de associação.</span><span class="sxs-lookup"><span data-stu-id="93f64-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="93f64-641">O namespace XML (indicado pelo atributo **xmlns** ) de um elemento **Schema** é o namespace padrão para elementos filho e atributos do elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="93f64-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="93f64-642">Os namespaces XML do formulário https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (em que AAAA e MM representam um ano e um mês respectivamente) são reservados para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="93f64-643">Atributos e elementos personalizados não podem estar em namespaces que tenham esse formulário.</span><span class="sxs-lookup"><span data-stu-id="93f64-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="93f64-644">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="93f64-644">Applicable Attributes</span></span>

<span data-ttu-id="93f64-645">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="93f64-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="93f64-646">Nome do Atributo</span><span class="sxs-lookup"><span data-stu-id="93f64-646">Attribute Name</span></span>            | <span data-ttu-id="93f64-647">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="93f64-647">Is Required</span></span> | <span data-ttu-id="93f64-648">Valor</span><span class="sxs-lookup"><span data-stu-id="93f64-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="93f64-649">**Namespace**</span></span>             | <span data-ttu-id="93f64-650">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-650">Yes</span></span>         | <span data-ttu-id="93f64-651">O namespace do modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-651">The namespace of the storage model.</span></span> <span data-ttu-id="93f64-652">O valor do atributo **namespace** é usado para formar o nome totalmente qualificado de um tipo.</span><span class="sxs-lookup"><span data-stu-id="93f64-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="93f64-653">Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace ExampleModel. Store, o nome totalmente qualificado do **EntityType** será ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="93f64-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="93f64-654">As cadeias de caracteres a seguir não podem ser usadas como o valor do atributo de **namespace** : **System**, **transitório**ou **EDM**.</span><span class="sxs-lookup"><span data-stu-id="93f64-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="93f64-655">O valor do atributo **namespace** não pode ser o mesmo que o valor do atributo **namespace** no elemento de esquema CSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="93f64-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="93f64-656">**Alias**</span></span>                 | <span data-ttu-id="93f64-657">Não</span><span class="sxs-lookup"><span data-stu-id="93f64-657">No</span></span>          | <span data-ttu-id="93f64-658">Um identificador usado no lugar do nome do namespace.</span><span class="sxs-lookup"><span data-stu-id="93f64-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="93f64-659">Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace ExampleModel. Store e o valor do atributo **alias** for *StorageModel*, você poderá usar StorageModel. Customer como o nome totalmente qualificado do **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="93f64-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="93f64-660">**Provedor**</span><span class="sxs-lookup"><span data-stu-id="93f64-660">**Provider**</span></span>              | <span data-ttu-id="93f64-661">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-661">Yes</span></span>         | <span data-ttu-id="93f64-662">O provedor de dados.</span><span class="sxs-lookup"><span data-stu-id="93f64-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="93f64-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="93f64-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="93f64-664">Sim</span><span class="sxs-lookup"><span data-stu-id="93f64-664">Yes</span></span>         | <span data-ttu-id="93f64-665">Um token que indica ao provedor que o manifesto do provedor deve retornar.</span><span class="sxs-lookup"><span data-stu-id="93f64-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="93f64-666">Nenhum formato para o token está definido.</span><span class="sxs-lookup"><span data-stu-id="93f64-666">No format for the token is defined.</span></span> <span data-ttu-id="93f64-667">Os valores para o token são definidos pelo provedor.</span><span class="sxs-lookup"><span data-stu-id="93f64-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="93f64-668">Para obter informações sobre SQL Server tokens de manifesto do provedor, consulte SqlClient para Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="93f64-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="93f64-669">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-669">Example</span></span>

<span data-ttu-id="93f64-670">O exemplo a seguir mostra um elemento de **esquema** que contém um elemento **EntityContainer** , dois elementos **EntityType** e um elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="93f64-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="93f64-671">Atributos de anotação</span><span class="sxs-lookup"><span data-stu-id="93f64-671">Annotation Attributes</span></span>

<span data-ttu-id="93f64-672">Os atributos de anotação no SSDL (Store Schema Definition Language) são atributos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre os elementos no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="93f64-673">Além de ter uma estrutura XML válida, as seguintes restrições se aplicam aos atributos de anotação:</span><span class="sxs-lookup"><span data-stu-id="93f64-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="93f64-674">Os atributos de anotação não devem estar em nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="93f64-675">Os nomes totalmente qualificados de quaisquer dois atributos de anotação não devem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="93f64-676">Mais de um atributo Annotation pode ser aplicado a um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="93f64-677">Os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="93f64-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-678">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-678">Example</span></span>

<span data-ttu-id="93f64-679">O exemplo a seguir mostra um elemento EntityType que tem um atributo Annotation aplicado à propriedade **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="93f64-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="93f64-680">O exemplo também mostra um elemento Annotation adicionado ao elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="93f64-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="93f64-681">Elementos de anotação (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="93f64-682">Os elementos de anotação no SSDL (Store Schema Definition Language) são elementos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre o modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="93f64-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="93f64-683">Além de ter uma estrutura XML válida, as seguintes restrições se aplicam aos elementos de anotação:</span><span class="sxs-lookup"><span data-stu-id="93f64-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="93f64-684">Os elementos de anotação não devem estar em nenhum namespace XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="93f64-685">Os nomes totalmente qualificados de quaisquer dois elementos de anotação não devem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="93f64-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="93f64-686">Os elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="93f64-687">Mais de um elemento Annotation pode ser um filho de um determinado elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="93f64-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="93f64-688">A partir do .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="93f64-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="93f64-689">Exemplo</span><span class="sxs-lookup"><span data-stu-id="93f64-689">Example</span></span>

<span data-ttu-id="93f64-690">O exemplo a seguir mostra um elemento EntityType que tem um elemento Annotation (**customelement**).</span><span class="sxs-lookup"><span data-stu-id="93f64-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="93f64-691">O exemplo também mostra um atributo Annotation aplicado à propriedade **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="93f64-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="93f64-692">Facetas (SSDL)</span><span class="sxs-lookup"><span data-stu-id="93f64-692">Facets (SSDL)</span></span>

<span data-ttu-id="93f64-693">As facetas na linguagem de definição de esquema de repositório (SSDL) representam restrições em tipos de coluna que são especificados em elementos de propriedade.</span><span class="sxs-lookup"><span data-stu-id="93f64-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="93f64-694">As facetas são implementadas como atributos XML em elementos de **Propriedade** .</span><span class="sxs-lookup"><span data-stu-id="93f64-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="93f64-695">A tabela a seguir descreve as facetas com suporte no SSDL:</span><span class="sxs-lookup"><span data-stu-id="93f64-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="93f64-696">Aspecto</span><span class="sxs-lookup"><span data-stu-id="93f64-696">Facet</span></span>           | <span data-ttu-id="93f64-697">Descrição</span><span class="sxs-lookup"><span data-stu-id="93f64-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="93f64-698">**Agrupamento**</span><span class="sxs-lookup"><span data-stu-id="93f64-698">**Collation**</span></span>   | <span data-ttu-id="93f64-699">Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="93f64-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="93f64-700">**Cadeia**</span><span class="sxs-lookup"><span data-stu-id="93f64-700">**FixedLength**</span></span> | <span data-ttu-id="93f64-701">Especifica se o comprimento do valor da coluna pode variar.</span><span class="sxs-lookup"><span data-stu-id="93f64-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="93f64-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="93f64-702">**MaxLength**</span></span>   | <span data-ttu-id="93f64-703">Especifica o comprimento máximo do valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="93f64-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="93f64-704">**Precisão**</span><span class="sxs-lookup"><span data-stu-id="93f64-704">**Precision**</span></span>   | <span data-ttu-id="93f64-705">Para propriedades do tipo **decimal**, especifica o número de dígitos que um valor de propriedade pode ter.</span><span class="sxs-lookup"><span data-stu-id="93f64-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="93f64-706">Para propriedades do tipo **time**, **DateTime**e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="93f64-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="93f64-707">**Ajustar Escala**</span><span class="sxs-lookup"><span data-stu-id="93f64-707">**Scale**</span></span>       | <span data-ttu-id="93f64-708">Especifica o número de dígitos à direita do ponto decimal para o valor da coluna.</span><span class="sxs-lookup"><span data-stu-id="93f64-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="93f64-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="93f64-709">**Unicode**</span></span>     | <span data-ttu-id="93f64-710">Indica se o valor da coluna é armazenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="93f64-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
