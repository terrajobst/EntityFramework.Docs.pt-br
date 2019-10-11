---
title: Especificação MSL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182561"
---
# <a name="msl-specification"></a><span data-ttu-id="469c6-102">Especificação de MSL</span><span class="sxs-lookup"><span data-stu-id="469c6-102">MSL Specification</span></span>
<span data-ttu-id="469c6-103">O MSL (Mapping Specification Language) é uma linguagem baseada em XML que descreve o mapeamento entre o modelo conceitual e o modelo de armazenamento de um aplicativo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="469c6-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="469c6-104">Em um aplicativo Entity Framework, os metadados de mapeamento são carregados de um arquivo. MSL (escrito em MSL) no momento da compilação.</span><span class="sxs-lookup"><span data-stu-id="469c6-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="469c6-105">O Entity Framework usa metadados de mapeamento em tempo de execução para converter consultas em relação ao modelo conceitual para armazenar comandos específicos.</span><span class="sxs-lookup"><span data-stu-id="469c6-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="469c6-106">O Entity Framework Designer (EF designer) armazena informações de mapeamento em um arquivo. edmx em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="469c6-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="469c6-107">No momento da compilação, o Entity Designer usa informações em um arquivo. edmx para criar o arquivo. MSL que é necessário pelo Entity Framework em tempo de execução</span><span class="sxs-lookup"><span data-stu-id="469c6-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="469c6-108">Os nomes de todos os tipos de modelo conceituais ou de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace.</span><span class="sxs-lookup"><span data-stu-id="469c6-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="469c6-109">Para obter informações sobre o nome do namespace do modelo conceitual, consulte [especificação CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="469c6-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="469c6-110">Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte [especificação de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="469c6-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="469c6-111">As versões do MSL são diferenciadas por namespaces XML.</span><span class="sxs-lookup"><span data-stu-id="469c6-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="469c6-112">Versão do MSL</span><span class="sxs-lookup"><span data-stu-id="469c6-112">MSL Version</span></span> | <span data-ttu-id="469c6-113">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="469c6-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="469c6-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="469c6-114">MSL v1</span></span>      | <span data-ttu-id="469c6-115">urn: schemas-microsoft-com: Windows: Storage: Mapping: CS</span><span class="sxs-lookup"><span data-stu-id="469c6-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="469c6-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="469c6-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="469c6-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="469c6-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="469c6-118">Elemento alias (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-118">Alias Element (MSL)</span></span>

<span data-ttu-id="469c6-119">O elemento **alias** no MSL (Mapping Specification Language) é um filho do elemento Mapping que é usado para definir aliases para namespaces de modelo conceitual e modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="469c6-120">Os nomes de todos os tipos de modelo conceituais ou de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace.</span><span class="sxs-lookup"><span data-stu-id="469c6-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="469c6-121">Para obter informações sobre o nome do namespace do modelo conceitual, consulte elemento Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="469c6-122">Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte elemento de esquema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="469c6-123">O elemento **alias** não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="469c6-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-124">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-124">Applicable Attributes</span></span>

<span data-ttu-id="469c6-125">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **alias** .</span><span class="sxs-lookup"><span data-stu-id="469c6-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="469c6-126">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-126">Attribute Name</span></span> | <span data-ttu-id="469c6-127">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-127">Is Required</span></span> | <span data-ttu-id="469c6-128">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="469c6-129">**Chave**</span><span class="sxs-lookup"><span data-stu-id="469c6-129">**Key**</span></span>        | <span data-ttu-id="469c6-130">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-130">Yes</span></span>         | <span data-ttu-id="469c6-131">O alias para o namespace que é especificado pelo atributo **Value** .</span><span class="sxs-lookup"><span data-stu-id="469c6-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="469c6-132">**Valor**</span><span class="sxs-lookup"><span data-stu-id="469c6-132">**Value**</span></span>      | <span data-ttu-id="469c6-133">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-133">Yes</span></span>         | <span data-ttu-id="469c6-134">O namespace para o qual o valor do elemento de **chave** é um alias.</span><span class="sxs-lookup"><span data-stu-id="469c6-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="469c6-135">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-135">Example</span></span>

<span data-ttu-id="469c6-136">O exemplo a seguir mostra um elemento **alias** que define um alias, `c`, para tipos que são definidos no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

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

## <a name="associationend-element-msl"></a><span data-ttu-id="469c6-137">Elemento AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="469c6-138">O elemento **AssociationEnd** na MSL (linguagem de especificação de mapeamento) é usado quando as funções de modificação de um tipo de entidade no modelo conceitual são mapeadas para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="469c6-139">Se um procedimento armazenado de modificação usa um parâmetro cujo valor é mantido em uma propriedade de associação, o elemento **AssociationEnd** mapeia o valor da propriedade para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="469c6-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="469c6-140">Para obter mais informações, consulte o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="469c6-140">For more information, see the example below.</span></span>

<span data-ttu-id="469c6-141">Para obter mais informações sobre como mapear funções de modificação de tipos de entidade para procedimentos armazenados, consulte ModificationFunctionMapping Element (MSL) e Walkthrough: Mapeando uma entidade para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="469c6-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="469c6-142">O elemento **AssociationEnd** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="469c6-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-144">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-144">Applicable Attributes</span></span>

<span data-ttu-id="469c6-145">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="469c6-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="469c6-146">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-146">Attribute Name</span></span>     | <span data-ttu-id="469c6-147">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-147">Is Required</span></span> | <span data-ttu-id="469c6-148">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="469c6-149">**AssociationSet**</span></span> | <span data-ttu-id="469c6-150">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-150">Yes</span></span>         | <span data-ttu-id="469c6-151">O nome da associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="469c6-152">**From**</span><span class="sxs-lookup"><span data-stu-id="469c6-152">**From**</span></span>           | <span data-ttu-id="469c6-153">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-153">Yes</span></span>         | <span data-ttu-id="469c6-154">O valor do atributo **FromRole** da propriedade de navegação que corresponde à associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="469c6-155">Para obter mais informações, consulte o elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="469c6-156">**To**</span><span class="sxs-lookup"><span data-stu-id="469c6-156">**To**</span></span>             | <span data-ttu-id="469c6-157">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-157">Yes</span></span>         | <span data-ttu-id="469c6-158">O valor do atributo **ToRole** da propriedade de navegação que corresponde à associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="469c6-159">Para obter mais informações, consulte o elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="469c6-160">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-160">Example</span></span>

<span data-ttu-id="469c6-161">Considere o seguinte tipo de entidade de modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="469c6-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="469c6-162">Considere também o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="469c6-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="469c6-163">Para mapear a função Update da entidade `Course` para esse procedimento armazenado, você deve fornecer um valor para o parâmetro **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="469c6-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="469c6-164">O valor de `DepartmentID` não corresponde a uma propriedade no tipo de entidade; Ele está contido em uma associação independente cujo mapeamento é mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="469c6-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="469c6-165">O código a seguir mostra o elemento **AssociationEnd** usado para mapear a propriedade **DepartmentID** da Associação **FK @ no__t-3Course @ no__t-4Department** para o procedimento armazenado **UpdateCourse** (para o qual a função Update do O tipo de entidade do **curso** é mapeado):</span><span class="sxs-lookup"><span data-stu-id="469c6-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="469c6-166">Elemento AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="469c6-167">O elemento **AssociationSetMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma associação no modelo conceitual e colunas de tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="469c6-168">As associações no modelo conceitual são tipos cujas propriedades representam as colunas de chave primária e estrangeira no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="469c6-169">O elemento **AssociationSetMapping** usa dois elementos EndProperty para definir os mapeamentos entre as propriedades e as colunas do tipo de associação no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="469c6-170">Você pode posicionar as condições nesses mapeamentos com o elemento Condition.</span><span class="sxs-lookup"><span data-stu-id="469c6-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="469c6-171">Mapeie as funções INSERT, Update e Delete para associações a procedimentos armazenados no banco de dados com o elemento ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="469c6-172">Defina mapeamentos somente leitura entre associações e colunas de tabela usando uma Entity SQL cadeia de caracteres em um elemento QueryView.</span><span class="sxs-lookup"><span data-stu-id="469c6-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-173">Se uma restrição referencial for definida para uma associação no modelo conceitual, a associação não precisará ser mapeada com um elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="469c6-174">Se um elemento **AssociationSetMapping** estiver presente para uma associação que tenha uma restrição referencial, os mapeamentos definidos no elemento **AssociationSetMapping** serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="469c6-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="469c6-175">Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="469c6-176">O elemento **AssociationSetMapping** pode ter os seguintes elementos filho</span><span class="sxs-lookup"><span data-stu-id="469c6-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="469c6-177">QueryView (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="469c6-178">EndProperty (zero ou dois)</span><span class="sxs-lookup"><span data-stu-id="469c6-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="469c6-179">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="469c6-180">ModificationFunctionMapping (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-181">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-181">Applicable Attributes</span></span>

<span data-ttu-id="469c6-182">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="469c6-183">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-183">Attribute Name</span></span>     | <span data-ttu-id="469c6-184">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-184">Is Required</span></span> | <span data-ttu-id="469c6-185">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="469c6-186">**Name**</span></span>           | <span data-ttu-id="469c6-187">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-187">Yes</span></span>         | <span data-ttu-id="469c6-188">O nome do conjunto de associação do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="469c6-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="469c6-189">**TypeName**</span></span>       | <span data-ttu-id="469c6-190">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-190">No</span></span>          | <span data-ttu-id="469c6-191">O nome qualificado do namespace do tipo de associação de modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="469c6-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="469c6-192">**StoreEntitySet**</span></span> | <span data-ttu-id="469c6-193">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-193">No</span></span>          | <span data-ttu-id="469c6-194">O nome da tabela que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="469c6-195">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-195">Example</span></span>

<span data-ttu-id="469c6-196">O exemplo a seguir mostra um elemento **AssociationSetMapping** no qual o conjunto de associação **FK @ no__t-2Course @ no__t-3Department** no modelo conceitual é mapeado para a tabela do **curso** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="469c6-197">Os mapeamentos entre as propriedades do tipo de associação e as colunas de tabela são especificados nos elementos filho **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="469c6-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="469c6-198">Elemento ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="469c6-199">Um elemento **ComplexProperty** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma propriedade de tipo complexo em um tipo de entidade de modelo conceitual e colunas de tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="469c6-200">Os mapeamentos de coluna de propriedade são especificados em elementos ScalarProperty filho.</span><span class="sxs-lookup"><span data-stu-id="469c6-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="469c6-201">O elemento de propriedade **complexType** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-202">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="469c6-203">**ComplexProperty** (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="469c6-204">ComplextTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="469c6-205">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-206">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-206">Applicable Attributes</span></span>

<span data-ttu-id="469c6-207">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ComplexProperty** :</span><span class="sxs-lookup"><span data-stu-id="469c6-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="469c6-208">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-208">Attribute Name</span></span> | <span data-ttu-id="469c6-209">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-209">Is Required</span></span> | <span data-ttu-id="469c6-210">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="469c6-211">**Name**</span></span>       | <span data-ttu-id="469c6-212">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-212">Yes</span></span>         | <span data-ttu-id="469c6-213">O nome da propriedade complexa de um tipo de entidade no modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="469c6-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="469c6-214">**TypeName**</span></span>   | <span data-ttu-id="469c6-215">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-215">No</span></span>          | <span data-ttu-id="469c6-216">O nome qualificado do namespace do tipo de Propriedade do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="469c6-217">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-217">Example</span></span>

<span data-ttu-id="469c6-218">O exemplo a seguir é baseado no modelo escolar.</span><span class="sxs-lookup"><span data-stu-id="469c6-218">The following example is based on the School model.</span></span> <span data-ttu-id="469c6-219">O seguinte tipo complexo foi adicionado ao modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="469c6-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="469c6-220">As propriedades **LastName** e **FirstName** do tipo de entidade **Person** foram substituídas por uma propriedade complexa, **Name**:</span><span class="sxs-lookup"><span data-stu-id="469c6-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="469c6-221">O seguinte MSL mostra o elemento **ComplexProperty** usado para mapear a propriedade **Name** para colunas no banco de dados subjacente:</span><span class="sxs-lookup"><span data-stu-id="469c6-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="469c6-222">Elemento ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="469c6-223">O elemento **ComplexTypeMapping** na MSL (linguagem de especificação de mapeamento) é um filho do elemento ResultMapping e define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado no banco de dados subjacente quando o seguinte são verdadeiros:</span><span class="sxs-lookup"><span data-stu-id="469c6-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="469c6-224">A função Import retorna um tipo complexo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="469c6-225">Os nomes das colunas retornadas pelo procedimento armazenado não correspondem exatamente aos nomes das propriedades no tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="469c6-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="469c6-226">Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo complexo baseia-se nos nomes de coluna e propriedade.</span><span class="sxs-lookup"><span data-stu-id="469c6-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="469c6-227">Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, você deverá usar o elemento **ComplexTypeMapping** para definir o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="469c6-228">Para obter um exemplo do mapeamento padrão, consulte elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="469c6-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="469c6-229">O elemento **ComplexTypeMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-230">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-231">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-231">Applicable Attributes</span></span>

<span data-ttu-id="469c6-232">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ComplexTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="469c6-233">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-233">Attribute Name</span></span> | <span data-ttu-id="469c6-234">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-234">Is Required</span></span> | <span data-ttu-id="469c6-235">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="469c6-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="469c6-236">**TypeName**</span></span>   | <span data-ttu-id="469c6-237">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-237">Yes</span></span>         | <span data-ttu-id="469c6-238">O nome qualificado do namespace do tipo complexo que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-239">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-239">Example</span></span>

<span data-ttu-id="469c6-240">Considere o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="469c6-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="469c6-241">Considere também o seguinte tipo complexo de modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="469c6-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="469c6-242">Para criar uma importação de função que retorne instâncias do tipo complexo anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um elemento **ComplexTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="469c6-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="469c6-243">Elemento Condition (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-243">Condition Element (MSL)</span></span>

<span data-ttu-id="469c6-244">O elemento **Condition** no MSL (Mapping Specification Language) coloca condições em mapeamentos entre o modelo conceitual e o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="469c6-245">O mapeamento definido dentro de um nó XML será válido se todas as condições, conforme especificado nos elementos de **condição** filho, forem atendidas.</span><span class="sxs-lookup"><span data-stu-id="469c6-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="469c6-246">Caso contrário, o mapeamento não será válido.</span><span class="sxs-lookup"><span data-stu-id="469c6-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="469c6-247">Por exemplo, se um elemento MappingFragment contiver um ou mais elementos filho **Condition** , o mapeamento definido no nó **MappingFragment** só será válido se todas as condições dos elementos de **condição** filho forem atendidas.</span><span class="sxs-lookup"><span data-stu-id="469c6-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="469c6-248">Cada condição pode se aplicar a um **nome** (o nome de uma propriedade de entidade modelo conceitual, especificada pelo atributo **Name** ) ou um **ColumnName** (o nome de uma coluna no banco de dados, especificado pelo atributo **ColumnName** ).</span><span class="sxs-lookup"><span data-stu-id="469c6-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="469c6-249">Quando o atributo **Name** é definido, a condição é verificada em relação a um valor de propriedade de entidade.</span><span class="sxs-lookup"><span data-stu-id="469c6-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="469c6-250">Quando o atributo **ColumnName** é definido, a condição é verificada em relação a um valor de coluna.</span><span class="sxs-lookup"><span data-stu-id="469c6-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="469c6-251">Somente um dos atributos **Name** ou **ColumnName** pode ser especificado em um elemento **Condition** .</span><span class="sxs-lookup"><span data-stu-id="469c6-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-252">Quando o elemento **Condition** é usado em um elemento FunctionImportMapping, somente o atributo **Name** não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="469c6-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="469c6-253">O elemento **Condition** pode ser um filho dos seguintes elementos:</span><span class="sxs-lookup"><span data-stu-id="469c6-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="469c6-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="469c6-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="469c6-255">ComplexProperty</span></span>
-   <span data-ttu-id="469c6-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-256">EntitySetMapping</span></span>
-   <span data-ttu-id="469c6-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="469c6-257">MappingFragment</span></span>
-   <span data-ttu-id="469c6-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-258">EntityTypeMapping</span></span>

<span data-ttu-id="469c6-259">O elemento **Condition** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="469c6-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-260">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-260">Applicable Attributes</span></span>

<span data-ttu-id="469c6-261">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **Condition** :</span><span class="sxs-lookup"><span data-stu-id="469c6-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="469c6-262">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-262">Attribute Name</span></span> | <span data-ttu-id="469c6-263">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-263">Is Required</span></span> | <span data-ttu-id="469c6-264">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="469c6-265">**ColumnName**</span></span> | <span data-ttu-id="469c6-266">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-266">No</span></span>          | <span data-ttu-id="469c6-267">O nome da coluna de tabela cujo valor é usado para avaliar a condição.</span><span class="sxs-lookup"><span data-stu-id="469c6-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="469c6-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="469c6-268">**IsNull**</span></span>     | <span data-ttu-id="469c6-269">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-269">No</span></span>          | <span data-ttu-id="469c6-270">**True** ou **False**.</span><span class="sxs-lookup"><span data-stu-id="469c6-270">**True** or **False**.</span></span> <span data-ttu-id="469c6-271">Se o valor for **true** e o valor da coluna for **NULL**ou se o valor for **false** e o valor da coluna não for **NULL**, a condição será true.</span><span class="sxs-lookup"><span data-stu-id="469c6-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="469c6-272">Caso contrário, a condição será falsa.</span><span class="sxs-lookup"><span data-stu-id="469c6-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="469c6-273">Os atributos **IsNull** e **Value** não podem ser usados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="469c6-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="469c6-274">**Valor**</span><span class="sxs-lookup"><span data-stu-id="469c6-274">**Value**</span></span>      | <span data-ttu-id="469c6-275">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-275">No</span></span>          | <span data-ttu-id="469c6-276">O valor com o qual o valor da coluna é comparado.</span><span class="sxs-lookup"><span data-stu-id="469c6-276">The value with which the column value is compared.</span></span> <span data-ttu-id="469c6-277">Se os valores forem iguais, a condição será verdadeira.</span><span class="sxs-lookup"><span data-stu-id="469c6-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="469c6-278">Caso contrário, a condição será falsa.</span><span class="sxs-lookup"><span data-stu-id="469c6-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="469c6-279">Os atributos **IsNull** e **Value** não podem ser usados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="469c6-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="469c6-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="469c6-280">**Name**</span></span>       | <span data-ttu-id="469c6-281">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-281">No</span></span>          | <span data-ttu-id="469c6-282">O nome da propriedade de entidade do modelo conceitual cujo valor é usado para avaliar a condição.</span><span class="sxs-lookup"><span data-stu-id="469c6-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="469c6-283">Esse atributo não será aplicável se o elemento **Condition** for usado dentro de um elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="469c6-284">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-284">Example</span></span>

<span data-ttu-id="469c6-285">O exemplo a seguir mostra os elementos **Condition** como filhos dos elementos **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="469c6-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="469c6-286">Quando **HireDate** não é nulo e **EnrollmentDate** é nulo, os dados são mapeados entre o tipo **SchoolModel. instrutor** e as colunas **PersonID** e **HireDate** da tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="469c6-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="469c6-287">Quando **EnrollmentDate** não é nulo e **HireDate** é NULL, os dados são mapeados entre o tipo **SchoolModel. Student** e as colunas **PersonID** e **registro** da tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="469c6-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="469c6-288">Elemento DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="469c6-289">O elemento **DeleteFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Delete de um tipo de entidade ou associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="469c6-290">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="469c6-291">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-292">Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.</span><span class="sxs-lookup"><span data-stu-id="469c6-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="469c6-293">DeleteFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="469c6-294">Quando aplicado ao elemento EntityTypeMapping, o elemento **DeleteFunction** mapeia a função Delete de um tipo de entidade no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="469c6-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="469c6-295">O elemento **DeleteFunction** pode ter os seguintes elementos filho quando aplicados a um elemento **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="469c6-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="469c6-296">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="469c6-297">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="469c6-298">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="469c6-299">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-299">Applicable Attributes</span></span>

<span data-ttu-id="469c6-300">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **DeleteFunction** quando ele é aplicado a um elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="469c6-301">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-301">Attribute Name</span></span>            | <span data-ttu-id="469c6-302">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-302">Is Required</span></span> | <span data-ttu-id="469c6-303">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="469c6-304">**FunctionName**</span></span>          | <span data-ttu-id="469c6-305">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-305">Yes</span></span>         | <span data-ttu-id="469c6-306">O nome qualificado do namespace do procedimento armazenado para o qual a função Delete está mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="469c6-307">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="469c6-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="469c6-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="469c6-309">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-309">No</span></span>          | <span data-ttu-id="469c6-310">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="469c6-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="469c6-311">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-311">Example</span></span>

<span data-ttu-id="469c6-312">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **DeleteFunction** mapeando a função Delete do tipo de entidade **Person** para o procedimento armazenado **DeletePerson** .</span><span class="sxs-lookup"><span data-stu-id="469c6-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="469c6-313">O procedimento armazenado **DeletePerson** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="469c6-314">DeleteFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="469c6-315">Quando aplicado ao elemento AssociationSetMapping, o elemento **DeleteFunction** mapeia a função Delete de uma associação no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="469c6-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="469c6-316">O elemento **DeleteFunction** pode ter os seguintes elementos filho quando aplicado ao elemento **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="469c6-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="469c6-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="469c6-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="469c6-318">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-318">Applicable Attributes</span></span>

<span data-ttu-id="469c6-319">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **DeleteFunction** quando ele é aplicado ao elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="469c6-320">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-320">Attribute Name</span></span>            | <span data-ttu-id="469c6-321">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-321">Is Required</span></span> | <span data-ttu-id="469c6-322">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="469c6-323">**FunctionName**</span></span>          | <span data-ttu-id="469c6-324">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-324">Yes</span></span>         | <span data-ttu-id="469c6-325">O nome qualificado do namespace do procedimento armazenado para o qual a função Delete está mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="469c6-326">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="469c6-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="469c6-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="469c6-328">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-328">No</span></span>          | <span data-ttu-id="469c6-329">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="469c6-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="469c6-330">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-330">Example</span></span>

<span data-ttu-id="469c6-331">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **DeleteFunction** usado para mapear a função Delete da Associação **CourseInstructor** para o procedimento armazenado **DeleteCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="469c6-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="469c6-332">O procedimento armazenado **DeleteCourseInstructor** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="469c6-333">Elemento EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="469c6-334">O elemento **EndProperty** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma função End ou uma modificação de uma associação de modelo conceitual e o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="469c6-335">O mapeamento de coluna de propriedade é especificado em um elemento ScalarProperty filho.</span><span class="sxs-lookup"><span data-stu-id="469c6-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="469c6-336">Quando um elemento **EndProperty** é usado para definir o mapeamento para o final de uma associação de modelo conceitual, ele é um filho de um elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="469c6-337">Quando o elemento **EndProperty** é usado para definir o mapeamento para uma função de modificação de uma associação de modelo conceitual, ele é um filho de um elemento InsertFunction ou um elemento DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="469c6-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="469c6-338">O elemento **EndProperty** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-339">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-340">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-340">Applicable Attributes</span></span>

<span data-ttu-id="469c6-341">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **EndProperty** :</span><span class="sxs-lookup"><span data-stu-id="469c6-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="469c6-342">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-342">Attribute Name</span></span> | <span data-ttu-id="469c6-343">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-343">Is Required</span></span> | <span data-ttu-id="469c6-344">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="469c6-345">Nome</span><span class="sxs-lookup"><span data-stu-id="469c6-345">Name</span></span>           | <span data-ttu-id="469c6-346">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-346">Yes</span></span>         | <span data-ttu-id="469c6-347">O nome da extremidade de associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-348">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-348">Example</span></span>

<span data-ttu-id="469c6-349">O exemplo a seguir mostra um elemento **AssociationSetMapping** no qual a associação de **CE @ no__t-2Course @ no__t-3Department** no modelo conceitual é mapeada para a tabela do **curso** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="469c6-350">Os mapeamentos entre as propriedades do tipo de associação e as colunas de tabela são especificados nos elementos filho **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="469c6-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="469c6-351">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-351">Example</span></span>

<span data-ttu-id="469c6-352">O exemplo a seguir mostra o elemento **EndProperty** que mapeia as funções INSERT e Delete de uma associação (**CourseInstructor**) para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="469c6-353">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="469c6-354">Elemento EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="469c6-355">O elemento **EntityContainerMapping** na MSL (linguagem de especificação de mapeamento) mapeia o contêiner de entidade no modelo conceitual para o contêiner de entidade no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="469c6-356">O elemento **EntityContainerMapping** é um filho do elemento Mapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="469c6-357">O elemento **EntityContainerMapping** pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="469c6-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="469c6-358">EntitySetMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="469c6-359">AssociationSetMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="469c6-360">FunctionImportMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-361">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-361">Applicable Attributes</span></span>

<span data-ttu-id="469c6-362">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="469c6-363">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-363">Attribute Name</span></span>            | <span data-ttu-id="469c6-364">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-364">Is Required</span></span> | <span data-ttu-id="469c6-365">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="469c6-366">**StorageModelContainer**</span></span> | <span data-ttu-id="469c6-367">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-367">Yes</span></span>         | <span data-ttu-id="469c6-368">O nome do contêiner de entidade do modelo de armazenamento que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="469c6-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="469c6-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="469c6-370">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-370">Yes</span></span>         | <span data-ttu-id="469c6-371">O nome do contêiner de entidade do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="469c6-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="469c6-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="469c6-373">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-373">No</span></span>          | <span data-ttu-id="469c6-374">**True** ou **False**.</span><span class="sxs-lookup"><span data-stu-id="469c6-374">**True** or **False**.</span></span> <span data-ttu-id="469c6-375">Se **for false**, nenhum modo de exibição de atualização será gerado.</span><span class="sxs-lookup"><span data-stu-id="469c6-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="469c6-376">Esse atributo deve ser definido como **false** quando você tem um mapeamento somente leitura que seria inválido porque os dados não podem fazer viagens de ida e volta com êxito.</span><span class="sxs-lookup"><span data-stu-id="469c6-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="469c6-377">O valor padrão é **true**.</span><span class="sxs-lookup"><span data-stu-id="469c6-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-378">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-378">Example</span></span>

<span data-ttu-id="469c6-379">O exemplo a seguir mostra um elemento **EntityContainerMapping** que mapeia o contêiner **SchoolModelEntities** (o contêiner de entidade modelo conceitual) para o contêiner **SchoolModelStoreContainer** (a entidade modelo de armazenamento contêiner):</span><span class="sxs-lookup"><span data-stu-id="469c6-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="469c6-380">Elemento EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="469c6-381">O elemento **EntitySetMapping** no mapear Language (MSL) mapeia todos os tipos em uma entidade de modelo conceitual definida para conjuntos de entidades no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="469c6-382">Um conjunto de entidades no modelo conceitual é um contêiner lógico para instâncias de entidades do mesmo tipo (e tipos derivados).</span><span class="sxs-lookup"><span data-stu-id="469c6-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="469c6-383">Uma entidade definida no modelo de armazenamento representa uma tabela ou exibição no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="469c6-384">O conjunto de entidades do modelo conceitual é especificado pelo valor do atributo **Name** do elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="469c6-385">A tabela ou exibição mapeada é especificada pelo atributo **StoreEntitySet** em cada elemento MappingFragment filho ou no elemento **EntitySetMapping** em si.</span><span class="sxs-lookup"><span data-stu-id="469c6-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="469c6-386">O elemento **EntitySetMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-387">EntityTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="469c6-388">QueryView (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="469c6-389">MappingFragment (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-390">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-390">Applicable Attributes</span></span>

<span data-ttu-id="469c6-391">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="469c6-392">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-392">Attribute Name</span></span>           | <span data-ttu-id="469c6-393">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-393">Is Required</span></span> | <span data-ttu-id="469c6-394">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="469c6-395">**Name**</span></span>                 | <span data-ttu-id="469c6-396">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-396">Yes</span></span>         | <span data-ttu-id="469c6-397">O nome do conjunto de entidades do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="469c6-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="469c6-398">**TypeName** **1**</span></span>       | <span data-ttu-id="469c6-399">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-399">No</span></span>          | <span data-ttu-id="469c6-400">O nome do tipo de entidade do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="469c6-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="469c6-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="469c6-402">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-402">No</span></span>          | <span data-ttu-id="469c6-403">O nome do conjunto de entidades do modelo de armazenamento que está sendo mapeado para.</span><span class="sxs-lookup"><span data-stu-id="469c6-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="469c6-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="469c6-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="469c6-405">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-405">No</span></span>          | <span data-ttu-id="469c6-406">**True** ou **false** dependendo se apenas linhas distintas forem retornadas.</span><span class="sxs-lookup"><span data-stu-id="469c6-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="469c6-407">Se esse atributo for definido como **true**, o atributo **GenerateUpdateViews** do elemento EntityContainerMapping deverá ser definido como **false**.</span><span class="sxs-lookup"><span data-stu-id="469c6-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="469c6-408">**1** os atributos **TypeName** e **StoreEntitySet** podem ser usados no lugar dos elementos filho EntityTypeMapping e MappingFragment para mapear um único tipo de entidade para uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="469c6-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="469c6-409">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-409">Example</span></span>

<span data-ttu-id="469c6-410">O exemplo a seguir mostra um elemento **EntitySetMapping** que mapeia três tipos (um tipo base e dois tipos derivados) no conjunto de entidades de **cursos** do modelo conceitual para três tabelas diferentes no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="469c6-411">As tabelas são especificadas pelo atributo **StoreEntitySet** em cada elemento **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="469c6-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="469c6-412">Elemento EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="469c6-413">O elemento **EntityTypeMapping** na MSL (mapeation Specification Language) define o mapeamento entre um tipo de entidade no modelo conceitual e as tabelas ou exibições no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="469c6-414">Para obter informações sobre tipos de entidade de modelo conceitual e tabelas ou exibições de banco de dados subjacentes, consulte elemento EntityType (CSDL) e elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="469c6-415">O tipo de entidade modelo conceitual que está sendo mapeado é especificado pelo atributo **TypeName** do elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="469c6-416">A tabela ou exibição que está sendo mapeada é especificada pelo atributo **StoreEntitySet** do elemento MappingFragment filho.</span><span class="sxs-lookup"><span data-stu-id="469c6-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="469c6-417">O elemento filho ModificationFunctionMapping pode ser usado para mapear as funções INSERT, Update ou Delete de tipos de entidade para procedimentos armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="469c6-418">O elemento **EntityTypeMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-419">MappingFragment (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="469c6-420">ModificationFunctionMapping (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="469c6-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="469c6-421">ScalarProperty</span></span>
-   <span data-ttu-id="469c6-422">Condição</span><span class="sxs-lookup"><span data-stu-id="469c6-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-423">Os elementos **MappingFragment** e **ModificationFunctionMapping** não podem ser elementos filho do elemento **EntityTypeMapping** ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="469c6-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="469c6-424">Os elementos **ScalarProperty** e **Condition** só podem ser elementos filho do elemento **EntityTypeMapping** quando ele é usado dentro de um elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-425">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-425">Applicable Attributes</span></span>

<span data-ttu-id="469c6-426">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="469c6-427">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-427">Attribute Name</span></span> | <span data-ttu-id="469c6-428">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-428">Is Required</span></span> | <span data-ttu-id="469c6-429">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="469c6-430">**TypeName**</span></span>   | <span data-ttu-id="469c6-431">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-431">Yes</span></span>         | <span data-ttu-id="469c6-432">O nome qualificado do namespace do tipo de entidade do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="469c6-433">Se o tipo for abstrato ou um tipo derivado, o valor deverá ser `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="469c6-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-434">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-434">Example</span></span>

<span data-ttu-id="469c6-435">O exemplo a seguir mostra um elemento EntitySetMapping com dois elementos filho **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="469c6-436">No primeiro elemento **EntityTypeMapping** , o tipo de entidade **SchoolModel. Person** é mapeado para a tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="469c6-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="469c6-437">No segundo elemento **EntityTypeMapping** , a funcionalidade de atualização do tipo **SchoolModel. Person** é mapeada para um procedimento armazenado, **UpdatePerson**, no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="469c6-438">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-438">Example</span></span>

<span data-ttu-id="469c6-439">O exemplo a seguir mostra o mapeamento de uma hierarquia de tipo na qual o tipo de raiz é abstrato.</span><span class="sxs-lookup"><span data-stu-id="469c6-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="469c6-440">Observe o uso da sintaxe `IsOfType` para os atributos **TypeName** .</span><span class="sxs-lookup"><span data-stu-id="469c6-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="469c6-441">Elemento FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="469c6-442">O elemento **FunctionImportMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado ou uma função no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="469c6-443">As importações de função devem ser declaradas no modelo conceitual e os procedimentos armazenados devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="469c6-444">Para obter mais informações, consulte elemento FunctionImport (CSDL) e elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-445">Por padrão, se uma importação de função retornar um tipo de entidade de modelo conceitual ou tipo complexo, os nomes das colunas retornadas pelo procedimento armazenado subjacente devem corresponder exatamente aos nomes das propriedades no tipo de modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="469c6-446">Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, o mapeamento deverá ser definido em um elemento ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="469c6-447">O elemento **FunctionImportMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-448">ResultMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-449">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-449">Applicable Attributes</span></span>

<span data-ttu-id="469c6-450">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **FunctionImportMapping** :</span><span class="sxs-lookup"><span data-stu-id="469c6-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="469c6-451">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-451">Attribute Name</span></span>         | <span data-ttu-id="469c6-452">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-452">Is Required</span></span> | <span data-ttu-id="469c6-453">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="469c6-454">**FunctionImportName**</span></span> | <span data-ttu-id="469c6-455">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-455">Yes</span></span>         | <span data-ttu-id="469c6-456">O nome da função de importação no modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="469c6-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="469c6-457">**FunctionName**</span></span>       | <span data-ttu-id="469c6-458">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-458">Yes</span></span>         | <span data-ttu-id="469c6-459">O nome qualificado do namespace da função no modelo de armazenamento que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-460">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-460">Example</span></span>

<span data-ttu-id="469c6-461">O exemplo a seguir é baseado no modelo escolar.</span><span class="sxs-lookup"><span data-stu-id="469c6-461">The following example is based on the School model.</span></span> <span data-ttu-id="469c6-462">Considere a seguinte função no modelo de armazenamento:</span><span class="sxs-lookup"><span data-stu-id="469c6-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="469c6-463">Considere também essa importação de função no modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="469c6-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="469c6-464">O exemplo a seguir mostra um elemento **FunctionImportMapping** usado para mapear a função e a importação de função acima entre si:</span><span class="sxs-lookup"><span data-stu-id="469c6-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="469c6-465">Elemento InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="469c6-466">O elemento **InsertFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Insert de um tipo de entidade ou associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="469c6-467">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="469c6-468">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-469">Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.</span><span class="sxs-lookup"><span data-stu-id="469c6-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="469c6-470">O elemento **InsertFunction** pode ser filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping ou ao elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="469c6-471">InsertFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="469c6-472">Quando aplicado ao elemento EntityTypeMapping, o elemento **InsertFunction** mapeia a função Insert de um tipo de entidade no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="469c6-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="469c6-473">O elemento **InsertFunction** pode ter os seguintes elementos filho quando aplicados a um elemento **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="469c6-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="469c6-474">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="469c6-475">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="469c6-476">Resultbinding (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="469c6-477">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="469c6-478">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-478">Applicable Attributes</span></span>

<span data-ttu-id="469c6-479">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **InsertFunction** quando aplicados a um elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="469c6-480">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-480">Attribute Name</span></span>            | <span data-ttu-id="469c6-481">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-481">Is Required</span></span> | <span data-ttu-id="469c6-482">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="469c6-483">**FunctionName**</span></span>          | <span data-ttu-id="469c6-484">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-484">Yes</span></span>         | <span data-ttu-id="469c6-485">O nome qualificado do namespace do procedimento armazenado para o qual a função Insert é mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="469c6-486">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="469c6-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="469c6-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="469c6-488">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-488">No</span></span>          | <span data-ttu-id="469c6-489">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="469c6-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="469c6-490">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-490">Example</span></span>

<span data-ttu-id="469c6-491">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **InsertFunction** usado para mapear a função Insert do tipo de entidade Person para o procedimento armazenado **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="469c6-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="469c6-492">O procedimento armazenado **InsertPerson** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="469c6-493">InsertFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="469c6-494">Quando aplicado ao elemento AssociationSetMapping, o elemento **InsertFunction** mapeia a função Insert de uma associação no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="469c6-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="469c6-495">O elemento **InsertFunction** pode ter os seguintes elementos filho quando aplicado ao elemento **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="469c6-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="469c6-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="469c6-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="469c6-497">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-497">Applicable Attributes</span></span>

<span data-ttu-id="469c6-498">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **InsertFunction** quando ele é aplicado ao elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="469c6-499">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-499">Attribute Name</span></span>            | <span data-ttu-id="469c6-500">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-500">Is Required</span></span> | <span data-ttu-id="469c6-501">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="469c6-502">**FunctionName**</span></span>          | <span data-ttu-id="469c6-503">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-503">Yes</span></span>         | <span data-ttu-id="469c6-504">O nome qualificado do namespace do procedimento armazenado para o qual a função Insert é mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="469c6-505">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="469c6-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="469c6-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="469c6-507">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-507">No</span></span>          | <span data-ttu-id="469c6-508">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="469c6-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="469c6-509">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-509">Example</span></span>

<span data-ttu-id="469c6-510">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **InsertFunction** usado para mapear a função Insert da Associação **CourseInstructor** para o procedimento armazenado **InsertCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="469c6-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="469c6-511">O procedimento armazenado **InsertCourseInstructor** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="469c6-512">Elemento Mapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="469c6-513">O elemento **Mapping** na MSL (Mapping Specification Language) contém informações para mapear objetos que são definidos em um modelo conceitual para um banco de dados (conforme descrito em um modelo de armazenamento).</span><span class="sxs-lookup"><span data-stu-id="469c6-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="469c6-514">Para obter mais informações, consulte Especificação de CSDL e especificação de SSDL.</span><span class="sxs-lookup"><span data-stu-id="469c6-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="469c6-515">O elemento **Mapping** é o elemento raiz para uma especificação de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="469c6-516">O namespace XML para especificações de mapeamento é https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="469c6-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="469c6-517">O elemento Mapping pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="469c6-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="469c6-518">Alias (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="469c6-519">EntityContainerMapping (exatamente um)</span><span class="sxs-lookup"><span data-stu-id="469c6-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="469c6-520">Os nomes dos tipos de modelo conceitual e de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace.</span><span class="sxs-lookup"><span data-stu-id="469c6-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="469c6-521">Para obter informações sobre o nome do namespace do modelo conceitual, consulte elemento Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="469c6-522">Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte elemento de esquema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="469c6-523">Aliases para namespaces que são usados em MSL podem ser definidos com o elemento alias.</span><span class="sxs-lookup"><span data-stu-id="469c6-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-524">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-524">Applicable Attributes</span></span>

<span data-ttu-id="469c6-525">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Mapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="469c6-526">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-526">Attribute Name</span></span> | <span data-ttu-id="469c6-527">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-527">Is Required</span></span> | <span data-ttu-id="469c6-528">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="469c6-529">**Espaço**</span><span class="sxs-lookup"><span data-stu-id="469c6-529">**Space**</span></span>      | <span data-ttu-id="469c6-530">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-530">Yes</span></span>         | <span data-ttu-id="469c6-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="469c6-531">**C-S**.</span></span> <span data-ttu-id="469c6-532">Este é um valor fixo e não pode ser alterado.</span><span class="sxs-lookup"><span data-stu-id="469c6-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-533">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-533">Example</span></span>

<span data-ttu-id="469c6-534">O exemplo a seguir mostra um elemento **Mapping** que é baseado em parte do modelo escolar.</span><span class="sxs-lookup"><span data-stu-id="469c6-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="469c6-535">Para obter mais informações sobre o modelo escolar, consulte início rápido (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="469c6-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="469c6-536">Elemento MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="469c6-537">O elemento **MappingFragment** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre as propriedades de um tipo de entidade de modelo conceitual e uma tabela ou exibição no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="469c6-538">Para obter informações sobre tipos de entidade de modelo conceitual e tabelas ou exibições de banco de dados subjacentes, consulte elemento EntityType (CSDL) e elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="469c6-539">O **MappingFragment** pode ser um elemento filho do elemento EntityTypeMapping ou do elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="469c6-540">O elemento **MappingFragment** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-541">ComplexType (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="469c6-542">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="469c6-543">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-544">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-544">Applicable Attributes</span></span>

<span data-ttu-id="469c6-545">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="469c6-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="469c6-546">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-546">Attribute Name</span></span>          | <span data-ttu-id="469c6-547">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-547">Is Required</span></span> | <span data-ttu-id="469c6-548">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="469c6-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="469c6-550">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-550">Yes</span></span>         | <span data-ttu-id="469c6-551">O nome da tabela ou exibição que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="469c6-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="469c6-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="469c6-553">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-553">No</span></span>          | <span data-ttu-id="469c6-554">**True** ou **false** dependendo se apenas linhas distintas forem retornadas.</span><span class="sxs-lookup"><span data-stu-id="469c6-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="469c6-555">Se esse atributo for definido como **true**, o atributo **GenerateUpdateViews** do elemento EntityContainerMapping deverá ser definido como **false**.</span><span class="sxs-lookup"><span data-stu-id="469c6-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-556">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-556">Example</span></span>

<span data-ttu-id="469c6-557">O exemplo a seguir mostra um elemento **MappingFragment** como o filho de um elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="469c6-558">Neste exemplo, as propriedades do tipo de **curso** no modelo conceitual são mapeadas para colunas da tabela do **curso** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="469c6-559">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-559">Example</span></span>

<span data-ttu-id="469c6-560">O exemplo a seguir mostra um elemento **MappingFragment** como o filho de um elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="469c6-561">Como no exemplo acima, as propriedades do tipo de **curso** no modelo conceitual são mapeadas para colunas da tabela do **curso** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="469c6-562">Elemento ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="469c6-563">O elemento **ModificationFunctionMapping** na MSL (linguagem de especificação de mapeamento) mapeia as funções de inserção, atualização e exclusão de um tipo de entidade modelo conceitual para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="469c6-564">O elemento **ModificationFunctionMapping** também pode mapear as funções INSERT e Delete para associações muitos para muitos no modelo conceitual para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="469c6-565">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="469c6-566">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-567">Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.</span><span class="sxs-lookup"><span data-stu-id="469c6-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="469c6-568">Se as funções de modificação para uma entidade em uma hierarquia de herança forem mapeadas para procedimentos armazenados, as funções de modificação para todos os tipos na hierarquia deverão ser mapeadas para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="469c6-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="469c6-569">O elemento **ModificationFunctionMapping** pode ser filho do elemento EntityTypeMapping ou do elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="469c6-570">O elemento **ModificationFunctionMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-571">DeleteFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="469c6-572">InsertFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="469c6-573">UpdateFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="469c6-574">Nenhum atributo é aplicável ao elemento **ModificationFunctionMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="469c6-575">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-575">Example</span></span>

<span data-ttu-id="469c6-576">O exemplo a seguir mostra o mapeamento do conjunto de entidades para a entidade **pessoas** definida no modelo School.</span><span class="sxs-lookup"><span data-stu-id="469c6-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="469c6-577">Além do mapeamento de coluna para o tipo de entidade **Person** , o mapeamento das funções INSERT, Update e Delete do tipo **Person** é mostrado.</span><span class="sxs-lookup"><span data-stu-id="469c6-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="469c6-578">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="469c6-579">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-579">Example</span></span>

<span data-ttu-id="469c6-580">O exemplo a seguir mostra o mapeamento do conjunto de associação para o conjunto de associação **CourseInstructor** no modelo School.</span><span class="sxs-lookup"><span data-stu-id="469c6-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="469c6-581">Além do mapeamento de coluna para a associação **CourseInstructor** , o mapeamento das funções INSERT e Delete da Associação **CourseInstructor** é mostrado.</span><span class="sxs-lookup"><span data-stu-id="469c6-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="469c6-582">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="469c6-583">Elemento QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="469c6-584">O elemento **QueryView** na MSL (linguagem de especificação de mapeamento) define um mapeamento somente leitura entre um tipo de entidade ou associação no modelo conceitual e uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="469c6-585">O mapeamento é definido com uma consulta Entity SQL que é avaliada em relação ao modelo de armazenamento e você expressa o conjunto de resultados em termos de uma entidade ou associação no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="469c6-586">Como as exibições de consulta são somente leitura, você não pode usar comandos de atualização padrão para atualizar tipos que são definidos por exibições de consulta.</span><span class="sxs-lookup"><span data-stu-id="469c6-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="469c6-587">Você pode fazer atualizações para esses tipos usando funções de modificação.</span><span class="sxs-lookup"><span data-stu-id="469c6-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="469c6-588">Para obter mais informações, confira Como Mapear funções de modificação para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="469c6-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-589">No elemento **QueryView** , Entity SQL expressões que contêm **GroupBy**, agregações de grupo ou propriedades de navegação não têm suporte.</span><span class="sxs-lookup"><span data-stu-id="469c6-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="469c6-590">O elemento **QueryView** pode ser filho do elemento EntitySetMapping ou do elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="469c6-591">No primeiro caso, a exibição de consulta define um mapeamento somente leitura para uma entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="469c6-592">No último caso, a exibição de consulta define um mapeamento somente leitura para uma associação no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-593">Se o elemento **AssociationSetMapping** for para uma associação com uma restrição referencial, o elemento **AssociationSetMapping** será ignorado.</span><span class="sxs-lookup"><span data-stu-id="469c6-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="469c6-594">Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="469c6-595">O elemento **QueryView** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="469c6-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-596">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-596">Applicable Attributes</span></span>

<span data-ttu-id="469c6-597">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="469c6-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="469c6-598">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-598">Attribute Name</span></span> | <span data-ttu-id="469c6-599">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-599">Is Required</span></span> | <span data-ttu-id="469c6-600">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="469c6-601">**TypeName**</span></span>   | <span data-ttu-id="469c6-602">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-602">No</span></span>          | <span data-ttu-id="469c6-603">O nome do tipo de modelo conceitual que está sendo mapeado pelo modo de exibição de consulta.</span><span class="sxs-lookup"><span data-stu-id="469c6-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-604">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-604">Example</span></span>

<span data-ttu-id="469c6-605">O exemplo a seguir mostra o elemento **QueryView** como um filho do elemento **EntitySetMapping** e define um mapeamento de exibição de consulta para o tipo de entidade **Department** no modelo School.</span><span class="sxs-lookup"><span data-stu-id="469c6-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="469c6-606">Como a consulta retorna apenas um subconjunto dos membros do tipo de **Departamento** no modelo de armazenamento, o tipo de **Departamento** no modelo escolar foi modificado com base nesse mapeamento da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="469c6-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="469c6-607">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-607">Example</span></span>

<span data-ttu-id="469c6-608">O exemplo a seguir mostra o elemento **QueryView** como o filho de um elemento **AssociationSetMapping** e define um mapeamento somente leitura para a associação `FK_Course_Department` no modelo School.</span><span class="sxs-lookup"><span data-stu-id="469c6-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="469c6-609">Comentários</span><span class="sxs-lookup"><span data-stu-id="469c6-609">Comments</span></span>

<span data-ttu-id="469c6-610">Você pode definir modos de exibição de consulta para habilitar os seguintes cenários:</span><span class="sxs-lookup"><span data-stu-id="469c6-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="469c6-611">Defina uma entidade no modelo conceitual que não inclua todas as propriedades da entidade no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="469c6-612">Isso inclui propriedades que não têm valores padrão e não oferecem suporte a valores **nulos** .</span><span class="sxs-lookup"><span data-stu-id="469c6-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="469c6-613">Mapeie colunas computadas no modelo de armazenamento para propriedades de tipos de entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="469c6-614">Defina um mapeamento em que as condições usadas para particionar entidades no modelo conceitual não são baseadas na igualdade.</span><span class="sxs-lookup"><span data-stu-id="469c6-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="469c6-615">Quando você especifica um mapeamento condicional usando o elemento **Condition** , a condição fornecida deve ser igual ao valor especificado.</span><span class="sxs-lookup"><span data-stu-id="469c6-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="469c6-616">Para obter mais informações, consulte elemento Condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="469c6-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="469c6-617">Mapeie a mesma coluna no modelo de armazenamento para vários tipos no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="469c6-618">Mapeie vários tipos para a mesma tabela.</span><span class="sxs-lookup"><span data-stu-id="469c6-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="469c6-619">Defina associações no modelo conceitual que não se baseiam em chaves estrangeiras no esquema relacional.</span><span class="sxs-lookup"><span data-stu-id="469c6-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="469c6-620">Use a lógica de negócios personalizada para definir o valor das propriedades no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="469c6-621">Por exemplo, você pode mapear o valor de cadeia de caracteres "T" na fonte de dados para um valor de **true**, um booliano, no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="469c6-622">Defina filtros condicionais para resultados da consulta.</span><span class="sxs-lookup"><span data-stu-id="469c6-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="469c6-623">Impor menos restrições aos dados no modelo conceitual do que no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="469c6-624">Por exemplo, você poderia tornar uma propriedade no modelo conceitual anulável mesmo se a coluna para a qual ela está mapeada não oferecer suporte a valores **nulos**.</span><span class="sxs-lookup"><span data-stu-id="469c6-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="469c6-625">As seguintes considerações se aplicam quando você define modos de exibição de consulta para entidades:</span><span class="sxs-lookup"><span data-stu-id="469c6-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="469c6-626">As exibições de consulta são somente leitura.</span><span class="sxs-lookup"><span data-stu-id="469c6-626">Query views are read-only.</span></span> <span data-ttu-id="469c6-627">Você só pode fazer atualizações em entidades usando funções de modificação.</span><span class="sxs-lookup"><span data-stu-id="469c6-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="469c6-628">Ao definir um tipo de entidade por um modo de exibição de consulta, você também deve definir todas as entidades relacionadas por exibições de consulta.</span><span class="sxs-lookup"><span data-stu-id="469c6-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="469c6-629">Quando você mapeia uma associação muitos para muitos para uma entidade no modelo de armazenamento que representa uma tabela de vínculo no esquema relacional, você deve definir um elemento **QueryView** no elemento **AssociationSetMapping** para essa tabela de vínculo.</span><span class="sxs-lookup"><span data-stu-id="469c6-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="469c6-630">As exibições de consulta devem ser definidas para todos os tipos em uma hierarquia de tipo.</span><span class="sxs-lookup"><span data-stu-id="469c6-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="469c6-631">Você pode fazer isso das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="469c6-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="469c6-632">Com um único elemento **QueryView** que especifica uma única consulta de Entity SQL que retorna uma União de todos os tipos de entidade na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="469c6-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="469c6-633">Com um único elemento **QueryView** que especifica uma única consulta Entity SQL que usa o operador case para retornar um tipo de entidade específico na hierarquia com base em uma condição específica.</span><span class="sxs-lookup"><span data-stu-id="469c6-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="469c6-634">Com um elemento **QueryView** adicional para um tipo específico na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="469c6-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="469c6-635">Nesse caso, use o atributo **TypeName** do elemento **QueryView** para especificar o tipo de entidade para cada exibição.</span><span class="sxs-lookup"><span data-stu-id="469c6-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="469c6-636">Quando um modo de exibição de consulta é definido, você não pode especificar o atributo **StorageSetName** no elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="469c6-637">Quando um modo de exibição de consulta é definido, o elemento **EntitySetMapping**também não pode conter mapeamentos de **Propriedade** .</span><span class="sxs-lookup"><span data-stu-id="469c6-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="469c6-638">Elemento resultbinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="469c6-639">O elemento **resultbinding** no MSL (mapear Language Specification) mapeia valores de coluna que são retornados por procedimentos armazenados para propriedades de entidade no modelo conceitual quando as funções de modificação de tipo de entidade são mapeadas para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="469c6-640">Por exemplo, quando o valor de uma coluna de identidade é retornado por um procedimento armazenado INSERT, o elemento **resultbinding** mapeia o valor retornado para uma propriedade de tipo de entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="469c6-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="469c6-641">O elemento **resultbinding** pode ser filho do elemento InsertFunction ou do elemento UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="469c6-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="469c6-642">O elemento **resultbinding** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="469c6-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-643">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-643">Applicable Attributes</span></span>

<span data-ttu-id="469c6-644">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **resultbinding** :</span><span class="sxs-lookup"><span data-stu-id="469c6-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="469c6-645">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-645">Attribute Name</span></span> | <span data-ttu-id="469c6-646">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-646">Is Required</span></span> | <span data-ttu-id="469c6-647">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="469c6-648">**Name**</span></span>       | <span data-ttu-id="469c6-649">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-649">Yes</span></span>         | <span data-ttu-id="469c6-650">O nome da propriedade de entidade no modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="469c6-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="469c6-651">**ColumnName**</span></span> | <span data-ttu-id="469c6-652">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-652">Yes</span></span>         | <span data-ttu-id="469c6-653">O nome da coluna que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="469c6-654">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-654">Example</span></span>

<span data-ttu-id="469c6-655">O exemplo a seguir é baseado no modelo escolar e mostra um elemento **InsertFunction** usado para mapear a função Insert do tipo de entidade **Person** para o procedimento armazenado **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="469c6-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="469c6-656">(O procedimento armazenado **InsertPerson** é mostrado abaixo e é declarado no modelo de armazenamento.) Um elemento **resultbinding** é usado para mapear um valor de coluna que é retornado pelo procedimento armazenado (**NewPersonID**) para uma propriedade de tipo de entidade (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="469c6-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="469c6-657">O Transact-SQL a seguir descreve o procedimento armazenado **InsertPerson** :</span><span class="sxs-lookup"><span data-stu-id="469c6-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="469c6-658">Elemento ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="469c6-659">O elemento **ResultMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado no banco de dados subjacente quando o seguinte é verdadeiro:</span><span class="sxs-lookup"><span data-stu-id="469c6-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="469c6-660">A função Import retorna um tipo de entidade de modelo conceitual ou tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="469c6-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="469c6-661">Os nomes das colunas retornadas pelo procedimento armazenado não correspondem exatamente aos nomes das propriedades no tipo de entidade ou tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="469c6-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="469c6-662">Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo de entidade ou tipo complexo baseia-se nos nomes de coluna e propriedade.</span><span class="sxs-lookup"><span data-stu-id="469c6-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="469c6-663">Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, você deverá usar o elemento **ResultMapping** para definir o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="469c6-664">Para obter um exemplo do mapeamento padrão, consulte elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="469c6-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="469c6-665">O elemento **ResultMapping** é um elemento filho do elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="469c6-666">O elemento **ResultMapping** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-667">EntityTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="469c6-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-668">ComplexTypeMapping</span></span>

<span data-ttu-id="469c6-669">Nenhum atributo é aplicável ao elemento **ResultMapping** .</span><span class="sxs-lookup"><span data-stu-id="469c6-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="469c6-670">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-670">Example</span></span>

<span data-ttu-id="469c6-671">Considere o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="469c6-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="469c6-672">Considere também o seguinte tipo de entidade de modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="469c6-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="469c6-673">Para criar uma importação de função que retorne instâncias do tipo de entidade anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um elemento **ResultMapping** :</span><span class="sxs-lookup"><span data-stu-id="469c6-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="469c6-674">Elemento ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="469c6-675">O elemento **ScalarProperty** na MSL (linguagem de especificação de mapeamento) mapeia uma propriedade em um tipo de entidade de modelo conceitual, tipo complexo ou associação a uma coluna de tabela ou parâmetro de procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="469c6-676">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="469c6-677">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="469c6-678">O elemento **ScalarProperty** pode ser um filho dos seguintes elementos:</span><span class="sxs-lookup"><span data-stu-id="469c6-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="469c6-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="469c6-679">MappingFragment</span></span>
-   <span data-ttu-id="469c6-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="469c6-680">InsertFunction</span></span>
-   <span data-ttu-id="469c6-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="469c6-681">UpdateFunction</span></span>
-   <span data-ttu-id="469c6-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="469c6-682">DeleteFunction</span></span>
-   <span data-ttu-id="469c6-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="469c6-683">EndProperty</span></span>
-   <span data-ttu-id="469c6-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="469c6-684">ComplexProperty</span></span>
-   <span data-ttu-id="469c6-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="469c6-685">ResultMapping</span></span>

<span data-ttu-id="469c6-686">Como um filho do elemento **MappingFragment**, **ComplexProperty**ou **EndProperty** , o elemento **ScalarProperty** mapeia uma propriedade no modelo conceitual para uma coluna no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="469c6-687">Como um filho do elemento **InsertFunction**, **UpdateFunction**ou **DeleteFunction** , o elemento **ScalarProperty** mapeia uma propriedade no modelo conceitual para um parâmetro de procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="469c6-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="469c6-688">O elemento **ScalarProperty** não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="469c6-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-689">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-689">Applicable Attributes</span></span>

<span data-ttu-id="469c6-690">Os atributos que se aplicam ao elemento **ScalarProperty** diferem de acordo com a função do elemento.</span><span class="sxs-lookup"><span data-stu-id="469c6-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="469c6-691">A tabela a seguir descreve os atributos que são aplicáveis quando o elemento **ScalarProperty** é usado para mapear uma propriedade de modelo conceitual para uma coluna no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="469c6-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="469c6-692">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-692">Attribute Name</span></span> | <span data-ttu-id="469c6-693">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-693">Is Required</span></span> | <span data-ttu-id="469c6-694">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="469c6-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="469c6-695">**Name**</span></span>       | <span data-ttu-id="469c6-696">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-696">Yes</span></span>         | <span data-ttu-id="469c6-697">O nome da Propriedade do modelo conceitual que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="469c6-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="469c6-698">**ColumnName**</span></span> | <span data-ttu-id="469c6-699">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-699">Yes</span></span>         | <span data-ttu-id="469c6-700">O nome da coluna de tabela que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="469c6-701">A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ScalarProperty** quando ele é usado para mapear uma propriedade de modelo conceitual para um parâmetro de procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="469c6-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="469c6-702">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-702">Attribute Name</span></span>    | <span data-ttu-id="469c6-703">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-703">Is Required</span></span> | <span data-ttu-id="469c6-704">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="469c6-705">**Name**</span></span>          | <span data-ttu-id="469c6-706">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-706">Yes</span></span>         | <span data-ttu-id="469c6-707">O nome da Propriedade do modelo conceitual que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="469c6-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="469c6-708">**ParameterName**</span></span> | <span data-ttu-id="469c6-709">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-709">Yes</span></span>         | <span data-ttu-id="469c6-710">O nome do parâmetro que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="469c6-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="469c6-711">**Versão**</span><span class="sxs-lookup"><span data-stu-id="469c6-711">**Version**</span></span>       | <span data-ttu-id="469c6-712">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-712">No</span></span>          | <span data-ttu-id="469c6-713">**Atual** ou **original** , dependendo se o valor atual ou o valor original da propriedade deve ser usado para verificações de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="469c6-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="469c6-714">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-714">Example</span></span>

<span data-ttu-id="469c6-715">O exemplo a seguir mostra o elemento **ScalarProperty** usado de duas maneiras:</span><span class="sxs-lookup"><span data-stu-id="469c6-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="469c6-716">Para mapear as propriedades do tipo de entidade **Person** para as colunas da tabela **Person**.</span><span class="sxs-lookup"><span data-stu-id="469c6-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="469c6-717">Para mapear as propriedades do tipo de entidade **Person** para os parâmetros do procedimento armazenado **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="469c6-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="469c6-718">Os procedimentos armazenados são declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="469c6-719">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-719">Example</span></span>

<span data-ttu-id="469c6-720">O exemplo a seguir mostra o elemento **ScalarProperty** usado para mapear as funções INSERT e Delete de uma associação de modelo conceitual para procedimentos armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="469c6-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="469c6-721">Os procedimentos armazenados são declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="469c6-722">Elemento UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="469c6-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="469c6-723">O elemento **UpdateFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Update de um tipo de entidade no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="469c6-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="469c6-724">Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="469c6-725">Para obter mais informações, consulte elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="469c6-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="469c6-726">Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.</span><span class="sxs-lookup"><span data-stu-id="469c6-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="469c6-727">O elemento **UpdateFunction** pode ser filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="469c6-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="469c6-728">O elemento **UpdateFunction** pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="469c6-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="469c6-729">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="469c6-730">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="469c6-731">Resultbinding (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="469c6-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="469c6-732">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="469c6-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="469c6-733">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="469c6-733">Applicable Attributes</span></span>

<span data-ttu-id="469c6-734">A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **UpdateFunction** .</span><span class="sxs-lookup"><span data-stu-id="469c6-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="469c6-735">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="469c6-735">Attribute Name</span></span>            | <span data-ttu-id="469c6-736">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="469c6-736">Is Required</span></span> | <span data-ttu-id="469c6-737">Valor</span><span class="sxs-lookup"><span data-stu-id="469c6-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="469c6-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="469c6-738">**FunctionName**</span></span>          | <span data-ttu-id="469c6-739">Sim</span><span class="sxs-lookup"><span data-stu-id="469c6-739">Yes</span></span>         | <span data-ttu-id="469c6-740">O nome qualificado do namespace do procedimento armazenado para o qual a função Update é mapeada.</span><span class="sxs-lookup"><span data-stu-id="469c6-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="469c6-741">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="469c6-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="469c6-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="469c6-743">Não</span><span class="sxs-lookup"><span data-stu-id="469c6-743">No</span></span>          | <span data-ttu-id="469c6-744">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="469c6-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="469c6-745">Exemplo</span><span class="sxs-lookup"><span data-stu-id="469c6-745">Example</span></span>

<span data-ttu-id="469c6-746">O exemplo a seguir é baseado no modelo escolar e mostra o elemento **UpdateFunction** usado para mapear a função Update do tipo de entidade **Person** para o procedimento armazenado **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="469c6-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="469c6-747">O procedimento armazenado **UpdatePerson** é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="469c6-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
