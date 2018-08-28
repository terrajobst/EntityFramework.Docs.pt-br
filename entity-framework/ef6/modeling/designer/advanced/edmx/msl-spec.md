---
title: Especificação de MSL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 77dc7072c70b104188cd23974f32308960daebb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996025"
---
# <a name="msl-specification"></a><span data-ttu-id="17a13-102">Especificação de MSL</span><span class="sxs-lookup"><span data-stu-id="17a13-102">MSL Specification</span></span>
<span data-ttu-id="17a13-103">Mapeamento de linguagem de especificação (MSL) é uma linguagem baseada em XML que descreve o mapeamento entre o modelo conceitual e o modelo de armazenamento de um aplicativo do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="17a13-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="17a13-104">Em um aplicativo do Entity Framework, os metadados de mapeamento é carregado de um arquivo. MSL (escrito em MSL) no momento da compilação.</span><span class="sxs-lookup"><span data-stu-id="17a13-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="17a13-105">Entity Framework usa metadados de mapeamento em tempo de execução para converter consultas no modelo conceitual para comandos específicos do repositório.</span><span class="sxs-lookup"><span data-stu-id="17a13-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="17a13-106">O Entity Framework Designer (Designer de EF) armazena informações de mapeamento em um arquivo. edmx em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="17a13-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="17a13-107">No momento da compilação, o Entity Designer usa informações em um arquivo. edmx para criar o arquivo. MSL que é necessário pelo Entity Framework em tempo de execução</span><span class="sxs-lookup"><span data-stu-id="17a13-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="17a13-108">Nomes de todos os conceitual ou tipos de modelo de armazenamento que são referenciados em MSL devem ser qualificados por seus nomes de namespace respectivo.</span><span class="sxs-lookup"><span data-stu-id="17a13-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="17a13-109">Para obter informações sobre o nome do namespace de modelo conceitual, consulte [especificação de CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="17a13-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="17a13-110">Para obter informações sobre o nome de namespace do modelo de armazenamento, consulte [especificação de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="17a13-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="17a13-111">Versões do MSL são diferenciadas por namespaces XML.</span><span class="sxs-lookup"><span data-stu-id="17a13-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="17a13-112">Versão MSL</span><span class="sxs-lookup"><span data-stu-id="17a13-112">MSL Version</span></span> | <span data-ttu-id="17a13-113">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="17a13-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="17a13-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="17a13-114">MSL v1</span></span>      | <span data-ttu-id="17a13-115">urn: schemas-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="17a13-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="17a13-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="17a13-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="17a13-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="17a13-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="17a13-118">Elemento alias (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-118">Alias Element (MSL)</span></span>

<span data-ttu-id="17a13-119">O **Alias** elemento na linguagem de especificação (MSL) de mapeamento é um filho do elemento de mapeamento é usado para definir aliases para namespaces de modelos de armazenamento e o modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="17a13-120">Nomes de todos os conceitual ou tipos de modelo de armazenamento que são referenciados em MSL devem ser qualificados por seus nomes de namespace respectivo.</span><span class="sxs-lookup"><span data-stu-id="17a13-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="17a13-121">Para obter informações sobre o nome do namespace de modelo conceitual, consulte o elemento de esquema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="17a13-122">Para obter informações sobre o nome de namespace do modelo de armazenamento, consulte o elemento de esquema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="17a13-123">O **Alias** elemento não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="17a13-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-124">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-124">Applicable Attributes</span></span>

<span data-ttu-id="17a13-125">A tabela a seguir descreve os atributos que podem ser aplicados para o **Alias** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="17a13-126">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-126">Attribute Name</span></span> | <span data-ttu-id="17a13-127">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-127">Is Required</span></span> | <span data-ttu-id="17a13-128">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="17a13-129">**Chave**</span><span class="sxs-lookup"><span data-stu-id="17a13-129">**Key**</span></span>        | <span data-ttu-id="17a13-130">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-130">Yes</span></span>         | <span data-ttu-id="17a13-131">O alias do namespace que é especificado pela **valor** atributo.</span><span class="sxs-lookup"><span data-stu-id="17a13-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="17a13-132">**Value**</span><span class="sxs-lookup"><span data-stu-id="17a13-132">**Value**</span></span>      | <span data-ttu-id="17a13-133">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-133">Yes</span></span>         | <span data-ttu-id="17a13-134">O namespace para o qual o valor da **chave** elemento é um alias.</span><span class="sxs-lookup"><span data-stu-id="17a13-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="17a13-135">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-135">Example</span></span>

<span data-ttu-id="17a13-136">A exemplo a seguir mostra uma **Alias** elemento que define um alias, `c`, para tipos que são definidos no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a><span data-ttu-id="17a13-137">Elemento AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="17a13-138">O **AssociationEnd** elemento na linguagem de especificação (MSL) de mapeamento é usado quando as funções de modificação de um tipo de entidade no modelo conceitual são mapeadas para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="17a13-139">Se uma modificação de procedimento armazenado assume um parâmetro cujo valor é mantido em uma propriedade de associação, o **AssociationEnd** elemento é mapeado para o valor da propriedade para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="17a13-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="17a13-140">Para obter mais informações, consulte o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="17a13-140">For more information, see the example below.</span></span>

<span data-ttu-id="17a13-141">Para obter mais informações sobre funções de modificação de tipos de entidade de mapeamento para procedimentos armazenados, consulte o elemento ModificationFunctionMapping (MSL) e instruções passo a passo: mapeando uma entidade para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="17a13-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="17a13-142">O **AssociationEnd** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="17a13-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-144">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-144">Applicable Attributes</span></span>

<span data-ttu-id="17a13-145">A tabela a seguir descreve os atributos que são aplicáveis para o **AssociationEnd** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="17a13-146">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-146">Attribute Name</span></span>     | <span data-ttu-id="17a13-147">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-147">Is Required</span></span> | <span data-ttu-id="17a13-148">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="17a13-149">**AssociationSet**</span></span> | <span data-ttu-id="17a13-150">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-150">Yes</span></span>         | <span data-ttu-id="17a13-151">O nome da associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="17a13-152">**From**</span><span class="sxs-lookup"><span data-stu-id="17a13-152">**From**</span></span>           | <span data-ttu-id="17a13-153">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-153">Yes</span></span>         | <span data-ttu-id="17a13-154">O valor de **FromRole** atributo da propriedade de navegação que corresponde à associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="17a13-155">Para obter mais informações, consulte o elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="17a13-156">**To**</span><span class="sxs-lookup"><span data-stu-id="17a13-156">**To**</span></span>             | <span data-ttu-id="17a13-157">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-157">Yes</span></span>         | <span data-ttu-id="17a13-158">O valor de **ToRole** atributo da propriedade de navegação que corresponde à associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="17a13-159">Para obter mais informações, consulte o elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="17a13-160">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-160">Example</span></span>

<span data-ttu-id="17a13-161">Considere o seguinte tipo de entidade do modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="17a13-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="17a13-162">Também considere o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="17a13-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="17a13-163">Para mapear a função de atualização do `Course` entidade para esse procedimento armazenado, você deve fornecer um valor para o **DepartmentID** parâmetro.</span><span class="sxs-lookup"><span data-stu-id="17a13-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="17a13-164">O valor para `DepartmentID` não corresponde a uma propriedade no tipo de entidade; ele está contido em uma associação independente cujo mapeamento é mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="17a13-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="17a13-165">O código a seguir mostra a **AssociationEnd** elemento usado para mapear o **DepartmentID** propriedade do **FK\_curso\_departamento** associação para o **UpdateCourse** procedimento armazenado (para o qual a função de atualização de **curso** tipo de entidade é mapeado):</span><span class="sxs-lookup"><span data-stu-id="17a13-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="17a13-166">Elemento AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="17a13-167">O **AssociationSetMapping** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre uma associação nas colunas de tabela e de modelo conceituais no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="17a13-168">Associações no modelo conceitual são tipos cujas propriedades representem colunas de chave primárias e estrangeiras no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="17a13-169">O **AssociationSetMapping** elemento usa dois elementos EndProperty para definir os mapeamentos entre colunas e propriedades de tipo de associação no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="17a13-170">Você pode colocar as condições nesses mapeamentos com o elemento de condição.</span><span class="sxs-lookup"><span data-stu-id="17a13-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="17a13-171">Mapear o insert, update e delete funções para associações para procedimentos armazenados no banco de dados com o elemento ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="17a13-172">Defina mapeamentos de somente leitura entre as associações e colunas de tabela usando uma cadeia de caracteres de Entity SQL em um elemento QueryView.</span><span class="sxs-lookup"><span data-stu-id="17a13-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-173">Se uma restrição referencial está definida para uma associação no modelo conceitual, a associação não precisa ser mapeada com um **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="17a13-174">Se um **AssociationSetMapping** elemento está presente para uma associação que tem uma restrição referencial, os mapeamentos definidos na **AssociationSetMapping** elemento será ignorado.</span><span class="sxs-lookup"><span data-stu-id="17a13-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="17a13-175">Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="17a13-176">O **AssociationSetMapping** elemento pode ter os seguintes elementos filho</span><span class="sxs-lookup"><span data-stu-id="17a13-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="17a13-177">QueryView (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="17a13-178">EndProperty (zero ou dois)</span><span class="sxs-lookup"><span data-stu-id="17a13-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="17a13-179">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="17a13-180">ModificationFunctionMapping (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-181">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-181">Applicable Attributes</span></span>

<span data-ttu-id="17a13-182">A tabela a seguir descreve os atributos que podem ser aplicados para o **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="17a13-183">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-183">Attribute Name</span></span>     | <span data-ttu-id="17a13-184">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-184">Is Required</span></span> | <span data-ttu-id="17a13-185">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-186">**Nome**</span><span class="sxs-lookup"><span data-stu-id="17a13-186">**Name**</span></span>           | <span data-ttu-id="17a13-187">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-187">Yes</span></span>         | <span data-ttu-id="17a13-188">O nome do conjunto de associação de modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="17a13-189">**typeName**</span><span class="sxs-lookup"><span data-stu-id="17a13-189">**TypeName**</span></span>       | <span data-ttu-id="17a13-190">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-190">No</span></span>          | <span data-ttu-id="17a13-191">O nome qualificado de namespace do tipo de associação de modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="17a13-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="17a13-192">**StoreEntitySet**</span></span> | <span data-ttu-id="17a13-193">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-193">No</span></span>          | <span data-ttu-id="17a13-194">O nome da tabela que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="17a13-195">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-195">Example</span></span>

<span data-ttu-id="17a13-196">A exemplo a seguir mostra uma **AssociationSetMapping** elemento no qual o **FK\_curso\_departamento** conjunto de associações no modelo conceitual está mapeada para o  **Curso** tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="17a13-197">Mapeamentos entre colunas de tabela e propriedades de tipo de associação são especificados em filha **EndProperty** elementos.</span><span class="sxs-lookup"><span data-stu-id="17a13-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="17a13-198">Elemento ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="17a13-199">Um **ComplexProperty** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre uma propriedade de tipo complexo em um colunas de tipo e a tabela da entidade de modelo conceitual no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="17a13-200">Os mapeamentos de coluna de propriedade são especificados nos elementos ScalarProperty de filho.</span><span class="sxs-lookup"><span data-stu-id="17a13-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="17a13-201">O **ComplexType** elemento de propriedade pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-202">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="17a13-203">**ComplexProperty** (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="17a13-204">ComplextTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="17a13-205">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-206">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-206">Applicable Attributes</span></span>

<span data-ttu-id="17a13-207">A tabela a seguir descreve os atributos que são aplicáveis para o **ComplexProperty** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="17a13-208">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-208">Attribute Name</span></span> | <span data-ttu-id="17a13-209">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-209">Is Required</span></span> | <span data-ttu-id="17a13-210">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-211">**Nome**</span><span class="sxs-lookup"><span data-stu-id="17a13-211">**Name**</span></span>       | <span data-ttu-id="17a13-212">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-212">Yes</span></span>         | <span data-ttu-id="17a13-213">O nome da propriedade complexa de um tipo de entidade no modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="17a13-214">**typeName**</span><span class="sxs-lookup"><span data-stu-id="17a13-214">**TypeName**</span></span>   | <span data-ttu-id="17a13-215">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-215">No</span></span>          | <span data-ttu-id="17a13-216">O nome qualificado de namespace do tipo de propriedade do modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="17a13-217">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-217">Example</span></span>

<span data-ttu-id="17a13-218">O exemplo a seguir se baseia no modelo de escola.</span><span class="sxs-lookup"><span data-stu-id="17a13-218">The following example is based on the School model.</span></span> <span data-ttu-id="17a13-219">O tipo complexo a seguir foi adicionado ao modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="17a13-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="17a13-220">O **LastName** e **FirstName** propriedades do **pessoa** tipo de entidade foram substituídos por uma propriedade complexa, **nome**:</span><span class="sxs-lookup"><span data-stu-id="17a13-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="17a13-221">O MSL a seguir mostra a **ComplexProperty** elemento usado para mapear o **nome** propriedade para colunas no banco de dados subjacente:</span><span class="sxs-lookup"><span data-stu-id="17a13-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="17a13-222">Elemento ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="17a13-223">O **ComplexTypeMapping** elemento na linguagem de especificação (MSL) de mapeamento é um filho do elemento ResultMapping e define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado subjacente banco de dados quando as seguintes condições forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="17a13-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="17a13-224">A importação de função retorna um tipo complexo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="17a13-225">Os nomes das colunas retornadas pelo procedimento armazenado não exatamente coincidem com os nomes das propriedades no tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="17a13-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="17a13-226">Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo complexo é baseado em nomes de colunas e propriedades.</span><span class="sxs-lookup"><span data-stu-id="17a13-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="17a13-227">Se os nomes de coluna não correspondem exatamente os nomes de propriedade, você deve usar o **ComplexTypeMapping** elemento para definir o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="17a13-228">Para obter um exemplo de mapeamento padrão, consulte o elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="17a13-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="17a13-229">O **ComplexTypeMapping** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-230">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-231">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-231">Applicable Attributes</span></span>

<span data-ttu-id="17a13-232">A tabela a seguir descreve os atributos que são aplicáveis para o **ComplexTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="17a13-233">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-233">Attribute Name</span></span> | <span data-ttu-id="17a13-234">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-234">Is Required</span></span> | <span data-ttu-id="17a13-235">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="17a13-236">**typeName**</span><span class="sxs-lookup"><span data-stu-id="17a13-236">**TypeName**</span></span>   | <span data-ttu-id="17a13-237">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-237">Yes</span></span>         | <span data-ttu-id="17a13-238">O nome qualificado de namespace do tipo complexo que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-239">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-239">Example</span></span>

<span data-ttu-id="17a13-240">Considere o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="17a13-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="17a13-241">Além disso, considere o seguinte tipo complexo de modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="17a13-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="17a13-242">Para criar uma importação de função que retorna instâncias do tipo complexo anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um **ComplexTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="17a13-243">Elemento Condition (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-243">Condition Element (MSL)</span></span>

<span data-ttu-id="17a13-244">O **condição** elemento na linguagem de especificação (MSL) de mapeamento coloca condições nos mapeamentos entre o modelo conceitual e o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="17a13-245">O mapeamento que é definido dentro de um nó XML é válidas se todas as condições, como o filho especificado na **condição** elementos, sejam atendidos.</span><span class="sxs-lookup"><span data-stu-id="17a13-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="17a13-246">Caso contrário, o mapeamento não é válido.</span><span class="sxs-lookup"><span data-stu-id="17a13-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="17a13-247">Por exemplo, se um elemento MappingFragment contém um ou mais **condição** elementos filho, o mapeamento definido dentro de **MappingFragment** nó só será válida se todas as condições do filho  **Condição** elementos forem atendidos.</span><span class="sxs-lookup"><span data-stu-id="17a13-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="17a13-248">Cada condição pode ser aplicada a qualquer um uma **nome** (o nome de uma propriedade de entidade do modelo conceitual, especificado pelo **nome** atributo), ou uma **ColumnName** (o nome de uma coluna em o banco de dados, especificado pelo **ColumnName** atributo).</span><span class="sxs-lookup"><span data-stu-id="17a13-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="17a13-249">Quando o **nome** atributo é definido, a condição será verificada em relação a um valor de propriedade de entidade.</span><span class="sxs-lookup"><span data-stu-id="17a13-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="17a13-250">Quando o **ColumnName** atributo for definido, a condição será verificada em relação a um valor de coluna.</span><span class="sxs-lookup"><span data-stu-id="17a13-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="17a13-251">Somente um dos **nome** ou **ColumnName** atributo pode ser especificado em um **condição** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-252">Quando o **condição** elemento é usado dentro de um elemento FunctionImportMapping, somente os **nome** atributo não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="17a13-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="17a13-253">O **condição** elemento pode ser um filho dos elementos a seguir:</span><span class="sxs-lookup"><span data-stu-id="17a13-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="17a13-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="17a13-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="17a13-255">ComplexProperty</span></span>
-   <span data-ttu-id="17a13-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-256">EntitySetMapping</span></span>
-   <span data-ttu-id="17a13-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="17a13-257">MappingFragment</span></span>
-   <span data-ttu-id="17a13-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-258">EntityTypeMapping</span></span>

<span data-ttu-id="17a13-259">O **condição** elemento não pode ter nenhum elemento filho.</span><span class="sxs-lookup"><span data-stu-id="17a13-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-260">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-260">Applicable Attributes</span></span>

<span data-ttu-id="17a13-261">A tabela a seguir descreve os atributos que são aplicáveis para o **condição** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="17a13-262">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-262">Attribute Name</span></span> | <span data-ttu-id="17a13-263">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-263">Is Required</span></span> | <span data-ttu-id="17a13-264">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="17a13-265">**ColumnName**</span></span> | <span data-ttu-id="17a13-266">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-266">No</span></span>          | <span data-ttu-id="17a13-267">O nome da coluna da tabela cujo valor é usado para avaliar a condição.</span><span class="sxs-lookup"><span data-stu-id="17a13-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="17a13-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="17a13-268">**IsNull**</span></span>     | <span data-ttu-id="17a13-269">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-269">No</span></span>          | <span data-ttu-id="17a13-270">**Verdadeiro** ou **falso**.</span><span class="sxs-lookup"><span data-stu-id="17a13-270">**True** or **False**.</span></span> <span data-ttu-id="17a13-271">Se o valor for **verdadeira** e é o valor da coluna **nulo**, ou se o valor for **falso** e o valor da coluna não é **nulo**, a condição for verdadeira .</span><span class="sxs-lookup"><span data-stu-id="17a13-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="17a13-272">Caso contrário, a condição for falsa.</span><span class="sxs-lookup"><span data-stu-id="17a13-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="17a13-273">O **IsNull** e **valor** atributos não podem ser usados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="17a13-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="17a13-274">**Value**</span><span class="sxs-lookup"><span data-stu-id="17a13-274">**Value**</span></span>      | <span data-ttu-id="17a13-275">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-275">No</span></span>          | <span data-ttu-id="17a13-276">O valor com o qual o valor da coluna é comparado.</span><span class="sxs-lookup"><span data-stu-id="17a13-276">The value with which the column value is compared.</span></span> <span data-ttu-id="17a13-277">Se os valores forem iguais, a condição for verdadeira.</span><span class="sxs-lookup"><span data-stu-id="17a13-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="17a13-278">Caso contrário, a condição for falsa.</span><span class="sxs-lookup"><span data-stu-id="17a13-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="17a13-279">O **IsNull** e **valor** atributos não podem ser usados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="17a13-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="17a13-280">**Nome**</span><span class="sxs-lookup"><span data-stu-id="17a13-280">**Name**</span></span>       | <span data-ttu-id="17a13-281">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-281">No</span></span>          | <span data-ttu-id="17a13-282">O nome da propriedade de entidade do modelo conceitual, cujo valor é usado para avaliar a condição.</span><span class="sxs-lookup"><span data-stu-id="17a13-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="17a13-283">Esse atributo não é aplicável se o **condição** elemento é usado dentro de um elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="17a13-284">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-284">Example</span></span>

<span data-ttu-id="17a13-285">A exemplo a seguir mostra **condição** elementos como filhos do **MappingFragment** elementos.</span><span class="sxs-lookup"><span data-stu-id="17a13-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="17a13-286">Quando **HireDate** não for nulo e **EnrollmentDate** for nulo, os dados são mapeados entre o **SchoolModel.Instructor** tipo e o **PersonID**e **HireDate** colunas da **pessoa** tabela.</span><span class="sxs-lookup"><span data-stu-id="17a13-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="17a13-287">Quando **EnrollmentDate** não for nulo e **HireDate** for nulo, os dados são mapeados entre o **SchoolModel.Student** tipo e o **PersonID** e **registro** colunas da **pessoa** tabela.</span><span class="sxs-lookup"><span data-stu-id="17a13-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="17a13-288">Elemento DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="17a13-289">O **DeleteFunction** elemento na linguagem de especificação (MSL) de mapeamento mapeia a função de exclusão de um tipo de entidade ou a associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="17a13-290">Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="17a13-291">Para obter mais informações, consulte o elemento de função (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-292">Se você não mapear todos os três da inserção, atualização ou exclusão operações de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executado em tempo de execução e um UpdateException é lançada.</span><span class="sxs-lookup"><span data-stu-id="17a13-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="17a13-293">DeleteFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="17a13-294">Quando aplicado ao elemento EntityTypeMapping, o **DeleteFunction** elemento é mapeado para a função de exclusão de um tipo de entidade no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="17a13-295">O **DeleteFunction** elemento pode ter os seguintes elementos filho quando aplicado a um **EntityTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="17a13-296">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="17a13-297">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="17a13-298">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="17a13-299">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-299">Applicable Attributes</span></span>

<span data-ttu-id="17a13-300">A tabela a seguir descreve os atributos que podem ser aplicados para o **DeleteFunction** elemento quando ele é aplicado a um **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="17a13-301">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-301">Attribute Name</span></span>            | <span data-ttu-id="17a13-302">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-302">Is Required</span></span> | <span data-ttu-id="17a13-303">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="17a13-304">**FunctionName**</span></span>          | <span data-ttu-id="17a13-305">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-305">Yes</span></span>         | <span data-ttu-id="17a13-306">O nome qualificado de namespace do procedimento armazenado para o qual a função de exclusão é mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="17a13-307">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="17a13-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="17a13-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="17a13-309">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-309">No</span></span>          | <span data-ttu-id="17a13-310">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="17a13-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="17a13-311">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-311">Example</span></span>

<span data-ttu-id="17a13-312">O exemplo a seguir é baseado no modelo de escola e mostra a **DeleteFunction** elemento de mapeamento de função de exclusão do **pessoa** tipo de entidade para o **DeletePerson** procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="17a13-313">O **DeletePerson** procedimento armazenado é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="17a13-314">DeleteFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="17a13-315">Quando aplicado ao elemento AssociationSetMapping, o **DeleteFunction** elemento é mapeado para a função de exclusão de uma associação no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="17a13-316">O **DeleteFunction** elemento pode ter os seguintes elementos filho quando aplicado ao **AssociationSetMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="17a13-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="17a13-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="17a13-318">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-318">Applicable Attributes</span></span>

<span data-ttu-id="17a13-319">A tabela a seguir descreve os atributos que podem ser aplicados para o **DeleteFunction** elemento quando ele é aplicado para o **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="17a13-320">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-320">Attribute Name</span></span>            | <span data-ttu-id="17a13-321">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-321">Is Required</span></span> | <span data-ttu-id="17a13-322">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="17a13-323">**FunctionName**</span></span>          | <span data-ttu-id="17a13-324">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-324">Yes</span></span>         | <span data-ttu-id="17a13-325">O nome qualificado de namespace do procedimento armazenado para o qual a função de exclusão é mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="17a13-326">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="17a13-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="17a13-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="17a13-328">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-328">No</span></span>          | <span data-ttu-id="17a13-329">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="17a13-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="17a13-330">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-330">Example</span></span>

<span data-ttu-id="17a13-331">O exemplo a seguir é baseado no modelo de escola e mostra a **DeleteFunction** elemento usado para mapear a função de exclusão do **CourseInstructor** associação para o  **DeleteCourseInstructor** procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="17a13-332">O **DeleteCourseInstructor** procedimento armazenado é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="17a13-333">Elemento EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="17a13-334">O **EndProperty** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre um final ou uma função de modificação de uma associação de modelo conceitual e o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="17a13-335">O mapeamento de coluna de propriedade é especificado em um elemento ScalarProperty de filho.</span><span class="sxs-lookup"><span data-stu-id="17a13-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="17a13-336">Quando um **EndProperty** elemento é usado para definir o mapeamento para o final de uma associação de modelo conceitual, ele é um filho de um elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="17a13-337">Quando o **EndProperty** elemento é usado para definir o mapeamento para uma função de modificação de uma associação de modelo conceitual, ele é um filho de um elemento de InsertFunction ou o elemento DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="17a13-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="17a13-338">O **EndProperty** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-339">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-340">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-340">Applicable Attributes</span></span>

<span data-ttu-id="17a13-341">A tabela a seguir descreve os atributos que são aplicáveis para o **EndProperty** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="17a13-342">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-342">Attribute Name</span></span> | <span data-ttu-id="17a13-343">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-343">Is Required</span></span> | <span data-ttu-id="17a13-344">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="17a13-345">Nome</span><span class="sxs-lookup"><span data-stu-id="17a13-345">Name</span></span>           | <span data-ttu-id="17a13-346">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-346">Yes</span></span>         | <span data-ttu-id="17a13-347">O nome da extremidade da associação que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-348">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-348">Example</span></span>

<span data-ttu-id="17a13-349">A exemplo a seguir mostra uma **AssociationSetMapping** elemento no qual o **FK\_curso\_departamento** associação no modelo conceitual está mapeada para o **Curso** tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="17a13-350">Mapeamentos entre colunas de tabela e propriedades de tipo de associação são especificados em filha **EndProperty** elementos.</span><span class="sxs-lookup"><span data-stu-id="17a13-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="17a13-351">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-351">Example</span></span>

<span data-ttu-id="17a13-352">A exemplo a seguir mostra a **EndProperty** elemento as funções de inserção e exclusão de uma associação de mapeamento (**CourseInstructor**) para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="17a13-353">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="17a13-354">Elemento EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="17a13-355">O **EntityContainerMapping** elemento na linguagem de especificação (MSL) de mapeamento mapeia o contêiner de entidade no modelo conceitual para o contêiner de entidade no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="17a13-356">O **EntityContainerMapping** elemento é um filho do elemento de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="17a13-357">O **EntityContainerMapping** elemento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="17a13-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="17a13-358">EntitySetMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="17a13-359">AssociationSetMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="17a13-360">FunctionImportMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-361">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-361">Applicable Attributes</span></span>

<span data-ttu-id="17a13-362">A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityContainerMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="17a13-363">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-363">Attribute Name</span></span>            | <span data-ttu-id="17a13-364">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-364">Is Required</span></span> | <span data-ttu-id="17a13-365">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="17a13-366">**StorageModelContainer**</span></span> | <span data-ttu-id="17a13-367">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-367">Yes</span></span>         | <span data-ttu-id="17a13-368">O nome do contêiner de entidade de modelo de armazenamento que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="17a13-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="17a13-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="17a13-370">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-370">Yes</span></span>         | <span data-ttu-id="17a13-371">O nome do contêiner de entidade do modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="17a13-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="17a13-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="17a13-373">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-373">No</span></span>          | <span data-ttu-id="17a13-374">**Verdadeiro** ou **falso**.</span><span class="sxs-lookup"><span data-stu-id="17a13-374">**True** or **False**.</span></span> <span data-ttu-id="17a13-375">Se **falsos**, nenhum modo de exibição de atualização é gerados.</span><span class="sxs-lookup"><span data-stu-id="17a13-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="17a13-376">Esse atributo deve ser definido como **falsos** quando você tiver um mapeamento de somente leitura que seria inválido porque dados podem ser ida e volta com êxito.</span><span class="sxs-lookup"><span data-stu-id="17a13-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="17a13-377">O valor padrão é **verdadeira**.</span><span class="sxs-lookup"><span data-stu-id="17a13-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-378">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-378">Example</span></span>

<span data-ttu-id="17a13-379">A exemplo a seguir mostra uma **EntityContainerMapping** elemento que mapeia o **SchoolModelEntities** contêiner (o contêiner de entidade do modelo conceitual) para o  **SchoolModelStoreContainer** contêiner (o modelo entidade contêiner de armazenamento):</span><span class="sxs-lookup"><span data-stu-id="17a13-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="17a13-380">Elemento EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="17a13-381">O **EntitySetMapping** define um elemento em mapas MSL (linguagem) de especificação, todos os tipos em uma entidade de modelo conceitual são definidos como entidade de mapeamento no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="17a13-382">Uma entidade definida no modelo conceitual é um contêiner lógico para instâncias de entidades do mesmo tipo (e tipos derivados).</span><span class="sxs-lookup"><span data-stu-id="17a13-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="17a13-383">Uma entidade definida no modelo de armazenamento representa uma tabela ou exibição no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="17a13-384">O conjunto de entidades do modelo conceitual é especificado pelo valor da **nome** atributo da **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="17a13-385">Mapeado para a tabela ou exibição é especificada pelo **StoreEntitySet** atributo em cada elemento do filho MappingFragment ou nos **EntitySetMapping** próprio elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="17a13-386">O **EntitySetMapping** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-387">EntityTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="17a13-388">QueryView (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="17a13-389">MappingFragment (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-390">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-390">Applicable Attributes</span></span>

<span data-ttu-id="17a13-391">A tabela a seguir descreve os atributos que podem ser aplicados para o **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="17a13-392">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-392">Attribute Name</span></span>           | <span data-ttu-id="17a13-393">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-393">Is Required</span></span> | <span data-ttu-id="17a13-394">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-395">**Nome**</span><span class="sxs-lookup"><span data-stu-id="17a13-395">**Name**</span></span>                 | <span data-ttu-id="17a13-396">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-396">Yes</span></span>         | <span data-ttu-id="17a13-397">O nome do conjunto de entidades de modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="17a13-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="17a13-398">**TypeName** **1**</span></span>       | <span data-ttu-id="17a13-399">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-399">No</span></span>          | <span data-ttu-id="17a13-400">O nome do tipo de entidade de modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="17a13-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="17a13-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="17a13-402">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-402">No</span></span>          | <span data-ttu-id="17a13-403">O nome do conjunto de entidades de modelo de armazenamento que está sendo mapeado para.</span><span class="sxs-lookup"><span data-stu-id="17a13-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="17a13-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="17a13-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="17a13-405">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-405">No</span></span>          | <span data-ttu-id="17a13-406">**Verdadeiro** ou **falso** dependendo se linhas distintas só são retornadas.</span><span class="sxs-lookup"><span data-stu-id="17a13-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="17a13-407">Se esse atributo for definido como **verdadeira**, o **GenerateUpdateViews** atributo do elemento EntityContainerMapping deve ser definido como **False**.</span><span class="sxs-lookup"><span data-stu-id="17a13-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="17a13-408">**1** as **TypeName** e **StoreEntitySet** atributos podem ser usados no lugar de elementos filho EntityTypeMapping e MappingFragment para mapear um único tipo de entidade para uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="17a13-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="17a13-409">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-409">Example</span></span>

<span data-ttu-id="17a13-410">A exemplo a seguir mostra uma **EntitySetMapping** elemento que mapeia tipos de três (um tipo base e dois tipos derivados) nos **cursos** conjunto de entidades do modelo conceitual para três tabelas diferentes no o banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="17a13-411">As tabelas são especificadas pela **StoreEntitySet** atributo em cada **MappingFragment** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="17a13-412">Elemento EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="17a13-413">O **EntityTypeMapping** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre um tipo de entidade no modelo conceitual e tabelas ou exibições no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="17a13-414">Para obter informações sobre tipos de entidade do modelo conceitual e tabelas de banco de dados subjacente ou modos de exibição, consulte o elemento EntityType (CSDL) e o elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="17a13-415">O tipo de entidade do modelo conceitual que está sendo mapeado é especificado pelo **TypeName** atributo da **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="17a13-416">A tabela ou exibição que está sendo mapeada é especificada pelo **StoreEntitySet** atributo do elemento filho MappingFragment.</span><span class="sxs-lookup"><span data-stu-id="17a13-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="17a13-417">O ModificationFunctionMapping elemento filho pode ser usado para mapear a inserção, atualização ou exclusão de funções dos tipos de entidade para procedimentos armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="17a13-418">O **EntityTypeMapping** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-419">MappingFragment (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="17a13-420">ModificationFunctionMapping (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="17a13-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="17a13-421">ScalarProperty</span></span>
-   <span data-ttu-id="17a13-422">Condição</span><span class="sxs-lookup"><span data-stu-id="17a13-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-423">**MappingFragment** e **ModificationFunctionMapping** elementos não podem ser elementos filho do **EntityTypeMapping** elemento ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="17a13-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="17a13-424">O **ScalarProperty** e **condição** elementos só podem ser elementos filho do **EntityTypeMapping** elemento quando ele é usado dentro de um elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-425">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-425">Applicable Attributes</span></span>

<span data-ttu-id="17a13-426">A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="17a13-427">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-427">Attribute Name</span></span> | <span data-ttu-id="17a13-428">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-428">Is Required</span></span> | <span data-ttu-id="17a13-429">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-430">**typeName**</span><span class="sxs-lookup"><span data-stu-id="17a13-430">**TypeName**</span></span>   | <span data-ttu-id="17a13-431">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-431">Yes</span></span>         | <span data-ttu-id="17a13-432">O nome qualificado de namespace do tipo de entidade de modelo conceitual que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="17a13-433">Se o tipo é abstrato ou um tipo derivado, o valor deve ser `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="17a13-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-434">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-434">Example</span></span>

<span data-ttu-id="17a13-435">O exemplo a seguir mostra um elemento EntitySetMapping com dois filhos **EntityTypeMapping** elementos.</span><span class="sxs-lookup"><span data-stu-id="17a13-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="17a13-436">No primeiro **EntityTypeMapping** elemento, o **SchoolModel.Person** tipo de entidade é mapeado para o **pessoa** tabela.</span><span class="sxs-lookup"><span data-stu-id="17a13-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="17a13-437">Na segunda **EntityTypeMapping** elemento, a funcionalidade de atualização do **SchoolModel.Person** tipo é mapeado para um procedimento armazenado **UpdatePerson**, no banco de dados .</span><span class="sxs-lookup"><span data-stu-id="17a13-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="17a13-438">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-438">Example</span></span>

<span data-ttu-id="17a13-439">O exemplo a seguir mostra o mapeamento de uma hierarquia de tipo no qual o tipo de raiz é abstrato.</span><span class="sxs-lookup"><span data-stu-id="17a13-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="17a13-440">Observe o uso do `IsOfType` sintaxe para o **TypeName** atributos.</span><span class="sxs-lookup"><span data-stu-id="17a13-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="17a13-441">Elemento FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="17a13-442">O **FunctionImportMapping** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado ou função no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="17a13-443">Importações de função devem ser declaradas no modelo conceitual e procedimentos armazenados devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="17a13-444">Para obter mais informações, consulte o elemento FunctionImport (CSDL) e o elemento de função (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-445">Por padrão, se uma importação de função retorna um tipo de entidade do modelo conceitual ou de um tipo complexo, em seguida, os nomes das colunas retornadas pelo procedimento armazenado subjacente devem corresponder exatamente aos nomes das propriedades no tipo de modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="17a13-446">Se os nomes de coluna não correspondem exatamente os nomes de propriedade, o mapeamento deve ser definido em um elemento ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="17a13-447">O **FunctionImportMapping** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-448">ResultMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-449">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-449">Applicable Attributes</span></span>

<span data-ttu-id="17a13-450">A tabela a seguir descreve os atributos que são aplicáveis para o **FunctionImportMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="17a13-451">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-451">Attribute Name</span></span>         | <span data-ttu-id="17a13-452">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-452">Is Required</span></span> | <span data-ttu-id="17a13-453">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="17a13-454">**FunctionImportName**</span></span> | <span data-ttu-id="17a13-455">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-455">Yes</span></span>         | <span data-ttu-id="17a13-456">O nome da função de importação no modelo conceitual que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="17a13-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="17a13-457">**FunctionName**</span></span>       | <span data-ttu-id="17a13-458">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-458">Yes</span></span>         | <span data-ttu-id="17a13-459">O nome qualificado de namespace da função no modelo de armazenamento que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-460">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-460">Example</span></span>

<span data-ttu-id="17a13-461">O exemplo a seguir se baseia no modelo de escola.</span><span class="sxs-lookup"><span data-stu-id="17a13-461">The following example is based on the School model.</span></span> <span data-ttu-id="17a13-462">Considere a seguinte função no modelo de armazenamento:</span><span class="sxs-lookup"><span data-stu-id="17a13-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="17a13-463">Além disso, considere esta importação de função no modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="17a13-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="17a13-464">Apresentação do exemplo a seguir uma **FunctionImportMapping** elemento usado para mapear a função e a função importação acima uns aos outros:</span><span class="sxs-lookup"><span data-stu-id="17a13-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="17a13-465">Elemento InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="17a13-466">O **InsertFunction** elemento na linguagem de especificação (MSL) de mapeamento mapeia a função de inserção de um tipo de entidade ou a associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="17a13-467">Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="17a13-468">Para obter mais informações, consulte o elemento de função (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-469">Se você não mapear todos os três da inserção, atualização ou exclusão operações de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executado em tempo de execução e um UpdateException é lançada.</span><span class="sxs-lookup"><span data-stu-id="17a13-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="17a13-470">O **InsertFunction** elemento pode ser um filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping ou o elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="17a13-471">InsertFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="17a13-472">Quando aplicado ao elemento EntityTypeMapping, o **InsertFunction** elemento é mapeado para a função de inserção de um tipo de entidade no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="17a13-473">O **InsertFunction** elemento pode ter os seguintes elementos filho quando aplicado a um **EntityTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="17a13-474">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="17a13-475">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="17a13-476">ResultBinding (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="17a13-477">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="17a13-478">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-478">Applicable Attributes</span></span>

<span data-ttu-id="17a13-479">A tabela a seguir descreve os atributos que podem ser aplicados para o **InsertFunction** elemento quando aplicado a um **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="17a13-480">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-480">Attribute Name</span></span>            | <span data-ttu-id="17a13-481">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-481">Is Required</span></span> | <span data-ttu-id="17a13-482">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="17a13-483">**FunctionName**</span></span>          | <span data-ttu-id="17a13-484">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-484">Yes</span></span>         | <span data-ttu-id="17a13-485">O nome qualificado de namespace do procedimento armazenado para o qual a função de inserção é mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="17a13-486">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="17a13-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="17a13-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="17a13-488">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-488">No</span></span>          | <span data-ttu-id="17a13-489">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="17a13-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="17a13-490">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-490">Example</span></span>

<span data-ttu-id="17a13-491">O exemplo a seguir é baseado no modelo de escola e mostra a **InsertFunction** elemento usado para mapear a função de inserção do tipo de entidade Person para o **InsertPerson** procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="17a13-492">O **InsertPerson** procedimento armazenado é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="17a13-493">InsertFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="17a13-494">Quando aplicado ao elemento AssociationSetMapping, o **InsertFunction** elemento é mapeado para a função de inserção de uma associação no modelo conceitual para um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="17a13-495">O **InsertFunction** elemento pode ter os seguintes elementos filho quando aplicado ao **AssociationSetMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="17a13-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="17a13-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="17a13-497">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-497">Applicable Attributes</span></span>

<span data-ttu-id="17a13-498">A tabela a seguir descreve os atributos que podem ser aplicados para o **InsertFunction** elemento quando ele é aplicado para o **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="17a13-499">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-499">Attribute Name</span></span>            | <span data-ttu-id="17a13-500">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-500">Is Required</span></span> | <span data-ttu-id="17a13-501">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="17a13-502">**FunctionName**</span></span>          | <span data-ttu-id="17a13-503">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-503">Yes</span></span>         | <span data-ttu-id="17a13-504">O nome qualificado de namespace do procedimento armazenado para o qual a função de inserção é mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="17a13-505">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="17a13-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="17a13-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="17a13-507">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-507">No</span></span>          | <span data-ttu-id="17a13-508">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="17a13-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="17a13-509">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-509">Example</span></span>

<span data-ttu-id="17a13-510">O exemplo a seguir é baseado no modelo de escola e mostra a **InsertFunction** elemento usado para mapear a função de inserção da **CourseInstructor** associação para o  **InsertCourseInstructor** procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="17a13-511">O **InsertCourseInstructor** procedimento armazenado é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="17a13-512">Elemento de mapeamento (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="17a13-513">O **mapeamento** elemento na linguagem de especificação (MSL) de mapeamento contém informações para o mapeamento de objetos que são definidos em um modelo conceitual para um banco de dados (conforme descrito em um modelo de armazenamento).</span><span class="sxs-lookup"><span data-stu-id="17a13-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="17a13-514">Para obter mais informações, consulte a especificação de CSDL e SSDL especificação.</span><span class="sxs-lookup"><span data-stu-id="17a13-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="17a13-515">O **mapeamento** é o elemento raiz para uma especificação de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="17a13-516">O namespace XML para o mapeamento de especificações é http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="17a13-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="17a13-517">O elemento de mapeamento pode ter os seguintes elementos filho (na ordem listada):</span><span class="sxs-lookup"><span data-stu-id="17a13-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="17a13-518">Alias (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="17a13-519">EntityContainerMapping (exatamente uma)</span><span class="sxs-lookup"><span data-stu-id="17a13-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="17a13-520">Nomes de conceitual e tipos de modelo de armazenamento que são referenciados em MSL devem ser qualificados por seus nomes de namespace respectivo.</span><span class="sxs-lookup"><span data-stu-id="17a13-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="17a13-521">Para obter informações sobre o nome do namespace de modelo conceitual, consulte o elemento de esquema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="17a13-522">Para obter informações sobre o nome de namespace do modelo de armazenamento, consulte o elemento de esquema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="17a13-523">Aliases para namespaces que são usados em MSL podem ser definidos com o elemento de Alias.</span><span class="sxs-lookup"><span data-stu-id="17a13-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-524">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-524">Applicable Attributes</span></span>

<span data-ttu-id="17a13-525">A tabela a seguir descreve os atributos que podem ser aplicados para o **mapeamento** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="17a13-526">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-526">Attribute Name</span></span> | <span data-ttu-id="17a13-527">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-527">Is Required</span></span> | <span data-ttu-id="17a13-528">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="17a13-529">**Espaço**</span><span class="sxs-lookup"><span data-stu-id="17a13-529">**Space**</span></span>      | <span data-ttu-id="17a13-530">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-530">Yes</span></span>         | <span data-ttu-id="17a13-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="17a13-531">**C-S**.</span></span> <span data-ttu-id="17a13-532">Isso é um valor fixo e não pode ser alterado.</span><span class="sxs-lookup"><span data-stu-id="17a13-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-533">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-533">Example</span></span>

<span data-ttu-id="17a13-534">A exemplo a seguir mostra uma **mapeamento** elemento baseia-se parte do modelo de escola.</span><span class="sxs-lookup"><span data-stu-id="17a13-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="17a13-535">Para obter mais informações sobre o modelo de escola, consulte o guia de início rápido (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="17a13-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="17a13-536">Elemento MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="17a13-537">O **MappingFragment** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre as propriedades de um tipo de entidade do modelo conceitual e uma tabela ou exibição no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="17a13-538">Para obter informações sobre tipos de entidade do modelo conceitual e tabelas de banco de dados subjacente ou modos de exibição, consulte o elemento EntityType (CSDL) e o elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="17a13-539">O **MappingFragment** pode ser um elemento filho do elemento EntityTypeMapping ou elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="17a13-540">O **MappingFragment** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-541">ComplexType (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="17a13-542">ScalarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="17a13-543">Condição (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-544">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-544">Applicable Attributes</span></span>

<span data-ttu-id="17a13-545">A tabela a seguir descreve os atributos que podem ser aplicados para o **MappingFragment** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="17a13-546">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-546">Attribute Name</span></span>          | <span data-ttu-id="17a13-547">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-547">Is Required</span></span> | <span data-ttu-id="17a13-548">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="17a13-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="17a13-550">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-550">Yes</span></span>         | <span data-ttu-id="17a13-551">O nome da tabela ou exibição que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="17a13-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="17a13-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="17a13-553">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-553">No</span></span>          | <span data-ttu-id="17a13-554">**Verdadeiro** ou **falso** dependendo se linhas distintas só são retornadas.</span><span class="sxs-lookup"><span data-stu-id="17a13-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="17a13-555">Se esse atributo for definido como **verdadeira**, o **GenerateUpdateViews** atributo do elemento EntityContainerMapping deve ser definido como **False**.</span><span class="sxs-lookup"><span data-stu-id="17a13-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-556">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-556">Example</span></span>

<span data-ttu-id="17a13-557">A exemplo a seguir mostra uma **MappingFragment** elemento como o filho de um **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="17a13-558">Neste exemplo, as propriedades do **curso** tipo no modelo conceitual são mapeadas para colunas da **curso** tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="17a13-559">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-559">Example</span></span>

<span data-ttu-id="17a13-560">A exemplo a seguir mostra uma **MappingFragment** elemento como o filho de um **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="17a13-561">Como no exemplo acima, as propriedades do **curso** tipo no modelo conceitual são mapeadas para colunas da **curso** tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="17a13-562">Elemento ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="17a13-563">O **ModificationFunctionMapping** elemento na linguagem de especificação (MSL) de mapeamento mapeia a inserir, atualizar e excluir funções de um tipo de entidade do modelo conceitual para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="17a13-564">O **ModificationFunctionMapping** elemento também pode mapear a inserção e excluir funções para associações de muitos-para-muitos no modelo conceitual para procedimentos armazenados no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="17a13-565">Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="17a13-566">Para obter mais informações, consulte o elemento de função (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-567">Se você não mapear todos os três da inserção, atualização ou exclusão operações de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executado em tempo de execução e um UpdateException é lançada.</span><span class="sxs-lookup"><span data-stu-id="17a13-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="17a13-568">Se as funções de modificação de uma entidade em uma hierarquia de herança são mapeadas para procedimentos armazenados, funções de modificação para todos os tipos na hierarquia devem ser mapeadas para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="17a13-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="17a13-569">O **ModificationFunctionMapping** elemento pode ser um filho do elemento EntityTypeMapping ou o elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="17a13-570">O **ModificationFunctionMapping** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-571">DeleteFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="17a13-572">InsertFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="17a13-573">UpdateFunction (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="17a13-574">Atributos não são aplicáveis para o **ModificationFunctionMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="17a13-575">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-575">Example</span></span>

<span data-ttu-id="17a13-576">O exemplo a seguir mostra o mapeamento de conjunto de entidades de **pessoas** entidade definida no modelo de escola.</span><span class="sxs-lookup"><span data-stu-id="17a13-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="17a13-577">Além do mapeamento de coluna para o **pessoa** tipo de entidade, o mapeamento da inserção, atualização e exclusão de funções da **pessoa** tipo são mostrados.</span><span class="sxs-lookup"><span data-stu-id="17a13-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="17a13-578">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="17a13-579">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-579">Example</span></span>

<span data-ttu-id="17a13-580">O exemplo a seguir mostra a associação ao definir o mapeamento para o **CourseInstructor** associação definida no modelo de escola.</span><span class="sxs-lookup"><span data-stu-id="17a13-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="17a13-581">Além do mapeamento de coluna para o **CourseInstructor** associação, o mapeamento das funções de insert e delete a **CourseInstructor** associação são mostrados.</span><span class="sxs-lookup"><span data-stu-id="17a13-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="17a13-582">As funções que são mapeadas para são declaradas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="17a13-583">Elemento QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="17a13-584">O **QueryView** elemento na linguagem de especificação (MSL) de mapeamento define um mapeamento entre um tipo de entidade ou a associação de somente leitura no modelo conceitual e uma tabela no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="17a13-585">O mapeamento é definido com uma consulta Entity SQL que é avaliada em relação ao modelo de armazenamento, e você expressar o resultado definido em termos de uma entidade ou a associação no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="17a13-586">Como os modos de exibição de consulta são somente leitura, você não pode usar comandos de atualização padrão para atualizar tipos que são definidos por modos de exibição de consulta.</span><span class="sxs-lookup"><span data-stu-id="17a13-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="17a13-587">Você pode fazer atualizações para esses tipos usando funções de modificação.</span><span class="sxs-lookup"><span data-stu-id="17a13-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="17a13-588">Para obter mais informações, consulte como: funções de modificação de mapa para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="17a13-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-589">No **QueryView** elemento, expressões de Entity SQL que contêm **GroupBy**, não há suporte para agregações de grupo, ou as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="17a13-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="17a13-590">O **QueryView** elemento pode ser um filho do elemento EntitySetMapping ou AssociationSetMapping elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="17a13-591">No caso anterior, a exibição de consulta define um mapeamento de somente leitura para uma entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="17a13-592">No último caso, a exibição de consulta define um mapeamento de somente leitura para uma associação no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-593">Se o **AssociationSetMapping** elemento é para uma associação com uma restrição referencial, o **AssociationSetMapping** elemento será ignorado.</span><span class="sxs-lookup"><span data-stu-id="17a13-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="17a13-594">Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="17a13-595">O **QueryView** elemento não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="17a13-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-596">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-596">Applicable Attributes</span></span>

<span data-ttu-id="17a13-597">A tabela a seguir descreve os atributos que podem ser aplicados para o **QueryView** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="17a13-598">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-598">Attribute Name</span></span> | <span data-ttu-id="17a13-599">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-599">Is Required</span></span> | <span data-ttu-id="17a13-600">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-601">**typeName**</span><span class="sxs-lookup"><span data-stu-id="17a13-601">**TypeName**</span></span>   | <span data-ttu-id="17a13-602">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-602">No</span></span>          | <span data-ttu-id="17a13-603">O nome do tipo de modelo conceitual que está sendo mapeado pelo modo de exibição de consulta.</span><span class="sxs-lookup"><span data-stu-id="17a13-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-604">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-604">Example</span></span>

<span data-ttu-id="17a13-605">A exemplo a seguir mostra a **QueryView** elemento como um filho a **EntitySetMapping** elemento e define um mapeamento de exibição de consulta para o **departamento** tipo de entidade no Modelo de escola.</span><span class="sxs-lookup"><span data-stu-id="17a13-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="17a13-606">Como a consulta retorna apenas um subconjunto dos membros do **departamento** tipo no modelo de armazenamento, o **departamento** no modelo de escola foi modificado com base nesse mapeamento da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="17a13-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="17a13-607">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-607">Example</span></span>

<span data-ttu-id="17a13-608">A exemplo a seguir mostra a **QueryView** elemento como o filho de um **AssociationSetMapping** elemento e define um mapeamento de somente leitura para o `FK_Course_Department` associação no modelo de escola.</span><span class="sxs-lookup"><span data-stu-id="17a13-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="17a13-609">Comentários</span><span class="sxs-lookup"><span data-stu-id="17a13-609">Comments</span></span>

<span data-ttu-id="17a13-610">Você pode definir os modos de exibição de consulta para habilitar os seguintes cenários:</span><span class="sxs-lookup"><span data-stu-id="17a13-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="17a13-611">Defina uma entidade no modelo conceitual que não inclui todas as propriedades da entidade no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="17a13-612">Isso inclui as propriedades que não têm valores padrão e não dão suporte a **nulo** valores.</span><span class="sxs-lookup"><span data-stu-id="17a13-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="17a13-613">Mapear colunas calculadas no modelo de armazenamento para as propriedades de tipos de entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="17a13-614">Defina um mapeamento em que condições usadas para entidades de partição no modelo conceitual não sejam baseadas em igualdade.</span><span class="sxs-lookup"><span data-stu-id="17a13-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="17a13-615">Quando você especifica um mapeamento condicional usando o **condição** elemento, o valor especificado deve ser igual a condição fornecida.</span><span class="sxs-lookup"><span data-stu-id="17a13-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="17a13-616">Para obter mais informações, consulte o elemento de condição (MSL).</span><span class="sxs-lookup"><span data-stu-id="17a13-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="17a13-617">Mapear a mesma coluna no modelo de armazenamento para vários tipos no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="17a13-618">Mapear vários tipos para a mesma tabela.</span><span class="sxs-lookup"><span data-stu-id="17a13-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="17a13-619">Defina associações no modelo conceitual que não são baseados em chaves estrangeiras no esquema relacional.</span><span class="sxs-lookup"><span data-stu-id="17a13-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="17a13-620">Use a lógica de negócios personalizada para definir o valor das propriedades no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="17a13-621">Por exemplo, você pode mapear o valor de cadeia de caracteres "T" na fonte de dados para um valor de **verdadeira**, um valor booliano, no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="17a13-622">Defina filtros condicionais para os resultados da consulta.</span><span class="sxs-lookup"><span data-stu-id="17a13-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="17a13-623">Impor menos restrições em dados no modelo conceitual que no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="17a13-624">Por exemplo, você poderia fazer uma propriedade no modelo conceitual que permitem valor nulo mesmo se a coluna para a qual está mapeada não suporta **nulo**valores.</span><span class="sxs-lookup"><span data-stu-id="17a13-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="17a13-625">As seguintes considerações se aplicam quando você define modos de exibição de consulta para entidades:</span><span class="sxs-lookup"><span data-stu-id="17a13-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="17a13-626">Modos de exibição de consulta são somente leitura.</span><span class="sxs-lookup"><span data-stu-id="17a13-626">Query views are read-only.</span></span> <span data-ttu-id="17a13-627">Você só pode fazer atualizações para entidades usando funções de modificação.</span><span class="sxs-lookup"><span data-stu-id="17a13-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="17a13-628">Quando você define um tipo de entidade por uma exibição de consulta, você também deve definir todas as entidades relacionadas por modos de exibição de consulta.</span><span class="sxs-lookup"><span data-stu-id="17a13-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="17a13-629">Quando você mapeia uma associação de muitos-para-muitos a uma entidade no modelo de armazenamento que representa uma tabela de link no esquema relacional, você deve definir um **QueryView** elemento o **AssociationSetMapping** elemento para essa tabela de vínculo.</span><span class="sxs-lookup"><span data-stu-id="17a13-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="17a13-630">Modos de exibição de consulta devem ser definidos para todos os tipos em uma hierarquia de tipo.</span><span class="sxs-lookup"><span data-stu-id="17a13-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="17a13-631">Você pode fazer isso das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="17a13-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="17a13-632">Com um único **QueryView** elemento que especifica uma única consulta Entity SQL que retorna uma união de todos os tipos de entidade na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="17a13-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="17a13-633">Com um único **QueryView** elemento que especifica uma única consulta Entity SQL que usa o operador de maiusculas para retornar um tipo de entidade específico na hierarquia com base em uma condição específica.</span><span class="sxs-lookup"><span data-stu-id="17a13-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="17a13-634">Com adicional **QueryView** elemento para um tipo específico na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="17a13-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="17a13-635">Nesse caso, use o **TypeName** atributo da **QueryView** elemento para especificar o tipo de entidade para cada modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="17a13-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="17a13-636">Quando uma exibição de consulta é definida, você não pode especificar o **StorageSetName** atributo as **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="17a13-637">Quando uma exibição de consulta é definida, o **EntitySetMapping**elemento também não pode conter **propriedade** mapeamentos.</span><span class="sxs-lookup"><span data-stu-id="17a13-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="17a13-638">Elemento ResultBinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="17a13-639">O **ResultBinding** elemento na linguagem de especificação (MSL) de mapeamento mapeia os valores de coluna que são retornados por procedimentos armazenados às propriedades de entidade no modelo conceitual quando funções de modificação do tipo de entidade são mapeadas para armazenado procedimentos no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="17a13-640">Por exemplo, quando o valor de uma coluna de identidade é retornado por uma inserção procedimento armazenado, o **ResultBinding** elemento é mapeado para o valor retornado para uma propriedade de tipo de entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="17a13-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="17a13-641">O **ResultBinding** elemento pode ser filho do elemento InsertFunction ou o elemento UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="17a13-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="17a13-642">O **ResultBinding** elemento não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="17a13-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-643">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-643">Applicable Attributes</span></span>

<span data-ttu-id="17a13-644">A tabela a seguir descreve os atributos que são aplicáveis para o **ResultBinding** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="17a13-645">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-645">Attribute Name</span></span> | <span data-ttu-id="17a13-646">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-646">Is Required</span></span> | <span data-ttu-id="17a13-647">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-648">**Nome**</span><span class="sxs-lookup"><span data-stu-id="17a13-648">**Name**</span></span>       | <span data-ttu-id="17a13-649">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-649">Yes</span></span>         | <span data-ttu-id="17a13-650">O nome da propriedade de entidade no modelo conceitual que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="17a13-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="17a13-651">**ColumnName**</span></span> | <span data-ttu-id="17a13-652">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-652">Yes</span></span>         | <span data-ttu-id="17a13-653">O nome da coluna que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="17a13-654">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-654">Example</span></span>

<span data-ttu-id="17a13-655">O exemplo a seguir é baseado no modelo de escola e mostra uma **InsertFunction** elemento usado para mapear a função de inserção da **pessoa** tipo de entidade para o **InsertPerson** procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="17a13-656">(O **InsertPerson** procedimento armazenado é mostrado abaixo e é declarado no modelo de armazenamento.) Um **ResultBinding** elemento é usado para mapear um valor de coluna que é retornado pelo procedimento armazenado (**NewPersonID**) para uma propriedade de tipo de entidade (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="17a13-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="17a13-657">O Transact-SQL a seguir descreve o **InsertPerson** procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="17a13-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="17a13-658">Elemento ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="17a13-659">O **ResultMapping** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado no banco de dados subjacente, quando as seguintes condições forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="17a13-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="17a13-660">A importação de função retorna um tipo de entidade do modelo conceitual ou tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="17a13-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="17a13-661">Os nomes das colunas retornadas pelo procedimento armazenado não exatamente coincidem com os nomes das propriedades no tipo de entidade ou tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="17a13-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="17a13-662">Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo de entidade ou tipo complexo é baseado em nomes de colunas e propriedades.</span><span class="sxs-lookup"><span data-stu-id="17a13-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="17a13-663">Se os nomes de coluna não correspondem exatamente os nomes de propriedade, você deve usar o **ResultMapping** elemento para definir o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="17a13-664">Para obter um exemplo de mapeamento padrão, consulte o elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="17a13-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="17a13-665">O **ResultMapping** é um elemento filho do elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="17a13-666">O **ResultMapping** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-667">EntityTypeMapping (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="17a13-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-668">ComplexTypeMapping</span></span>

<span data-ttu-id="17a13-669">Atributos não são aplicáveis para o **ResultMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="17a13-670">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-670">Example</span></span>

<span data-ttu-id="17a13-671">Considere o seguinte procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="17a13-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="17a13-672">Além disso, considere o seguinte tipo de entidade do modelo conceitual:</span><span class="sxs-lookup"><span data-stu-id="17a13-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="17a13-673">Para criar uma importação de função que retorna instâncias do tipo de entidade anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um **ResultMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="17a13-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="17a13-674">Elemento ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="17a13-675">O **ScalarProperty** elemento na linguagem de especificação (MSL) de mapeamento mapeia uma propriedade em um tipo de entidade do modelo conceitual, um tipo complexo ou uma associação a uma coluna de tabela ou um parâmetro de procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="17a13-676">Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="17a13-677">Para obter mais informações, consulte o elemento de função (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="17a13-678">O **ScalarProperty** elemento pode ser um filho dos elementos a seguir:</span><span class="sxs-lookup"><span data-stu-id="17a13-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="17a13-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="17a13-679">MappingFragment</span></span>
-   <span data-ttu-id="17a13-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="17a13-680">InsertFunction</span></span>
-   <span data-ttu-id="17a13-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="17a13-681">UpdateFunction</span></span>
-   <span data-ttu-id="17a13-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="17a13-682">DeleteFunction</span></span>
-   <span data-ttu-id="17a13-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="17a13-683">EndProperty</span></span>
-   <span data-ttu-id="17a13-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="17a13-684">ComplexProperty</span></span>
-   <span data-ttu-id="17a13-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="17a13-685">ResultMapping</span></span>

<span data-ttu-id="17a13-686">Como um filho de **MappingFragment**, **ComplexProperty**, ou **EndProperty** elemento, o **ScalarProperty** elemento é mapeado para uma propriedade no modelo conceitual para uma coluna no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="17a13-687">Como um filho de **InsertFunction**, **UpdateFunction**, ou **DeleteFunction** elemento, o **ScalarProperty** elemento é mapeado para uma propriedade no modelo conceitual para um parâmetro de procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="17a13-688">O **ScalarProperty** elemento não pode ter elementos filho.</span><span class="sxs-lookup"><span data-stu-id="17a13-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-689">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-689">Applicable Attributes</span></span>

<span data-ttu-id="17a13-690">Os atributos que se aplicam para o **ScalarProperty** elemento diferem dependendo da função do elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="17a13-691">A tabela a seguir descreve os atributos que são aplicáveis quando a **ScalarProperty** elemento é usado para mapear uma propriedade de modelo conceitual para uma coluna no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="17a13-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="17a13-692">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-692">Attribute Name</span></span> | <span data-ttu-id="17a13-693">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-693">Is Required</span></span> | <span data-ttu-id="17a13-694">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="17a13-695">**Nome**</span><span class="sxs-lookup"><span data-stu-id="17a13-695">**Name**</span></span>       | <span data-ttu-id="17a13-696">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-696">Yes</span></span>         | <span data-ttu-id="17a13-697">O nome da propriedade do modelo conceitual que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="17a13-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="17a13-698">**ColumnName**</span></span> | <span data-ttu-id="17a13-699">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-699">Yes</span></span>         | <span data-ttu-id="17a13-700">O nome da coluna da tabela que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="17a13-701">A tabela a seguir descreve os atributos que são aplicáveis para o **ScalarProperty** elemento quando ele é usado para mapear uma propriedade de modelo conceitual para um parâmetro de procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="17a13-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="17a13-702">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-702">Attribute Name</span></span>    | <span data-ttu-id="17a13-703">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-703">Is Required</span></span> | <span data-ttu-id="17a13-704">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-705">**Nome**</span><span class="sxs-lookup"><span data-stu-id="17a13-705">**Name**</span></span>          | <span data-ttu-id="17a13-706">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-706">Yes</span></span>         | <span data-ttu-id="17a13-707">O nome da propriedade do modelo conceitual que está sendo mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="17a13-708">**Nome do parâmetro**</span><span class="sxs-lookup"><span data-stu-id="17a13-708">**ParameterName**</span></span> | <span data-ttu-id="17a13-709">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-709">Yes</span></span>         | <span data-ttu-id="17a13-710">O nome do parâmetro que está sendo mapeado.</span><span class="sxs-lookup"><span data-stu-id="17a13-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="17a13-711">**Versão**</span><span class="sxs-lookup"><span data-stu-id="17a13-711">**Version**</span></span>       | <span data-ttu-id="17a13-712">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-712">No</span></span>          | <span data-ttu-id="17a13-713">**Atual** ou **Original** dependendo se o valor atual ou o valor original da propriedade deve ser usado para verificações de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="17a13-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="17a13-714">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-714">Example</span></span>

<span data-ttu-id="17a13-715">A exemplo a seguir mostra a **ScalarProperty** elemento usado de duas maneiras:</span><span class="sxs-lookup"><span data-stu-id="17a13-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="17a13-716">Para mapear as propriedades do **pessoa** as colunas de tipo de entidade a **pessoa**tabela.</span><span class="sxs-lookup"><span data-stu-id="17a13-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="17a13-717">Para mapear as propriedades do **pessoa** os parâmetros de tipo de entidade a **UpdatePerson** procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="17a13-718">Os procedimentos armazenados são declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="17a13-719">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-719">Example</span></span>

<span data-ttu-id="17a13-720">A exemplo a seguir mostra a **ScalarProperty** elemento usado para mapear a inserção e exclusão de funções de uma associação de modelo conceitual para procedimentos armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17a13-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="17a13-721">Os procedimentos armazenados são declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="17a13-722">Elemento UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="17a13-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="17a13-723">O **UpdateFunction** elemento na linguagem de especificação (MSL) de mapeamento mapeia a função de atualização de um tipo de entidade no modelo conceitual para um procedimento armazenado no banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="17a13-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="17a13-724">Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="17a13-725">Para obter mais informações, consulte o elemento de função (SSDL).</span><span class="sxs-lookup"><span data-stu-id="17a13-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="17a13-726">Se você não mapear todos os três da inserção, atualização ou exclusão operações de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executado em tempo de execução e um UpdateException é lançada.</span><span class="sxs-lookup"><span data-stu-id="17a13-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="17a13-727">O **UpdateFunction** elemento pode ser um filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="17a13-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="17a13-728">O **UpdateFunction** elemento pode ter os seguintes elementos filho:</span><span class="sxs-lookup"><span data-stu-id="17a13-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="17a13-729">AssociationEnd (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="17a13-730">ComplexProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="17a13-731">ResultBinding (zero ou um)</span><span class="sxs-lookup"><span data-stu-id="17a13-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="17a13-732">ScarlarProperty (zero ou mais)</span><span class="sxs-lookup"><span data-stu-id="17a13-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="17a13-733">Atributos aplicáveis</span><span class="sxs-lookup"><span data-stu-id="17a13-733">Applicable Attributes</span></span>

<span data-ttu-id="17a13-734">A tabela a seguir descreve os atributos que podem ser aplicados para o **UpdateFunction** elemento.</span><span class="sxs-lookup"><span data-stu-id="17a13-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="17a13-735">Nome do atributo</span><span class="sxs-lookup"><span data-stu-id="17a13-735">Attribute Name</span></span>            | <span data-ttu-id="17a13-736">É obrigatório</span><span class="sxs-lookup"><span data-stu-id="17a13-736">Is Required</span></span> | <span data-ttu-id="17a13-737">Valor</span><span class="sxs-lookup"><span data-stu-id="17a13-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a13-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="17a13-738">**FunctionName**</span></span>          | <span data-ttu-id="17a13-739">Sim</span><span class="sxs-lookup"><span data-stu-id="17a13-739">Yes</span></span>         | <span data-ttu-id="17a13-740">O nome qualificado de namespace do procedimento armazenado para o qual a função de atualização é mapeada.</span><span class="sxs-lookup"><span data-stu-id="17a13-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="17a13-741">O procedimento armazenado deve ser declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="17a13-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="17a13-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="17a13-743">Não</span><span class="sxs-lookup"><span data-stu-id="17a13-743">No</span></span>          | <span data-ttu-id="17a13-744">O nome do parâmetro de saída que retorna o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="17a13-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="17a13-745">Exemplo</span><span class="sxs-lookup"><span data-stu-id="17a13-745">Example</span></span>

<span data-ttu-id="17a13-746">O exemplo a seguir é baseado no modelo de escola e mostra a **UpdateFunction** elemento usado para mapear a função de atualização do **pessoa** tipo de entidade para o **UpdatePerson** procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="17a13-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="17a13-747">O **UpdatePerson** procedimento armazenado é declarado no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="17a13-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
