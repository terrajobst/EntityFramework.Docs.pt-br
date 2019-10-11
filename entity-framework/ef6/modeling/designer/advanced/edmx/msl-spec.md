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
# <a name="msl-specification"></a>Especificação de MSL
O MSL (Mapping Specification Language) é uma linguagem baseada em XML que descreve o mapeamento entre o modelo conceitual e o modelo de armazenamento de um aplicativo Entity Framework.

Em um aplicativo Entity Framework, os metadados de mapeamento são carregados de um arquivo. MSL (escrito em MSL) no momento da compilação. O Entity Framework usa metadados de mapeamento em tempo de execução para converter consultas em relação ao modelo conceitual para armazenar comandos específicos.

O Entity Framework Designer (EF designer) armazena informações de mapeamento em um arquivo. edmx em tempo de design. No momento da compilação, o Entity Designer usa informações em um arquivo. edmx para criar o arquivo. MSL que é necessário pelo Entity Framework em tempo de execução

Os nomes de todos os tipos de modelo conceituais ou de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace. Para obter informações sobre o nome do namespace do modelo conceitual, consulte [especificação CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte [especificação de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

As versões do MSL são diferenciadas por namespaces XML.

| Versão do MSL | Namespace XML                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: schemas-microsoft-com: Windows: Storage: Mapping: CS |
| MSL v2      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| MSL v3      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Elemento alias (MSL)

O elemento **alias** no MSL (Mapping Specification Language) é um filho do elemento Mapping que é usado para definir aliases para namespaces de modelo conceitual e modelo de armazenamento. Os nomes de todos os tipos de modelo conceituais ou de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace. Para obter informações sobre o nome do namespace do modelo conceitual, consulte elemento Schema (CSDL). Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte elemento de esquema (SSDL).

O elemento **alias** não pode ter elementos filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **alias** .

| Nome do atributo | É obrigatório | Valor                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Chave**        | Sim         | O alias para o namespace que é especificado pelo atributo **Value** . |
| **Valor**      | Sim         | O namespace para o qual o valor do elemento de **chave** é um alias.     |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **alias** que define um alias, `c`, para tipos que são definidos no modelo conceitual.

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

## <a name="associationend-element-msl"></a>Elemento AssociationEnd (MSL)

O elemento **AssociationEnd** na MSL (linguagem de especificação de mapeamento) é usado quando as funções de modificação de um tipo de entidade no modelo conceitual são mapeadas para procedimentos armazenados no banco de dados subjacente. Se um procedimento armazenado de modificação usa um parâmetro cujo valor é mantido em uma propriedade de associação, o elemento **AssociationEnd** mapeia o valor da propriedade para o parâmetro. Para obter mais informações, consulte o exemplo a seguir.

Para obter mais informações sobre como mapear funções de modificação de tipos de entidade para procedimentos armazenados, consulte ModificationFunctionMapping Element (MSL) e Walkthrough: Mapeando uma entidade para procedimentos armazenados.

O elemento **AssociationEnd** pode ter os seguintes elementos filho:

-   ScalarProperty

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis ao elemento **AssociationEnd** .

| Nome do atributo     | É obrigatório | Valor                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Sim         | O nome da associação que está sendo mapeada.                                                                                                                                 |
| **From**           | Sim         | O valor do atributo **FromRole** da propriedade de navegação que corresponde à associação que está sendo mapeada. Para obter mais informações, consulte o elemento NavigationProperty (CSDL). |
| **To**             | Sim         | O valor do atributo **ToRole** da propriedade de navegação que corresponde à associação que está sendo mapeada. Para obter mais informações, consulte o elemento NavigationProperty (CSDL).   |

### <a name="example"></a>Exemplo

Considere o seguinte tipo de entidade de modelo conceitual:

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

Considere também o seguinte procedimento armazenado:

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

Para mapear a função Update da entidade `Course` para esse procedimento armazenado, você deve fornecer um valor para o parâmetro **DepartmentID** . O valor de `DepartmentID` não corresponde a uma propriedade no tipo de entidade; Ele está contido em uma associação independente cujo mapeamento é mostrado aqui:

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

O código a seguir mostra o elemento **AssociationEnd** usado para mapear a propriedade **DepartmentID** da Associação **FK @ no__t-3Course @ no__t-4Department** para o procedimento armazenado **UpdateCourse** (para o qual a função Update do O tipo de entidade do **curso** é mapeado):

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

O elemento **AssociationSetMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma associação no modelo conceitual e colunas de tabela no banco de dados subjacente.

As associações no modelo conceitual são tipos cujas propriedades representam as colunas de chave primária e estrangeira no banco de dados subjacente. O elemento **AssociationSetMapping** usa dois elementos EndProperty para definir os mapeamentos entre as propriedades e as colunas do tipo de associação no banco de dados. Você pode posicionar as condições nesses mapeamentos com o elemento Condition. Mapeie as funções INSERT, Update e Delete para associações a procedimentos armazenados no banco de dados com o elemento ModificationFunctionMapping. Defina mapeamentos somente leitura entre associações e colunas de tabela usando uma Entity SQL cadeia de caracteres em um elemento QueryView.

> [!NOTE]
> Se uma restrição referencial for definida para uma associação no modelo conceitual, a associação não precisará ser mapeada com um elemento **AssociationSetMapping** . Se um elemento **AssociationSetMapping** estiver presente para uma associação que tenha uma restrição referencial, os mapeamentos definidos no elemento **AssociationSetMapping** serão ignorados. Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).

O elemento **AssociationSetMapping** pode ter os seguintes elementos filho

-   QueryView (zero ou um)
-   EndProperty (zero ou dois)
-   Condição (zero ou mais)
-   ModificationFunctionMapping (zero ou um)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **AssociationSetMapping** .

| Nome do atributo     | É obrigatório | Valor                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Name**           | Sim         | O nome do conjunto de associação do modelo conceitual que está sendo mapeado.                      |
| **TypeName**       | Não          | O nome qualificado do namespace do tipo de associação de modelo conceitual que está sendo mapeado. |
| **StoreEntitySet** | Não          | O nome da tabela que está sendo mapeada.                                                 |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **AssociationSetMapping** no qual o conjunto de associação **FK @ no__t-2Course @ no__t-3Department** no modelo conceitual é mapeado para a tabela do **curso** no banco de dados. Os mapeamentos entre as propriedades do tipo de associação e as colunas de tabela são especificados nos elementos filho **EndProperty** .

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

Um elemento **ComplexProperty** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma propriedade de tipo complexo em um tipo de entidade de modelo conceitual e colunas de tabela no banco de dados subjacente. Os mapeamentos de coluna de propriedade são especificados em elementos ScalarProperty filho.

O elemento de propriedade **complexType** pode ter os seguintes elementos filho:

-   ScalarProperty (zero ou mais)
-   **ComplexProperty** (zero ou mais)
-   ComplextTypeMapping (zero ou mais)
-   Condição (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ComplexProperty** :

| Nome do atributo | É obrigatório | Valor                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome da propriedade complexa de um tipo de entidade no modelo conceitual que está sendo mapeado. |
| **TypeName**   | Não          | O nome qualificado do namespace do tipo de Propriedade do modelo conceitual.                              |

### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo escolar. O seguinte tipo complexo foi adicionado ao modelo conceitual:

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

As propriedades **LastName** e **FirstName** do tipo de entidade **Person** foram substituídas por uma propriedade complexa, **Name**:

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

O seguinte MSL mostra o elemento **ComplexProperty** usado para mapear a propriedade **Name** para colunas no banco de dados subjacente:

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

O elemento **ComplexTypeMapping** na MSL (linguagem de especificação de mapeamento) é um filho do elemento ResultMapping e define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado no banco de dados subjacente quando o seguinte são verdadeiros:

-   A função Import retorna um tipo complexo conceitual.
-   Os nomes das colunas retornadas pelo procedimento armazenado não correspondem exatamente aos nomes das propriedades no tipo complexo.

Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo complexo baseia-se nos nomes de coluna e propriedade. Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, você deverá usar o elemento **ComplexTypeMapping** para definir o mapeamento. Para obter um exemplo do mapeamento padrão, consulte elemento FunctionImportMapping (MSL).

O elemento **ComplexTypeMapping** pode ter os seguintes elementos filho:

-   ScalarProperty (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ComplexTypeMapping** .

| Nome do atributo | É obrigatório | Valor                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **TypeName**   | Sim         | O nome qualificado do namespace do tipo complexo que está sendo mapeado. |

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

Considere também o seguinte tipo complexo de modelo conceitual:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Para criar uma importação de função que retorne instâncias do tipo complexo anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um elemento **ComplexTypeMapping** :

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

O elemento **Condition** no MSL (Mapping Specification Language) coloca condições em mapeamentos entre o modelo conceitual e o banco de dados subjacente. O mapeamento definido dentro de um nó XML será válido se todas as condições, conforme especificado nos elementos de **condição** filho, forem atendidas. Caso contrário, o mapeamento não será válido. Por exemplo, se um elemento MappingFragment contiver um ou mais elementos filho **Condition** , o mapeamento definido no nó **MappingFragment** só será válido se todas as condições dos elementos de **condição** filho forem atendidas.

Cada condição pode se aplicar a um **nome** (o nome de uma propriedade de entidade modelo conceitual, especificada pelo atributo **Name** ) ou um **ColumnName** (o nome de uma coluna no banco de dados, especificado pelo atributo **ColumnName** ). Quando o atributo **Name** é definido, a condição é verificada em relação a um valor de propriedade de entidade. Quando o atributo **ColumnName** é definido, a condição é verificada em relação a um valor de coluna. Somente um dos atributos **Name** ou **ColumnName** pode ser especificado em um elemento **Condition** .

> [!NOTE]
> Quando o elemento **Condition** é usado em um elemento FunctionImportMapping, somente o atributo **Name** não é aplicável.

O elemento **Condition** pode ser um filho dos seguintes elementos:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

O elemento **Condition** não pode ter nenhum elemento filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis ao elemento **Condition** :

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | Não          | O nome da coluna de tabela cujo valor é usado para avaliar a condição.                                                                                                                                                                                                                   |
| **IsNull**     | Não          | **True** ou **False**. Se o valor for **true** e o valor da coluna for **NULL**ou se o valor for **false** e o valor da coluna não for **NULL**, a condição será true. Caso contrário, a condição será falsa. <br/> Os atributos **IsNull** e **Value** não podem ser usados ao mesmo tempo. |
| **Valor**      | Não          | O valor com o qual o valor da coluna é comparado. Se os valores forem iguais, a condição será verdadeira. Caso contrário, a condição será falsa. <br/> Os atributos **IsNull** e **Value** não podem ser usados ao mesmo tempo.                                                                       |
| **Name**       | Não          | O nome da propriedade de entidade do modelo conceitual cujo valor é usado para avaliar a condição. <br/> Esse atributo não será aplicável se o elemento **Condition** for usado dentro de um elemento FunctionImportMapping.                                                                           |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra os elementos **Condition** como filhos dos elementos **MappingFragment** . Quando **HireDate** não é nulo e **EnrollmentDate** é nulo, os dados são mapeados entre o tipo **SchoolModel. instrutor** e as colunas **PersonID** e **HireDate** da tabela **Person** . Quando **EnrollmentDate** não é nulo e **HireDate** é NULL, os dados são mapeados entre o tipo **SchoolModel. Student** e as colunas **PersonID** e **registro** da tabela **Person** .

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

O elemento **DeleteFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Delete de um tipo de entidade ou associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente. Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte elemento Function (SSDL).

> [!NOTE]
> Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.

### <a name="deletefunction-applied-to-entitytypemapping"></a>DeleteFunction aplicado a EntityTypeMapping

Quando aplicado ao elemento EntityTypeMapping, o elemento **DeleteFunction** mapeia a função Delete de um tipo de entidade no modelo conceitual para um procedimento armazenado.

O elemento **DeleteFunction** pode ter os seguintes elementos filho quando aplicados a um elemento **EntityTypeMapping** :

-   AssociationEnd (zero ou mais)
-   ComplexProperty (zero ou mais)
-   ScarlarProperty (zero ou mais)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **DeleteFunction** quando ele é aplicado a um elemento **EntityTypeMapping** .

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado do namespace do procedimento armazenado para o qual a função Delete está mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

#### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo escolar e mostra o elemento **DeleteFunction** mapeando a função Delete do tipo de entidade **Person** para o procedimento armazenado **DeletePerson** . O procedimento armazenado **DeletePerson** é declarado no modelo de armazenamento.

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

Quando aplicado ao elemento AssociationSetMapping, o elemento **DeleteFunction** mapeia a função Delete de uma associação no modelo conceitual para um procedimento armazenado.

O elemento **DeleteFunction** pode ter os seguintes elementos filho quando aplicado ao elemento **AssociationSetMapping** :

-   EndProperty

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **DeleteFunction** quando ele é aplicado ao elemento **AssociationSetMapping** .

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado do namespace do procedimento armazenado para o qual a função Delete está mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

#### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo escolar e mostra o elemento **DeleteFunction** usado para mapear a função Delete da Associação **CourseInstructor** para o procedimento armazenado **DeleteCourseInstructor** . O procedimento armazenado **DeleteCourseInstructor** é declarado no modelo de armazenamento.

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

O elemento **EndProperty** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma função End ou uma modificação de uma associação de modelo conceitual e o banco de dados subjacente. O mapeamento de coluna de propriedade é especificado em um elemento ScalarProperty filho.

Quando um elemento **EndProperty** é usado para definir o mapeamento para o final de uma associação de modelo conceitual, ele é um filho de um elemento AssociationSetMapping. Quando o elemento **EndProperty** é usado para definir o mapeamento para uma função de modificação de uma associação de modelo conceitual, ele é um filho de um elemento InsertFunction ou um elemento DeleteFunction.

O elemento **EndProperty** pode ter os seguintes elementos filho:

-   ScalarProperty (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis ao elemento **EndProperty** :

| Nome do atributo | É obrigatório | Valor                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Nome           | Sim         | O nome da extremidade de associação que está sendo mapeada. |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **AssociationSetMapping** no qual a associação de **CE @ no__t-2Course @ no__t-3Department** no modelo conceitual é mapeada para a tabela do **curso** no banco de dados. Os mapeamentos entre as propriedades do tipo de associação e as colunas de tabela são especificados nos elementos filho **EndProperty** .

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

O exemplo a seguir mostra o elemento **EndProperty** que mapeia as funções INSERT e Delete de uma associação (**CourseInstructor**) para procedimentos armazenados no banco de dados subjacente. As funções que são mapeadas para são declaradas no modelo de armazenamento.

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

O elemento **EntityContainerMapping** na MSL (linguagem de especificação de mapeamento) mapeia o contêiner de entidade no modelo conceitual para o contêiner de entidade no modelo de armazenamento. O elemento **EntityContainerMapping** é um filho do elemento Mapping.

O elemento **EntityContainerMapping** pode ter os seguintes elementos filho (na ordem listada):

-   EntitySetMapping (zero ou mais)
-   AssociationSetMapping (zero ou mais)
-   FunctionImportMapping (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityContainerMapping** .

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Sim         | O nome do contêiner de entidade do modelo de armazenamento que está sendo mapeado.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Sim         | O nome do contêiner de entidade do modelo conceitual que está sendo mapeado.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Não          | **True** ou **False**. Se **for false**, nenhum modo de exibição de atualização será gerado. Esse atributo deve ser definido como **false** quando você tem um mapeamento somente leitura que seria inválido porque os dados não podem fazer viagens de ida e volta com êxito. <br/> O valor padrão é **true**. |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityContainerMapping** que mapeia o contêiner **SchoolModelEntities** (o contêiner de entidade modelo conceitual) para o contêiner **SchoolModelStoreContainer** (a entidade modelo de armazenamento contêiner):

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

O elemento **EntitySetMapping** no mapear Language (MSL) mapeia todos os tipos em uma entidade de modelo conceitual definida para conjuntos de entidades no modelo de armazenamento. Um conjunto de entidades no modelo conceitual é um contêiner lógico para instâncias de entidades do mesmo tipo (e tipos derivados). Uma entidade definida no modelo de armazenamento representa uma tabela ou exibição no banco de dados subjacente. O conjunto de entidades do modelo conceitual é especificado pelo valor do atributo **Name** do elemento **EntitySetMapping** . A tabela ou exibição mapeada é especificada pelo atributo **StoreEntitySet** em cada elemento MappingFragment filho ou no elemento **EntitySetMapping** em si.

O elemento **EntitySetMapping** pode ter os seguintes elementos filho:

-   EntityTypeMapping (zero ou mais)
-   QueryView (zero ou um)
-   MappingFragment (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntitySetMapping** .

| Nome do atributo           | É obrigatório | Valor                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                 | Sim         | O nome do conjunto de entidades do modelo conceitual que está sendo mapeado.                                                                                                                                                             |
| **TypeName** **1**       | Não          | O nome do tipo de entidade do modelo conceitual que está sendo mapeado.                                                                                                                                                            |
| **StoreEntitySet** **1** | Não          | O nome do conjunto de entidades do modelo de armazenamento que está sendo mapeado para.                                                                                                                                                             |
| **MakeColumnsDistinct**  | Não          | **True** ou **false** dependendo se apenas linhas distintas forem retornadas. <br/> Se esse atributo for definido como **true**, o atributo **GenerateUpdateViews** do elemento EntityContainerMapping deverá ser definido como **false**. |

 

**1** os atributos **TypeName** e **StoreEntitySet** podem ser usados no lugar dos elementos filho EntityTypeMapping e MappingFragment para mapear um único tipo de entidade para uma única tabela.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntitySetMapping** que mapeia três tipos (um tipo base e dois tipos derivados) no conjunto de entidades de **cursos** do modelo conceitual para três tabelas diferentes no banco de dados subjacente. As tabelas são especificadas pelo atributo **StoreEntitySet** em cada elemento **MappingFragment** .

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

O elemento **EntityTypeMapping** na MSL (mapeation Specification Language) define o mapeamento entre um tipo de entidade no modelo conceitual e as tabelas ou exibições no banco de dados subjacente. Para obter informações sobre tipos de entidade de modelo conceitual e tabelas ou exibições de banco de dados subjacentes, consulte elemento EntityType (CSDL) e elemento EntitySet (SSDL). O tipo de entidade modelo conceitual que está sendo mapeado é especificado pelo atributo **TypeName** do elemento **EntityTypeMapping** . A tabela ou exibição que está sendo mapeada é especificada pelo atributo **StoreEntitySet** do elemento MappingFragment filho.

O elemento filho ModificationFunctionMapping pode ser usado para mapear as funções INSERT, Update ou Delete de tipos de entidade para procedimentos armazenados no banco de dados.

O elemento **EntityTypeMapping** pode ter os seguintes elementos filho:

-   MappingFragment (zero ou mais)
-   ModificationFunctionMapping (zero ou um)
-   ScalarProperty
-   Condição

> [!NOTE]
> Os elementos **MappingFragment** e **ModificationFunctionMapping** não podem ser elementos filho do elemento **EntityTypeMapping** ao mesmo tempo.


> [!NOTE]
> Os elementos **ScalarProperty** e **Condition** só podem ser elementos filho do elemento **EntityTypeMapping** quando ele é usado dentro de um elemento FunctionImportMapping.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityTypeMapping** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **TypeName**   | Sim         | O nome qualificado do namespace do tipo de entidade do modelo conceitual que está sendo mapeado. <br/> Se o tipo for abstrato ou um tipo derivado, o valor deverá ser `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento EntitySetMapping com dois elementos filho **EntityTypeMapping** . No primeiro elemento **EntityTypeMapping** , o tipo de entidade **SchoolModel. Person** é mapeado para a tabela **Person** . No segundo elemento **EntityTypeMapping** , a funcionalidade de atualização do tipo **SchoolModel. Person** é mapeada para um procedimento armazenado, **UpdatePerson**, no banco de dados.

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

O exemplo a seguir mostra o mapeamento de uma hierarquia de tipo na qual o tipo de raiz é abstrato. Observe o uso da sintaxe `IsOfType` para os atributos **TypeName** .

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

O elemento **FunctionImportMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado ou uma função no banco de dados subjacente. As importações de função devem ser declaradas no modelo conceitual e os procedimentos armazenados devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte elemento FunctionImport (CSDL) e elemento Function (SSDL).

> [!NOTE]
> Por padrão, se uma importação de função retornar um tipo de entidade de modelo conceitual ou tipo complexo, os nomes das colunas retornadas pelo procedimento armazenado subjacente devem corresponder exatamente aos nomes das propriedades no tipo de modelo conceitual. Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, o mapeamento deverá ser definido em um elemento ResultMapping.

O elemento **FunctionImportMapping** pode ter os seguintes elementos filho:

-   ResultMapping (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis ao elemento **FunctionImportMapping** :

| Nome do atributo         | É obrigatório | Valor                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Sim         | O nome da função de importação no modelo conceitual que está sendo mapeado.           |
| **FunctionName**       | Sim         | O nome qualificado do namespace da função no modelo de armazenamento que está sendo mapeado. |

### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo escolar. Considere a seguinte função no modelo de armazenamento:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Considere também essa importação de função no modelo conceitual:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

O exemplo a seguir mostra um elemento **FunctionImportMapping** usado para mapear a função e a importação de função acima entre si:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>Elemento InsertFunction (MSL)

O elemento **InsertFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Insert de um tipo de entidade ou associação no modelo conceitual para um procedimento armazenado no banco de dados subjacente. Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte elemento Function (SSDL).

> [!NOTE]
> Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.

O elemento **InsertFunction** pode ser filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping ou ao elemento AssociationSetMapping.

### <a name="insertfunction-applied-to-entitytypemapping"></a>InsertFunction aplicado a EntityTypeMapping

Quando aplicado ao elemento EntityTypeMapping, o elemento **InsertFunction** mapeia a função Insert de um tipo de entidade no modelo conceitual para um procedimento armazenado.

O elemento **InsertFunction** pode ter os seguintes elementos filho quando aplicados a um elemento **EntityTypeMapping** :

-   AssociationEnd (zero ou mais)
-   ComplexProperty (zero ou mais)
-   Resultbinding (zero ou um)
-   ScarlarProperty (zero ou mais)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **InsertFunction** quando aplicados a um elemento **EntityTypeMapping** .

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado do namespace do procedimento armazenado para o qual a função Insert é mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

#### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo escolar e mostra o elemento **InsertFunction** usado para mapear a função Insert do tipo de entidade Person para o procedimento armazenado **InsertPerson** . O procedimento armazenado **InsertPerson** é declarado no modelo de armazenamento.

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

Quando aplicado ao elemento AssociationSetMapping, o elemento **InsertFunction** mapeia a função Insert de uma associação no modelo conceitual para um procedimento armazenado.

O elemento **InsertFunction** pode ter os seguintes elementos filho quando aplicado ao elemento **AssociationSetMapping** :

-   EndProperty

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **InsertFunction** quando ele é aplicado ao elemento **AssociationSetMapping** .

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado do namespace do procedimento armazenado para o qual a função Insert é mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

#### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo escolar e mostra o elemento **InsertFunction** usado para mapear a função Insert da Associação **CourseInstructor** para o procedimento armazenado **InsertCourseInstructor** . O procedimento armazenado **InsertCourseInstructor** é declarado no modelo de armazenamento.

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

## <a name="mapping-element-msl"></a>Elemento Mapping (MSL)

O elemento **Mapping** na MSL (Mapping Specification Language) contém informações para mapear objetos que são definidos em um modelo conceitual para um banco de dados (conforme descrito em um modelo de armazenamento). Para obter mais informações, consulte Especificação de CSDL e especificação de SSDL.

O elemento **Mapping** é o elemento raiz para uma especificação de mapeamento. O namespace XML para especificações de mapeamento é https://schemas.microsoft.com/ado/2009/11/mapping/cs.

O elemento Mapping pode ter os seguintes elementos filho (na ordem listada):

-   Alias (zero ou mais)
-   EntityContainerMapping (exatamente um)

Os nomes dos tipos de modelo conceitual e de armazenamento referenciados no MSL devem ser qualificados por seus respectivos nomes de namespace. Para obter informações sobre o nome do namespace do modelo conceitual, consulte elemento Schema (CSDL). Para obter informações sobre o nome do namespace do modelo de armazenamento, consulte elemento de esquema (SSDL). Aliases para namespaces que são usados em MSL podem ser definidos com o elemento alias.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Mapping** .

| Nome do atributo | É obrigatório | Valor                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Espaço**      | Sim         | **C-S**. Este é um valor fixo e não pode ser alterado. |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Mapping** que é baseado em parte do modelo escolar. Para obter mais informações sobre o modelo escolar, consulte início rápido (Entity Framework):

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

## <a name="mappingfragment-element-msl"></a>Elemento MappingFragment (MSL)

O elemento **MappingFragment** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre as propriedades de um tipo de entidade de modelo conceitual e uma tabela ou exibição no banco de dados. Para obter informações sobre tipos de entidade de modelo conceitual e tabelas ou exibições de banco de dados subjacentes, consulte elemento EntityType (CSDL) e elemento EntitySet (SSDL). O **MappingFragment** pode ser um elemento filho do elemento EntityTypeMapping ou do elemento EntitySetMapping.

O elemento **MappingFragment** pode ter os seguintes elementos filho:

-   ComplexType (zero ou mais)
-   ScalarProperty (zero ou mais)
-   Condição (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **MappingFragment** .

| Nome do atributo          | É obrigatório | Valor                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Sim         | O nome da tabela ou exibição que está sendo mapeada.                                                                                                                                                                           |
| **MakeColumnsDistinct** | Não          | **True** ou **false** dependendo se apenas linhas distintas forem retornadas. <br/> Se esse atributo for definido como **true**, o atributo **GenerateUpdateViews** do elemento EntityContainerMapping deverá ser definido como **false**. |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **MappingFragment** como o filho de um elemento **EntityTypeMapping** . Neste exemplo, as propriedades do tipo de **curso** no modelo conceitual são mapeadas para colunas da tabela do **curso** no banco de dados.

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

O exemplo a seguir mostra um elemento **MappingFragment** como o filho de um elemento **EntitySetMapping** . Como no exemplo acima, as propriedades do tipo de **curso** no modelo conceitual são mapeadas para colunas da tabela do **curso** no banco de dados.

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

O elemento **ModificationFunctionMapping** na MSL (linguagem de especificação de mapeamento) mapeia as funções de inserção, atualização e exclusão de um tipo de entidade modelo conceitual para procedimentos armazenados no banco de dados subjacente. O elemento **ModificationFunctionMapping** também pode mapear as funções INSERT e Delete para associações muitos para muitos no modelo conceitual para procedimentos armazenados no banco de dados subjacente. Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte elemento Function (SSDL).

> [!NOTE]
> Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.


> [!NOTE]
> Se as funções de modificação para uma entidade em uma hierarquia de herança forem mapeadas para procedimentos armazenados, as funções de modificação para todos os tipos na hierarquia deverão ser mapeadas para procedimentos armazenados.

O elemento **ModificationFunctionMapping** pode ser filho do elemento EntityTypeMapping ou do elemento AssociationSetMapping.

O elemento **ModificationFunctionMapping** pode ter os seguintes elementos filho:

-   DeleteFunction (zero ou um)
-   InsertFunction (zero ou um)
-   UpdateFunction (zero ou um)

Nenhum atributo é aplicável ao elemento **ModificationFunctionMapping** .

### <a name="example"></a>Exemplo

O exemplo a seguir mostra o mapeamento do conjunto de entidades para a entidade **pessoas** definida no modelo School. Além do mapeamento de coluna para o tipo de entidade **Person** , o mapeamento das funções INSERT, Update e Delete do tipo **Person** é mostrado. As funções que são mapeadas para são declaradas no modelo de armazenamento.

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

O exemplo a seguir mostra o mapeamento do conjunto de associação para o conjunto de associação **CourseInstructor** no modelo School. Além do mapeamento de coluna para a associação **CourseInstructor** , o mapeamento das funções INSERT e Delete da Associação **CourseInstructor** é mostrado. As funções que são mapeadas para são declaradas no modelo de armazenamento.

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

O elemento **QueryView** na MSL (linguagem de especificação de mapeamento) define um mapeamento somente leitura entre um tipo de entidade ou associação no modelo conceitual e uma tabela no banco de dados subjacente. O mapeamento é definido com uma consulta Entity SQL que é avaliada em relação ao modelo de armazenamento e você expressa o conjunto de resultados em termos de uma entidade ou associação no modelo conceitual. Como as exibições de consulta são somente leitura, você não pode usar comandos de atualização padrão para atualizar tipos que são definidos por exibições de consulta. Você pode fazer atualizações para esses tipos usando funções de modificação. Para obter mais informações, confira Como Mapear funções de modificação para procedimentos armazenados.

> [!NOTE]
> No elemento **QueryView** , Entity SQL expressões que contêm **GroupBy**, agregações de grupo ou propriedades de navegação não têm suporte.

 

O elemento **QueryView** pode ser filho do elemento EntitySetMapping ou do elemento AssociationSetMapping. No primeiro caso, a exibição de consulta define um mapeamento somente leitura para uma entidade no modelo conceitual. No último caso, a exibição de consulta define um mapeamento somente leitura para uma associação no modelo conceitual.

> [!NOTE]
> Se o elemento **AssociationSetMapping** for para uma associação com uma restrição referencial, o elemento **AssociationSetMapping** será ignorado. Para obter mais informações, consulte elemento ReferentialConstraint (CSDL).

O elemento **QueryView** não pode ter nenhum elemento filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **QueryView** .

| Nome do atributo | É obrigatório | Valor                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **TypeName**   | Não          | O nome do tipo de modelo conceitual que está sendo mapeado pelo modo de exibição de consulta. |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra o elemento **QueryView** como um filho do elemento **EntitySetMapping** e define um mapeamento de exibição de consulta para o tipo de entidade **Department** no modelo School.

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

Como a consulta retorna apenas um subconjunto dos membros do tipo de **Departamento** no modelo de armazenamento, o tipo de **Departamento** no modelo escolar foi modificado com base nesse mapeamento da seguinte maneira:

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

O exemplo a seguir mostra o elemento **QueryView** como o filho de um elemento **AssociationSetMapping** e define um mapeamento somente leitura para a associação `FK_Course_Department` no modelo School.

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

Você pode definir modos de exibição de consulta para habilitar os seguintes cenários:

-   Defina uma entidade no modelo conceitual que não inclua todas as propriedades da entidade no modelo de armazenamento. Isso inclui propriedades que não têm valores padrão e não oferecem suporte a valores **nulos** .
-   Mapeie colunas computadas no modelo de armazenamento para propriedades de tipos de entidade no modelo conceitual.
-   Defina um mapeamento em que as condições usadas para particionar entidades no modelo conceitual não são baseadas na igualdade. Quando você especifica um mapeamento condicional usando o elemento **Condition** , a condição fornecida deve ser igual ao valor especificado. Para obter mais informações, consulte elemento Condition (MSL).
-   Mapeie a mesma coluna no modelo de armazenamento para vários tipos no modelo conceitual.
-   Mapeie vários tipos para a mesma tabela.
-   Defina associações no modelo conceitual que não se baseiam em chaves estrangeiras no esquema relacional.
-   Use a lógica de negócios personalizada para definir o valor das propriedades no modelo conceitual. Por exemplo, você pode mapear o valor de cadeia de caracteres "T" na fonte de dados para um valor de **true**, um booliano, no modelo conceitual.
-   Defina filtros condicionais para resultados da consulta.
-   Impor menos restrições aos dados no modelo conceitual do que no modelo de armazenamento. Por exemplo, você poderia tornar uma propriedade no modelo conceitual anulável mesmo se a coluna para a qual ela está mapeada não oferecer suporte a valores **nulos**.

As seguintes considerações se aplicam quando você define modos de exibição de consulta para entidades:

-   As exibições de consulta são somente leitura. Você só pode fazer atualizações em entidades usando funções de modificação.
-   Ao definir um tipo de entidade por um modo de exibição de consulta, você também deve definir todas as entidades relacionadas por exibições de consulta.
-   Quando você mapeia uma associação muitos para muitos para uma entidade no modelo de armazenamento que representa uma tabela de vínculo no esquema relacional, você deve definir um elemento **QueryView** no elemento **AssociationSetMapping** para essa tabela de vínculo.
-   As exibições de consulta devem ser definidas para todos os tipos em uma hierarquia de tipo. Você pode fazer isso das seguintes maneiras:
-   -   Com um único elemento **QueryView** que especifica uma única consulta de Entity SQL que retorna uma União de todos os tipos de entidade na hierarquia.
    -   Com um único elemento **QueryView** que especifica uma única consulta Entity SQL que usa o operador case para retornar um tipo de entidade específico na hierarquia com base em uma condição específica.
    -   Com um elemento **QueryView** adicional para um tipo específico na hierarquia. Nesse caso, use o atributo **TypeName** do elemento **QueryView** para especificar o tipo de entidade para cada exibição.
-   Quando um modo de exibição de consulta é definido, você não pode especificar o atributo **StorageSetName** no elemento **EntitySetMapping** .
-   Quando um modo de exibição de consulta é definido, o elemento **EntitySetMapping**também não pode conter mapeamentos de **Propriedade** .

## <a name="resultbinding-element-msl"></a>Elemento resultbinding (MSL)

O elemento **resultbinding** no MSL (mapear Language Specification) mapeia valores de coluna que são retornados por procedimentos armazenados para propriedades de entidade no modelo conceitual quando as funções de modificação de tipo de entidade são mapeadas para procedimentos armazenados no banco de dados subjacente. Por exemplo, quando o valor de uma coluna de identidade é retornado por um procedimento armazenado INSERT, o elemento **resultbinding** mapeia o valor retornado para uma propriedade de tipo de entidade no modelo conceitual.

O elemento **resultbinding** pode ser filho do elemento InsertFunction ou do elemento UpdateFunction.

O elemento **resultbinding** não pode ter nenhum elemento filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que são aplicáveis ao elemento **resultbinding** :

| Nome do atributo | É obrigatório | Valor                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome da propriedade de entidade no modelo conceitual que está sendo mapeado. |
| **ColumnName** | Sim         | O nome da coluna que está sendo mapeada.                                          |

### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo escolar e mostra um elemento **InsertFunction** usado para mapear a função Insert do tipo de entidade **Person** para o procedimento armazenado **InsertPerson** . (O procedimento armazenado **InsertPerson** é mostrado abaixo e é declarado no modelo de armazenamento.) Um elemento **resultbinding** é usado para mapear um valor de coluna que é retornado pelo procedimento armazenado (**NewPersonID**) para uma propriedade de tipo de entidade (**PersonID**).

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

O Transact-SQL a seguir descreve o procedimento armazenado **InsertPerson** :

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

O elemento **ResultMapping** na MSL (linguagem de especificação de mapeamento) define o mapeamento entre uma importação de função no modelo conceitual e um procedimento armazenado no banco de dados subjacente quando o seguinte é verdadeiro:

-   A função Import retorna um tipo de entidade de modelo conceitual ou tipo complexo.
-   Os nomes das colunas retornadas pelo procedimento armazenado não correspondem exatamente aos nomes das propriedades no tipo de entidade ou tipo complexo.

Por padrão, o mapeamento entre as colunas retornadas por um procedimento armazenado e um tipo de entidade ou tipo complexo baseia-se nos nomes de coluna e propriedade. Se os nomes de coluna não corresponderem exatamente aos nomes de propriedade, você deverá usar o elemento **ResultMapping** para definir o mapeamento. Para obter um exemplo do mapeamento padrão, consulte elemento FunctionImportMapping (MSL).

O elemento **ResultMapping** é um elemento filho do elemento FunctionImportMapping.

O elemento **ResultMapping** pode ter os seguintes elementos filho:

-   EntityTypeMapping (zero ou mais)
-   ComplexTypeMapping

Nenhum atributo é aplicável ao elemento **ResultMapping** .

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

Considere também o seguinte tipo de entidade de modelo conceitual:

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

Para criar uma importação de função que retorne instâncias do tipo de entidade anterior, o mapeamento entre as colunas retornadas pelo procedimento armazenado e o tipo de entidade deve ser definido em um elemento **ResultMapping** :

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

O elemento **ScalarProperty** na MSL (linguagem de especificação de mapeamento) mapeia uma propriedade em um tipo de entidade de modelo conceitual, tipo complexo ou associação a uma coluna de tabela ou parâmetro de procedimento armazenado no banco de dados subjacente.

> [!NOTE]
> Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte elemento Function (SSDL).

O elemento **ScalarProperty** pode ser um filho dos seguintes elementos:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Como um filho do elemento **MappingFragment**, **ComplexProperty**ou **EndProperty** , o elemento **ScalarProperty** mapeia uma propriedade no modelo conceitual para uma coluna no banco de dados. Como um filho do elemento **InsertFunction**, **UpdateFunction**ou **DeleteFunction** , o elemento **ScalarProperty** mapeia uma propriedade no modelo conceitual para um parâmetro de procedimento armazenado.

O elemento **ScalarProperty** não pode ter nenhum elemento filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

Os atributos que se aplicam ao elemento **ScalarProperty** diferem de acordo com a função do elemento.

A tabela a seguir descreve os atributos que são aplicáveis quando o elemento **ScalarProperty** é usado para mapear uma propriedade de modelo conceitual para uma coluna no banco de dados:

| Nome do atributo | É obrigatório | Valor                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Sim         | O nome da Propriedade do modelo conceitual que está sendo mapeada. |
| **ColumnName** | Sim         | O nome da coluna de tabela que está sendo mapeada.              |

A tabela a seguir descreve os atributos que são aplicáveis ao elemento **ScalarProperty** quando ele é usado para mapear uma propriedade de modelo conceitual para um parâmetro de procedimento armazenado:

| Nome do atributo    | É obrigatório | Valor                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**          | Sim         | O nome da Propriedade do modelo conceitual que está sendo mapeada.                                                                                 |
| **ParameterName** | Sim         | O nome do parâmetro que está sendo mapeado.                                                                                                 |
| **Versão**       | Não          | **Atual** ou **original** , dependendo se o valor atual ou o valor original da propriedade deve ser usado para verificações de simultaneidade. |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra o elemento **ScalarProperty** usado de duas maneiras:

-   Para mapear as propriedades do tipo de entidade **Person** para as colunas da tabela **Person**.
-   Para mapear as propriedades do tipo de entidade **Person** para os parâmetros do procedimento armazenado **UpdatePerson** . Os procedimentos armazenados são declarados no modelo de armazenamento.

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

O exemplo a seguir mostra o elemento **ScalarProperty** usado para mapear as funções INSERT e Delete de uma associação de modelo conceitual para procedimentos armazenados no banco de dados. Os procedimentos armazenados são declarados no modelo de armazenamento.

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

O elemento **UpdateFunction** na MSL (linguagem de especificação de mapeamento) mapeia a função Update de um tipo de entidade no modelo conceitual para um procedimento armazenado no banco de dados subjacente. Os procedimentos armazenados para os quais as funções de modificação são mapeadas devem ser declarados no modelo de armazenamento. Para obter mais informações, consulte elemento Function (SSDL).

> [!NOTE]
>  Se você não mapear todas as três das operações de inserção, atualização ou exclusão de um tipo de entidade para procedimentos armazenados, as operações não mapeadas falharão se executadas em tempo de execução e uma UpdateException for gerada.

O elemento **UpdateFunction** pode ser filho do elemento ModificationFunctionMapping e aplicado ao elemento EntityTypeMapping.

O elemento **UpdateFunction** pode ter os seguintes elementos filho:

-   AssociationEnd (zero ou mais)
-   ComplexProperty (zero ou mais)
-   Resultbinding (zero ou um)
-   ScarlarProperty (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **UpdateFunction** .

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sim         | O nome qualificado do namespace do procedimento armazenado para o qual a função Update é mapeada. O procedimento armazenado deve ser declarado no modelo de armazenamento. |
| **RowsAffectedParameter** | Não          | O nome do parâmetro de saída que retorna o número de linhas afetadas.                                                                               |

### <a name="example"></a>Exemplo

O exemplo a seguir é baseado no modelo escolar e mostra o elemento **UpdateFunction** usado para mapear a função Update do tipo de entidade **Person** para o procedimento armazenado **UpdatePerson** . O procedimento armazenado **UpdatePerson** é declarado no modelo de armazenamento.

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
