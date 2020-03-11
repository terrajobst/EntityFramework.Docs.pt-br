---
title: Especificação MSL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418728"
---
# <a name="msl-specification"></a><span data-ttu-id="cd0f0-102">Especificação de MSL</span><span class="sxs-lookup"><span data-stu-id="cd0f0-102">MSL Specification</span></span>
<span data-ttu-id="cd0f0-103">O MSL (Mapping Specification Language) é uma linguagem baseada em XML que descreve o mapeamento entre o modelo conceitual e o modelo de armazenamento de um aplicativo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="cd0f0-104">Em um aplicativo Entity Framework, os metadados de mapeamento são carregados de um arquivo. MSL (escrito em MSL) no momento da compilação.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="cd0f0-105">O Entity Framework usa metadados de mapeamento em tempo de execução para converter consultas em relação ao modelo conceitual para armazenar comandos específicos.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="cd0f0-106">O Entity Framework Designer (EF designer) armazena informações de mapeamento em um arquivo. edmx em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="cd0f0-107">No momento da compilação, o Entity Designer usa informações em um arquivo. edmx para criar o arquivo. MSL que é necessário pelo Entity Framework em tempo de execução</span><span class="sxs-lookup"><span data-stu-id="cd0f0-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="cd0f0-108">Os nomes de todos os tipos de modelo conceituais ou de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="cd0f0-109">Para obter informações sobre o nome do namespace do modelo conceitual, consulte [especificação CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="cd0f0-110">Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte [especificação de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="cd0f0-111">As versões do MSL são diferenciadas por namespaces XML.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="cd0f0-112">Versão do MSL</span><span class="sxs-lookup"><span data-stu-id="cd0f0-112">MSL Version</span></span> | <span data-ttu-id="cd0f0-113">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="cd0f0-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="cd0f0-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="cd0f0-114">MSL v1</span></span>      | <span data-ttu-id="cd0f0-115">urn: schemas-microsoft-com: Windows: Storage: Mapping: CS</span><span class="sxs-lookup"><span data-stu-id="cd0f0-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="cd0f0-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="cd0f0-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="cd0f0-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="cd0f0-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="cd0f0-118">Elemento alias (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-118">Alias Element (MSL)</span></span>

<span data-ttu-id="cd0f0-119">O elemento **alias** no MSL (Mapping Specification Language) é um filho do elemento Mapping que é usado para definir aliases para namespaces de modelo conceitual e modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="cd0f0-120">Os nomes de todos os tipos de modelo conceituais ou de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="cd0f0-121">Para obter informações sobre o nome do namespace do modelo conceitual, consulte elemento Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="cd0f0-122">Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte elemento de esquema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="cd0f0-123">O elemento **alias** não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-124">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-124">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-125">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **alias** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="cd0f0-126">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-126">Attribute Name</span></span> | <span data-ttu-id="cd0f0-127">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-127">Is Required</span></span> | <span data-ttu-id="cd0f0-128">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-129">**Chave**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-129">**Key**</span></span>        | <span data-ttu-id="cd0f0-130">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-130">Yes</span></span>         | <span data-ttu-id="cd0f0-131">O alias para o namespace que é especificado pelo atributo **Value** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="cd0f0-132">**Valor**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-132">**Value**</span></span>      | <span data-ttu-id="cd0f0-133">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-133">Yes</span></span>         | <span data-ttu-id="cd0f0-134">O namespace para o qual o valor do elemento de **chave** é um alias.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="cd0f0-135">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-135">Example</span></span>

<span data-ttu-id="cd0f0-136">O exemplo a seguir mostra um elemento **alias** que define um alias, `c`, para tipos que são definidos no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a><span data-ttu-id="cd0f0-137">Elemento AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="cd0f0-138">O elemento **AssociationEnd** na MSL (linguagem de especificação de mapeamento) é usado quando as funções de modificação de um tipo de entidade no modelo conceitual são mapeadas para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="cd0f0-139">Se um procedimento armazenado de modificação usa um parâmetro cujo valor é mantido em uma propriedade de associação, o elemento **AssociationEnd** mapeia o valor da propriedade para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="cd0f0-140">Para obter mais informações, consulte o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-140">For more information, see the example below.</span></span>

<span data-ttu-id="cd0f0-141">Para obter mais informações sobre como mapear funções de modificação de tipos de entidade para procedimentos armazenados, consulte ModificationFunctionMapping Element (MSL) e Walkthrough: Mappinging a Entity to stored procedures.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="cd0f0-142">O elemento **AssociationEnd** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="cd0f0-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-144">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-144">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-145">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="cd0f0-146">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-146">Attribute Name</span></span>     | <span data-ttu-id="cd0f0-147">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-147">Is Required</span></span> | <span data-ttu-id="cd0f0-148">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-149">**AssociationSet**</span></span> | <span data-ttu-id="cd0f0-150">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-150">Yes</span></span>         | <span data-ttu-id="cd0f0-151">O nome da associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="cd0f0-152">**De**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-152">**From**</span></span>           | <span data-ttu-id="cd0f0-153">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-153">Yes</span></span>         | <span data-ttu-id="cd0f0-154">O valor do atributo **FromRole** da propriedade de navegação que corresponde à associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="cd0f0-155">Para obter mais informações, consulte o elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="cd0f0-156">**Para**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-156">**To**</span></span>             | <span data-ttu-id="cd0f0-157">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-157">Yes</span></span>         | <span data-ttu-id="cd0f0-158">O valor do atributo **ToRole** da propriedade de navegação que corresponde à associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="cd0f0-159">Para obter mais informações, consulte o elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="cd0f0-160">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-160">Example</span></span>

<span data-ttu-id="cd0f0-161">Considere o seguinte tipo de entidade de modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-161">Consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

<span data-ttu-id="cd0f0-162">Considere também o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-162">Also consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

<span data-ttu-id="cd0f0-163">Para mapear a função Update da entidade `Course` para esse procedimento armazenado, você deve fornecer um valor para o parâmetro **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="cd0f0-164">O valor de `DepartmentID` não corresponde a uma propriedade no tipo de entidade; Ele está contido em uma associação independente cujo mapeamento é mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

<span data-ttu-id="cd0f0-165">O código a seguir mostra o elemento **AssociationEnd** usado para mapear a propriedade **DepartmentID** do **curso de\_de FK\_** Associação de departamento para o procedimento armazenado **UpdateCourse** (para o qual a função Update do tipo de entidade **curso** é mapeada):</span><span class="sxs-lookup"><span data-stu-id="cd0f0-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="cd0f0-166">Elemento AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-167">O elemento **AssociationSetMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma associação no modelo conceitual e colunas de tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="cd0f0-168">As associações no modelo conceitual são tipos cujas propriedades representam as colunas de chave primária e estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="cd0f0-169">O elemento **AssociationSetMapping** usa dois elementos EndProperty para definir os mapeamentos entre as propriedades e as colunas do tipo de associação no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="cd0f0-170">Você pode posicionar as condições nesses mapeamentos com o elemento Condition.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="cd0f0-171">Mapeie as funções INSERT, Update e Delete para associações a procedimentos armazenados no banco de dados com o elemento ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="cd0f0-172">Defina mapeamentos somente leitura entre associações e colunas de tabela usando uma Entity SQL cadeia de caracteres em um elemento QueryView.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-173">Se uma restrição referencial for definida para uma associação no modelo conceitual, a associação não precisará ser mapeada com um elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="cd0f0-174">Se um elemento **AssociationSetMapping** estiver presente para uma associação que tenha uma restrição referencial, os mapeamentos definidos no elemento **AssociationSetMapping** serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="cd0f0-175">Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="cd0f0-176">O elemento **AssociationSetMapping** pode ter os seguintes elementos filho</span><span class="sxs-lookup"><span data-stu-id="cd0f0-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="cd0f0-177">QueryView (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="cd0f0-178">EndProperty (zero ou dois)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="cd0f0-179">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-180">ModificationFunctionMapping (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-181">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-181">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-182">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="cd0f0-183">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-183">Attribute Name</span></span>     | <span data-ttu-id="cd0f0-184">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-184">Is Required</span></span> | <span data-ttu-id="cd0f0-185">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-186">**Nome**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-186">**Name**</span></span>           | <span data-ttu-id="cd0f0-187">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-187">Yes</span></span>         | <span data-ttu-id="cd0f0-188">O nome do conjunto de associação do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="cd0f0-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-189">**TypeName**</span></span>       | <span data-ttu-id="cd0f0-190">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-190">No</span></span>          | <span data-ttu-id="cd0f0-191">O nome qualificado do namespace do tipo de associação de modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="cd0f0-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-192">**StoreEntitySet**</span></span> | <span data-ttu-id="cd0f0-193">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-193">No</span></span>          | <span data-ttu-id="cd0f0-194">O nome da tabela que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="cd0f0-195">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-195">Example</span></span>

<span data-ttu-id="cd0f0-196">O exemplo a seguir mostra um elemento **AssociationSetMapping** no qual o **curso de\_de FK\_** conjunto de associação de departamento no modelo conceitual é mapeado para a tabela do **curso** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="cd0f0-197">Os mapeamentos entre as propriedades do tipo de associação e as colunas de tabela são especificados nos elementos filho **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

## <a name="complexproperty-element-msl"></a><span data-ttu-id="cd0f0-198">Elemento ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="cd0f0-199">Um elemento **ComplexProperty** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma propriedade de tipo complexo em um tipo de entidade de modelo conceitual e colunas de tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="cd0f0-200">Os mapeamentos de coluna de propriedade são especificados em elementos ScalarProperty filho.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="cd0f0-201">O elemento de propriedade **complexType** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-202">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-203">**ComplexProperty** (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-204">ComplextTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-205">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-206">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-206">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-207">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ComplexProperty** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="cd0f0-208">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-208">Attribute Name</span></span> | <span data-ttu-id="cd0f0-209">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-209">Is Required</span></span> | <span data-ttu-id="cd0f0-210">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-211">**Nome**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-211">**Name**</span></span>       | <span data-ttu-id="cd0f0-212">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-212">Yes</span></span>         | <span data-ttu-id="cd0f0-213">O nome da propriedade complexa de um tipo de entidade no modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="cd0f0-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-214">**TypeName**</span></span>   | <span data-ttu-id="cd0f0-215">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-215">No</span></span>          | <span data-ttu-id="cd0f0-216">O nome qualificado do namespace do tipo de Propriedade do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="cd0f0-217">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-217">Example</span></span>

<span data-ttu-id="cd0f0-218">O exemplo a seguir é baseado no modelo escolar.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-218">The following example is based on the School model.</span></span> <span data-ttu-id="cd0f0-219">O seguinte tipo complexo foi adicionado ao modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-219">The following complex type has been added to the conceptual model:</span></span>

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

<span data-ttu-id="cd0f0-220">As propriedades **LastName** e **FirstName** do tipo de entidade **Person** foram substituídas por uma propriedade complexa, **Name**:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

<span data-ttu-id="cd0f0-221">O seguinte MSL mostra o elemento **ComplexProperty** usado para mapear a propriedade **Name** para colunas no banco de dados subjacente:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="cd0f0-222">Elemento ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-223">O elemento **ComplexTypeMapping** na MSL (linguagem de especificação de mapeamento) é um filho do elemento ResultMapping e define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado no banco de dados subjacente quando o seguinte é verdadeiro:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="cd0f0-224">A função Import retorna um tipo complexo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="cd0f0-225">Os nomes das colunas retornadas pelo procedimento armazenado não correspondem exatamente aos nomes das propriedades no tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="cd0f0-226">Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo complexo baseia-se nos nomes de coluna e propriedade.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="cd0f0-227">Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, você deverá usar o elemento **ComplexTypeMapping** para definir o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="cd0f0-228">Para obter um exemplo do mapeamento padrão, consulte elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="cd0f0-229">O elemento **ComplexTypeMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-230">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-231">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-231">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-232">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ComplexTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="cd0f0-233">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-233">Attribute Name</span></span> | <span data-ttu-id="cd0f0-234">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-234">Is Required</span></span> | <span data-ttu-id="cd0f0-235">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-236">**TypeName**</span></span>   | <span data-ttu-id="cd0f0-237">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-237">Yes</span></span>         | <span data-ttu-id="cd0f0-238">O nome qualificado do namespace do tipo complexo que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-239">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-239">Example</span></span>

<span data-ttu-id="cd0f0-240">Considere o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-240">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="cd0f0-241">Considere também o seguinte tipo complexo de modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="cd0f0-242">Para criar uma importação de função que retorne instâncias do tipo complexo anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um elemento **ComplexTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a><span data-ttu-id="cd0f0-243">Elemento Condition (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-243">Condition Element (MSL)</span></span>

<span data-ttu-id="cd0f0-244">O elemento **Condition** no MSL (Mapping Specification Language) coloca condições em mapeamentos entre o modelo conceitual e o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="cd0f0-245">O mapeamento definido dentro de um nó XML será válido se todas as condições, conforme especificado nos elementos de **condição** filho, forem atendidas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="cd0f0-246">Caso contrário, o mapeamento não será válido.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="cd0f0-247">Por exemplo, se um elemento MappingFragment contiver um ou mais elementos filho **Condition** , o mapeamento definido no nó **MappingFragment** só será válido se todas as condições dos elementos de **condição** filho forem atendidas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="cd0f0-248">Cada condição pode se aplicar a um **nome** (o nome de uma propriedade de entidade modelo conceitual, especificada pelo atributo **Name** ) ou um **ColumnName** (o nome de uma coluna no banco de dados, especificado pelo atributo **ColumnName** ).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="cd0f0-249">Quando o atributo **Name** é definido, a condição é verificada em relação a um valor de propriedade de entidade.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="cd0f0-250">Quando o atributo **ColumnName** é definido, a condição é verificada em relação a um valor de coluna.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="cd0f0-251">Somente um dos atributos **Name** ou **ColumnName** pode ser especificado em um elemento **Condition** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-252">Quando o elemento **Condition** é usado em um elemento FunctionImportMapping, somente o atributo **Name** não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="cd0f0-253">O elemento **Condition** pode ser um filho dos seguintes elementos:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="cd0f0-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="cd0f0-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="cd0f0-255">ComplexProperty</span></span>
-   <span data-ttu-id="cd0f0-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-256">EntitySetMapping</span></span>
-   <span data-ttu-id="cd0f0-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="cd0f0-257">MappingFragment</span></span>
-   <span data-ttu-id="cd0f0-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-258">EntityTypeMapping</span></span>

<span data-ttu-id="cd0f0-259">O elemento **Condition** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-260">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-260">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-261">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **Condition** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="cd0f0-262">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-262">Attribute Name</span></span> | <span data-ttu-id="cd0f0-263">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-263">Is Required</span></span> | <span data-ttu-id="cd0f0-264">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-265">**ColumnName**</span></span> | <span data-ttu-id="cd0f0-266">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-266">No</span></span>          | <span data-ttu-id="cd0f0-267">O nome da coluna de tabela cujo valor é usado para avaliar a condição.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="cd0f0-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-268">**IsNull**</span></span>     | <span data-ttu-id="cd0f0-269">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-269">No</span></span>          | <span data-ttu-id="cd0f0-270">**True** ou **False**.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-270">**True** or **False**.</span></span> <span data-ttu-id="cd0f0-271">Se o valor for **true** e o valor da coluna for **NULL**ou se o valor for **false** e o valor da coluna não for **NULL**, a condição será true.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="cd0f0-272">Caso contrário, a condição será falsa.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="cd0f0-273">Os atributos **IsNull** e **Value** não podem ser usados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="cd0f0-274">**Valor**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-274">**Value**</span></span>      | <span data-ttu-id="cd0f0-275">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-275">No</span></span>          | <span data-ttu-id="cd0f0-276">O valor com o qual o valor da coluna é comparado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-276">The value with which the column value is compared.</span></span> <span data-ttu-id="cd0f0-277">Se os valores forem iguais, a condição será verdadeira.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="cd0f0-278">Caso contrário, a condição será falsa.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="cd0f0-279">Os atributos **IsNull** e **Value** não podem ser usados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="cd0f0-280">**Nome**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-280">**Name**</span></span>       | <span data-ttu-id="cd0f0-281">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-281">No</span></span>          | <span data-ttu-id="cd0f0-282">O nome da propriedade de entidade do modelo conceitual cujo valor é usado para avaliar a condição.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="cd0f0-283">Esse atributo não será aplicável se o elemento **Condition** for usado dentro de um elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="cd0f0-284">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-284">Example</span></span>

<span data-ttu-id="cd0f0-285">O exemplo a seguir mostra os elementos **Condition** como filhos dos elementos **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="cd0f0-286">Quando **HireDate** não é nulo e **EnrollmentDate** é nulo, os dados são mapeados entre o tipo **SchoolModel. instrutor** e as colunas **PersonID** e **HireDate** da tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="cd0f0-287">Quando **EnrollmentDate** não é nulo e **HireDate** é NULL, os dados são mapeados entre o tipo **SchoolModel. Student** e as colunas **PersonID** e **registro** da tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a><span data-ttu-id="cd0f0-288">Elemento DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="cd0f0-289">O elemento **DeleteFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Delete de um tipo de entidade ou associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="cd0f0-290">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="cd0f0-291">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-292">Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="cd0f0-293">DeleteFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="cd0f0-294">Quando aplicado ao elemento EntityTypeMapping, o elemento **DeleteFunction** mapeia a função Delete de um tipo de entidade no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="cd0f0-295">O elemento **DeleteFunction** pode ter os seguintes elementos filho quando aplicados a um elemento **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="cd0f0-296">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-297">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-298">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-299">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-299">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-300">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **DeleteFunction** quando ele é aplicado a um elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="cd0f0-301">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-301">Attribute Name</span></span>            | <span data-ttu-id="cd0f0-302">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-302">Is Required</span></span> | <span data-ttu-id="cd0f0-303">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-304">**FunctionName**</span></span>          | <span data-ttu-id="cd0f0-305">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-305">Yes</span></span>         | <span data-ttu-id="cd0f0-306">O nome qualificado do namespace do procedimento armazenado para o qual a função Delete está mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="cd0f0-307">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="cd0f0-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="cd0f0-309">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-309">No</span></span>          | <span data-ttu-id="cd0f0-310">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="cd0f0-311">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-311">Example</span></span>

<span data-ttu-id="cd0f0-312">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **DeleteFunction** mapeando a função Delete do tipo de entidade **Person** para o procedimento armazenado **DeletePerson** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="cd0f0-313">O procedimento armazenado **DeletePerson** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="cd0f0-314">DeleteFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="cd0f0-315">Quando aplicado ao elemento AssociationSetMapping, o elemento **DeleteFunction** mapeia a função Delete de uma associação no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="cd0f0-316">O elemento **DeleteFunction** pode ter os seguintes elementos filho quando aplicado ao elemento **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="cd0f0-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="cd0f0-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-318">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-318">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-319">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **DeleteFunction** quando ele é aplicado ao elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="cd0f0-320">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-320">Attribute Name</span></span>            | <span data-ttu-id="cd0f0-321">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-321">Is Required</span></span> | <span data-ttu-id="cd0f0-322">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-323">**FunctionName**</span></span>          | <span data-ttu-id="cd0f0-324">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-324">Yes</span></span>         | <span data-ttu-id="cd0f0-325">O nome qualificado do namespace do procedimento armazenado para o qual a função Delete está mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="cd0f0-326">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="cd0f0-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="cd0f0-328">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-328">No</span></span>          | <span data-ttu-id="cd0f0-329">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="cd0f0-330">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-330">Example</span></span>

<span data-ttu-id="cd0f0-331">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **DeleteFunction** usado para mapear a função Delete da Associação **CourseInstructor** para o procedimento armazenado **DeleteCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="cd0f0-332">O procedimento armazenado **DeleteCourseInstructor** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a><span data-ttu-id="cd0f0-333">Elemento EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="cd0f0-334">O elemento **EndProperty** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma função End ou uma modificação de uma associação de modelo conceitual e o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="cd0f0-335">O mapeamento de coluna de propriedade é especificado em um elemento ScalarProperty filho.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="cd0f0-336">Quando um elemento **EndProperty** é usado para definir o mapeamento para o final de uma associação de modelo conceitual, ele é um filho de um elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="cd0f0-337">Quando o elemento **EndProperty** é usado para definir o mapeamento para uma função de modificação de uma associação de modelo conceitual, ele é um filho de um elemento InsertFunction ou um elemento DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="cd0f0-338">O elemento **EndProperty** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-339">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-340">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-340">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-341">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **EndProperty** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="cd0f0-342">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-342">Attribute Name</span></span> | <span data-ttu-id="cd0f0-343">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-343">Is Required</span></span> | <span data-ttu-id="cd0f0-344">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="cd0f0-345">Nome</span><span class="sxs-lookup"><span data-stu-id="cd0f0-345">Name</span></span>           | <span data-ttu-id="cd0f0-346">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-346">Yes</span></span>         | <span data-ttu-id="cd0f0-347">O nome da extremidade de associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-348">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-348">Example</span></span>

<span data-ttu-id="cd0f0-349">O exemplo a seguir mostra um elemento **AssociationSetMapping** no qual o **curso de\_FK\_** Associação de departamento no modelo conceitual é mapeado para a tabela do **curso** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="cd0f0-350">Os mapeamentos entre as propriedades do tipo de associação e as colunas de tabela são especificados nos elementos filho **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a><span data-ttu-id="cd0f0-351">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-351">Example</span></span>

<span data-ttu-id="cd0f0-352">O exemplo a seguir mostra o elemento **EndProperty** que mapeia as funções INSERT e Delete de uma associação (**CourseInstructor**) para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="cd0f0-353">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-353">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="cd0f0-354">Elemento EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-355">O elemento **EntityContainerMapping** na MSL (linguagem de especificação de mapeamento) mapeia o contêiner de entidade no modelo conceitual para o contêiner de entidade no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="cd0f0-356">O elemento **EntityContainerMapping** é um filho do elemento Mapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="cd0f0-357">O elemento **EntityContainerMapping** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="cd0f0-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd0f0-358">EntitySetMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-359">AssociationSetMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-360">FunctionImportMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-361">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-361">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-362">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="cd0f0-363">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-363">Attribute Name</span></span>            | <span data-ttu-id="cd0f0-364">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-364">Is Required</span></span> | <span data-ttu-id="cd0f0-365">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-366">**StorageModelContainer**</span></span> | <span data-ttu-id="cd0f0-367">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-367">Yes</span></span>         | <span data-ttu-id="cd0f0-368">O nome do contêiner de entidade do modelo de armazenamento que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="cd0f0-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="cd0f0-370">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-370">Yes</span></span>         | <span data-ttu-id="cd0f0-371">O nome do contêiner de entidade do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="cd0f0-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="cd0f0-373">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-373">No</span></span>          | <span data-ttu-id="cd0f0-374">**True** ou **False**.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-374">**True** or **False**.</span></span> <span data-ttu-id="cd0f0-375">Se **for false**, nenhum modo de exibição de atualização será gerado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="cd0f0-376">Esse atributo deve ser definido como **false** quando você tem um mapeamento somente leitura que seria inválido porque os dados não podem fazer viagens de ida e volta com êxito.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="cd0f0-377">O valor padrão é **True**.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-378">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-378">Example</span></span>

<span data-ttu-id="cd0f0-379">O exemplo a seguir mostra um elemento **EntityContainerMapping** que mapeia o contêiner **SchoolModelEntities** (o contêiner de entidade modelo conceitual) para o contêiner **SchoolModelStoreContainer** (o contêiner de entidade modelo de armazenamento):</span><span class="sxs-lookup"><span data-stu-id="cd0f0-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="cd0f0-380">Elemento EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-381">O elemento **EntitySetMapping** no mapear Language (MSL) mapeia todos os tipos em uma entidade de modelo conceitual definida para conjuntos de entidades no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="cd0f0-382">Um conjunto de entidades no modelo conceitual é um contêiner lógico para instâncias de entidades do mesmo tipo (e tipos derivados).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="cd0f0-383">Uma entidade definida no modelo de armazenamento representa uma tabela ou exibição no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="cd0f0-384">O conjunto de entidades do modelo conceitual é especificado pelo valor do atributo **Name** do elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="cd0f0-385">A tabela ou exibição mapeada é especificada pelo atributo **StoreEntitySet** em cada elemento MappingFragment filho ou no elemento **EntitySetMapping** em si.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="cd0f0-386">O elemento **EntitySetMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-387">EntityTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-388">QueryView (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="cd0f0-389">MappingFragment (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-390">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-390">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-391">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="cd0f0-392">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-392">Attribute Name</span></span>           | <span data-ttu-id="cd0f0-393">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-393">Is Required</span></span> | <span data-ttu-id="cd0f0-394">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-395">**Nome**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-395">**Name**</span></span>                 | <span data-ttu-id="cd0f0-396">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-396">Yes</span></span>         | <span data-ttu-id="cd0f0-397">O nome do conjunto de entidades do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="cd0f0-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-398">**TypeName** **1**</span></span>       | <span data-ttu-id="cd0f0-399">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-399">No</span></span>          | <span data-ttu-id="cd0f0-400">O nome do tipo de entidade do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="cd0f0-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="cd0f0-402">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-402">No</span></span>          | <span data-ttu-id="cd0f0-403">O nome do conjunto de entidades do modelo de armazenamento que está sendo mapeado para.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="cd0f0-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="cd0f0-405">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-405">No</span></span>          | <span data-ttu-id="cd0f0-406">**True** ou **false** dependendo se apenas linhas distintas forem retornadas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="cd0f0-407">Se esse atributo for definido como **true**, o atributo **GenerateUpdateViews** do elemento EntityContainerMapping deverá ser definido como **false**.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="cd0f0-408">**1** os atributos **TypeName** e **StoreEntitySet** podem ser usados no lugar dos elementos filho EntityTypeMapping e MappingFragment para mapear um único tipo de entidade para uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="cd0f0-409">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-409">Example</span></span>

<span data-ttu-id="cd0f0-410">O exemplo a seguir mostra um elemento **EntitySetMapping** que mapeia três tipos (um tipo base e dois tipos derivados) no conjunto de entidades de **cursos** do modelo conceitual para três tabelas diferentes no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="cd0f0-411">As tabelas são especificadas pelo atributo **StoreEntitySet** em cada elemento **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="cd0f0-412">Elemento EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-413">O elemento **EntityTypeMapping** na MSL (mapeation Specification Language) define o mapeamento entre um tipo de entidade no modelo conceitual e as tabelas ou exibições no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="cd0f0-414">Para obter informações sobre tipos de entidade de modelo conceitual e tabelas ou exibições de banco de dados subjacentes, consulte elemento EntityType (CSDL) e elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="cd0f0-415">O tipo de entidade modelo conceitual que está sendo mapeado é especificado pelo atributo **TypeName** do elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="cd0f0-416">A tabela ou exibição que está sendo mapeada é especificada pelo atributo **StoreEntitySet** do elemento MappingFragment filho.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="cd0f0-417">O elemento filho ModificationFunctionMapping pode ser usado para mapear as funções INSERT, Update ou Delete de tipos de entidade para procedimentos armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="cd0f0-418">O elemento **EntityTypeMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-419">MappingFragment (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-420">ModificationFunctionMapping (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="cd0f0-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="cd0f0-421">ScalarProperty</span></span>
-   <span data-ttu-id="cd0f0-422">Condição</span><span class="sxs-lookup"><span data-stu-id="cd0f0-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-423">Os elementos **MappingFragment** e **ModificationFunctionMapping** não podem ser elementos filho do elemento **EntityTypeMapping** ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="cd0f0-424">Os elementos **ScalarProperty** e **Condition** só podem ser elementos filho do elemento **EntityTypeMapping** quando ele é usado dentro de um elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-425">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-425">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-426">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="cd0f0-427">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-427">Attribute Name</span></span> | <span data-ttu-id="cd0f0-428">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-428">Is Required</span></span> | <span data-ttu-id="cd0f0-429">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-430">**TypeName**</span></span>   | <span data-ttu-id="cd0f0-431">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-431">Yes</span></span>         | <span data-ttu-id="cd0f0-432">O nome qualificado do namespace do tipo de entidade do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="cd0f0-433">Se o tipo for abstrato ou um tipo derivado, o valor deverá ser `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-434">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-434">Example</span></span>

<span data-ttu-id="cd0f0-435">O exemplo a seguir mostra um elemento EntitySetMapping com dois elementos filho **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="cd0f0-436">No primeiro elemento **EntityTypeMapping** , o tipo de entidade **SchoolModel. Person** é mapeado para a tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="cd0f0-437">No segundo elemento **EntityTypeMapping** , a funcionalidade de atualização do tipo **SchoolModel. Person** é mapeada para um procedimento armazenado, **UpdatePerson**, no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="cd0f0-438">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-438">Example</span></span>

<span data-ttu-id="cd0f0-439">O exemplo a seguir mostra o mapeamento de uma hierarquia de tipo na qual o tipo de raiz é abstrato.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="cd0f0-440">Observe o uso da sintaxe de `IsOfType` para os atributos **TypeName** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="cd0f0-441">Elemento FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-442">O elemento **FunctionImportMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado ou uma função no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="cd0f0-443">As importações de função devem ser declaradas no modelo conceitual e os procedimentos armazenados devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="cd0f0-444">Para obter mais informações, consulte elemento FunctionImport (CSDL) e elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-445">Por padrão, se uma importação de função retornar um tipo de entidade de modelo conceitual ou tipo complexo, os nomes das colunas retornadas pelo procedimento armazenado subjacente devem corresponder exatamente aos nomes das propriedades no tipo de modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="cd0f0-446">Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, o mapeamento deverá ser definido em um elemento ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="cd0f0-447">O elemento **FunctionImportMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-448">ResultMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-449">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-449">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-450">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **FunctionImportMapping** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="cd0f0-451">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-451">Attribute Name</span></span>         | <span data-ttu-id="cd0f0-452">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-452">Is Required</span></span> | <span data-ttu-id="cd0f0-453">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-454">**FunctionImportName**</span></span> | <span data-ttu-id="cd0f0-455">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-455">Yes</span></span>         | <span data-ttu-id="cd0f0-456">O nome da função de importação no modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="cd0f0-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-457">**FunctionName**</span></span>       | <span data-ttu-id="cd0f0-458">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-458">Yes</span></span>         | <span data-ttu-id="cd0f0-459">O nome qualificado do namespace da função no modelo de armazenamento que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-460">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-460">Example</span></span>

<span data-ttu-id="cd0f0-461">O exemplo a seguir é baseado no modelo escolar.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-461">The following example is based on the School model.</span></span> <span data-ttu-id="cd0f0-462">Considere a seguinte função no modelo de armazenamento:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="cd0f0-463">Considere também essa importação de função no modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="cd0f0-464">O exemplo a seguir mostra um elemento **FunctionImportMapping** usado para mapear a função e a importação de função acima entre si:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="cd0f0-465">Elemento InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="cd0f0-466">O elemento **InsertFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Insert de um tipo de entidade ou associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="cd0f0-467">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="cd0f0-468">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-469">Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="cd0f0-470">O elemento **InsertFunction** pode ser filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping ou ao elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="cd0f0-471">InsertFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="cd0f0-472">Quando aplicado ao elemento EntityTypeMapping, o elemento **InsertFunction** mapeia a função Insert de um tipo de entidade no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="cd0f0-473">O elemento **InsertFunction** pode ter os seguintes elementos filho quando aplicados a um elemento **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="cd0f0-474">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-475">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-476">Resultbinding (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="cd0f0-477">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-478">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-478">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-479">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **InsertFunction** quando aplicados a um elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="cd0f0-480">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-480">Attribute Name</span></span>            | <span data-ttu-id="cd0f0-481">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-481">Is Required</span></span> | <span data-ttu-id="cd0f0-482">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-483">**FunctionName**</span></span>          | <span data-ttu-id="cd0f0-484">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-484">Yes</span></span>         | <span data-ttu-id="cd0f0-485">O nome qualificado do namespace do procedimento armazenado para o qual a função Insert é mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="cd0f0-486">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="cd0f0-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="cd0f0-488">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-488">No</span></span>          | <span data-ttu-id="cd0f0-489">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="cd0f0-490">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-490">Example</span></span>

<span data-ttu-id="cd0f0-491">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **InsertFunction** usado para mapear a função Insert do tipo de entidade Person para o procedimento armazenado **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="cd0f0-492">O procedimento armazenado **InsertPerson** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="cd0f0-493">InsertFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="cd0f0-494">Quando aplicado ao elemento AssociationSetMapping, o elemento **InsertFunction** mapeia a função Insert de uma associação no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="cd0f0-495">O elemento **InsertFunction** pode ter os seguintes elementos filho quando aplicado ao elemento **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="cd0f0-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="cd0f0-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-497">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-497">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-498">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **InsertFunction** quando ele é aplicado ao elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="cd0f0-499">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-499">Attribute Name</span></span>            | <span data-ttu-id="cd0f0-500">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-500">Is Required</span></span> | <span data-ttu-id="cd0f0-501">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-502">**FunctionName**</span></span>          | <span data-ttu-id="cd0f0-503">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-503">Yes</span></span>         | <span data-ttu-id="cd0f0-504">O nome qualificado do namespace do procedimento armazenado para o qual a função Insert é mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="cd0f0-505">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="cd0f0-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="cd0f0-507">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-507">No</span></span>          | <span data-ttu-id="cd0f0-508">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="cd0f0-509">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-509">Example</span></span>

<span data-ttu-id="cd0f0-510">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **InsertFunction** usado para mapear a função Insert da Associação **CourseInstructor** para o procedimento armazenado **InsertCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="cd0f0-511">O procedimento armazenado **InsertCourseInstructor** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a><span data-ttu-id="cd0f0-512">Elemento Mapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-513">O elemento **Mapping** na MSL (Mapping Specification Language) contém informações para mapear objetos que são definidos em um modelo conceitual para um banco de dados (conforme descrito em um modelo de armazenamento).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="cd0f0-514">Para obter mais informações, consulte Especificação de CSDL e especificação de SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="cd0f0-515">O elemento **Mapping** é o elemento raiz para uma especificação de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="cd0f0-516">O namespace XML para especificações de mapeamento é https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="cd0f0-517">O elemento Mapping pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="cd0f0-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd0f0-518">Alias (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-519">EntityContainerMapping (exatamente um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="cd0f0-520">Os nomes dos tipos de modelo conceitual e de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="cd0f0-521">Para obter informações sobre o nome do namespace do modelo conceitual, consulte elemento Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="cd0f0-522">Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte elemento de esquema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="cd0f0-523">Aliases para namespaces que são usados em MSL podem ser definidos com o elemento alias.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-524">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-524">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-525">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Mapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="cd0f0-526">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-526">Attribute Name</span></span> | <span data-ttu-id="cd0f0-527">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-527">Is Required</span></span> | <span data-ttu-id="cd0f0-528">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="cd0f0-529">**Espaço**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-529">**Space**</span></span>      | <span data-ttu-id="cd0f0-530">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-530">Yes</span></span>         | <span data-ttu-id="cd0f0-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-531">**C-S**.</span></span> <span data-ttu-id="cd0f0-532">Este é um valor fixo e não pode ser alterado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-533">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-533">Example</span></span>

<span data-ttu-id="cd0f0-534">O exemplo a seguir mostra um elemento **Mapping** que é baseado em parte do modelo escolar.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="cd0f0-535">Para obter mais informações sobre o modelo escolar, consulte início rápido (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="cd0f0-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="cd0f0-536">Elemento MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="cd0f0-537">O elemento **MappingFragment** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre as propriedades de um tipo de entidade de modelo conceitual e uma tabela ou exibição no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="cd0f0-538">Para obter informações sobre tipos de entidade de modelo conceitual e tabelas ou exibições de banco de dados subjacentes, consulte elemento EntityType (CSDL) e elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="cd0f0-539">O **MappingFragment** pode ser um elemento filho do elemento EntityTypeMapping ou do elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="cd0f0-540">O elemento **MappingFragment** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-541">ComplexType (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-542">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-543">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-544">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-544">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-545">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="cd0f0-546">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-546">Attribute Name</span></span>          | <span data-ttu-id="cd0f0-547">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-547">Is Required</span></span> | <span data-ttu-id="cd0f0-548">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="cd0f0-550">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-550">Yes</span></span>         | <span data-ttu-id="cd0f0-551">O nome da tabela ou exibição que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="cd0f0-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="cd0f0-553">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-553">No</span></span>          | <span data-ttu-id="cd0f0-554">**True** ou **false** dependendo se apenas linhas distintas forem retornadas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="cd0f0-555">Se esse atributo for definido como **true**, o atributo **GenerateUpdateViews** do elemento EntityContainerMapping deverá ser definido como **false**.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-556">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-556">Example</span></span>

<span data-ttu-id="cd0f0-557">O exemplo a seguir mostra um elemento **MappingFragment** como o filho de um elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="cd0f0-558">Neste exemplo, as propriedades do tipo de **curso** no modelo conceitual são mapeadas para colunas da tabela do **curso** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="cd0f0-559">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-559">Example</span></span>

<span data-ttu-id="cd0f0-560">O exemplo a seguir mostra um elemento **MappingFragment** como o filho de um elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="cd0f0-561">Como no exemplo acima, as propriedades do tipo de **curso** no modelo conceitual são mapeadas para colunas da tabela do **curso** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="cd0f0-562">Elemento ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-563">O elemento **ModificationFunctionMapping** na MSL (linguagem de especificação de mapeamento) mapeia as funções de inserção, atualização e exclusão de um tipo de entidade modelo conceitual para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="cd0f0-564">O elemento **ModificationFunctionMapping** também pode mapear as funções INSERT e Delete para associações muitos para muitos no modelo conceitual para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="cd0f0-565">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="cd0f0-566">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-567">Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="cd0f0-568">Se as funções de modificação para uma entidade em uma hierarquia de herança forem mapeadas para procedimentos armazenados, as funções de modificação para todos os tipos na hierarquia deverão ser mapeadas para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="cd0f0-569">O elemento **ModificationFunctionMapping** pode ser filho do elemento EntityTypeMapping ou do elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="cd0f0-570">O elemento **ModificationFunctionMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-571">DeleteFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="cd0f0-572">InsertFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="cd0f0-573">UpdateFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="cd0f0-574">Nenhum atributo é aplicável ao elemento **ModificationFunctionMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="cd0f0-575">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-575">Example</span></span>

<span data-ttu-id="cd0f0-576">O exemplo a seguir mostra o mapeamento do conjunto de entidades para a entidade **pessoas** definida no modelo School.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="cd0f0-577">Além do mapeamento de coluna para o tipo de entidade **Person** , o mapeamento das funções INSERT, Update e Delete do tipo **Person** é mostrado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="cd0f0-578">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-578">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="cd0f0-579">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-579">Example</span></span>

<span data-ttu-id="cd0f0-580">O exemplo a seguir mostra o mapeamento do conjunto de associação para o conjunto de associação **CourseInstructor** no modelo School.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="cd0f0-581">Além do mapeamento de coluna para a associação **CourseInstructor** , o mapeamento das funções INSERT e Delete da Associação **CourseInstructor** é mostrado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="cd0f0-582">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-582">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="cd0f0-583">Elemento QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="cd0f0-584">O elemento **QueryView** na MSL (linguagem de especificação de mapeamento) define um mapeamento somente leitura entre um tipo de entidade ou associação no modelo conceitual e uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="cd0f0-585">O mapeamento é definido com uma consulta Entity SQL que é avaliada em relação ao modelo de armazenamento e você expressa o conjunto de resultados em termos de uma entidade ou associação no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="cd0f0-586">Como as exibições de consulta são somente leitura, você não pode usar comandos de atualização padrão para atualizar tipos que são definidos por exibições de consulta.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="cd0f0-587">Você pode fazer atualizações para esses tipos usando funções de modificação.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="cd0f0-588">Para obter mais informações, consulte como mapear funções de modificação para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-589">No elemento **QueryView** , Entity SQL expressões que contêm **GroupBy**, agregações de grupo ou propriedades de navegação não têm suporte.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="cd0f0-590">O elemento **QueryView** pode ser filho do elemento EntitySetMapping ou do elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="cd0f0-591">No primeiro caso, a exibição de consulta define um mapeamento somente leitura para uma entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="cd0f0-592">No último caso, a exibição de consulta define um mapeamento somente leitura para uma associação no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-593">Se o elemento **AssociationSetMapping** for para uma associação com uma restrição referencial, o elemento **AssociationSetMapping** será ignorado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="cd0f0-594">Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="cd0f0-595">O elemento **QueryView** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-596">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-596">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-597">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="cd0f0-598">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-598">Attribute Name</span></span> | <span data-ttu-id="cd0f0-599">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-599">Is Required</span></span> | <span data-ttu-id="cd0f0-600">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-601">**TypeName**</span></span>   | <span data-ttu-id="cd0f0-602">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-602">No</span></span>          | <span data-ttu-id="cd0f0-603">O nome do tipo de modelo conceitual que está sendo mapeado pelo modo de exibição de consulta.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-604">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-604">Example</span></span>

<span data-ttu-id="cd0f0-605">O exemplo a seguir mostra o elemento **QueryView** como um filho do elemento **EntitySetMapping** e define um mapeamento de exibição de consulta para o tipo de entidade **Department** no modelo School.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

<span data-ttu-id="cd0f0-606">Como a consulta retorna apenas um subconjunto dos membros do tipo de **Departamento** no modelo de armazenamento, o tipo de **Departamento** no modelo escolar foi modificado com base nesse mapeamento da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a><span data-ttu-id="cd0f0-607">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-607">Example</span></span>

<span data-ttu-id="cd0f0-608">O exemplo a seguir mostra o elemento **QueryView** como o filho de um elemento **AssociationSetMapping** e define um mapeamento somente leitura para a associação de `FK_Course_Department` no modelo escolar.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a><span data-ttu-id="cd0f0-609">Comentários</span><span class="sxs-lookup"><span data-stu-id="cd0f0-609">Comments</span></span>

<span data-ttu-id="cd0f0-610">Você pode definir modos de exibição de consulta para habilitar os seguintes cenários:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="cd0f0-611">Defina uma entidade no modelo conceitual que não inclua todas as propriedades da entidade no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="cd0f0-612">Isso inclui propriedades que não têm valores padrão e não oferecem suporte a valores **nulos** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="cd0f0-613">Mapeie colunas computadas no modelo de armazenamento para propriedades de tipos de entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="cd0f0-614">Defina um mapeamento em que as condições usadas para particionar entidades no modelo conceitual não são baseadas na igualdade.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="cd0f0-615">Quando você especifica um mapeamento condicional usando o elemento **Condition** , a condição fornecida deve ser igual ao valor especificado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="cd0f0-616">Para obter mais informações, consulte elemento Condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="cd0f0-617">Mapeie a mesma coluna no modelo de armazenamento para vários tipos no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="cd0f0-618">Mapeie vários tipos para a mesma tabela.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="cd0f0-619">Defina associações no modelo conceitual que não se baseiam em chaves estrangeiras no esquema relacional.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="cd0f0-620">Use a lógica de negócios personalizada para definir o valor das propriedades no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="cd0f0-621">Por exemplo, você pode mapear o valor de cadeia de caracteres "T" na fonte de dados para um valor de **true**, um booliano, no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="cd0f0-622">Defina filtros condicionais para resultados da consulta.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="cd0f0-623">Impor menos restrições aos dados no modelo conceitual do que no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="cd0f0-624">Por exemplo, você poderia tornar uma propriedade no modelo conceitual anulável mesmo se a coluna para a qual ela está mapeada não oferecer suporte a valores **nulos**.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="cd0f0-625">As seguintes considerações se aplicam quando você define modos de exibição de consulta para entidades:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="cd0f0-626">As exibições de consulta são somente leitura.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-626">Query views are read-only.</span></span> <span data-ttu-id="cd0f0-627">Você só pode fazer atualizações em entidades usando funções de modificação.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="cd0f0-628">Ao definir um tipo de entidade por um modo de exibição de consulta, você também deve definir todas as entidades relacionadas por exibições de consulta.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="cd0f0-629">Quando você mapeia uma associação muitos para muitos para uma entidade no modelo de armazenamento que representa uma tabela de vínculo no esquema relacional, você deve definir um elemento **QueryView** no elemento **AssociationSetMapping** para essa tabela de vínculo.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="cd0f0-630">As exibições de consulta devem ser definidas para todos os tipos em uma hierarquia de tipo.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="cd0f0-631">Você pode fazer isso das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="cd0f0-632">Com um único elemento **QueryView** que especifica uma única consulta de Entity SQL que retorna uma União de todos os tipos de entidade na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="cd0f0-633">Com um único elemento **QueryView** que especifica uma única consulta Entity SQL que usa o operador case para retornar um tipo de entidade específico na hierarquia com base em uma condição específica.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="cd0f0-634">Com um elemento **QueryView** adicional para um tipo específico na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="cd0f0-635">Nesse caso, use o atributo **TypeName** do elemento **QueryView** para especificar o tipo de entidade para cada exibição.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="cd0f0-636">Quando um modo de exibição de consulta é definido, você não pode especificar o atributo **StorageSetName** no elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="cd0f0-637">Quando um modo de exibição de consulta é definido, o elemento **EntitySetMapping**também não pode conter mapeamentos de **Propriedade** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="cd0f0-638">Elemento resultbinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="cd0f0-639">O elemento **resultbinding** no MSL (mapear Language Specification) mapeia valores de coluna que são retornados por procedimentos armazenados para propriedades de entidade no modelo conceitual quando funções de modificação de tipo de entidade são mapeadas para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="cd0f0-640">Por exemplo, quando o valor de uma coluna de identidade é retornado por um procedimento armazenado INSERT, o elemento **resultbinding** mapeia o valor retornado para uma propriedade de tipo de entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="cd0f0-641">O elemento **resultbinding** pode ser filho do elemento InsertFunction ou do elemento UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="cd0f0-642">O elemento **resultbinding** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-643">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-643">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-644">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **resultbinding** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="cd0f0-645">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-645">Attribute Name</span></span> | <span data-ttu-id="cd0f0-646">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-646">Is Required</span></span> | <span data-ttu-id="cd0f0-647">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-648">**Nome**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-648">**Name**</span></span>       | <span data-ttu-id="cd0f0-649">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-649">Yes</span></span>         | <span data-ttu-id="cd0f0-650">O nome da propriedade de entidade no modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="cd0f0-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-651">**ColumnName**</span></span> | <span data-ttu-id="cd0f0-652">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-652">Yes</span></span>         | <span data-ttu-id="cd0f0-653">O nome da coluna que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="cd0f0-654">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-654">Example</span></span>

<span data-ttu-id="cd0f0-655">O exemplo a seguir é baseado no modelo escolar e mostra um elemento **InsertFunction** usado para mapear a função Insert do tipo de entidade **Person** para o procedimento armazenado **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="cd0f0-656">(O procedimento armazenado **InsertPerson** é mostrado abaixo e é declarado no modelo de armazenamento.) Um elemento **resultbinding** é usado para mapear um valor de coluna que é retornado pelo procedimento armazenado (**NewPersonID**) para uma propriedade de tipo de entidade (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

<span data-ttu-id="cd0f0-657">O Transact-SQL a seguir descreve o procedimento armazenado **InsertPerson** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a><span data-ttu-id="cd0f0-658">Elemento ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="cd0f0-659">O elemento **ResultMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado no banco de dados subjacente quando o seguinte é verdadeiro:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="cd0f0-660">A função Import retorna um tipo de entidade de modelo conceitual ou tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="cd0f0-661">Os nomes das colunas retornadas pelo procedimento armazenado não correspondem exatamente aos nomes das propriedades no tipo de entidade ou tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="cd0f0-662">Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo de entidade ou tipo complexo baseia-se nos nomes de coluna e propriedade.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="cd0f0-663">Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, você deverá usar o elemento **ResultMapping** para definir o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="cd0f0-664">Para obter um exemplo do mapeamento padrão, consulte elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="cd0f0-665">O elemento **ResultMapping** é um elemento filho do elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="cd0f0-666">O elemento **ResultMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-667">EntityTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-668">ComplexTypeMapping</span></span>

<span data-ttu-id="cd0f0-669">Nenhum atributo é aplicável ao elemento **ResultMapping** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="cd0f0-670">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-670">Example</span></span>

<span data-ttu-id="cd0f0-671">Considere o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-671">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="cd0f0-672">Considere também o seguinte tipo de entidade de modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-672">Also consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

<span data-ttu-id="cd0f0-673">Para criar uma importação de função que retorne instâncias do tipo de entidade anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um elemento **ResultMapping** :</span><span class="sxs-lookup"><span data-stu-id="cd0f0-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="cd0f0-674">Elemento ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="cd0f0-675">O elemento **ScalarProperty** na MSL (linguagem de especificação de mapeamento) mapeia uma propriedade em um tipo de entidade de modelo conceitual, tipo complexo ou associação a uma coluna de tabela ou parâmetro de procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0f0-676">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="cd0f0-677">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="cd0f0-678">O elemento **ScalarProperty** pode ser um filho dos seguintes elementos:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="cd0f0-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="cd0f0-679">MappingFragment</span></span>
-   <span data-ttu-id="cd0f0-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="cd0f0-680">InsertFunction</span></span>
-   <span data-ttu-id="cd0f0-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="cd0f0-681">UpdateFunction</span></span>
-   <span data-ttu-id="cd0f0-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="cd0f0-682">DeleteFunction</span></span>
-   <span data-ttu-id="cd0f0-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="cd0f0-683">EndProperty</span></span>
-   <span data-ttu-id="cd0f0-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="cd0f0-684">ComplexProperty</span></span>
-   <span data-ttu-id="cd0f0-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="cd0f0-685">ResultMapping</span></span>

<span data-ttu-id="cd0f0-686">Como um filho do elemento **MappingFragment**, **ComplexProperty**ou **EndProperty** , o elemento **ScalarProperty** mapeia uma propriedade no modelo conceitual para uma coluna no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="cd0f0-687">Como um filho do elemento **InsertFunction**, **UpdateFunction**ou **DeleteFunction** , o elemento **ScalarProperty** mapeia uma propriedade no modelo conceitual para um parâmetro de procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="cd0f0-688">O elemento **ScalarProperty** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-689">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-689">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-690">Os atributos que se aplicam ao elemento **ScalarProperty** diferem de acordo com a função do elemento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="cd0f0-691">A tabela a seguir descreve os atributos que são aplicáveis quando o elemento **ScalarProperty** é usado para mapear uma propriedade de modelo conceitual para uma coluna no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="cd0f0-692">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-692">Attribute Name</span></span> | <span data-ttu-id="cd0f0-693">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-693">Is Required</span></span> | <span data-ttu-id="cd0f0-694">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="cd0f0-695">**Nome**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-695">**Name**</span></span>       | <span data-ttu-id="cd0f0-696">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-696">Yes</span></span>         | <span data-ttu-id="cd0f0-697">O nome da Propriedade do modelo conceitual que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="cd0f0-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-698">**ColumnName**</span></span> | <span data-ttu-id="cd0f0-699">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-699">Yes</span></span>         | <span data-ttu-id="cd0f0-700">O nome da coluna de tabela que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="cd0f0-701">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ScalarProperty** quando ele é usado para mapear uma propriedade de modelo conceitual para um parâmetro de procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="cd0f0-702">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-702">Attribute Name</span></span>    | <span data-ttu-id="cd0f0-703">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-703">Is Required</span></span> | <span data-ttu-id="cd0f0-704">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-705">**Nome**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-705">**Name**</span></span>          | <span data-ttu-id="cd0f0-706">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-706">Yes</span></span>         | <span data-ttu-id="cd0f0-707">O nome da Propriedade do modelo conceitual que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="cd0f0-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-708">**ParameterName**</span></span> | <span data-ttu-id="cd0f0-709">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-709">Yes</span></span>         | <span data-ttu-id="cd0f0-710">O nome do parâmetro que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="cd0f0-711">**Versão**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-711">**Version**</span></span>       | <span data-ttu-id="cd0f0-712">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-712">No</span></span>          | <span data-ttu-id="cd0f0-713">**Atual** ou **original** , dependendo se o valor atual ou o valor original da propriedade deve ser usado para verificações de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="cd0f0-714">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-714">Example</span></span>

<span data-ttu-id="cd0f0-715">O exemplo a seguir mostra o elemento **ScalarProperty** usado de duas maneiras:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="cd0f0-716">Para mapear as propriedades do tipo de entidade **Person** para as colunas da tabela **Person**.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="cd0f0-717">Para mapear as propriedades do tipo de entidade **Person** para os parâmetros do procedimento armazenado **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="cd0f0-718">Os procedimentos armazenados são declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-718">The stored procedures are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="cd0f0-719">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-719">Example</span></span>

<span data-ttu-id="cd0f0-720">O exemplo a seguir mostra o elemento **ScalarProperty** usado para mapear as funções INSERT e Delete de uma associação de modelo conceitual para procedimentos armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="cd0f0-721">Os procedimentos armazenados são declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-721">The stored procedures are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a><span data-ttu-id="cd0f0-722">Elemento UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="cd0f0-723">O elemento **UpdateFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Update de um tipo de entidade no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="cd0f0-724">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="cd0f0-725">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd0f0-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="cd0f0-726">Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="cd0f0-727">O elemento **UpdateFunction** pode ser filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="cd0f0-728">O elemento **UpdateFunction** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="cd0f0-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd0f0-729">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-730">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="cd0f0-731">Resultbinding (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="cd0f0-732">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="cd0f0-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd0f0-733">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="cd0f0-733">Applicable Attributes</span></span>

<span data-ttu-id="cd0f0-734">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **UpdateFunction** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="cd0f0-735">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-735">Attribute Name</span></span>            | <span data-ttu-id="cd0f0-736">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="cd0f0-736">Is Required</span></span> | <span data-ttu-id="cd0f0-737">Valor</span><span class="sxs-lookup"><span data-stu-id="cd0f0-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd0f0-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-738">**FunctionName**</span></span>          | <span data-ttu-id="cd0f0-739">Sim</span><span class="sxs-lookup"><span data-stu-id="cd0f0-739">Yes</span></span>         | <span data-ttu-id="cd0f0-740">O nome qualificado do namespace do procedimento armazenado para o qual a função Update é mapeada.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="cd0f0-741">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="cd0f0-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="cd0f0-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="cd0f0-743">Não</span><span class="sxs-lookup"><span data-stu-id="cd0f0-743">No</span></span>          | <span data-ttu-id="cd0f0-744">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="cd0f0-745">Exemplo</span><span class="sxs-lookup"><span data-stu-id="cd0f0-745">Example</span></span>

<span data-ttu-id="cd0f0-746">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **UpdateFunction** usado para mapear a função Update do tipo de entidade **Person** para o procedimento armazenado **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="cd0f0-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="cd0f0-747">O procedimento armazenado **UpdatePerson** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="cd0f0-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
