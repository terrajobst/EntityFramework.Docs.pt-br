---
title: Especificação de MSL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 6bff1f5407bc0546e60b5bee1178be9aa4748bd8
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460131"
---
# <a name="msl-specification"></a>Especificação de MSL
Mapeamento de linguagem de especificação (MSL) é uma linguagem baseada em XML que descreve o mapeamento entre o modelo conceitual e o modelo de armazenamento de um aplicativo do Entity Framework.

Em um aplicativo do Entity Framework, os metadados de mapeamento é carregado de um arquivo. MSL (escrito em MSL) no momento da compilação. Entity Framework usa metadados de mapeamento em tempo de execução para converter consultas no modelo conceitual para comandos específicos do repositório.

O Entity Framework Designer (Designer de EF) armazena informações de mapeamento em um arquivo. edmx em tempo de design. No momento da compilação, o Entity Designer usa informações em um arquivo. edmx para criar o arquivo. MSL que é necessário pelo Entity Framework em tempo de execução

Nomes de todos os conceitual ou tipos de modelo de armazenamento que são referenciados em MSL devem ser qualificados por seus nomes de namespace respectivo. Para obter informações sobre o nome do namespace de modelo conceitual, consulte [especificação de CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Para obter informações sobre o nome de namespace do modelo de armazenamento, consulte [especificação de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Versões do MSL são diferenciadas por namespaces XML.

| Versão MSL | Namespace XML                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: schemas-microsoft-com:windows:storage:mapping:CS |
| MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Elemento alias (MSL)

O **Alias** elemento na linguagem de especificação (MSL) de mapeamento é um filho do elemento de mapeamento é usado para definir aliases para namespaces de modelos de armazenamento e o modelo conceitual. Nomes de todos os conceitual ou tipos de modelo de armazenamento que são referenciados em MSL devem ser qualificados por seus nomes de namespace respectivo. Para obter informações sobre o nome do namespace de modelo conceitual, consulte o elemento de esquema (CSDL). Para obter informações sobre o nome de namespace do modelo de armazenamento, consulte o elemento de esquema (SSDL).

O **Alias** elemento não pode ter elementos filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **Alias** elemento.

| Nome do atributo | É obrigatório | Valor                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Chave**        | Sim         | O alias do namespace que é especificado pela **valor** atributo. |
| **Value**      | Sim         | O namespace para o qual o valor da **chave** elemento é um alias.     |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **Alias** elemento que define um alias, `c`, para tipos que são definidos no modelo conceitual.

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

## <a name="associationend-element-msl"></a>Elemento AssociationEnd (MSL)

O **AssociationEnd** elemento na linguagem de especificação (MSL) de mapeamento é usado quando as funções de modificação de um tipo de entidade no modelo conceitual são mapeadas para procedimentos armazenados no banco de dados subjacente. Se uma modificação de procedimento armazenado assume um parâmetro cujo valor é mantido em uma propriedade de associação, o **AssociationEnd** elemento é mapeado para o valor da propriedade para o parâmetro. Para obter mais informações, consulte o exemplo a seguir.

Para obter mais informações sobre funções de modificação de tipos de entidade de mapeamento para procedimentos armazenados, consulte o elemento ModificationFunctionMapping (MSL) e instruções passo a passo: mapeando uma entidade para procedimentos armazenados.

O **AssociationEnd** elemento pode ter os seguintes elementos filho:

-   ScalarProperty

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis para o **AssociationEnd** elemento.

| Nome do atributo     | É obrigatório | Valor                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Sim         | O nome da associação que está sendo mapeada.                                                                                                                                 |
| **From**           | Sim         | O valor de **FromRole** atributo da propriedade de navegação que corresponde à associação que está sendo mapeada. Para obter mais informações, consulte o elemento NavigationProperty (CSDL). |
| **To**             | Sim         | O valor de **ToRole** atributo da propriedade de navegação que corresponde à associação que está sendo mapeada. Para obter mais informações, consulte o elemento NavigationProperty (CSDL).   |

### <a name="example"></a>Exemplo

Considere o seguinte tipo de entidade do modelo conceitual:

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

Também considere o seguinte procedimento armazenado:

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

Para mapear a função de atualização do `Course` entidade para esse procedimento armazenado, você deve fornecer um valor para o **DepartmentID** parâmetro. O valor para `DepartmentID` não corresponde a uma propriedade no tipo de entidade; ele está contido em uma associação independente cujo mapeamento é mostrado aqui:

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

O código a seguir mostra a **AssociationEnd** elemento usado para mapear o **DepartmentID** propriedade do **FK\_curso\_departamento** associação para o **UpdateCourse** procedimento armazenado (para o qual a função de atualização de **curso** tipo de entidade é mapeado):

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

## <a name="associationsetmapping-element-msl"></a>Elemento AssociationSetMapping (MSL)

O **AssociationSetMapping** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre uma associação nas colunas de tabela e de modelo conceituais no banco de dados subjacente.

Associações no modelo conceitual são tipos cujas propriedades representem colunas de chave primárias e estrangeiras no banco de dados subjacente. O **AssociationSetMapping** elemento usa dois elementos EndProperty para definir os mapeamentos entre colunas e propriedades de tipo de associação no banco de dados. Você pode colocar as condições nesses mapeamentos com o elemento de condição. Mapear o insert, update e delete funções para associações para procedimentos armazenados no banco de dados com o elemento ModificationFunctionMapping. Defina mapeamentos de somente leitura entre as associações e colunas de tabela usando uma cadeia de caracteres de Entity SQL em um elemento QueryView.

> [!NOTE]
> Se uma restrição referencial está definida para uma associação no modelo conceitual, a associação não precisa ser mapeada com um **AssociationSetMapping** elemento. Se um **AssociationSetMapping** elemento está presente para uma associação que tem uma restrição referencial, os mapeamentos definidos na **AssociationSetMapping** elemento será ignorado. Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).

O **AssociationSetMapping** elemento pode ter os seguintes elementos filho

-   QueryView (zero ou um)
-   EndProperty (zero ou dois)
-   Condição (zero ou mais)
-   ModificationFunctionMapping (zero ou um)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **AssociationSetMapping** elemento.

| Nome do atributo     | É obrigatório | Valor                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Nome**           | Sim         | O nome do conjunto de associação de modelo conceitual que está sendo mapeado.                      |
| **typeName**       | Não          | O nome qualificado de namespace do tipo de associação de modelo conceitual que está sendo mapeado. |
| **StoreEntitySet** | Não          | O nome da tabela que está sendo mapeada.                                                 |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **AssociationSetMapping** elemento no qual o **FK\_curso\_departamento** conjunto de associações no modelo conceitual está mapeada para o  **Curso** tabela no banco de dados. Mapeamentos entre colunas de tabela e propriedades de tipo de associação são especificados em filha **EndProperty** elementos.

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

## <a name="complexproperty-element-msl"></a>Elemento ComplexProperty (MSL)

Um **ComplexProperty** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre uma propriedade de tipo complexo em um colunas de tipo e a tabela da entidade de modelo conceitual no banco de dados subjacente. Os mapeamentos de coluna de propriedade são especificados nos elementos ScalarProperty de filho.

O **ComplexType** elemento de propriedade pode ter os seguintes elementos filho:

-   ScalarProperty (zero ou mais)
-   **ComplexProperty** (zero ou mais)
-   ComplextTypeMapping (zero ou mais)
-   Condição (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis para o **ComplexProperty** elemento:

| Nome do atributo | É obrigatório | Valor                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome da propriedade complexa de um tipo de entidade no modelo conceitual que está sendo mapeado. |
| **typeName**   | Não          | O nome qualificado de namespace do tipo de propriedade do modelo conceitual.                              |

### <a name="example"></a>Exemplo

O exemplo a seguir se baseia no modelo de escola. O tipo complexo a seguir foi adicionado ao modelo conceitual:

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

O **LastName** e **FirstName** propriedades do **pessoa** tipo de entidade foram substituídos por uma propriedade complexa, **nome**:

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

O MSL a seguir mostra a **ComplexProperty** elemento usado para mapear o **nome** propriedade para colunas no banco de dados subjacente:

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

## <a name="complextypemapping-element-msl"></a>Elemento ComplexTypeMapping (MSL)

O **ComplexTypeMapping** elemento na linguagem de especificação (MSL) de mapeamento é um filho do elemento ResultMapping e define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado subjacente banco de dados quando as seguintes condições forem verdadeiras:

-   A importação de função retorna um tipo complexo conceitual.
-   Os nomes das colunas retornadas pelo procedimento armazenado não exatamente coincidem com os nomes das propriedades no tipo complexo.

Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo complexo é baseado em nomes de colunas e propriedades. Se os nomes de coluna não correspondem exatamente os nomes de propriedade, você deve usar o **ComplexTypeMapping** elemento para definir o mapeamento. Para obter um exemplo de mapeamento padrão, consulte o elemento FunctionImportMapping (MSL).

O **ComplexTypeMapping** elemento pode ter os seguintes elementos filho:

-   ScalarProperty (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis para o **ComplexTypeMapping** elemento.

| Nome do atributo | É obrigatório | Valor                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **typeName**   | Sim         | O nome qualificado de namespace do tipo complexo que está sendo mapeado. |

### <a name="example"></a>Exemplo

Considere o seguinte procedimento armazenado:

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

Além disso, considere o seguinte tipo complexo de modelo conceitual:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Para criar uma importação de função que retorna instâncias do tipo complexo anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um **ComplexTypeMapping** elemento:

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

## <a name="condition-element-msl"></a>Elemento Condition (MSL)

O **condição** elemento na linguagem de especificação (MSL) de mapeamento coloca condições nos mapeamentos entre o modelo conceitual e o banco de dados subjacente. O mapeamento que é definido dentro de um nó XML é válidas se todas as condições, como o filho especificado na **condição** elementos, sejam atendidos. Caso contrário, o mapeamento não é válido. Por exemplo, se um elemento MappingFragment contém um ou mais **condição** elementos filho, o mapeamento definido dentro de **MappingFragment** nó só será válida se todas as condições do filho  **Condição** elementos forem atendidos.

Cada condição pode ser aplicada a qualquer um uma **nome** (o nome de uma propriedade de entidade do modelo conceitual, especificado pelo **nome** atributo), ou uma **ColumnName** (o nome de uma coluna em o banco de dados, especificado pelo **ColumnName** atributo). Quando o **nome** atributo é definido, a condição será verificada em relação a um valor de propriedade de entidade. Quando o **ColumnName** atributo for definido, a condição será verificada em relação a um valor de coluna. Somente um dos **nome** ou **ColumnName** atributo pode ser especificado em um **condição** elemento.

> [!NOTE]
> Quando o **condição** elemento é usado dentro de um elemento FunctionImportMapping, somente os **nome** atributo não é aplicável.

O **condição** elemento pode ser um filho dos elementos a seguir:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

O **condição** elemento não pode ter nenhum elemento filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis para o **condição** elemento:

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | Não          | O nome da coluna da tabela cujo valor é usado para avaliar a condição.                                                                                                                                                                                                                   |
| **IsNull**     | Não          | **Verdadeiro** ou **falso**. Se o valor for **verdadeira** e é o valor da coluna **nulo**, ou se o valor for **falso** e o valor da coluna não é **nulo**, a condição for verdadeira . Caso contrário, a condição for falsa. <br/> O **IsNull** e **valor** atributos não podem ser usados ao mesmo tempo. |
| **Value**      | Não          | O valor com o qual o valor da coluna é comparado. Se os valores forem iguais, a condição for verdadeira. Caso contrário, a condição for falsa. <br/> O **IsNull** e **valor** atributos não podem ser usados ao mesmo tempo.                                                                       |
| **Nome**       | Não          | O nome da propriedade de entidade do modelo conceitual, cujo valor é usado para avaliar a condição. <br/> Esse atributo não é aplicável se o **condição** elemento é usado dentro de um elemento FunctionImportMapping.                                                                           |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra **condição** elementos como filhos do **MappingFragment** elementos. Quando **HireDate** não for nulo e **EnrollmentDate** for nulo, os dados são mapeados entre o **SchoolModel.Instructor** tipo e o **PersonID**e **HireDate** colunas da **pessoa** tabela. Quando **EnrollmentDate** não for nulo e **HireDate** for nulo, os dados são mapeados entre o **SchoolModel.Student** tipo e o **PersonID** e **registro** colunas da **pessoa** tabela.

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

## <a name="deletefunction-element-msl"></a>Elemento DeleteFunction (MSL)

O **DeleteFunction** elemento na linguagem de especificação (MSL) de mapeamento mapeia a função de exclusão de um tipo de entidade ou a associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente. Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte o elemento de função (SSDL).

> [!NOTE]
> Se você não mapear todos os três da inserção, atualização ou exclusão operações de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executado em tempo de execução e um UpdateException é lançada.

### <a name="deletefunction-applied-to-entitytypemapping"></a>DeleteFunction aplicado a EntityTypeMapping

Quando aplicado ao elemento EntityTypeMapping, o **DeleteFunction** elemento é mapeado para a função de exclusão de um tipo de entidade no modelo conceitual para um procedimento armazenado.

O **DeleteFunction** elemento pode ter os seguintes elementos filho quando aplicado a um **EntityTypeMapping** elemento:

-   AssociationEnd (zero ou mais)
-   ComplexProperty (zero ou mais)
-   ScarlarProperty (zero ou mais)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **DeleteFunction** elemento quando ele é aplicado a um **EntityTypeMapping** elemento.

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado de namespace do procedimento armazenado para o qual a função de exclusão é mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

#### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo de escola e mostra a **DeleteFunction** elemento de mapeamento de função de exclusão do **pessoa** tipo de entidade para o **DeletePerson** procedimento armazenado. O **DeletePerson** procedimento armazenado é declarado no modelo de armazenamento.

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

### <a name="deletefunction-applied-to-associationsetmapping"></a>DeleteFunction aplicado a AssociationSetMapping

Quando aplicado ao elemento AssociationSetMapping, o **DeleteFunction** elemento é mapeado para a função de exclusão de uma associação no modelo conceitual para um procedimento armazenado.

O **DeleteFunction** elemento pode ter os seguintes elementos filho quando aplicado ao **AssociationSetMapping** elemento:

-   EndProperty

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **DeleteFunction** elemento quando ele é aplicado para o **AssociationSetMapping** elemento.

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado de namespace do procedimento armazenado para o qual a função de exclusão é mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

#### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo de escola e mostra a **DeleteFunction** elemento usado para mapear a função de exclusão do **CourseInstructor** associação para o  **DeleteCourseInstructor** procedimento armazenado. O **DeleteCourseInstructor** procedimento armazenado é declarado no modelo de armazenamento.

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

## <a name="endproperty-element-msl"></a>Elemento EndProperty (MSL)

O **EndProperty** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre um final ou uma função de modificação de uma associação de modelo conceitual e o banco de dados subjacente. O mapeamento de coluna de propriedade é especificado em um elemento ScalarProperty de filho.

Quando um **EndProperty** elemento é usado para definir o mapeamento para o final de uma associação de modelo conceitual, ele é um filho de um elemento AssociationSetMapping. Quando o **EndProperty** elemento é usado para definir o mapeamento para uma função de modificação de uma associação de modelo conceitual, ele é um filho de um elemento de InsertFunction ou o elemento DeleteFunction.

O **EndProperty** elemento pode ter os seguintes elementos filho:

-   ScalarProperty (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis para o **EndProperty** elemento:

| Nome do atributo | É obrigatório | Valor                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Nome           | Sim         | O nome da extremidade da associação que está sendo mapeada. |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **AssociationSetMapping** elemento no qual o **FK\_curso\_departamento** associação no modelo conceitual está mapeada para o **Curso** tabela no banco de dados. Mapeamentos entre colunas de tabela e propriedades de tipo de associação são especificados em filha **EndProperty** elementos.

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

### <a name="example"></a>Exemplo

A exemplo a seguir mostra a **EndProperty** elemento as funções de inserção e exclusão de uma associação de mapeamento (**CourseInstructor**) para procedimentos armazenados no banco de dados subjacente. As funções que são mapeadas para são declaradas no modelo de armazenamento.

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

## <a name="entitycontainermapping-element-msl"></a>Elemento EntityContainerMapping (MSL)

O **EntityContainerMapping** elemento na linguagem de especificação (MSL) de mapeamento mapeia o contêiner de entidade no modelo conceitual para o contêiner de entidade no modelo de armazenamento. O **EntityContainerMapping** elemento é um filho do elemento de mapeamento.

O **EntityContainerMapping** elemento pode ter os seguintes elementos filho (na ordem listada):

-   EntitySetMapping (zero ou mais)
-   AssociationSetMapping (zero ou mais)
-   FunctionImportMapping (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityContainerMapping** elemento.

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Sim         | O nome do contêiner de entidade de modelo de armazenamento que está sendo mapeado.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Sim         | O nome do contêiner de entidade do modelo conceitual que está sendo mapeado.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Não          | **Verdadeiro** ou **falso**. Se **falsos**, nenhum modo de exibição de atualização é gerados. Esse atributo deve ser definido como **falsos** quando você tiver um mapeamento de somente leitura que seria inválido porque dados podem ser ida e volta com êxito. <br/> O valor padrão é **verdadeira**. |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityContainerMapping** elemento que mapeia o **SchoolModelEntities** contêiner (o contêiner de entidade do modelo conceitual) para o  **SchoolModelStoreContainer** contêiner (o modelo entidade contêiner de armazenamento):

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

## <a name="entitysetmapping-element-msl"></a>Elemento EntitySetMapping (MSL)

O **EntitySetMapping** define um elemento em mapas MSL (linguagem) de especificação, todos os tipos em uma entidade de modelo conceitual são definidos como entidade de mapeamento no modelo de armazenamento. Uma entidade definida no modelo conceitual é um contêiner lógico para instâncias de entidades do mesmo tipo (e tipos derivados). Uma entidade definida no modelo de armazenamento representa uma tabela ou exibição no banco de dados subjacente. O conjunto de entidades do modelo conceitual é especificado pelo valor da **nome** atributo da **EntitySetMapping** elemento. Mapeado para a tabela ou exibição é especificada pelo **StoreEntitySet** atributo em cada elemento do filho MappingFragment ou nos **EntitySetMapping** próprio elemento.

O **EntitySetMapping** elemento pode ter os seguintes elementos filho:

-   EntityTypeMapping (zero ou mais)
-   QueryView (zero ou um)
-   MappingFragment (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EntitySetMapping** elemento.

| Nome do atributo           | É obrigatório | Valor                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**                 | Sim         | O nome do conjunto de entidades de modelo conceitual que está sendo mapeado.                                                                                                                                                             |
| **TypeName** **1**       | Não          | O nome do tipo de entidade de modelo conceitual que está sendo mapeado.                                                                                                                                                            |
| **StoreEntitySet** **1** | Não          | O nome do conjunto de entidades de modelo de armazenamento que está sendo mapeado para.                                                                                                                                                             |
| **MakeColumnsDistinct**  | Não          | **Verdadeiro** ou **falso** dependendo se linhas distintas só são retornadas. <br/> Se esse atributo for definido como **verdadeira**, o **GenerateUpdateViews** atributo do elemento EntityContainerMapping deve ser definido como **False**. |

 

**1** as **TypeName** e **StoreEntitySet** atributos podem ser usados no lugar de elementos filho EntityTypeMapping e MappingFragment para mapear um único tipo de entidade para uma única tabela.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntitySetMapping** elemento que mapeia tipos de três (um tipo base e dois tipos derivados) nos **cursos** conjunto de entidades do modelo conceitual para três tabelas diferentes no o banco de dados subjacente. As tabelas são especificadas pela **StoreEntitySet** atributo em cada **MappingFragment** elemento.

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

## <a name="entitytypemapping-element-msl"></a>Elemento EntityTypeMapping (MSL)

O **EntityTypeMapping** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre um tipo de entidade no modelo conceitual e tabelas ou exibições no banco de dados subjacente. Para obter informações sobre tipos de entidade do modelo conceitual e tabelas de banco de dados subjacente ou modos de exibição, consulte o elemento EntityType (CSDL) e o elemento EntitySet (SSDL). O tipo de entidade do modelo conceitual que está sendo mapeado é especificado pelo **TypeName** atributo da **EntityTypeMapping** elemento. A tabela ou exibição que está sendo mapeada é especificada pelo **StoreEntitySet** atributo do elemento filho MappingFragment.

O ModificationFunctionMapping elemento filho pode ser usado para mapear a inserção, atualização ou exclusão de funções dos tipos de entidade para procedimentos armazenados no banco de dados.

O **EntityTypeMapping** elemento pode ter os seguintes elementos filho:

-   MappingFragment (zero ou mais)
-   ModificationFunctionMapping (zero ou um)
-   ScalarProperty
-   Condição

> [!NOTE]
> **MappingFragment** e **ModificationFunctionMapping** elementos não podem ser elementos filho do **EntityTypeMapping** elemento ao mesmo tempo.


> [!NOTE]
> O **ScalarProperty** e **condição** elementos só podem ser elementos filho do **EntityTypeMapping** elemento quando ele é usado dentro de um elemento FunctionImportMapping.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityTypeMapping** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **typeName**   | Sim         | O nome qualificado de namespace do tipo de entidade de modelo conceitual que está sendo mapeado. <br/> Se o tipo é abstrato ou um tipo derivado, o valor deve ser `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento EntitySetMapping com dois filhos **EntityTypeMapping** elementos. No primeiro **EntityTypeMapping** elemento, o **SchoolModel.Person** tipo de entidade é mapeado para o **pessoa** tabela. Na segunda **EntityTypeMapping** elemento, a funcionalidade de atualização do **SchoolModel.Person** tipo é mapeado para um procedimento armazenado **UpdatePerson**, no banco de dados .

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

### <a name="example"></a>Exemplo

O exemplo a seguir mostra o mapeamento de uma hierarquia de tipo no qual o tipo de raiz é abstrato. Observe o uso do `IsOfType` sintaxe para o **TypeName** atributos.

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

## <a name="functionimportmapping-element-msl"></a>Elemento FunctionImportMapping (MSL)

O **FunctionImportMapping** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado ou função no banco de dados subjacente. Importações de função devem ser declaradas no modelo conceitual e procedimentos armazenados devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte o elemento FunctionImport (CSDL) e o elemento de função (SSDL).

> [!NOTE]
> Por padrão, se uma importação de função retorna um tipo de entidade do modelo conceitual ou de um tipo complexo, em seguida, os nomes das colunas retornadas pelo procedimento armazenado subjacente devem corresponder exatamente aos nomes das propriedades no tipo de modelo conceitual. Se os nomes de coluna não correspondem exatamente os nomes de propriedade, o mapeamento deve ser definido em um elemento ResultMapping.

O **FunctionImportMapping** elemento pode ter os seguintes elementos filho:

-   ResultMapping (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis para o **FunctionImportMapping** elemento:

| Nome do atributo         | É obrigatório | Valor                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Sim         | O nome da função de importação no modelo conceitual que está sendo mapeada.           |
| **FunctionName**       | Sim         | O nome qualificado de namespace da função no modelo de armazenamento que está sendo mapeada. |

### <a name="example"></a>Exemplo

O exemplo a seguir se baseia no modelo de escola. Considere a seguinte função no modelo de armazenamento:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Além disso, considere esta importação de função no modelo conceitual:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

Apresentação do exemplo a seguir uma **FunctionImportMapping** elemento usado para mapear a função e a função importação acima uns aos outros:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>Elemento InsertFunction (MSL)

O **InsertFunction** elemento na linguagem de especificação (MSL) de mapeamento mapeia a função de inserção de um tipo de entidade ou a associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente. Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte o elemento de função (SSDL).

> [!NOTE]
> Se você não mapear todos os três da inserção, atualização ou exclusão operações de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executado em tempo de execução e um UpdateException é lançada.

O **InsertFunction** elemento pode ser um filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping ou o elemento AssociationSetMapping.

### <a name="insertfunction-applied-to-entitytypemapping"></a>InsertFunction aplicado a EntityTypeMapping

Quando aplicado ao elemento EntityTypeMapping, o **InsertFunction** elemento é mapeado para a função de inserção de um tipo de entidade no modelo conceitual para um procedimento armazenado.

O **InsertFunction** elemento pode ter os seguintes elementos filho quando aplicado a um **EntityTypeMapping** elemento:

-   AssociationEnd (zero ou mais)
-   ComplexProperty (zero ou mais)
-   ResultBinding (zero ou um)
-   ScarlarProperty (zero ou mais)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **InsertFunction** elemento quando aplicado a um **EntityTypeMapping** elemento.

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado de namespace do procedimento armazenado para o qual a função de inserção é mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

#### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo de escola e mostra a **InsertFunction** elemento usado para mapear a função de inserção do tipo de entidade Person para o **InsertPerson** procedimento armazenado. O **InsertPerson** procedimento armazenado é declarado no modelo de armazenamento.

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
### <a name="insertfunction-applied-to-associationsetmapping"></a>InsertFunction aplicado a AssociationSetMapping

Quando aplicado ao elemento AssociationSetMapping, o **InsertFunction** elemento é mapeado para a função de inserção de uma associação no modelo conceitual para um procedimento armazenado.

O **InsertFunction** elemento pode ter os seguintes elementos filho quando aplicado ao **AssociationSetMapping** elemento:

-   EndProperty

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **InsertFunction** elemento quando ele é aplicado para o **AssociationSetMapping** elemento.

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado de namespace do procedimento armazenado para o qual a função de inserção é mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

#### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo de escola e mostra a **InsertFunction** elemento usado para mapear a função de inserção da **CourseInstructor** associação para o  **InsertCourseInstructor** procedimento armazenado. O **InsertCourseInstructor** procedimento armazenado é declarado no modelo de armazenamento.

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

## <a name="mapping-element-msl"></a>Elemento de mapeamento (MSL)

O **mapeamento** elemento na linguagem de especificação (MSL) de mapeamento contém informações para o mapeamento de objetos que são definidos em um modelo conceitual para um banco de dados (conforme descrito em um modelo de armazenamento). Para obter mais informações, consulte a especificação de CSDL e SSDL especificação.

O **mapeamento** é o elemento raiz para uma especificação de mapeamento. O namespace XML para o mapeamento de especificações é http://schemas.microsoft.com/ado/2009/11/mapping/cs.

O elemento de mapeamento pode ter os seguintes elementos filho (na ordem listada):

-   Alias (zero ou mais)
-   EntityContainerMapping (exatamente uma)

Nomes de conceitual e tipos de modelo de armazenamento que são referenciados em MSL devem ser qualificados por seus nomes de namespace respectivo. Para obter informações sobre o nome do namespace de modelo conceitual, consulte o elemento de esquema (CSDL). Para obter informações sobre o nome de namespace do modelo de armazenamento, consulte o elemento de esquema (SSDL). Aliases para namespaces que são usados em MSL podem ser definidos com o elemento de Alias.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **mapeamento** elemento.

| Nome do atributo | É obrigatório | Valor                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Espaço**      | Sim         | **C-S**. Isso é um valor fixo e não pode ser alterado. |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **mapeamento** elemento baseia-se parte do modelo de escola. Para obter mais informações sobre o modelo de escola, consulte o guia de início rápido (Entity Framework):

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

## <a name="mappingfragment-element-msl"></a>Elemento MappingFragment (MSL)

O **MappingFragment** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre as propriedades de um tipo de entidade do modelo conceitual e uma tabela ou exibição no banco de dados. Para obter informações sobre tipos de entidade do modelo conceitual e tabelas de banco de dados subjacente ou modos de exibição, consulte o elemento EntityType (CSDL) e o elemento EntitySet (SSDL). O **MappingFragment** pode ser um elemento filho do elemento EntityTypeMapping ou elemento EntitySetMapping.

O **MappingFragment** elemento pode ter os seguintes elementos filho:

-   ComplexType (zero ou mais)
-   ScalarProperty (zero ou mais)
-   Condição (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **MappingFragment** elemento.

| Nome do atributo          | É obrigatório | Valor                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Sim         | O nome da tabela ou exibição que está sendo mapeada.                                                                                                                                                                           |
| **MakeColumnsDistinct** | Não          | **Verdadeiro** ou **falso** dependendo se linhas distintas só são retornadas. <br/> Se esse atributo for definido como **verdadeira**, o **GenerateUpdateViews** atributo do elemento EntityContainerMapping deve ser definido como **False**. |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **MappingFragment** elemento como o filho de um **EntityTypeMapping** elemento. Neste exemplo, as propriedades do **curso** tipo no modelo conceitual são mapeadas para colunas da **curso** tabela no banco de dados.

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

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **MappingFragment** elemento como o filho de um **EntitySetMapping** elemento. Como no exemplo acima, as propriedades do **curso** tipo no modelo conceitual são mapeadas para colunas da **curso** tabela no banco de dados.

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

## <a name="modificationfunctionmapping-element-msl"></a>Elemento ModificationFunctionMapping (MSL)

O **ModificationFunctionMapping** elemento na linguagem de especificação (MSL) de mapeamento mapeia a inserir, atualizar e excluir funções de um tipo de entidade do modelo conceitual para procedimentos armazenados no banco de dados subjacente. O **ModificationFunctionMapping** elemento também pode mapear a inserção e excluir funções para associações de muitos-para-muitos no modelo conceitual para procedimentos armazenados no banco de dados subjacente. Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte o elemento de função (SSDL).

> [!NOTE]
> Se você não mapear todos os três da inserção, atualização ou exclusão operações de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executado em tempo de execução e um UpdateException é lançada.


> [!NOTE]
> Se as funções de modificação de uma entidade em uma hierarquia de herança são mapeadas para procedimentos armazenados, funções de modificação para todos os tipos na hierarquia devem ser mapeadas para procedimentos armazenados.

O **ModificationFunctionMapping** elemento pode ser um filho do elemento EntityTypeMapping ou o elemento AssociationSetMapping.

O **ModificationFunctionMapping** elemento pode ter os seguintes elementos filho:

-   DeleteFunction (zero ou um)
-   InsertFunction (zero ou um)
-   UpdateFunction (zero ou um)

Atributos não são aplicáveis para o **ModificationFunctionMapping** elemento.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra o mapeamento de conjunto de entidades de **pessoas** entidade definida no modelo de escola. Além do mapeamento de coluna para o **pessoa** tipo de entidade, o mapeamento da inserção, atualização e exclusão de funções da **pessoa** tipo são mostrados. As funções que são mapeadas para são declaradas no modelo de armazenamento.

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

### <a name="example"></a>Exemplo

O exemplo a seguir mostra a associação ao definir o mapeamento para o **CourseInstructor** associação definida no modelo de escola. Além do mapeamento de coluna para o **CourseInstructor** associação, o mapeamento das funções de insert e delete a **CourseInstructor** associação são mostrados. As funções que são mapeadas para são declaradas no modelo de armazenamento.

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
 

 

## <a name="queryview-element-msl"></a>Elemento QueryView (MSL)

O **QueryView** elemento na linguagem de especificação (MSL) de mapeamento define um mapeamento entre um tipo de entidade ou a associação de somente leitura no modelo conceitual e uma tabela no banco de dados subjacente. O mapeamento é definido com uma consulta Entity SQL que é avaliada em relação ao modelo de armazenamento, e você expressar o resultado definido em termos de uma entidade ou a associação no modelo conceitual. Como os modos de exibição de consulta são somente leitura, você não pode usar comandos de atualização padrão para atualizar tipos que são definidos por modos de exibição de consulta. Você pode fazer atualizações para esses tipos usando funções de modificação. Para obter mais informações, consulte como: funções de modificação de mapa para procedimentos armazenados.

> [!NOTE]
> No **QueryView** elemento, expressões de Entity SQL que contêm **GroupBy**, não há suporte para agregações de grupo, ou as propriedades de navegação.

 

O **QueryView** elemento pode ser um filho do elemento EntitySetMapping ou AssociationSetMapping elemento. No caso anterior, a exibição de consulta define um mapeamento de somente leitura para uma entidade no modelo conceitual. No último caso, a exibição de consulta define um mapeamento de somente leitura para uma associação no modelo conceitual.

> [!NOTE]
> Se o **AssociationSetMapping** elemento é para uma associação com uma restrição referencial, o **AssociationSetMapping** elemento será ignorado. Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).

O **QueryView** elemento não pode ter elementos filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **QueryView** elemento.

| Nome do atributo | É obrigatório | Valor                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **typeName**   | Não          | O nome do tipo de modelo conceitual que está sendo mapeado pelo modo de exibição de consulta. |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra a **QueryView** elemento como um filho a **EntitySetMapping** elemento e define um mapeamento de exibição de consulta para o **departamento** tipo de entidade no Modelo de escola.

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

Como a consulta retorna apenas um subconjunto dos membros do **departamento** tipo no modelo de armazenamento, o **departamento** no modelo de escola foi modificado com base nesse mapeamento da seguinte maneira:

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

### <a name="example"></a>Exemplo

A exemplo a seguir mostra a **QueryView** elemento como o filho de um **AssociationSetMapping** elemento e define um mapeamento de somente leitura para o `FK_Course_Department` associação no modelo de escola.

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
 
### <a name="comments"></a>Comentários

Você pode definir os modos de exibição de consulta para habilitar os seguintes cenários:

-   Defina uma entidade no modelo conceitual que não inclui todas as propriedades da entidade no modelo de armazenamento. Isso inclui as propriedades que não têm valores padrão e não dão suporte a **nulo** valores.
-   Mapear colunas calculadas no modelo de armazenamento para as propriedades de tipos de entidade no modelo conceitual.
-   Defina um mapeamento em que condições usadas para entidades de partição no modelo conceitual não sejam baseadas em igualdade. Quando você especifica um mapeamento condicional usando o **condição** elemento, o valor especificado deve ser igual a condição fornecida. Para obter mais informações, consulte o elemento de condição (MSL).
-   Mapear a mesma coluna no modelo de armazenamento para vários tipos no modelo conceitual.
-   Mapear vários tipos para a mesma tabela.
-   Defina associações no modelo conceitual que não são baseados em chaves estrangeiras no esquema relacional.
-   Use a lógica de negócios personalizada para definir o valor das propriedades no modelo conceitual. Por exemplo, você pode mapear o valor de cadeia de caracteres "T" na fonte de dados para um valor de **verdadeira**, um valor booliano, no modelo conceitual.
-   Defina filtros condicionais para os resultados da consulta.
-   Impor menos restrições em dados no modelo conceitual que no modelo de armazenamento. Por exemplo, você poderia fazer uma propriedade no modelo conceitual que permitem valor nulo mesmo se a coluna para a qual está mapeada não suporta **nulo**valores.

As seguintes considerações se aplicam quando você define modos de exibição de consulta para entidades:

-   Modos de exibição de consulta são somente leitura. Você só pode fazer atualizações para entidades usando funções de modificação.
-   Quando você define um tipo de entidade por uma exibição de consulta, você também deve definir todas as entidades relacionadas por modos de exibição de consulta.
-   Quando você mapeia uma associação de muitos-para-muitos a uma entidade no modelo de armazenamento que representa uma tabela de link no esquema relacional, você deve definir um **QueryView** elemento o **AssociationSetMapping** elemento para essa tabela de vínculo.
-   Modos de exibição de consulta devem ser definidos para todos os tipos em uma hierarquia de tipo. Você pode fazer isso das seguintes maneiras:
-   -   Com um único **QueryView** elemento que especifica uma única consulta Entity SQL que retorna uma união de todos os tipos de entidade na hierarquia.
    -   Com um único **QueryView** elemento que especifica uma única consulta Entity SQL que usa o operador de maiusculas para retornar um tipo de entidade específico na hierarquia com base em uma condição específica.
    -   Com adicional **QueryView** elemento para um tipo específico na hierarquia. Nesse caso, use o **TypeName** atributo da **QueryView** elemento para especificar o tipo de entidade para cada modo de exibição.
-   Quando uma exibição de consulta é definida, você não pode especificar o **StorageSetName** atributo as **EntitySetMapping** elemento.
-   Quando uma exibição de consulta é definida, o **EntitySetMapping**elemento também não pode conter **propriedade** mapeamentos.

## <a name="resultbinding-element-msl"></a>Elemento ResultBinding (MSL)

O **ResultBinding** elemento na linguagem de especificação (MSL) de mapeamento mapeia os valores de coluna que são retornados por procedimentos armazenados às propriedades de entidade no modelo conceitual quando funções de modificação do tipo de entidade são mapeadas para armazenado procedimentos no banco de dados subjacente. Por exemplo, quando o valor de uma coluna de identidade é retornado por uma inserção procedimento armazenado, o **ResultBinding** elemento é mapeado para o valor retornado para uma propriedade de tipo de entidade no modelo conceitual.

O **ResultBinding** elemento pode ser filho do elemento InsertFunction ou o elemento UpdateFunction.

O **ResultBinding** elemento não pode ter elementos filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis para o **ResultBinding** elemento:

| Nome do atributo | É obrigatório | Valor                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome da propriedade de entidade no modelo conceitual que está sendo mapeada. |
| **ColumnName** | Sim         | O nome da coluna que está sendo mapeado.                                          |

### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo de escola e mostra uma **InsertFunction** elemento usado para mapear a função de inserção da **pessoa** tipo de entidade para o **InsertPerson** procedimento armazenado. (O **InsertPerson** procedimento armazenado é mostrado abaixo e é declarado no modelo de armazenamento.) Um **ResultBinding** elemento é usado para mapear um valor de coluna que é retornado pelo procedimento armazenado (**NewPersonID**) para uma propriedade de tipo de entidade (**PersonID**).

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

O Transact-SQL a seguir descreve o **InsertPerson** procedimento armazenado:

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

## <a name="resultmapping-element-msl"></a>Elemento ResultMapping (MSL)

O **ResultMapping** elemento na linguagem de especificação (MSL) de mapeamento define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado no banco de dados subjacente, quando as seguintes condições forem verdadeiras:

-   A importação de função retorna um tipo de entidade do modelo conceitual ou tipo complexo.
-   Os nomes das colunas retornadas pelo procedimento armazenado não exatamente coincidem com os nomes das propriedades no tipo de entidade ou tipo complexo.

Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo de entidade ou tipo complexo é baseado em nomes de colunas e propriedades. Se os nomes de coluna não correspondem exatamente os nomes de propriedade, você deve usar o **ResultMapping** elemento para definir o mapeamento. Para obter um exemplo de mapeamento padrão, consulte o elemento FunctionImportMapping (MSL).

O **ResultMapping** é um elemento filho do elemento FunctionImportMapping.

O **ResultMapping** elemento pode ter os seguintes elementos filho:

-   EntityTypeMapping (zero ou mais)
-   ComplexTypeMapping

Atributos não são aplicáveis para o **ResultMapping** elemento.

### <a name="example"></a>Exemplo

Considere o seguinte procedimento armazenado:

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

Além disso, considere o seguinte tipo de entidade do modelo conceitual:

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

Para criar uma importação de função que retorna instâncias do tipo de entidade anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um **ResultMapping** elemento:

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

## <a name="scalarproperty-element-msl"></a>Elemento ScalarProperty (MSL)

O **ScalarProperty** elemento na linguagem de especificação (MSL) de mapeamento mapeia uma propriedade em um tipo de entidade do modelo conceitual, um tipo complexo ou uma associação a uma coluna de tabela ou um parâmetro de procedimento armazenado no banco de dados subjacente.

> [!NOTE]
> Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte o elemento de função (SSDL).

O **ScalarProperty** elemento pode ser um filho dos elementos a seguir:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Como um filho de **MappingFragment**, **ComplexProperty**, ou **EndProperty** elemento, o **ScalarProperty** elemento é mapeado para uma propriedade no modelo conceitual para uma coluna no banco de dados. Como um filho de **InsertFunction**, **UpdateFunction**, ou **DeleteFunction** elemento, o **ScalarProperty** elemento é mapeado para uma propriedade no modelo conceitual para um parâmetro de procedimento armazenado.

O **ScalarProperty** elemento não pode ter elementos filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

Os atributos que se aplicam para o **ScalarProperty** elemento diferem dependendo da função do elemento.

A tabela a seguir descreve os atributos que são aplicáveis quando a **ScalarProperty** elemento é usado para mapear uma propriedade de modelo conceitual para uma coluna no banco de dados:

| Nome do atributo | É obrigatório | Valor                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Nome**       | Sim         | O nome da propriedade do modelo conceitual que está sendo mapeada. |
| **ColumnName** | Sim         | O nome da coluna da tabela que está sendo mapeada.              |

A tabela a seguir descreve os atributos que são aplicáveis para o **ScalarProperty** elemento quando ele é usado para mapear uma propriedade de modelo conceitual para um parâmetro de procedimento armazenado:

| Nome do atributo    | É obrigatório | Valor                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**          | Sim         | O nome da propriedade do modelo conceitual que está sendo mapeada.                                                                                 |
| **Nome do parâmetro** | Sim         | O nome do parâmetro que está sendo mapeado.                                                                                                 |
| **Versão**       | Não          | **Atual** ou **Original** dependendo se o valor atual ou o valor original da propriedade deve ser usado para verificações de simultaneidade. |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra a **ScalarProperty** elemento usado de duas maneiras:

-   Para mapear as propriedades do **pessoa** as colunas de tipo de entidade a **pessoa**tabela.
-   Para mapear as propriedades do **pessoa** os parâmetros de tipo de entidade a **UpdatePerson** procedimento armazenado. Os procedimentos armazenados são declarados no modelo de armazenamento.

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

### <a name="example"></a>Exemplo

A exemplo a seguir mostra a **ScalarProperty** elemento usado para mapear a inserção e exclusão de funções de uma associação de modelo conceitual para procedimentos armazenados no banco de dados. Os procedimentos armazenados são declarados no modelo de armazenamento.

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

## <a name="updatefunction-element-msl"></a>Elemento UpdateFunction (MSL)

O **UpdateFunction** elemento na linguagem de especificação (MSL) de mapeamento mapeia a função de atualização de um tipo de entidade no modelo conceitual para um procedimento armazenado no banco de dados subjacente. Procedimentos armazenados para modificação de quais funções são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte o elemento de função (SSDL).

> [!NOTE]
>  Se você não mapear todos os três da inserção, atualização ou exclusão operações de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executado em tempo de execução e um UpdateException é lançada.

O **UpdateFunction** elemento pode ser um filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping.

O **UpdateFunction** elemento pode ter os seguintes elementos filho:

-   AssociationEnd (zero ou mais)
-   ComplexProperty (zero ou mais)
-   ResultBinding (zero ou um)
-   ScarlarProperty (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **UpdateFunction** elemento.

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado de namespace do procedimento armazenado para o qual a função de atualização é mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo de escola e mostra a **UpdateFunction** elemento usado para mapear a função de atualização do **pessoa** tipo de entidade para o **UpdatePerson** procedimento armazenado. O **UpdatePerson** procedimento armazenado é declarado no modelo de armazenamento.

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
