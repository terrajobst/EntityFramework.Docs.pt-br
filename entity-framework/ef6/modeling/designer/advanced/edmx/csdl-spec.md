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
# <a name="csdl-specification"></a>Especificação de CSDL
Linguagem de definição de esquema conceitual (CSDL) é uma linguagem baseada em XML que descreve as entidades, relações e funções que compõem um modelo conceitual de um aplicativo controlado por dados. Esse modelo conceitual pode ser usado pelo Entity Framework ou do WCF Data Services. Os metadados que é descrito com a CSDL é usado pelo Entity Framework para mapear entidades e relações que são definidas em um modelo conceitual para uma fonte de dados. Para obter mais informações, consulte [especificação de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) e [especificação de MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL é a implementação do Entity Framework do modelo de dados de entidade.

Em um aplicativo do Entity Framework, os metadados de modelo conceitual é carregado de um arquivo. CSDL (escrito em CSDL) em uma instância da System.Data.Metadata.Edm.EdmItemCollection em são acessados por meio de métodos no Classe System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework usa metadados do modelo conceitual para converter consultas no modelo conceitual para comandos de específico da fonte de dados.

O EF Designer armazena informações de modelo conceitual em um arquivo. edmx em tempo de design. No momento da compilação, o EF Designer usa as informações em um arquivo. edmx para criar o arquivo. CSDL que é necessário pelo Entity Framework em tempo de execução.

Versões do CSDL são diferenciadas por namespaces XML.

| Versão CSDL | Namespace XML                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| V3 CSDL      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Elemento Association (CSDL)

Uma **associação** elemento define uma relação entre dois tipos de entidade. Uma associação deve especificar os tipos de entidade envolvidos na relação e o número possível de tipos de entidade em cada extremidade da relação, que é conhecida como a multiplicidade. A multiplicidade de uma extremidade de associação pode ter um valor de um (1), zero ou um (entre 0 e 1) ou muitas (\*). Essas informações são especificadas em dois elementos filho de término.

Instâncias do tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras, se elas são expostas em um tipo de entidade.

Em um aplicativo, uma instância de uma associação representa uma associação específica entre instâncias de tipos de entidade. Instâncias de associação são agrupadas logicamente em um conjunto de associações.

Uma **associação** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   End (elementos exatamente 2)
-   ReferentialConstraint (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **associação** elemento.

| Nome do atributo | É obrigatório | Valor                        |
|:---------------|:------------|:-----------------------------|
| **Nome**       | Sim         | O nome da associação. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **associação** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **associação** elemento que define o **CustomerOrders** associação quando as chaves estrangeiras não foram expostas no **cliente** e  **Ordem** tipos de entidade. O **multiplicidade** valores para cada **final** da associação indicam que muitos **pedidos** pode ser associado com um **cliente**, mas somente um **Customer** pode ser associado com um **ordem**. Além disso, o **OnDelete** elemento indica que todos os **pedidos** que estão relacionados a um determinado **cliente** e terem sido carregados para o ObjectContext será excluído Se o **cliente** é excluído.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

A exemplo a seguir mostra uma **associação** elemento que define o **CustomerOrders** associação quando foram expostas em chaves estrangeiras a **cliente** e  **Ordem** tipos de entidade. Com chaves estrangeiras expostas, a relação entre as entidades é gerenciada com um **ReferentialConstraint** elemento. Um elemento AssociationSetMapping correspondente não é necessário para mapear esta associação para a fonte de dados.

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
 

 

## <a name="associationset-element-csdl"></a>Elemento AssociationSet (CSDL)

O **AssociationSet** elemento na linguagem de definição de esquema conceitual (CSDL) é um contêiner lógico para instâncias do mesmo tipo de associação. Um conjunto de associações fornece uma definição para o agrupamento de instâncias de associação para que eles possam ser mapeadas para uma fonte de dados.  

O **AssociationSet** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou mais elementos permitidos)
-   End (exatamente dois elementos necessários)
-   Elementos de anotação (zero ou mais elementos permitidos)

O **associação** atributo especifica o tipo de associação que contém um conjunto de associações. Os conjuntos de entidades que compõem as extremidades de um conjunto de associações são especificados com exatamente dois filhos **final** elementos.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **AssociationSet** elemento.

| Nome do atributo  | É obrigatório | Valor                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**        | Sim         | O nome do conjunto de entidades. O valor da **nome** atributo não pode ser o mesmo que o valor da **associação** atributo.                                 |
| **Associação** | Sim         | O nome totalmente qualificado da associação que definir a associação contém instâncias de. A associação deve estar no mesmo namespace que o conjunto de associações. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **AssociationSet** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityContainer** elemento com dois **AssociationSet** elementos:

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
 

 

## <a name="collectiontype-element-csdl"></a>Elemento CollectionType (CSDL)

O **CollectionType** elemento na linguagem de definição de esquema conceitual (CSDL) Especifica que um parâmetro de função ou função de tipo de retorno é uma coleção. O **CollectionType** elemento pode ser um filho do elemento de parâmetro ou o elemento de ReturnType (função). O tipo de coleção pode ser especificado usando o **tipo** atributo ou um dos seguintes elementos filhos:

-   **CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> Um modelo não validará se o tipo de uma coleção é especificado com ambos os **tipo** atributo e um elemento filho.

 

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **CollectionType** elemento. Observe que o **DefaultValue**, **MaxLength**, **FixedLength**, **precisão**, **escala**,  **Unicode**, e **agrupamento** atributos só são aplicáveis a coleções de **EDMSimpleTypes**.

| Nome do atributo                                                          | É obrigatório | Valor                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**                                                                | Não          | O tipo de coleção.                                                                                                                                                                                                      |
| **Permite valor nulo**                                                            | Não          | **Verdadeiro** (o valor padrão) ou **falso** dependendo se a propriedade pode ter um valor nulo. <br/> [!NOTE]                                                                                                                 |
| > V1 CSDL, uma propriedade de tipo complexo deve ter `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | Não          | O valor padrão da propriedade.                                                                                                                                                                                               |
| **MaxLength**                                                           | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                        |
| **FixedLength**                                                         | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                           |
| **Precisão**                                                           | Não          | A precisão do valor da propriedade.                                                                                                                                                                                             |
| **Ajustar Escala**                                                               | Não          | A escala do valor da propriedade.                                                                                                                                                                                                 |
| **SRID**                                                                | Não          | Identificador de referência espacial do sistema. Válido somente para as propriedades de tipos espaciais.   Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                                |
| **Agrupamento**                                                           | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.                                                                                                                                                    |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **CollectionType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função definida pelo modelo que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de **pessoa** tipos de entidade (conforme especificado com o **ElementType** atributo).

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
 

O exemplo a seguir mostra uma função definida modelo que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de linhas (conforme especificado na **RowType** elemento).

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
 

O exemplo a seguir mostra uma função definida modelo que usa o **CollectionType** elemento para especificar que a função aceita como um parâmetro de uma coleção de **departamento** tipos de entidade.

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
 

 

## <a name="complextype-element-csdl"></a>Elemento ComplexType (CSDL)

Um **ComplexType** elemento define uma estrutura de dados composta **EdmSimpleType** propriedades ou outros tipos complexos.  Um tipo complexo pode ser uma propriedade de um tipo de entidade ou outro tipo complexo. Um tipo complexo é semelhante a um tipo de entidade em que um tipo complexo define os dados. No entanto, há algumas diferenças entre tipos complexos e tipos de entidade:

-   Tipos complexos não têm identidades (ou chaves) e, portanto, não podem existir independente. Tipos complexos podem existir somente como propriedades de tipos de entidade ou outros tipos complexos.
-   Tipos complexos não podem participar de associações. Nem o final de uma associação pode ser um tipo complexo e, portanto, as propriedades de navegação não podem ser definidas para tipos complexos.
-   Uma propriedade de tipo complexo não pode ter um valor nulo, embora as propriedades escalares de um tipo complexo podem cada ser definidas como null.

Um **ComplexType** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Propriedade (zero ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

A tabela a seguir descreve os atributos que podem ser aplicados para o **ComplexType** elemento.

| Nome do atributo                                                                                                 | É obrigatório | Valor                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nome                                                                                                           | Sim         | O nome do tipo complexo. O nome de um tipo complexo não pode ser igual ao nome de outro tipo complexo, tipo de entidade ou associação que está dentro do escopo do modelo. |
| BaseType                                                                                                       | Não          | O nome de outro tipo complexo que é o tipo base do tipo complexo que está sendo definido. <br/> [!NOTE]                                                                     |
| > Se esse atributo não é aplicável na CSDL v1. Não há suporte para a herança para tipos complexos nessa versão. |             |                                                                                                                                                                                     |
| Abstrato                                                                                                       | Não          | **Verdadeiro** ou **falso** (o valor padrão), dependendo se o tipo complexo é um tipo abstrato. <br/> [!NOTE]                                                                  |
| > Se esse atributo não é aplicável na CSDL v1. Tipos complexos em que a versão não podem ser de tipos abstratos.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ComplexType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um tipo complexo, **endereço**, com o **EdmSimpleType** as propriedades **StreetAddress**, **Cidade**,  **StateOrProvince**, **país**, e **PostalCode**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Para definir o tipo complexo **endereço** (acima) como uma propriedade de um tipo de entidade, você deve declarar o tipo de propriedade na definição de tipo de entidade. A exemplo a seguir mostra a **endereço** propriedade como um tipo complexo em um tipo de entidade (**Publisher**):

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
 

 

## <a name="definingexpression-element-csdl"></a>Elemento DefiningExpression (CSDL)

O **DefiningExpression** elemento na linguagem de definição de esquema conceitual (CSDL) contém uma expressão de Entity SQL que define uma função no modelo conceitual.  

> [!NOTE]
> Para fins de validação, um **DefiningExpression** elemento pode conter conteúdo arbitrário. No entanto, Entity Framework irá acionar uma exceção em tempo de execução se um **DefiningExpression** elemento não contém Entity SQL válido.

 

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **DefiningExpression** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

O exemplo a seguir usa uma **DefiningExpression** elemento para definir uma função que retorna o número de anos desde um livro foi publicado. O conteúdo a **DefiningExpression** elemento é escrito em Entity SQL.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Elemento dependente (CSDL)

O **dependentes** elemento na linguagem de definição de esquema conceitual (CSDL) é um elemento filho ao elemento ReferentialConstraint e define o final dependente de uma restrição referencial. Um **ReferentialConstraint** elemento define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional. Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados podem fazer referência a chave primária de outra tabela, uma propriedade (ou propriedades) de um tipo de entidade podem fazer referência à chave de entidade de outro tipo de entidade. O tipo de entidade que é referenciado é chamado de *final principal* da restrição. O tipo de entidade que faz referência a extremidade de entidade é chamado de *final dependente* da restrição. **PropertyRef** elementos são usados para especificar quais teclas a extremidade de entidade de referência.

O **dependentes** elemento pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **dependentes** elemento.

| Nome do atributo | É obrigatório | Valor                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Função**       | Sim         | O nome do tipo de entidade na extremidade dependente da associação. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **dependentes** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **ReferentialConstraint** elemento que está sendo usado como parte da definição da **PublishedBy** associação. O **PublisherId** propriedade da **livro** tipo de entidade constitui o final dependente da restrição referencial.

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
 

 

## <a name="documentation-element-csdl"></a>Elemento de documentação (CSDL)

O **documentação** elemento na linguagem de definição de esquema conceitual (CSDL) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai. Em um arquivo. edmx, quando o **documentação** elemento é um filho de um elemento que aparece como um objeto na superfície de design do Designer de EF (como uma entidade, associação ou propriedade), o conteúdo do **documentação**  elemento será exibido no Visual Studio **propriedades** janela para o objeto.

O **documentação** elemento pode ter os seguintes elementos filho (na ordem listada):

-   **Resumo**: uma breve descrição do elemento pai. (zero ou um elemento)
-   **LongDescription**: uma descrição abrangente do elemento pai. (zero ou um elemento)
-   Elementos de anotação. (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **documentação** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra a **documentação** como um elemento filho de um elemento EntityType. Se o trecho a seguir foram no CSDL conteúdo de um. edmx arquivo, o conteúdo do **resumo** e **LongDescription** elementos apareceria no Visual Studio **propriedades** janela quando você clica no `Customer` tipo de entidade.

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
 

 

## <a name="end-element-csdl"></a>Elemento end (CSDL)

O **final** elemento na linguagem de definição de esquema conceitual (CSDL) pode ser um filho do elemento de associação ou o elemento AssociationSet. Em cada caso, a função do **final** elemento é diferente e os atributos aplicáveis são diferentes.

### <a name="end-element-as-a-child-of-the-association-element"></a>Elemento final como um filho do elemento de associação

Uma **final** elemento (como um filho de **associação** elemento) identifica o tipo de entidade em uma extremidade de uma associação e o número de instâncias de tipo de entidade que podem existir na fim de uma associação. Termina de associação são definidas como parte de uma associação; uma associação deve ter exatamente duas termina de associação. Instâncias do tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras se são expostos em um tipo de entidade.  

Uma **final** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   OnDelete (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **final** elemento quando ele for o filho de um **associação** elemento.

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**         | Sim         | O nome do tipo de entidade em uma extremidade da associação.                                                                                                                                                                                                                                                                                                                                                         |
| **Função**         | Não          | Um nome para o final da associação. Se nenhum nome for fornecido, será usado o nome do tipo de entidade no final da associação.                                                                                                                                                                                                                                                                                           |
| **Multiplicidade** | Sim         | **1**, **entre 0 e 1**, ou **\*** dependendo do número de instâncias do tipo de entidade que podem estar no final da associação. <br/> **1** indica essa instância de tipo exatamente uma entidade existe no final da associação. <br/> **entre 0 e 1** indica que zero ou uma instância de tipo de entidade existe no final da associação. <br/> **\*** indica que zero, um ou mais instâncias de tipo de entidade existem no final da associação. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **final** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

#### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **associação** elemento que define o **CustomerOrders** associação. O **multiplicidade** valores para cada **final** da associação indicam que muitos **pedidos** pode ser associado com um **cliente**, mas somente um **Customer** pode ser associado com um **ordem**. Além disso, o **OnDelete** elemento indica que todos os **pedidos** que estão relacionados a um determinado **cliente** e que tiverem sido carregados no ObjectContext será excluído, se o **cliente** é excluído.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Elemento final como um filho do elemento AssociationSet

O **final** elemento Especifica uma extremidade de um conjunto de associações. O **AssociationSet** elemento deve conter dois **final** elementos. As informações contidas em um **final** elemento é usado em uma associação definida como uma fonte de dados de mapeamento.

Uma **final** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

> [!NOTE]
> Elementos de anotação devem aparecer após todos os outros elementos filho. Elementos de anotação só são permitidos no CSDL v2 e posterior.

 

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **final** elemento quando ele for o filho de um **AssociationSet** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Sim         | O nome da **EntitySet** elemento que define uma extremidade do pai **AssociationSet** elemento. O **EntitySet** elemento deve ser definido no mesmo contêiner de entidade como o pai **AssociationSet** elemento. |
| **Função**       | Não          | O nome da associação de conjunto final. Se o **função** atributo não for usado, o nome da extremidade da associação de conjunto será o nome do conjunto de entidades.                                                                   |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **final** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

#### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityContainer** elemento com dois **AssociationSet** elementos, cada um com dois **final** elementos:

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
 

 

## <a name="entitycontainer-element-csdl"></a>Elemento EntityContainer (CSDL)

O **EntityContainer** elemento na linguagem de definição de esquema conceitual (CSDL) é um contêiner lógico para conjuntos de entidades, conjuntos de associações e importações de função. Um contêiner de entidade do modelo conceitual mapeia para um contêiner de entidade do modelo de armazenamento por meio do elemento EntityContainerMapping. Um contêiner de entidade do modelo de armazenamento descreve a estrutura do banco de dados: tabelas de descrevem conjuntos de entidades, conjuntos de associações descrevem restrições de chave estrangeira e importações de função descrevem os procedimentos armazenados em um banco de dados.

Uma **EntityContainer** elemento pode ter zero ou mais elementos de documentação. Se um **documentação** elemento estiver presente, ela deve preceder todos **EntitySet**, **AssociationSet**, e **FunctionImport** elementos.

Uma **EntityContainer** elemento pode ter zero ou mais dos seguintes elementos filho (na ordem listada):

-   EntitySet
-   AssociationSet
-   FunctionImport
-   Elementos de anotação

Você pode estender uma **EntityContainer** elemento para incluir o conteúdo de outro **EntityContainer** que esteja dentro do mesmo namespace. Para incluir o conteúdo de outro **EntityContainer**, em que a referência **EntityContainer** elemento, defina o valor da **estende** de atributo para o nome da  **EntityContainer** elemento que você deseja incluir. Todos os elementos filho dos incluídos **EntityContainer** elemento será tratado como elementos filho de referência **EntityContainer** elemento.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **Using** elemento.

| Nome do atributo | É obrigatório | Valor                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Nome**       | Sim         | O nome do contêiner de entidade.                               |
| **Estende**    | Não          | O nome do outro contêiner de entidade dentro do mesmo namespace. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntityContainer** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityContainer** elemento que define três conjuntos de entidade e duas associações.

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
 

 

## <a name="entityset-element-csdl"></a>Elemento EntitySet (CSDL)

O **EntitySet** elemento na linguagem de definição de esquema conceitual é um contêiner lógico para instâncias de um tipo de entidade e instâncias de qualquer tipo que é derivado do tipo de objeto. A relação entre um tipo de entidade e um conjunto de entidades é análoga a relação entre uma linha e uma tabela no banco de dados relacional. Como uma linha, um tipo de entidade define um conjunto de dados relacionados e, como uma tabela, um conjunto de entidades contém instâncias dessa definição. Um conjunto de entidades fornece uma compilação para o agrupamento de instâncias do tipo de entidade para que eles possam ser mapeadas para as estruturas de dados relacionados em uma fonte de dados.  

Mais de um conjunto de entidades para um tipo de entidade específico podem ser definidas.

> [!NOTE]
> O EF Designer não oferece suporte a modelos conceituais que contêm vários conjuntos de entidades por tipo.

 

O **EntitySet** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Elemento Documentation (zero ou mais elementos permitidos)
-   Elementos de anotação (zero ou mais elementos permitidos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EntitySet** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome do conjunto de entidades.                                                              |
| **EntityType** | Sim         | O nome totalmente qualificado do tipo de entidade para a qual o conjunto de entidades contém instâncias. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntitySet** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityContainer** elemento com três **EntitySet** elementos:

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
 

É possível definir vários conjuntos de entidades por tipo (MEST). O exemplo a seguir define um contêiner de entidade com dois conjuntos de entidades para o **livro** tipo de entidade:

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
 

 

## <a name="entitytype-element-csdl"></a>Elemento EntityType (CSDL)

O **EntityType** elemento representa a estrutura de um conceito de nível superior, como um cliente ou pedido, em um modelo conceitual. Um tipo de entidade é um modelo para instâncias de tipos de entidade em um aplicativo. Cada modelo contém as informações a seguir:

-   Um nome exclusivo. (Obrigatório.)
-   Uma chave de entidade que é definida por uma ou mais propriedades. (Obrigatório.)
-   Propriedades que contêm dados. (Opcional).
-   Propriedades de navegação que permitem a navegação de uma das extremidades de uma associação para a outra extremidade. (Opcional).

Em um aplicativo, uma instância de um tipo de entidade representa um objeto específico (como um cliente ou uma ordem específica.) Cada instância de um tipo de entidade deve ter uma chave de entidade exclusivo dentro de um conjunto de entidades.

Duas instâncias do tipo de entidade são consideradas iguais somente se são do mesmo tipo e os valores das chaves de entidade são os mesmos.

Uma **EntityType** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Chave (zero ou um elemento)
-   Propriedade (zero ou mais elementos)
-   NavigationProperty (zero ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityType** elemento.

| Nome do atributo                                                                                                                                  | É obrigatório | Valor                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Nome**                                                                                                                                        | Sim         | O nome do tipo de entidade.                                                                     |
| **BaseType**                                                                                                                                    | Não          | O nome de outro tipo de entidade que é o tipo base do tipo de entidade que está sendo definido.  |
| **Abstrato**                                                                                                                                    | Não          | **Verdadeiro** ou **falso**, dependendo se o tipo de entidade é um tipo abstrato.                 |
| **OpenType**                                                                                                                                    | Não          | **Verdadeiro** ou **falso** dependendo se o tipo de entidade é um tipo de entidade aberto. <br/> [!NOTE] |
| > O **OpenType** atributo só é aplicável aos tipos de entidade que são definidos em modelos conceituais que são usados com o ADO.NET Data Services. |             |                                                                                                  |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntityType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityType** elemento com três **propriedade** elementos e duas **NavigationProperty** elementos:

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
 

 

## <a name="enumtype-element-csdl"></a>Elemento EnumType (CSDL)

O **EnumType** elemento representa um tipo enumerado.

Uma **EnumType** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Membro (zero ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EnumType** elemento.

| Nome do atributo     | É obrigatório | Valor                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**           | Sim         | O nome do tipo de entidade.                                                                                                                                                                  |
| **IsFlags**        | Não          | **Verdadeiro** ou **falso**, dependendo se o tipo de enumeração pode ser usado como um conjunto de sinalizadores. O valor padrão é **falsos.**.                                                                     |
| **UnderlyingType** | Não          | **Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** ou **Edm.SByte** definindo o intervalo de valores do tipo.   O tipo dos elementos de enumeração subjacente padrão é **Edm.Int32.**. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EnumType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EnumType** elemento com três **membro** elementos:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Elemento de função (CSDL)

O **função** elemento na linguagem de definição de esquema conceitual (CSDL) é usado para definir ou declare as funções no modelo conceitual. Uma função é definida usando um elemento DefiningExpression.  

Um **função** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Parâmetro (zero ou mais elementos)
-   DefiningExpression (zero ou um elemento)
-   ReturnType (função) (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

Retornar de um tipo para uma função deve ser especificado com qualquer um de **ReturnType** elemento (função) ou o **ReturnType** atributo (veja abaixo), mas não ambos. Os tipos de retorno possíveis são qualquer EdmSimpleType, entidade tipo, tipo complexo, tipo de linha, ou tipo ref (ou uma coleção de um desses tipos).

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **função** elemento.

| Nome do atributo | É obrigatório | Valor                              |
|:---------------|:------------|:-----------------------------------|
| **Nome**       | Sim         | O nome da função.          |
| **ReturnType** | Não          | O tipo retornado pela função. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **função** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir usa uma **função** elemento para definir uma função que retorna o número de anos, desde que um instrutor foi contratado.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>Elemento FunctionImport (CSDL)

O **FunctionImport** elemento na linguagem de definição de esquema conceitual (CSDL) representa uma função que está definido na fonte de dados, mas está disponível para objetos por meio do modelo conceitual. Por exemplo, um elemento de função no modelo de armazenamento pode ser usado para representar um procedimento armazenado em um banco de dados. Um **FunctionImport** elemento no modelo conceitual representa a função correspondente em um aplicativo do Entity Framework e é mapeado para a função de modelo de armazenamento usando o elemento FunctionImportMapping. Quando a função é chamada no aplicativo, o procedimento armazenado correspondente é executado no banco de dados.

O **FunctionImport** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou mais elementos permitidos)
-   Parâmetro (zero ou mais elementos permitidos)
-   Elementos de anotação (zero ou mais elementos permitidos)
-   ReturnType (FunctionImport) (zero ou mais elementos permitidos)

Uma **parâmetro** elemento deve ser definido para cada parâmetro que a função aceita.

Retornar de um tipo para uma função deve ser especificado com qualquer um de **ReturnType** elemento (FunctionImport) ou o **ReturnType** atributo (veja abaixo), mas não ambos. O valor do tipo de retorno deve ser uma coleção de EdmSimpleType, EntityType ou ComplexType.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **FunctionImport** elemento.

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**         | Sim         | O nome da função importada.                                                                                                                                                                    |
| **ReturnType**   | Não          | O tipo que a função retorna. Não use esse atributo se a função não retorna um valor. Caso contrário, o valor deve ser uma coleção de EDMSimpleType, EntityType ou ComplexType.        |
| **EntitySet**    | Não          | Se a função retorna uma coleção de entidade tipos, o valor da **EntitySet** devem ser o conjunto de entidades ao qual pertence a coleção. Caso contrário, o **EntitySet** atributo não deve ser usado. |
| **IsComposable** | Não          | Se o valor for definido como true, a função é passível de composição (Table-valued Function) e pode ser usada em uma consulta LINQ.  O padrão é **false**.                                                           |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **FunctionImport** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **FunctionImport** elemento que aceita um parâmetro e retorna uma coleção de tipos de entidade:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Elemento key (CSDL)

O **chave** elemento é um elemento filho do elemento EntityType e define um *chave de entidade* (uma propriedade ou um conjunto de propriedades que determinam a identidade de um tipo de entidade). As propriedades que compõem uma chave de entidade são escolhidas em tempo de design. Os valores das propriedades de chave de entidade devem identificar exclusivamente uma instância do tipo de entidade dentro de uma entidade definida em tempo de execução. As propriedades que compõem uma chave de entidade devem ser escolhidas para garantir a exclusividade de instâncias em um conjunto de entidades. O **chave** elemento define uma chave de entidade, fazendo referência a um ou mais das propriedades de um tipo de entidade.

O **chave** elemento pode ter os seguintes elementos filho:

-   PropertyRef (um ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **chave** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

O exemplo a seguir define um tipo de entidade nomeado **livro**. A chave de entidade é definida fazendo referência a **ISBN** propriedade do tipo de entidade.

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
 

O **ISBN** propriedade é uma boa opção para a chave de entidade porque um número padrão de livro (ISBN) internacional identifica um livro.

O exemplo a seguir mostra um tipo de entidade (**autor**) que tem uma chave de entidade que consiste em duas propriedades, **nome** e **endereço**.

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
 

Usando o **nome** e **endereço** da entidade para a chave é uma opção razoável, porque dois autores de mesmo nome serão improvável de viver no mesmo endereço. No entanto, esta opção para uma chave de entidade não garante absolutamente chaves exclusivas de entidade em um conjunto de entidades. Adicionando uma propriedade, tal como **AuthorId**, que pode ser usado para identificar exclusivamente um autor seria recomendado nesse caso.

 

## <a name="member-element-csdl"></a>Elemento Member (CSDL)

O **membro** elemento é um elemento filho do elemento EnumType e define um membro do tipo enumerado.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **FunctionImport** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome do membro.                                                                                                                                                                  |
| **Value**      | Não          | O valor do membro. Por padrão, o primeiro membro tem o valor 0 e o valor de cada enumerador seguinte é incrementado em 1. Podem existir vários membros com os mesmos valores. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **FunctionImport** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EnumType** elemento com três **membro** elementos:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>Elemento NavigationProperty (CSDL)

Um **NavigationProperty** elemento define uma propriedade de navegação, que fornece uma referência a outra extremidade de uma associação. Ao contrário das propriedades definidas com o elemento de propriedade, as propriedades de navegação não definem a forma e características dos dados. Eles fornecem uma maneira de navegar de uma associação entre dois tipos de entidade.

Observe que as propriedades de navegação são opcionais em ambos os tipos de entidade termina de uma associação. Se você definir uma propriedade de navegação em um tipo de entidade no final de uma associação, você não precisa definir uma propriedade de navegação no tipo de entidade no outro extremo de associação.

O tipo de dados retornado por uma propriedade de navegação é determinado pela multiplicidade de extremidade remoto da associação. Por exemplo, suponha que uma propriedade de navegação **OrdersNavProp**, existe em um **Customer** tipo de entidade e navega de uma associação um-para-muitos entre **cliente** e  **Ordem**. Como o fim remoto da associação para a propriedade de navegação tem multiplicidade de muitos (\*), seu tipo de dados é uma coleção (de **ordem**). Da mesma forma, se uma propriedade de navegação **CustomerNavProp**, existe na **ordem** tipo de entidade, seu tipo de dados seria **cliente** , pois a multiplicidade de extremidade remoto é um (1).

Um **NavigationProperty** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **NavigationProperty** elemento.

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**         | Sim         | O nome da propriedade de navegação.                                                                                                                                                                                                             |
| **Relação** | Sim         | O nome de uma associação que está dentro do escopo do modelo.                                                                                                                                                                                |
| **ToRole**       | Sim         | O final da associação no qual a navegação é encerrada. O valor da **ToRole** atributo deve ser igual ao valor de um dos **função** atributos definidos em uma das extremidades da associação (definidas no elemento AssociationEnd).       |
| **FromRole**     | Sim         | O final da associação da qual iniciar a navegação. O valor da **FromRole** atributo deve ser igual ao valor de um dos **função** atributos definidos em uma das extremidades da associação (definidas no elemento AssociationEnd). |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **NavigationProperty** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir define um tipo de entidade (**livro**) com duas propriedades de navegação (**PublishedBy** e **WrittenBy**):

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
 

 

## <a name="ondelete-element-csdl"></a>Elemento OnDelete (CSDL)

O **OnDelete** elemento na linguagem de definição de esquema conceitual (CSDL) define o comportamento que está conectado com uma associação. Se o **ação** atributo é definido como **Cascade** relacionados em uma extremidade de uma associação, os tipos de entidade na outra extremidade da associação são excluídos quando o tipo de entidade no primeiro final é excluído. Se a associação entre dois tipos de entidade é uma relação de chave primária de chave para o primário, um objeto dependente carregado é excluído quando o objeto de entidade na outra extremidade da associação é excluído, independentemente do **OnDelete** especificação.  

> [!NOTE]
> O **OnDelete** elemento só afeta o comportamento de tempo de execução de um aplicativo; ele não afeta o comportamento da fonte de dados. O comportamento definido na fonte de dados deve ser o mesmo que o comportamento definido no aplicativo.

 

Uma **OnDelete** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **OnDelete** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ação**     | Sim         | **CASCADE** ou **None**. Se **Cascade**, tipos de entidade dependente serão excluídos quando o tipo de entidade de segurança de entidade é excluído. Se **None**, tipos de entidade dependente não serão excluídos quando o tipo de entidade de segurança de entidade é excluído. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **associação** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **associação** elemento que define o **CustomerOrders** associação. O **OnDelete** elemento indica que todos os **pedidos** que estão relacionados a um determinado **cliente** e terem sido carregados para o ObjectContext serão excluídos quando o  **Cliente** é excluído.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Elemento Parameter (CSDL)

O **parâmetro** elemento na linguagem de definição de esquema conceitual (CSDL) pode ser um filho do elemento FunctionImport ou o elemento de função.

### <a name="functionimport-element-application"></a>Elemento FunctionImport aplicativo

Um **parâmetro** elemento (como um filho de **FunctionImport** elemento) é usado para definir parâmetros de entrada e saídos para importações de função que são declarados no CSDL.

O **parâmetro** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou mais elementos permitidos)
-   Elementos de anotação (zero ou mais elementos permitidos)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **parâmetro** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome do parâmetro.                                                                                                                                                                                                      |
| **Tipo**       | Sim         | O tipo de parâmetro. O valor deve ser um **EDMSimpleType** ou um tipo complexo que está dentro do escopo do modelo.                                                                                                             |
| **Modo**       | Não          | **Na**, **Out**, ou **InOut** dependendo se o parâmetro for uma entrada, saída ou parâmetro de entrada/saída.                                                                                                                |
| **MaxLength**  | Não          | O comprimento máximo permitido do parâmetro.                                                                                                                                                                                    |
| **Precisão**  | Não          | A precisão do parâmetro.                                                                                                                                                                                                 |
| **Ajustar Escala**      | Não          | A escala do parâmetro.                                                                                                                                                                                                     |
| **SRID**       | Não          | Identificador de referência espacial do sistema. Válido somente para parâmetros de tipos espaciais. Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **parâmetro** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

#### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **FunctionImport** elemento com um **parâmetro** elemento filho. A função aceita um parâmetro de entrada e retorna uma coleção de tipos de entidade.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Aplicativo de elemento de função

Um **parâmetro** elemento (como um filho de **função** elemento) define os parâmetros para as funções que são definidas ou declarados em um modelo conceitual.

O **parâmetro** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou mais elementos)
-   CollectionType (zero ou mais elementos)
-   ReferenceType (zero ou mais elementos)
-   RowType (zero ou mais elementos)

> [!NOTE]
> Somente um dos **CollectionType**, **ReferenceType**, ou **RowType** elementos podem ser um elemento filho de um **propriedade** elemento.

 

-   Elementos de anotação (zero ou mais elementos permitidos)

> [!NOTE]
> Elementos de anotação devem aparecer após todos os outros elementos filho. Elementos de anotação só são permitidos no CSDL v2 e posterior.

 

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **parâmetro** elemento.

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**         | Sim         | O nome do parâmetro.                                                                                                                                                                                                      |
| **Tipo**         | Não          | O tipo de parâmetro. Um parâmetro pode ser qualquer um dos seguintes tipos de (ou coleções desses tipos): <br/> **EdmSimpleType** <br/> tipo de entidade <br/> tipo complexo <br/> tipo de linha <br/> tipo de referência                             |
| **Permite valor nulo**     | Não          | **Verdadeiro** (o valor padrão) ou **falsos** dependendo se a propriedade pode ter um **nulo** valor.                                                                                                                          |
| **DefaultValue** | Não          | O valor padrão da propriedade.                                                                                                                                                                                              |
| **MaxLength**    | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                       |
| **FixedLength**  | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                          |
| **Precisão**    | Não          | A precisão do valor da propriedade.                                                                                                                                                                                            |
| **Ajustar Escala**        | Não          | A escala do valor da propriedade.                                                                                                                                                                                                |
| **SRID**         | Não          | Identificador de referência espacial do sistema. Válido somente para as propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                               |
| **Agrupamento**    | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.                                                                                                                                                   |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **parâmetro** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

#### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **função** que usa um elemento **parâmetro** elemento filho para definir um parâmetro de função.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Elemento de entidade de segurança (CSDL)

O **Principal** na linguagem de definição de esquema conceitual (CSDL) é um elemento filho ao elemento ReferentialConstraint que define o final principal de uma restrição referencial. Um **ReferentialConstraint** elemento define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional. Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados podem fazer referência a chave primária de outra tabela, uma propriedade (ou propriedades) de um tipo de entidade podem fazer referência à chave de entidade de outro tipo de entidade. O tipo de entidade que é referenciado é chamado de *final principal* da restrição. O tipo de entidade que faz referência a extremidade de entidade é chamado de *final dependente* da restrição. **PropertyRef** elementos são usados para especificar quais teclas são referenciadas pelo final dependente.

O **Principal** elemento pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **Principal** elemento.

| Nome do atributo | É obrigatório | Valor                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Função**       | Sim         | O nome do tipo de entidade no final da associação de entidade de segurança. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **Principal** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **ReferentialConstraint** que faz parte da definição de elemento de **PublishedBy** associação. O **Id** propriedade da **Publisher** tipo de entidade constitui o final principal de restrição referencial.

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
 

 

## <a name="property-element-csdl"></a>Elemento de propriedade (CSDL)

O **propriedade** elemento na linguagem de definição de esquema conceitual (CSDL) pode ser um filho do elemento EntityType, ComplexType elemento ou o elemento RowType.

### <a name="entitytype-and-complextype-element-applications"></a>EntityType e aplicativos de elemento ComplexType

**Propriedade** elementos (como filhos **EntityType** ou **ComplexType** elementos) definem a forma e características dos dados que contém uma instância do tipo de entidade ou a instância de tipo complexo . Propriedades em um modelo conceitual são análogas às propriedades que são definidas em uma classe. Da mesma forma que as propriedades em uma classe definem a forma da classe e transportam informações sobre objetos, as propriedades em um modelo conceitual definem a forma de um tipo de entidade e transportam informações sobre as instâncias dos tipos de entidade.

O **propriedade** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Elemento Documentation (zero ou mais elementos permitidos)
-   Elementos de anotação (zero ou mais elementos permitidos)

Facetas podem ser aplicadas a um **propriedade** elemento: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **precisão**, **escala**, **Unicode**, **agrupamento**,  **ConcurrencyMode**. As facetas são atributos XML que fornecem informações sobre como os valores de propriedade são armazenados no armazenamento de dados.

> [!NOTE]
> Facetas só podem ser aplicadas às propriedades do tipo **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **propriedade** elemento.

| Nome do atributo                                                         | É obrigatório | Valor                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**                                                               | Sim         | O nome da propriedade.                                                                                                                                                                                                       |
| **Tipo**                                                               | Sim         | O tipo do valor da propriedade. O tipo de valor de propriedade deve ser um **EDMSimpleType** ou um tipo complexo (indicado por um nome totalmente qualificado) que está dentro do escopo do modelo.                                                 |
| **Permite valor nulo**                                                           | Não          | **Verdadeiro** (o valor padrão) ou <strong>falso</strong> dependendo se a propriedade pode ter um valor nulo. <br/> [!NOTE]                                                                                                   |
| > O v1 CSDL uma propriedade de tipo complexo deve ter `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | Não          | O valor padrão da propriedade.                                                                                                                                                                                              |
| **MaxLength**                                                          | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                       |
| **FixedLength**                                                        | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                          |
| **Precisão**                                                          | Não          | A precisão do valor da propriedade.                                                                                                                                                                                            |
| **Ajustar Escala**                                                              | Não          | A escala do valor da propriedade.                                                                                                                                                                                                |
| **SRID**                                                               | Não          | Identificador de referência espacial do sistema. Válido somente para as propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                               |
| **Agrupamento**                                                          | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | Não          | **None** (o valor padrão) ou **fixo**. Se o valor for definido como **fixo**, o valor da propriedade será usado na verificação de simultaneidade otimista.                                                                                  |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **propriedade** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

#### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityType** elemento com três **propriedade** elementos:

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
 

A exemplo a seguir mostra uma **ComplexType** elemento com cinco **propriedade** elementos:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>O elemento RowType aplicativo

**Propriedade** elementos (como os filhos de um **RowType** elemento) definem a forma e características dos dados que podem ser passadas para ou retornados de uma função definida pelo modelo.  

O **propriedade** elemento pode ter exatamente um dos seguintes elementos filhos:

-   CollectionType
-   ReferenceType
-   RowType

O **propriedade** elemento pode ter qualquer número elementos de anotação de filho.

> [!NOTE]
> Elementos de anotação só são permitidos no CSDL v2 e posterior.

 

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **propriedade** elemento.

| Nome do atributo                                                     | É obrigatório | Valor                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**                                                           | Sim         | O nome da propriedade.                                                                                                                                                                                                       |
| **Tipo**                                                           | Sim         | O tipo do valor da propriedade.                                                                                                                                                                                                 |
| **Permite valor nulo**                                                       | Não          | **Verdadeiro** (o valor padrão) ou **falso** dependendo se a propriedade pode ter um valor nulo. <br/> [!NOTE]                                                                                                                |
| > No CSDL v1 uma propriedade de tipo complexo deve ter `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Não          | O valor padrão da propriedade.                                                                                                                                                                                              |
| **MaxLength**                                                      | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                       |
| **FixedLength**                                                    | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                          |
| **Precisão**                                                      | Não          | A precisão do valor da propriedade.                                                                                                                                                                                            |
| **Ajustar Escala**                                                          | Não          | A escala do valor da propriedade.                                                                                                                                                                                                |
| **SRID**                                                           | Não          | Identificador de referência espacial do sistema. Válido somente para as propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                               |
| **Agrupamento**                                                      | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.                                                                                                                                                   |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **propriedade** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

#### <a name="example"></a>Exemplo

A exemplo a seguir mostra **propriedade** elementos usados para definir a forma do tipo de retorno de uma função definida pelo modelo.

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
 

 

## <a name="propertyref-element-csdl"></a>Elemento PropertyRef (CSDL)

O **PropertyRef** elemento na linguagem de definição de esquema conceitual (CSDL) faz referência a uma propriedade de um tipo de entidade para indicar que a propriedade realizará uma das seguintes funções:

-   Parte da chave da entidade (uma propriedade ou um conjunto de propriedades que determinam a identidade de um tipo de entidade). Um ou mais **PropertyRef** elementos podem ser usados para definir uma chave de entidade.
-   O final dependente ou entidade de segurança de uma restrição referencial.

O **PropertyRef** elemento só pode ter elementos de anotação (zero ou mais) como elementos filho.

> [!NOTE]
> Elementos de anotação só são permitidos no CSDL v2 e posterior.

 

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **PropertyRef** elemento.

| Nome do atributo | É obrigatório | Valor                                |
|:---------------|:------------|:-------------------------------------|
| **Nome**       | Sim         | O nome da propriedade referenciada. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **PropertyRef** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir define um tipo de entidade (**livro**). A chave de entidade é definida fazendo referência a **ISBN** propriedade do tipo de entidade.

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
 

No exemplo a seguir, dois **PropertyRef** elementos são usados para indicar se duas propriedades (**Id** e **PublisherId**) são as extremidades de entidade e dependentes de um referencial restrição.

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
 

 

## <a name="referencetype-element-csdl"></a>Elemento ReferenceType (CSDL)

O **ReferenceType** elemento na linguagem de definição de esquema conceitual (CSDL) especifica uma referência a um tipo de entidade. O **ReferenceType** elemento pode ser um filho dos elementos a seguir:

-   ReturnType (função)
-   Parâmetro
-   CollectionType

O **ReferenceType** elemento é usado ao definir um tipo de parâmetro ou retornado para uma função.

Um **ReferenceType** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **ReferenceType** elemento.

| Nome do atributo | É obrigatório | Valor                                         |
|:---------------|:------------|:----------------------------------------------|
| **Tipo**       | Sim         | O nome do tipo de entidade que está sendo referenciado. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReferenceType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra a **ReferenceType** elemento usado como um filho de um **parâmetro** elemento em uma função definida modelo que aceita uma referência a um **pessoa** entidade tipo:

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
 

A exemplo a seguir mostra a **ReferenceType** elemento usado como um filho de um **ReturnType** elemento (função) em uma função definida modelo que retorna uma referência a um **pessoa**tipo de entidade:

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
 

 

## <a name="referentialconstraint-element-csdl"></a>Elemento ReferentialConstraint (CSDL)

Um **ReferentialConstraint** elemento na linguagem de definição de esquema conceitual (CSDL) define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional. Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados podem fazer referência a chave primária de outra tabela, uma propriedade (ou propriedades) de um tipo de entidade podem fazer referência à chave de entidade de outro tipo de entidade. O tipo de entidade que é referenciado é chamado de *final principal* da restrição. O tipo de entidade que faz referência a extremidade de entidade é chamado de *final dependente* da restrição.

Se uma chave estrangeira que é exposta no tipo de uma entidade faz referência a uma propriedade em outro tipo de entidade, o **ReferentialConstraint** elemento define uma associação entre os dois tipos de entidade. Porque o **ReferentialConstraint** elemento fornece informações sobre como dois tipos de entidade são relacionados, nenhum elemento AssociationSetMapping correspondente é necessário na linguagem de especificação de mapeamento (MSL). Uma associação entre dois tipos de entidade que não têm chaves estrangeiras expostas deve ter um correspondente **AssociationSetMapping** elemento para mapear as informações de associação à fonte de dados.

Se uma chave estrangeira não é exposta em um tipo de entidade, o **ReferentialConstraint** elemento só pode definir uma restrição de chave primária chave para o primário entre o tipo de entidade e o outro tipo de entidade.

Um **ReferentialConstraint** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Entidade de segurança (exatamente um elemento)
-   Dependente (exatamente um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

O **ReferentialConstraint** elemento pode ter qualquer número de atributos de anotação (atributos personalizados de XML). No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **ReferentialConstraint** elemento que está sendo usado como parte da definição da **PublishedBy** associação.

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
 

 

## <a name="returntype-function-element-csdl"></a>Elemento ReturnType (função) (CSDL)

O **ReturnType** elemento (função) na linguagem de definição de esquema conceitual (CSDL) Especifica o tipo de retorno para uma função que é definida em um elemento de função. Uma função de tipo de retorno também pode ser especificado com um **ReturnType** atributo.

Retornar tipos podem ser qualquer **EdmSimpleType**, tipo de entidade, tipo complexo, tipo de linha, tipo ref ou uma coleção de um desses tipos.

O tipo de retorno de uma função pode ser especificado com o **tipo** atributo da **ReturnType** elemento (função), ou com um dos seguintes elementos filhos:

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> Um modelo não validará se você especificar uma função retornar um tipo com ambos os **tipo** atributo da **ReturnType** elemento (função) e um dos elementos filho.

 

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **ReturnType** elemento (função).

| Nome do atributo | É obrigatório | Valor                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | Não          | O tipo retornado pela função. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReturnType** elemento (função). No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir usa uma **função** elemento para definir uma função que retorna o número de anos que foi um livro na impressão. Observe que o tipo de retorno for especificado, o **tipo** atributo de um **ReturnType** elemento (função).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport) elemento (CSDL)

O **ReturnType** (FunctionImport) elemento na linguagem de definição de esquema conceitual (CSDL) Especifica o tipo de retorno para uma função que é definido em um elemento FunctionImport. Uma função de tipo de retorno também pode ser especificado com um **ReturnType** atributo.

Retornar tipos podem ser qualquer coleção de tipo de entidade, tipo complexo, ou **EdmSimpleType**,

O tipo de retorno de uma função é especificado com o **tipo** atributo da **ReturnType** elemento (FunctionImport).

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **ReturnType** elemento (FunctionImport).

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**       | Não          | O tipo que a função retorna. O valor deve ser uma coleção de EDMSimpleType, EntityType ou ComplexType.                                                                                      |
| **EntitySet**  | Não          | Se a função retorna uma coleção de entidade tipos, o valor da **EntitySet** devem ser o conjunto de entidades ao qual pertence a coleção. Caso contrário, o **EntitySet** atributo não deve ser usado. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReturnType** elemento (FunctionImport). No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir usa uma **FunctionImport** que retorna livros e editores. Observe que a função retorna dois conjuntos de resultados e, portanto, duas **ReturnType** elementos (FunctionImport) são especificados.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>Elemento RowType (CSDL)

Um **RowType** elemento na linguagem de definição de esquema conceitual (CSDL) define uma estrutura sem nome como um parâmetro ou tipo de retorno para uma função definida no modelo conceitual.

Um **RowType** elemento pode ser o filho dos elementos a seguir:

-   CollectionType
-   Parâmetro
-   ReturnType (função)

Um **RowType** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Propriedade (um ou mais)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **RowType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função definida modelo que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de linhas (conforme especificado na **RowType** elemento).

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

## <a name="schema-element-csdl"></a>Elemento de esquema (CSDL)

O **esquema** é o elemento raiz de uma definição de modelo conceitual. Ele contém definições para os contêineres que compõem um modelo conceitual, funções e objetos.

O **esquema** elemento pode conter zero ou mais dos seguintes elementos filho:

-   Usando o
-   EntityContainer
-   EntityType
-   EnumType
-   Associação
-   ComplexType
-   Função

Um **esquema** elemento pode conter zero ou mais elementos de anotação.

> [!NOTE]
> O **função** elemento e os elementos de anotação só são permitidos no CSDL v2 e posterior.

 

O **esquema** elemento usa o **Namespace** atributo para definir o namespace para o tipo de entidade, tipo complexo e objetos de associação em um modelo conceitual. Dentro de um namespace, não possui dois objetos podem ter o mesmo nome. Namespaces pode abranger vários **esquema** elementos e vários arquivos. CSDL.

Um namespace de modelo conceitual é diferente do namespace de XML o **esquema** elemento. Um namespace de modelo conceitual (conforme definido pela **Namespace** atributo) é um contêiner lógico para tipos de entidade, tipos complexos e tipos de associação. O namespace de XML (indicado pelo **xmlns** atributo) de uma **esquema** elemento é o namespace padrão para os elementos filho e atributos dos **esquema** elemento. Namespaces XML do formulário http://schemas.microsoft.com/ado/YYYY/MM/edm (em que aaaa e MM representam um ano e mês, respectivamente) são reservados para CSDL. Atributos e elementos personalizados não podem ser nos namespaces que têm esse formulário.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos podem ser aplicados para o **esquema** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Sim         | O namespace do modelo conceitual. O valor de **Namespace** atributo é usado para formar o nome totalmente qualificado de um tipo. Por exemplo, se um **EntityType** denominado *Customer* está no namespace Simple.Example.Model e, em seguida, o nome totalmente qualificado do **EntityType** é SimpleExampleModel.Customer. <br/> As seguintes cadeias de caracteres não podem ser usadas como o valor para o **Namespace** atributo: **sistema**, **transitório**, ou **Edm**. O valor para o **Namespace** atributo não pode ser o mesmo que o valor para o **Namespace** atributo no elemento do esquema de SSDL. |
| **Alias**      | Não          | Um identificador usado no lugar do nome de namespace. Por exemplo, se um **EntityType** denominado *Customer* está no namespace Simple.Example.Model e o valor da **Alias** atributo é *modelo*, em seguida, você pode usar Model.Customer como o nome totalmente qualificado do **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **esquema** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **esquema** elemento que contém uma **EntityContainer** elemento, dois **EntityType** elementos e um **associação** elemento.

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
 

 

## <a name="typeref-element-csdl"></a>Elemento TypeRef (CSDL)

O **TypeRef** elemento na linguagem de definição de esquema conceitual (CSDL) fornece uma referência a um tipo nomeado existente. O **TypeRef** elemento pode ser um filho do elemento CollectionType, que é usado para especificar que uma função tem uma coleção como um tipo de parâmetro ou retornado.

Um **TypeRef** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **TypeRef** elemento. Observe que o **DefaultValue**, **MaxLength**, **FixedLength**, **precisão**, **escala**,  **Unicode**, e **agrupamento** atributos só são aplicáveis ao **EDMSimpleTypes**.

| Nome do atributo                                                     | É obrigatório | Valor                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**                                                           | Não          | O nome do tipo que está sendo referenciado.                                                                                                                                                                                          |
| **Permite valor nulo**                                                       | Não          | **Verdadeiro** (o valor padrão) ou **falso** dependendo se a propriedade pode ter um valor nulo. <br/> [!NOTE]                                                                                                                |
| > No CSDL v1 uma propriedade de tipo complexo deve ter `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Não          | O valor padrão da propriedade.                                                                                                                                                                                              |
| **MaxLength**                                                      | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                       |
| **FixedLength**                                                    | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                          |
| **Precisão**                                                      | Não          | A precisão do valor da propriedade.                                                                                                                                                                                            |
| **Ajustar Escala**                                                          | Não          | A escala do valor da propriedade.                                                                                                                                                                                                |
| **SRID**                                                           | Não          | Identificador de referência espacial do sistema. Válido somente para as propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Não          | **Verdadeiro** ou **falso** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                               |
| **Agrupamento**                                                      | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.                                                                                                                                                   |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **CollectionType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função definida modelo que usa o **TypeRef** elemento (como um filho de um **CollectionType** elemento) para especificar que a função aceita uma coleção de  **Departamento** tipos de entidade.

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
 

 

## <a name="using-element-csdl"></a>Usando o elemento (CSDL)

O **Using** elemento na linguagem de definição de esquema conceitual (CSDL) importa o conteúdo de um modelo conceitual que existe em um namespace diferente. Definindo o valor da **Namespace** atributo, você pode consultar os tipos de entidade, tipos complexos e tipos de associação que são definidos em outro modelo conceitual. Mais de um **Using** elemento pode ser um filho de um **esquema** elemento.

> [!NOTE]
> O **Using** elemento no CSDL não funcionará exatamente como um **usando** instrução em uma linguagem de programação. Importando um namespace com um **usando** instrução em uma linguagem de programação, você não afetam os objetos no namespace original. Em CSDL, um namespace importado pode conter um tipo de entidade que é derivado de um tipo de entidade no namespace original. Isso pode afetar os conjuntos de entidades declarados no namespace original.

 

O **Using** elemento pode ter os seguintes elementos filho:

-   Documentação (zero ou mais elementos permitidos)
-   Elementos de anotação (zero ou mais elementos permitidos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos podem ser aplicados para o **Using** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Sim         | O nome do namespace importado.                                                                                                                                                |
| **Alias**      | Sim         | Um identificador usado no lugar do nome de namespace. Embora esse atributo é necessário, ele não é necessário que ele ser usado no lugar do nome do namespace para qualificar nomes de objeto. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **Using** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

 

### <a name="example"></a>Exemplo

O exemplo a seguir demonstra a **Using** elemento que está sendo usado para importar um namespace que é definida em outro lugar. Observe que o namespace para o **esquema** elemento mostrado é `BooksModel`. O `Address` propriedade no `Publisher` **EntityType** é um tipo complexo que é definido na `ExtendedBooksModel` namespace (importados com o **Using** elemento).

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
 

 

## <a name="annotation-attributes-csdl"></a>Atributos de anotação (CSDL)

Atributos de anotação na linguagem de definição de esquema conceitual (CSDL) são os atributos personalizados de XML no modelo conceitual. Além de ter a estrutura XML válida, o seguinte deve ser verdadeiro para atributos de anotação:

-   Atributos de anotação não devem estar em qualquer namespace XML que é reservado para CSDL.
-   Mais de um atributo de anotação pode ser aplicado a um determinado elemento CSDL.
-   Os nomes totalmente qualificados de todos os atributos dois anotação não pode ser o mesmo.

Atributos de anotação podem ser usados para fornecer metadados adicionais sobre os elementos em um modelo conceitual. Os metadados contidos em elementos de anotação podem ser acessado no tempo de execução usando classes no namespace Metadata.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityType** elemento com um atributo de anotação (**CustomAttribute**). O exemplo também mostra um elemento de anotação aplicado ao elemento de tipo de entidade.

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
 

O código a seguir recupera os metadados no atributo de anotação e grava-o no console:

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
 

O código acima pressupõe que o `School.csdl` arquivo está no diretório de saída do projeto e que você tenha adicionado o seguinte `Imports` e `Using` instruções ao seu projeto:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Elementos de anotação (CSDL)

Elementos de anotação na linguagem de definição de esquema conceitual (CSDL) são os elementos XML personalizados no modelo conceitual. Além de ter a estrutura XML válida, o seguinte deve ser verdadeiro para elementos de anotação:

-   Elementos de anotação não devem estar em qualquer namespace XML que é reservado para CSDL.
-   Mais de um elemento de anotação pode ser um filho de um determinado elemento CSDL.
-   Os nomes totalmente qualificados de quaisquer elementos de dois de anotação não pode ser o mesmo.
-   Elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento CSDL.

Elementos de anotação podem ser usados para fornecer metadados adicionais sobre os elementos em um modelo conceitual. Começando com o .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessado no tempo de execução usando classes no namespace Metadata.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityType** elemento com um elemento de anotação (**CustomElement**). O exemplo também mostra um atributo de anotação aplicado ao elemento de tipo de entidade.

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
 

O código a seguir recupera os metadados no elemento de anotação e grava-o no console:

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
 

O código acima presume que o arquivo de School está no diretório de saída do projeto e que você tenha adicionado o seguinte `Imports` e `Using` instruções ao seu projeto:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Tipos de modelo conceituais (CSDL)

Linguagem de definição de esquema conceitual (CSDL) dá suporte a um conjunto de tipos de dados primitivo abstrato, chamados **EDMSimpleTypes**, que definem as propriedades em um modelo conceitual. **EDMSimpleTypes** são proxies para os tipos de dados primitivos que têm suporte no ambiente de hospedagem ou armazenamento.

A tabela a seguir lista os tipos de dados primitivos que são compatíveis com a CSDL. A tabela também lista as facetas que podem ser aplicadas a cada **EDMSimpleType**.

| EDMSimpleType                    | Descrição                                                | Facetas aplicáveis                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **EDM**                   | Contém dados binários.                                      | MaxLength, FixedLength, anulável, opção                                |
| **EDM**                  | Contém o valor **verdadeira** ou **falso**.                  | Anulável, opção                                                        |
| **Edm.Byte**                     | Contém um valor inteiro de 8 bits sem sinal.                  | Precisão, anulável, opção                                             |
| **EDM**                 | Representa uma data e hora.                                | Precisão, anulável, opção                                             |
| **EDM**           | Contém uma data e hora como um deslocamento em minutos GMT. | Precisão, anulável, opção                                             |
| **Edm.Decimal**                  | Contém um valor numérico com precisão e escala fixa.   | Precisão, anulável, opção                                             |
| **EDM. Double**                   | Contém um número com precisão de 15 dígitos de ponto flutuante   | Precisão, anulável, opção                                             |
| **Edm.Float**                    | Contém um número com precisão de 7 dígitos de ponto flutuante.   | Precisão, anulável, opção                                             |
| **EDM. GUID**                     | Contém um identificador exclusivo de 16 bytes.                      | Precisão, anulável, opção                                             |
| **Edm.Int16**                    | Contém um valor inteiro com sinal de 16 bits.                    | Precisão, anulável, opção                                             |
| **Edm.Int32**                    | Contém um valor inteiro com sinal de 32 bits.                    | Precisão, anulável, opção                                             |
| **Edm.Int64**                    | Contém um valor inteiro com sinal de 64 bits.                    | Precisão, anulável, opção                                             |
| **Edm.SByte**                    | Contém um valor inteiro de 8 bits com sinal.                     | Precisão, anulável, opção                                             |
| **EDM. String**                   | Contém dados de caractere.                                   | Unicode, FixedLength, MaxLength, agrupamento, precisão, anulável, opção |
| **EDM**                     | Contém uma hora.                                    | Precisão, anulável, opção                                             |
| **Geography**                |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **EDM. geographypoint**           |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Geographypolygon**         |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **EDM**                 |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | Permite valor nulo, padrão, o SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | Permite valor nulo, padrão, o SRID                                                  |

## <a name="facets-csdl"></a>Facetas (CSDL)

Facetas na linguagem de definição de esquema conceitual (CSDL) representam as restrições nas propriedades dos tipos de entidade e tipos complexos. As facetas são exibidos como atributos XML em elementos CSDL a seguir:

-   Propriedade
-   TypeRef
-   Parâmetro

A tabela a seguir descreve as facetas que têm suporte no CSDL. Todas as facetas são opcionais. Algumas facetas listadas abaixo são usadas pelo Entity Framework ao gerar um banco de dados de um modelo conceitual.

> [!NOTE]
> Para obter informações sobre tipos de dados em um modelo conceitual, consulte tipos de modelo conceituais (CSDL).

| Aspecto               | Descrição                                                                                                                                                                                                                                                   | Aplica-se a                                                                                                                                                                                                                                                                                                                                                                           | Usado para a geração de banco de dados | Usado pelo tempo de execução |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Agrupamento**       | Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.                                                                                                               | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Sim                              | Não                  |
| **ConcurrencyMode** | Indica que o valor da propriedade deve ser usado para verificação de simultaneidade otimista.                                                                                                                                                                    | Todos os **EDMSimpleType** propriedades                                                                                                                                                                                                                                                                                                                                                     | Não                               | Sim                 |
| **Padrão**         | Especifica o valor de propriedade padrão se nenhum valor é fornecido em cima de instanciação.                                                                                                                                                                       | Todos os **EDMSimpleType** propriedades                                                                                                                                                                                                                                                                                                                                                     | Sim                              | Sim                 |
| **FixedLength**     | Especifica se o comprimento do valor da propriedade pode variar.                                                                                                                                                                                                  | **EDM**, **EDM. String**                                                                                                                                                                                                                                                                                                                                                       | Sim                              | Não                  |
| **MaxLength**       | Especifica o comprimento máximo de valor de propriedade.                                                                                                                                                                                                           | **EDM**, **EDM. String**                                                                                                                                                                                                                                                                                                                                                       | Sim                              | Não                  |
| **Permite valor nulo**        | Especifica se a propriedade pode ter um **nulo** valor.                                                                                                                                                                                                     | Todos os **EDMSimpleType** propriedades                                                                                                                                                                                                                                                                                                                                                     | Sim                              | Sim                 |
| **Precisão**       | Para propriedades do tipo **Decimal**, especifica o número de dígitos de um valor da propriedade pode ter. Para propriedades do tipo **tempo**, **DateTime**, e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da propriedade. | **EDM**, **EDM**, **Edm.Decimal**, **EDM**                                                                                                                                                                                                                                                                                                              | Sim                              | Não                  |
| **Ajustar Escala**           | Especifica o número de dígitos à direita do ponto decimal para o valor da propriedade.                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Sim                              | Não                  |
| **SRID**            | Especifica a ID de sistema de referência espacial sistema. Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **Geography, EDM. geographypoint, Edm.GeographyLineString, geographypolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, EDM, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | Não                               | Sim                 |
| **Unicode**         | Indica se o valor da propriedade é armazenado como Unicode.                                                                                                                                                                                                    | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Sim                              | Sim                 |

>[!NOTE]
> Ao gerar um banco de dados de um modelo conceitual, o Assistente de banco de dados gerar reconhecerão o valor da **StoreGeneratedPattern** o atributo em uma **propriedade** elemento se ele está a seguir namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation. São os valores com suporte para o atributo **Identity** e **computado**. Um valor de **identidade** produzirá uma coluna de banco de dados com um valor de identidade que é gerado no banco de dados. Um valor de **computado** produzirá uma coluna com um valor que é computada no banco de dados.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra as facetas aplicadas às propriedades de um tipo de entidade:

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
