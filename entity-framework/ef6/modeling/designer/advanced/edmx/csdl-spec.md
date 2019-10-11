---
title: Especificação de CSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182594"
---
# <a name="csdl-specification"></a>Especificação de CSDL
O CSDL (linguagem de definição de esquema conceitual) é uma linguagem baseada em XML que descreve as entidades, as relações e as funções que compõem um modelo conceitual de um aplicativo controlado por dados. Esse modelo conceitual pode ser usado pelo Entity Framework ou WCF Data Services. Os metadados que são descritos com CSDL são usados pelo Entity Framework para mapear entidades e relações que são definidas em um modelo conceitual para uma fonte de dados. Para obter mais informações, consulte Especificação de [SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) e [especificação MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL é a implementação do Entity Framework do Modelo de Dados de Entidade.

Em um aplicativo Entity Framework, os metadados do modelo conceitual são carregados de um arquivo. CSDL (escrito em CSDL) em uma instância do System. Data. Metadata. Edm. EdmItemCollection e é acessível usando métodos no Classe System. Data. Metadata. Edm. MetadataWorkspace. Entity Framework usa metadados de modelo conceitual para converter consultas em relação ao modelo conceitual para comandos específicos da fonte de dados.

O designer do EF armazena informações do modelo conceitual em um arquivo. edmx em tempo de design. No momento da compilação, o designer do EF usa informações em um arquivo. edmx para criar o arquivo. CSDL necessário pelo Entity Framework em tempo de execução.

As versões de CSDL são diferenciadas por namespaces XML.

| Versão de CSDL | Namespace XML                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | https://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Elemento Association (CSDL)

Um elemento **Association** define uma relação entre dois tipos de entidade. Uma associação deve especificar os tipos de entidade que estão envolvidos na relação e o número possível de tipos de entidade em cada extremidade da relação, que é conhecida como multiplicidade. A multiplicidade de uma extremidade de associação pode ter um valor de um (1), zero ou um (0.. 1) ou muitos (\*). Essas informações são especificadas em dois elementos finais filho.

Instâncias de tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras, se forem expostas em um tipo de entidade.

Em um aplicativo, uma instância de uma associação representa uma associação específica entre instâncias de tipos de entidade. As instâncias de associação são agrupadas logicamente em um conjunto de associação.

Um elemento **Association** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Final (exatamente 2 elementos)
-   ReferentialConstraint (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Association** .

| Nome do atributo | É obrigatório | Valor                        |
|:---------------|:------------|:-----------------------------|
| **Name**       | Sim         | O nome da associação. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Association** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Association** que define a associação **CustomerOrders** quando chaves estrangeiras não foram expostas nos tipos de entidade **Customer** e **Order** . Os valores de **multiplicidade** para cada **extremidade** da Associação indicam que muitos **pedidos** podem ser associados a um **cliente**, mas somente um **cliente** pode ser associado a um **pedido**. Além disso, o elemento **OnDelete** indica que todos os **pedidos** relacionados a um **cliente** específico e que foram carregados no ObjectContext serão excluídos se o **cliente** for excluído.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

O exemplo a seguir mostra um elemento **Association** que define a associação **CustomerOrders** quando chaves estrangeiras foram expostas nos tipos de entidade **Customer** e **Order** . Com as chaves estrangeiras expostas, a relação entre as entidades é gerenciada com um elemento **ReferentialConstraint** . Um elemento AssociationSetMapping correspondente não é necessário para mapear essa associação para a fonte de dados.

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

O elemento **AssociationSet** na CSDL (linguagem de definição de esquema conceitual) é um contêiner lógico para instâncias de associação do mesmo tipo. Um conjunto de associação fornece uma definição para Agrupamento de instâncias de associação para que elas possam ser mapeadas para uma fonte de dados.  

O elemento **AssociationSet** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento permitido)
-   End (exatamente dois elementos necessários)
-   Elementos de anotação (zero ou mais elementos permitidos)

O atributo **Association** especifica o tipo de associação que um conjunto de associação contém. Os conjuntos de entidades que compõem as extremidades de um conjunto de associação são especificados com exatamente dois elementos **end** filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **AssociationSet** .

| Nome do atributo  | É obrigatório | Valor                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**        | Sim         | O nome do conjunto de entidades. O valor do atributo **Name** não pode ser o mesmo que o valor do atributo **Association** .                                 |
| **Associação** | Sim         | O nome totalmente qualificado da associação que o conjunto de associação contém instâncias do. A associação deve estar no mesmo namespace que o conjunto de associação. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **AssociationSet** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityContainer** com dois elementos **AssociationSet** :

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

O elemento **CollectionType** na linguagem de definição de esquema conceitual (CSDL) especifica que um parâmetro de função ou tipo de retorno de função é uma coleção. O elemento **CollectionType** pode ser filho do elemento Parameter ou do elemento ReturnType (Function). O tipo de coleção pode ser especificado usando o atributo **Type** ou um dos seguintes elementos filho:

-   **CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> Um modelo não será validado se o tipo de uma coleção for especificado com o atributo **Type** e um elemento filho.

 

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **CollectionType** . Observe que os atributos **DefaultValue**, **MaxLength**, **cadeia**, **Precision**, **Scale**, **Unicode**e **Collation** são aplicáveis somente às coleções de **EDMSimpleTypes**.

| Nome do atributo                                                          | É obrigatório | Valor                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**                                                                | Não          | O tipo da coleção.                                                                                                                                                                                                      |
| **Anula**                                                            | Não          | **True** (o valor padrão) ou **false** , dependendo se a propriedade pode ter um valor nulo. <br/> [!NOTE]                                                                                                                 |
| > No CSDL v1, uma propriedade de tipo complexo deve ter `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | Não          | O valor padrão da propriedade.                                                                                                                                                                                               |
| **MaxLength**                                                           | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                        |
| **Cadeia**                                                         | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                           |
| **Precisão**                                                           | Não          | A precisão do valor da propriedade.                                                                                                                                                                                             |
| **Ajustar Escala**                                                               | Não          | A escala do valor da propriedade.                                                                                                                                                                                                 |
| **SRID**                                                                | Não          | Identificador de referência de sistema espacial. Válido somente para propriedades de tipos espaciais.   Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                                |
| **Agrupamento**                                                           | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.                                                                                                                                                    |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **CollectionType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função definida pelo modelo que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de tipos de entidade **Person** (conforme especificado com o atributo **ElementType** ).

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
 

O exemplo a seguir mostra uma função definida pelo modelo que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas (conforme especificado no elemento **RowType** ).

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
 

O exemplo a seguir mostra uma função definida pelo modelo que usa o elemento **CollectionType** para especificar que a função aceita como um parâmetro uma coleção de tipos de entidade **Department** .

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
 

 

## <a name="complextype-element-csdl"></a>Elemento complexType (CSDL)

Um elemento **complexType** define uma estrutura de dados composta por propriedades **EdmSimpleType** ou outros tipos complexos.  Um tipo complexo pode ser uma propriedade de um tipo de entidade ou outro tipo complexo. Um tipo complexo é semelhante a um tipo de entidade no que um tipo complexo define dados. No entanto, há algumas diferenças entre tipos complexos e tipos de entidade:

-   Tipos complexos não têm identidades (ou chaves) e, portanto, não podem existir de forma independente. Tipos complexos só podem existir como propriedades de tipos de entidade ou outros tipos complexos.
-   Tipos complexos não podem participar de associações. Nenhuma extremidade de uma associação pode ser um tipo complexo e, portanto, as propriedades de navegação não podem ser definidas para tipos complexos.
-   Uma propriedade de tipo complexo não pode ter um valor nulo, embora as propriedades escalares de um tipo complexo possam ser definidas como NULL.

Um elemento **complexType** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Propriedade (zero ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **complexType** .

| Nome do atributo                                                                                                 | É obrigatório | Valor                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nome                                                                                                           | Sim         | O nome do tipo complexo. O nome de um tipo complexo não pode ser igual ao nome de outro tipo complexo, tipo de entidade ou associação que está dentro do escopo do modelo. |
| BaseType                                                                                                       | Não          | O nome de outro tipo complexo que é o tipo base do tipo complexo que está sendo definido. <br/> [!NOTE]                                                                     |
| > Este atributo não é aplicável em CSDL v1. A herança para tipos complexos não tem suporte nessa versão. |             |                                                                                                                                                                                     |
| Resume                                                                                                       | Não          | **True** ou **false** (o valor padrão) dependendo se o tipo complexo é um tipo abstrato. <br/> [!NOTE]                                                                  |
| > Este atributo não é aplicável em CSDL v1. Tipos complexos nessa versão não podem ser tipos abstratos.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **complexType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um tipo complexo, **endereço**, com as propriedades **EdmSimpleType** **StreetAddress**, **City**, **EstadoOuProvíncia**, **Country**e **PostalCode**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Para definir o **endereço** de tipo complexo (acima) como uma propriedade de um tipo de entidade, você deve declarar o tipo de propriedade na definição de tipo de entidade. O exemplo a seguir mostra a propriedade **Address** como um tipo complexo em um tipo de entidade (**Editor**):

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
 

 

## <a name="definingexpression-element-csdl"></a>Elemento de definição (CSDL)

O elemento de **definição** da linguagem de definição de esquema conceitual (CSDL) contém uma Entity SQL expressão que define uma função no modelo conceitual.  

> [!NOTE]
> Para fins de validação, um elemento de **definição** pode conter conteúdo arbitrário. No entanto, Entity Framework gerará uma exceção em tempo de execução se um elemento de **definição** não contiver um Entity SQL válido.

 

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **definição** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir usa um elemento de **definição** para definir uma função que retorna o número de anos desde que um livro foi publicado. O conteúdo do elemento de **definição** é escrito em Entity SQL.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Elemento dependent (CSDL)

O elemento **dependente** na linguagem de definição de esquema conceitual (CSDL) é um elemento filho para o elemento ReferentialConstraint e define a extremidade dependente de uma restrição referencial. Um elemento **ReferentialConstraint** define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional. Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados pode referenciar a chave primária de outra tabela, uma propriedade (ou Propriedades) de um tipo de entidade pode referenciar a chave de entidade de outro tipo de entidade. O tipo de entidade referenciado é chamado de *extremidade principal* da restrição. O tipo de entidade que faz referência à extremidade principal é chamado de *extremidade dependente* da restrição. Os elementos **PropertyRef** são usados para especificar quais chaves fazem referência à extremidade principal.

O elemento **dependente** pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **dependente** .

| Nome do atributo | É obrigatório | Valor                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Função**       | Sim         | O nome do tipo de entidade na extremidade dependente da associação. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **dependente** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **ReferentialConstraint** que está sendo usado como parte da definição da Associação **PublishedBy** . A propriedade **PublisherID** do tipo de entidade **Book** compõe a extremidade dependente da restrição referencial.

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
 

 

## <a name="documentation-element-csdl"></a>Elemento Documentation (CSDL)

O elemento **Documentation** na CSDL (linguagem de definição de esquema conceitual) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai. Em um arquivo. edmx, quando o elemento de **documentação** é um filho de um elemento que aparece como um objeto na superfície de design do designer do EF (como uma entidade, associação ou Propriedade), o conteúdo do elemento de **documentação** aparecerá no Janela **Propriedades** do Visual Studio para o objeto.

O elemento de **documentação** pode ter os seguintes elementos filho (na ordem listada):

-   **Resumo**: Uma breve descrição do elemento pai. (zero ou um elemento)
-   **LongDescription**: Uma descrição abrangente do elemento pai. (zero ou um elemento)
-   Elementos de anotação. (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **documentação** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra o elemento de **documentação** como um elemento filho de um elemento EntityType. Se o trecho de código a seguir estivesse no conteúdo CSDL de um arquivo. edmx, o conteúdo dos elementos **Summary** e **longDescription** apareceria na janela **Propriedades** do Visual Studio quando você clicar no tipo de entidade `Customer`.

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

O elemento **end** na linguagem de definição de esquema conceitual (CSDL) pode ser filho do elemento Association ou do elemento AssociationSet. Em cada caso, a função do elemento **final** é diferente e os atributos aplicáveis são diferentes.

### <a name="end-element-as-a-child-of-the-association-element"></a>Elemento end como um filho do elemento Association

Um elemento **final** (como um filho do elemento **Association** ) identifica o tipo de entidade em uma extremidade de uma associação e o número de instâncias de tipo de entidade que podem existir no final de uma associação. Termina de associação são definidas como parte de uma associação; uma associação deve ter exatamente duas termina de associação. Instâncias de tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras se forem expostas em um tipo de entidade.  

Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   OnDelete (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **Association** .

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**         | Sim         | O nome do tipo de entidade em uma extremidade da associação.                                                                                                                                                                                                                                                                                                                                                         |
| **Função**         | Não          | Um nome para o final da associação. Se nenhum nome for fornecido, o nome do tipo de entidade na extremidade da Associação será usado.                                                                                                                                                                                                                                                                                           |
| **Multiplicidade** | Sim         | **1**, **0.. 1**ou **\*** , dependendo do número de instâncias de tipo de entidade que podem estar no final da associação. <br/> **1** indica que exatamente uma instância de tipo de entidade existe na extremidade da associação. <br/> **0.. 1** indica que nenhuma instância de tipo de entidade existe na extremidade da associação. <br/> **\*** indica que zero, uma ou mais instâncias de tipo de entidade existem na extremidade da associação. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Association** que define a associação **CustomerOrders** . Os valores de **multiplicidade** para cada **extremidade** da Associação indicam que muitos **pedidos** podem ser associados a um **cliente**, mas somente um **cliente** pode ser associado a um **pedido**. Além disso, o elemento **OnDelete** indica que todos os **pedidos** relacionados a um **cliente** específico e que foram carregados no ObjectContext serão excluídos se o **cliente** for excluído.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Elemento end como um filho do elemento AssociationSet

O elemento **end** especifica uma extremidade de um conjunto de associação. O elemento **AssociationSet** deve conter dois elementos **end** . As informações contidas em um elemento **final** são usadas no mapeamento de uma associação definida para uma fonte de dados.

Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

> [!NOTE]
> Os elementos de anotação devem aparecer após todos os outros elementos filho. Os elementos de anotação são permitidos somente no CSDL V2 e posterior.

 

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **AssociationSet** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Sim         | O nome do elemento **EntitySet** que define uma extremidade do elemento de **AssociationSet** pai. O elemento **EntitySet** deve ser definido no mesmo contêiner de entidade que o elemento de **AssociationSet** pai. |
| **Função**       | Não          | O nome da extremidade do conjunto de associação. Se o atributo **role** não for usado, o nome do conjunto de associação end será o nome do conjunto de entidades.                                                                   |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityContainer** com dois elementos **AssociationSet** , cada um com dois elementos **end** :

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

O elemento **EntityContainer** na linguagem de definição de esquema conceitual (CSDL) é um contêiner lógico para conjuntos de entidades, conjuntos de associação e importações de função. Um contêiner de entidade de modelo conceitual é mapeado para um contêiner de entidade de modelo de armazenamento por meio do elemento EntityContainerMapping. Um contêiner de entidade do modelo de armazenamento descreve a estrutura do banco de dados: os conjuntos de entidades descrevem tabelas, os conjuntos de associação descrevem as restrições de chave estrangeira e as importações de função descrevem procedimentos armazenados em um banco de dados.

Um elemento **EntityContainer** pode ter zero ou um elementos de documentação. Se um elemento de **documentação** estiver presente, ele deverá preceder todos os elementos **EntitySet**, **AssociationSet**e **FunctionImport** .

Um elemento **EntityContainer** pode ter zero ou mais dos seguintes elementos filho (na ordem listada):

-   EntitySet
-   AssociationSet
-   FunctionImport
-   Elementos de anotação

Você pode estender um elemento **EntityContainer** para incluir o conteúdo de outro **EntityContainer** que está dentro do mesmo namespace. Para incluir o conteúdo de outro **EntityContainer**, no elemento **EntityContainer** de referência, defina o valor do atributo **extends** como o nome do elemento **EntityContainer** que você deseja incluir. Todos os elementos filho do elemento **EntityContainer** incluído serão tratados como elementos filho do elemento **EntityContainer** de referência.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **using** .

| Nome do atributo | É obrigatório | Valor                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Sim         | O nome do contêiner de entidade.                               |
| **Estende**    | Não          | O nome de outro contêiner de entidade no mesmo namespace. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityContainer** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityContainer** que define três conjuntos de entidades e dois conjuntos de associação.

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

O elemento **EntitySet** na linguagem de definição de esquema conceitual é um contêiner lógico para instâncias de um tipo de entidade e instâncias de qualquer tipo que é derivado desse tipo de entidade. A relação entre um tipo de entidade e um conjunto de entidades é análoga à relação entre uma linha e uma tabela em um banco de dados relacional. Como uma linha, um tipo de entidade define um conjunto de dados relacionados e, como uma tabela, um conjunto de entidades contém instâncias dessa definição. Um conjunto de entidades fornece um constructo para agrupar instâncias de tipo de entidade para que elas possam ser mapeadas para estruturas de dados relacionadas em uma fonte de dados.  

Mais de um conjunto de entidades para um determinado tipo de entidade pode ser definido.

> [!NOTE]
> O designer do EF não oferece suporte a modelos conceituais que contêm vários conjuntos de entidades por tipo.

 

O elemento **EntitySet** pode ter os seguintes elementos filho (na ordem listada):

-   Elemento Documentation (zero ou um elementos permitidos)
-   Elementos de anotação (zero ou mais elementos permitidos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntitySet** .

| Nome do atributo | É obrigatório | Valor                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome do conjunto de entidades.                                                              |
| **EntityType** | Sim         | O nome totalmente qualificado do tipo de entidade para o qual o conjunto de entidades contém instâncias. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntitySet** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityContainer** com três elementos **EntitySet** :

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
 

É possível definir vários conjuntos de entidades por tipo (MEST). O exemplo a seguir define um contêiner de entidade com dois conjuntos de entidades para o tipo de entidade de **livro** :

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

O elemento **EntityType** representa a estrutura de um conceito de nível superior, como um cliente ou uma ordem, em um modelo conceitual. Um tipo de entidade é um modelo para instâncias de tipos de entidade em um aplicativo. Cada modelo contém as informações a seguir:

-   Um nome exclusivo. (Obrigatório.)
-   Uma chave de entidade que é definida por uma ou mais propriedades. (Obrigatório.)
-   Propriedades para conter dados. (Opcional).
-   Propriedades de navegação que permitem a navegação de uma extremidade de uma associação para a outra extremidade. (Opcional).

Em um aplicativo, uma instância de um tipo de entidade representa um objeto específico (como um cliente ou uma ordem específica.) Cada instância de um tipo de entidade deve ter uma chave de entidade exclusiva dentro de um conjunto de entidades.

Duas instâncias do tipo de entidade são consideradas iguais somente se são do mesmo tipo e os valores das chaves de entidade são os mesmos.

Um elemento **EntityType** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Chave (zero ou um elemento)
-   Propriedade (zero ou mais elementos)
-   NavigationProperty (zero ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityType** .

| Nome do atributo                                                                                                                                  | É obrigatório | Valor                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**                                                                                                                                        | Sim         | O nome do tipo de entidade.                                                                     |
| **BaseType**                                                                                                                                    | Não          | O nome de outro tipo de entidade que é o tipo base do tipo de entidade que está sendo definido.  |
| **Resume**                                                                                                                                    | Não          | **True** ou **false**, dependendo se o tipo de entidade é um tipo abstrato.                 |
| **OpenType**                                                                                                                                    | Não          | **True** ou **false** dependendo se o tipo de entidade é um tipo de entidade aberto. <br/> [!NOTE] |
| > O atributo **OpenType** só é aplicável a tipos de entidade que são definidos em modelos conceituais que são usados com os serviços de dados do ADO.net. |             |                                                                                                  |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityType** com três elementos **Property** e dois elementos **NavigationProperty** :

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

O elemento **enumType** representa um tipo enumerado.

Um elemento **enumType** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Membro (zero ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **enumType** .

| Nome do atributo     | É obrigatório | Valor                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**           | Sim         | O nome do tipo de entidade.                                                                                                                                                                  |
| **IsFlags**        | Não          | **True** ou **false**, dependendo se o tipo de enumeração pode ser usado como um conjunto de sinalizadores. O valor padrão é **false.** .                                                                     |
| **UnderlyingType** | Não          | **EDM. byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** ou **EDM. SByte** que definem o intervalo de valores do tipo.   O tipo subjacente padrão de elementos de enumeração é **EDM. Int32.** . |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **enumType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **enumType** com três elementos de **membro** :

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Elemento Function (CSDL)

O elemento **Function** na linguagem de definição de esquema conceitual (CSDL) é usado para definir ou declarar funções no modelo conceitual. Uma função é definida usando um elemento de definição.  

Um elemento **Function** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Parâmetro (zero ou mais elementos)
-   Definindo a definição (zero ou um elemento)
-   ReturnType (função) (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

Um tipo de retorno para uma função deve ser especificado com o elemento **ReturnType** (função) ou o atributo **ReturnType** (veja abaixo), mas não ambos. Os tipos de retorno possíveis são qualquer EdmSimpleType, tipo de entidade, tipo complexo, tipo de linha ou tipo de referência (ou uma coleção de um desses tipos).

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Function** .

| Nome do atributo | É obrigatório | Valor                              |
|:---------------|:------------|:-----------------------------------|
| **Name**       | Sim         | O nome da função.          |
| **ReturnType** | Não          | O tipo retornado pela função. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Function** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir usa um elemento **Function** para definir uma função que retorna o número de anos desde que um instrutor foi contratado.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>Elemento FunctionImport (CSDL)

O elemento **FunctionImport** na linguagem de definição de esquema conceitual (CSDL) representa uma função que é definida na fonte de dados, mas disponível para objetos por meio do modelo conceitual. Por exemplo, um elemento Function no modelo de armazenamento pode ser usado para representar um procedimento armazenado em um banco de dados. Um elemento **FunctionImport** no modelo conceitual representa a função correspondente em um aplicativo Entity Framework e é mapeado para a função de modelo de armazenamento usando o elemento FunctionImportMapping. Quando a função é chamada no aplicativo, o procedimento armazenado correspondente é executado no banco de dados.

O elemento **FunctionImport** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento permitido)
-   Parâmetro (zero ou mais elementos permitidos)
-   Elementos de anotação (zero ou mais elementos permitidos)
-   ReturnType (FunctionImport) (zero ou mais elementos permitidos)

Um elemento de **parâmetro** deve ser definido para cada parâmetro que a função aceita.

Um tipo de retorno para uma função deve ser especificado com o elemento **ReturnType** (FunctionImport) ou o atributo **ReturnType** (veja abaixo), mas não ambos. O valor do tipo de retorno deve ser uma coleção de EdmSimpleType, EntityType ou complexType.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **FunctionImport** .

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Sim         | O nome da função importada.                                                                                                                                                                    |
| **ReturnType**   | Não          | O tipo que a função retorna. Não use esse atributo se a função não retornar um valor. Caso contrário, o valor deve ser uma coleção de complexType, EntityType ou EDMSimpleType.        |
| **EntitySet**    | Não          | Se a função retornar uma coleção de tipos de entidade, o valor do **EntitySet** deverá ser o conjunto de entidades ao qual a coleção pertence. Caso contrário, o atributo **EntitySet** não deve ser usado. |
| **IsComposable** | Não          | Se o valor for definido como true, a função será combinável (função com valor de tabela) e poderá ser usada em uma consulta LINQ.  O padrão é **false**.                                                           |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **FunctionImport** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **FunctionImport** que aceita um parâmetro e retorna uma coleção de tipos de entidade:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Elemento Key (CSDL)

O elemento **Key** é um elemento filho do elemento EntityType e define uma *chave de entidade* (uma propriedade ou um conjunto de propriedades de um tipo de entidade que determinam a identidade). As propriedades que compõem uma chave de entidade são escolhidas em tempo de design. Os valores das propriedades de chave de entidade devem identificar exclusivamente uma instância de tipo de entidade dentro de uma entidade definida em tempo de execução. As propriedades que compõem uma chave de entidade devem ser escolhidas para garantir a exclusividade de instâncias em um conjunto de entidades. O elemento **Key** define uma chave de entidade referenciando uma ou mais das propriedades de um tipo de entidade.

O elemento **Key** pode ter os seguintes elementos filho:

-   PropertyRef (um ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **chave** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir define um tipo de entidade chamado **Book**. A chave de entidade é definida referenciando a propriedade **ISBN** do tipo de entidade.

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
 

A propriedade **ISBN** é uma boa opção para a chave de entidade porque um ISBN (número de livro padrão internacional) identifica exclusivamente um livro.

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
 

Usar o **nome** e o **endereço** para a chave de entidade é uma opção razoável, pois dois autores com o mesmo nome provavelmente não residirão no mesmo endereço. No entanto, esta opção para uma chave de entidade não garante absolutamente chaves exclusivas de entidade em um conjunto de entidades. A adição de uma propriedade, como **AuthorId**, que poderia ser usada para identificar exclusivamente um autor seria recomendável nesse caso.

 

## <a name="member-element-csdl"></a>Elemento member (CSDL)

O elemento **Member** é um elemento filho do elemento enumType e define um membro do tipo enumerado.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **FunctionImport** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome do membro.                                                                                                                                                                  |
| **Valor**      | Não          | O valor do membro. Por padrão, o primeiro membro tem o valor 0 e o valor de cada enumerador sucessivo é incrementado em 1. Vários membros com os mesmos valores podem existir. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **FunctionImport** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **enumType** com três elementos de **membro** :

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>Elemento NavigationProperty (CSDL)

Um elemento **NavigationProperty** define uma propriedade de navegação, que fornece uma referência à outra extremidade de uma associação. Ao contrário das propriedades definidas com o elemento Property, as propriedades de navegação não definem a forma e as características dos dados. Eles fornecem uma maneira de navegar por uma associação entre dois tipos de entidade.

Observe que as propriedades de navegação são opcionais em ambos os tipos de entidade termina de uma associação. Se você definir uma propriedade de navegação em um tipo de entidade no final de uma associação, você não precisa definir uma propriedade de navegação no tipo de entidade no outro extremo de associação.

O tipo de dados retornado por uma propriedade de navegação é determinado pela multiplicidade de sua extremidade de associação remota. Por exemplo, suponha que uma propriedade de navegação, **OrdersNavProp**, exista em um tipo de entidade **Customer** e navegue por uma associação um-para-muitos entre **Customer** e **Order**. Como a extremidade de associação remota para a propriedade de navegação tem muitas multiplicidades (\*), seu tipo de dados é uma coleção (de **ordem**). Da mesma forma, se uma propriedade de navegação, **CustomerNavProp**, existir no tipo de entidade **Order** , seu tipo de dados será **Customer** , pois a multiplicidade da extremidade remota é um (1).

Um elemento **NavigationProperty** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **NavigationProperty** .

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Sim         | O nome da propriedade de navegação.                                                                                                                                                                                                             |
| **Inter-relações** | Sim         | O nome de uma associação que está dentro do escopo do modelo.                                                                                                                                                                                |
| **ToRole**       | Sim         | O fim da associação na qual a navegação termina. O valor do atributo **ToRole** deve ser o mesmo que o valor de um dos atributos **role** definidos em uma das extremidades de associação (definido no elemento AssociationEnd).       |
| **FromRole**     | Sim         | O fim da associação da qual a navegação começa. O valor do atributo **FromRole** deve ser o mesmo que o valor de um dos atributos de **função** definidos em uma das extremidades de associação (definido no elemento AssociationEnd). |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **NavigationProperty** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir define um tipo de entidade (**Book**) com duas propriedades de navegação (**PublishedBy** e **WrittenBy**):

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

O elemento **OnDelete** na linguagem de definição de esquema conceitual (CSDL) define o comportamento que está conectado a uma associação. Se o atributo **Action** for definido como **Cascade** em uma extremidade de uma associação, os tipos de entidade relacionados na outra extremidade da Associação serão excluídos quando o tipo de entidade na primeira extremidade for excluído. Se a associação entre dois tipos de entidade for uma relação de chave primária de chave para primária, um objeto dependente carregado será excluído quando o objeto principal na outra extremidade da associação for excluído, independentemente da especificação **OnDelete** .  

> [!NOTE]
> O elemento **OnDelete** afeta apenas o comportamento de tempo de execução de um aplicativo; Ele não afeta o comportamento na fonte de dados. O comportamento definido na fonte de dados deve ser igual ao comportamento definido no aplicativo.

 

Um elemento **OnDelete** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **OnDelete** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ação**     | Sim         | **Cascade** ou **None**. Se os tipos de entidade **em cascata**, dependentes serão excluídos quando o tipo de entidade principal for excluído. Se **nenhum**, os tipos de entidade dependentes não serão excluídos quando o tipo de entidade principal for excluído. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Association** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Association** que define a associação **CustomerOrders** . O elemento **OnDelete** indica que todos os **pedidos** relacionados a um **cliente** específico e que foram carregados no ObjectContext serão excluídos quando o **cliente** for excluído.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Elemento Parameter (CSDL)

O elemento **Parameter** na linguagem de definição de esquema conceitual (CSDL) pode ser um filho do elemento FunctionImport ou do elemento Function.

### <a name="functionimport-element-application"></a>Aplicativo de elemento FunctionImport

Um elemento **Parameter** (como um filho do elemento **FunctionImport** ) é usado para definir parâmetros de entrada e saída para importações de função que são declaradas em CSDL.

O elemento **Parameter** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento permitido)
-   Elementos de anotação (zero ou mais elementos permitidos)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Parameter** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome do parâmetro.                                                                                                                                                                                                      |
| **Tipo**       | Sim         | O tipo de parâmetro. O valor deve ser um **EDMSimpleType** ou um tipo complexo que esteja dentro do escopo do modelo.                                                                                                             |
| **Modo**       | Não          | **In**, **out**ou **Inout** , dependendo se o parâmetro é um parâmetro de entrada, saída ou entrada/saída.                                                                                                                |
| **MaxLength**  | Não          | O comprimento máximo permitido do parâmetro.                                                                                                                                                                                    |
| **Precisão**  | Não          | A precisão do parâmetro.                                                                                                                                                                                                 |
| **Ajustar Escala**      | Não          | A escala do parâmetro.                                                                                                                                                                                                     |
| **SRID**       | Não          | Identificador de referência de sistema espacial. Válido somente para parâmetros de tipos espaciais. Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Parameter** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **FunctionImport** com um elemento filho de **parâmetro** . A função aceita um parâmetro de entrada e retorna uma coleção de tipos de entidade.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Aplicativo de elemento de função

Um elemento **Parameter** (como um filho do elemento **Function** ) define parâmetros para funções que são definidas ou declaradas em um modelo conceitual.

O elemento **Parameter** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   CollectionType (zero ou um elemento)
-   ReferenceType (zero ou um elemento)
-   RowType (zero ou um elemento)

> [!NOTE]
> Somente um dos elementos **CollectionType**, **ReferenceType**ou **RowType** pode ser um elemento filho de um elemento **Property** .

 

-   Elementos de anotação (zero ou mais elementos permitidos)

> [!NOTE]
> Os elementos de anotação devem aparecer após todos os outros elementos filho. Os elementos de anotação são permitidos somente no CSDL V2 e posterior.

 

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Parameter** .

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Sim         | O nome do parâmetro.                                                                                                                                                                                                      |
| **Tipo**         | Não          | O tipo de parâmetro. Um parâmetro pode ser qualquer um dos seguintes tipos (ou coleções desses tipos): <br/> **EdmSimpleType** <br/> tipo de entidade <br/> tipo complexo <br/> tipo de linha <br/> tipo de referência                             |
| **Anula**     | Não          | **True** (o valor padrão) ou **false** , dependendo se a propriedade pode ter um valor **nulo** .                                                                                                                          |
| **DefaultValue** | Não          | O valor padrão da propriedade.                                                                                                                                                                                              |
| **MaxLength**    | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                       |
| **Cadeia**  | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                          |
| **Precisão**    | Não          | A precisão do valor da propriedade.                                                                                                                                                                                            |
| **Ajustar Escala**        | Não          | A escala do valor da propriedade.                                                                                                                                                                                                |
| **SRID**         | Não          | Identificador de referência de sistema espacial. Válido somente para propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                               |
| **Agrupamento**    | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.                                                                                                                                                   |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Parameter** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Function** que usa um elemento filho de **parâmetro** para definir um parâmetro de função.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Elemento principal (CSDL)

O elemento **principal** na CSDL (linguagem de definição de esquema conceitual) é um elemento filho para o elemento ReferentialConstraint que define a extremidade principal de uma restrição referencial. Um elemento **ReferentialConstraint** define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional. Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados pode referenciar a chave primária de outra tabela, uma propriedade (ou Propriedades) de um tipo de entidade pode referenciar a chave de entidade de outro tipo de entidade. O tipo de entidade referenciado é chamado de *extremidade principal* da restrição. O tipo de entidade que faz referência à extremidade principal é chamado de *extremidade dependente* da restrição. Os elementos **PropertyRef** são usados para especificar quais chaves são referenciadas pela extremidade dependente.

O elemento **principal** pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais elementos)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **principal** .

| Nome do atributo | É obrigatório | Valor                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Função**       | Sim         | O nome do tipo de entidade na extremidade principal da associação. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **principal** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **ReferentialConstraint** que faz parte da definição da Associação **PublishedBy** . A propriedade **ID** do tipo de entidade **Editor** compõe a extremidade principal da restrição referencial.

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
 

 

## <a name="property-element-csdl"></a>Elemento Property (CSDL)

O elemento **Property** na linguagem de definição de esquema conceitual (CSDL) pode ser filho do elemento EntityType, do elemento complexType ou do elemento RowType.

### <a name="entitytype-and-complextype-element-applications"></a>Aplicativos de elemento EntityType e complexType

Os elementos de **Propriedade** (como filhos de **EntityType** ou elementos **complexType** ) definem a forma e as características dos dados que uma instância de tipo de entidade ou instância de tipo complexo conterá. As propriedades em um modelo conceitual são análogas às propriedades definidas em uma classe. Da mesma forma que as propriedades em uma classe definem a forma da classe e transportam informações sobre objetos, as propriedades em um modelo conceitual definem a forma de um tipo de entidade e transportam informações sobre as instâncias dos tipos de entidade.

O elemento **Property** pode ter os seguintes elementos filho (na ordem listada):

-   Elemento Documentation (zero ou um elementos permitidos)
-   Elementos de anotação (zero ou mais elementos permitidos)

As facetas a seguir podem ser aplicadas a um elemento de **Propriedade** : **Nullable**, **DefaultValue**, **MaxLength**, **cadeia**, **precisão**, **escala**, **Unicode**, **agrupamento**, **ConcurrencyMode**. Facetas são atributos XML que fornecem informações sobre como os valores de propriedade são armazenados no armazenamento de dados.

> [!NOTE]
> Facetas só podem ser aplicadas a propriedades do tipo **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Property** .

| Nome do atributo                                                         | É obrigatório | Valor                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                               | Sim         | O nome da propriedade.                                                                                                                                                                                                       |
| **Tipo**                                                               | Sim         | O tipo do valor da propriedade. O tipo de valor da propriedade deve ser um **EDMSimpleType** ou um tipo complexo (indicado por um nome totalmente qualificado) que esteja dentro do escopo do modelo.                                                 |
| **Anula**                                                           | Não          | **True** (o valor padrão) ou <strong>false</strong> , dependendo se a propriedade pode ter um valor nulo. <br/> [!NOTE]                                                                                                   |
| > No CSDL v1, uma propriedade de tipo complexo deve ter `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | Não          | O valor padrão da propriedade.                                                                                                                                                                                              |
| **MaxLength**                                                          | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                       |
| **Cadeia**                                                        | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                          |
| **Precisão**                                                          | Não          | A precisão do valor da propriedade.                                                                                                                                                                                            |
| **Ajustar Escala**                                                              | Não          | A escala do valor da propriedade.                                                                                                                                                                                                |
| **SRID**                                                               | Não          | Identificador de referência de sistema espacial. Válido somente para propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                               |
| **Agrupamento**                                                          | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | Não          | **Nenhum** (o valor padrão) ou **fixo**. Se o valor for definido como **Fixed**, o valor da propriedade será usado em verificações de simultaneidade otimistas.                                                                                  |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Property** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityType** com três elementos **Property** :

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
 

O exemplo a seguir mostra um elemento **complexType** com cinco elementos **Property** :

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>Aplicativo de elemento RowType

Elementos de **Propriedade** (como os filhos de um elemento **RowType** ) definem a forma e as características dos dados que podem ser passados para ou retornados de uma função definida pelo modelo.  

O elemento **Property** pode ter exatamente um dos seguintes elementos filho:

-   CollectionType
-   ReferenceType
-   RowType

O elemento **Property** pode ter qualquer elemento de anotação filho de número.

> [!NOTE]
> Os elementos de anotação são permitidos somente no CSDL V2 e posterior.

 

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Property** .

| Nome do atributo                                                     | É obrigatório | Valor                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                           | Sim         | O nome da propriedade.                                                                                                                                                                                                       |
| **Tipo**                                                           | Sim         | O tipo do valor da propriedade.                                                                                                                                                                                                 |
| **Anula**                                                       | Não          | **True** (o valor padrão) ou **false** , dependendo se a propriedade pode ter um valor nulo. <br/> [!NOTE]                                                                                                                |
| > Em CSDL v1, uma propriedade de tipo complexo deve ter `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Não          | O valor padrão da propriedade.                                                                                                                                                                                              |
| **MaxLength**                                                      | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                       |
| **Cadeia**                                                    | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                          |
| **Precisão**                                                      | Não          | A precisão do valor da propriedade.                                                                                                                                                                                            |
| **Ajustar Escala**                                                          | Não          | A escala do valor da propriedade.                                                                                                                                                                                                |
| **SRID**                                                           | Não          | Identificador de referência de sistema espacial. Válido somente para propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                               |
| **Agrupamento**                                                      | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.                                                                                                                                                   |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Property** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra os elementos de **Propriedade** usados para definir a forma do tipo de retorno de uma função definida pelo modelo.

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

O elemento **PropertyRef** na linguagem de definição de esquema conceitual (CSDL) faz referência a uma propriedade de um tipo de entidade para indicar que a propriedade executará uma das seguintes funções:

-   Parte da chave da entidade (uma propriedade ou um conjunto de propriedades de um tipo de entidade que determinam a identidade). Um ou mais elementos **PropertyRef** podem ser usados para definir uma chave de entidade.
-   A extremidade dependente ou principal de uma restrição referencial.

O elemento **PropertyRef** só pode ter elementos Annotation (zero ou mais) como elementos filho.

> [!NOTE]
> Os elementos de anotação são permitidos somente no CSDL V2 e posterior.

 

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **PropertyRef** .

| Nome do atributo | É obrigatório | Valor                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Sim         | O nome da propriedade referenciada. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **PropertyRef** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir define um tipo de entidade (**livro**). A chave de entidade é definida referenciando a propriedade **ISBN** do tipo de entidade.

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
 

No exemplo a seguir, dois elementos **PropertyRef** são usados para indicar que duas propriedades (**ID** e **PublisherID**) são as extremidades principal e dependente de uma restrição referencial.

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

O elemento **ReferenceType** na CSDL (linguagem de definição de esquema conceitual) especifica uma referência a um tipo de entidade. O elemento **ReferenceType** pode ser um filho dos seguintes elementos:

-   ReturnType (função)
-   Parâmetro
-   CollectionType

O elemento **ReferenceType** é usado ao definir um parâmetro ou tipo de retorno para uma função.

Um elemento **ReferenceType** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **ReferenceType** .

| Nome do atributo | É obrigatório | Valor                                         |
|:---------------|:------------|:----------------------------------------------|
| **Tipo**       | Sim         | O nome do tipo de entidade que está sendo referenciado. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReferenceType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra o elemento **ReferenceType** usado como um filho de um elemento **Parameter** em uma função definida pelo modelo que aceita uma referência a um tipo de entidade **Person** :

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
 

O exemplo a seguir mostra o elemento **ReferenceType** usado como um filho de um elemento **ReturnType** (Function) em uma função definida pelo modelo que retorna uma referência a um tipo de entidade **Person** :

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

Um elemento **ReferentialConstraint** na CSDL (linguagem de definição de esquema conceitual) define a funcionalidade que é semelhante a uma restrição de integridade referencial em um banco de dados relacional. Da mesma forma que uma coluna (ou colunas) de uma tabela de banco de dados pode referenciar a chave primária de outra tabela, uma propriedade (ou Propriedades) de um tipo de entidade pode referenciar a chave de entidade de outro tipo de entidade. O tipo de entidade referenciado é chamado de *extremidade principal* da restrição. O tipo de entidade que faz referência à extremidade principal é chamado de *extremidade dependente* da restrição.

Se uma chave estrangeira exposta em um tipo de entidade referenciar uma propriedade em outro tipo de entidade, o elemento **ReferentialConstraint** definirá uma associação entre os dois tipos de entidade. Como o elemento **ReferentialConstraint** fornece informações sobre como dois tipos de entidade estão relacionados, nenhum elemento AssociationSetMapping correspondente é necessário na MSL (linguagem de especificação de mapeamento). Uma associação entre dois tipos de entidade que não têm chaves estrangeiras expostas deve ter um elemento **AssociationSetMapping** correspondente para mapear informações de associação para a fonte de dados.

Se uma chave estrangeira não for exposta em um tipo de entidade, o elemento **ReferentialConstraint** só poderá definir uma restrição PRIMARY KEY para Primary Key entre o tipo de entidade e outro tipo de entidade.

Um elemento **ReferentialConstraint** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Principal (exatamente um elemento)
-   Dependente (exatamente um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

O elemento **ReferentialConstraint** pode ter qualquer número de atributos de anotação (atributos XML personalizados). No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **ReferentialConstraint** que está sendo usado como parte da definição da Associação **PublishedBy** .

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
 

 

## <a name="returntype-function-element-csdl"></a>Elemento ReturnType (Function) (CSDL)

O elemento **ReturnType** (Function) na linguagem de definição de esquema conceitual (CSDL) especifica o tipo de retorno para uma função que é definida em um elemento Function. Um tipo de retorno de função também pode ser especificado com um atributo **ReturnType** .

Os tipos de retorno podem ser qualquer **EdmSimpleType**, tipo de entidade, tipo complexo, tipo de linha, tipo de referência ou uma coleção de um desses tipos.

O tipo de retorno de uma função pode ser especificado com o atributo **Type** do elemento **ReturnType** (Function) ou com um dos seguintes elementos filho:

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> Um modelo não será validado se você especificar um tipo de retorno de função com o atributo **Type** do elemento **ReturnType** (função) e um dos elementos filho.

 

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **ReturnType** (função).

| Nome do atributo | É obrigatório | Valor                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | Não          | O tipo retornado pela função. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReturnType** (função). No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir usa um elemento **Function** para definir uma função que retorna o número de anos em que um livro foi impresso. Observe que o tipo de retorno é especificado pelo atributo **Type** de um elemento **ReturnType** (Function).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>Elemento ReturnType (FunctionImport) (CSDL)

O elemento **ReturnType** (FunctionImport) na linguagem de definição de esquema conceitual (CSDL) especifica o tipo de retorno para uma função que é definida em um elemento FunctionImport. Um tipo de retorno de função também pode ser especificado com um atributo **ReturnType** .

Tipos de retorno podem ser qualquer coleção de tipo de entidade, tipo complexo ou **EdmSimpleType**,

O tipo de retorno de uma função é especificado com o atributo **Type** do elemento **ReturnType** (FunctionImport).

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **ReturnType** (FunctionImport).

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**       | Não          | O tipo que a função retorna. O valor deve ser uma coleção de complexType, EntityType ou EDMSimpleType.                                                                                      |
| **EntitySet**  | Não          | Se a função retornar uma coleção de tipos de entidade, o valor do **EntitySet** deverá ser o conjunto de entidades ao qual a coleção pertence. Caso contrário, o atributo **EntitySet** não deve ser usado. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReturnType** (FunctionImport). No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir usa uma **FunctionImport** que retorna livros e publicadores. Observe que a função retorna dois conjuntos de resultados e, portanto, dois elementos **ReturnType** (FunctionImport) são especificados.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>Elemento RowType (CSDL)

Um elemento **RowType** na CSDL (linguagem de definição de esquema conceitual) define uma estrutura sem nome como um parâmetro ou tipo de retorno para uma função definida no modelo conceitual.

Um elemento **RowType** pode ser o filho dos seguintes elementos:

-   CollectionType
-   Parâmetro
-   ReturnType (função)

Um elemento **RowType** pode ter os seguintes elementos filho (na ordem listada):

-   Propriedade (um ou mais)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **RowType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função definida pelo modelo que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas (conforme especificado no elemento **RowType** ).

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

## <a name="schema-element-csdl"></a>Elemento Schema (CSDL)

O elemento **Schema** é o elemento raiz de uma definição de modelo conceitual. Ele contém definições para os objetos, funções e contêineres que compõem um modelo conceitual.

O elemento **Schema** pode conter zero ou mais dos seguintes elementos filho:

-   Usando
-   EntityContainer
-   EntityType
-   EnumType
-   Associação
-   ComplexType
-   Função

Um elemento de **esquema** pode conter zero ou um dos elementos de anotação.

> [!NOTE]
> Os elementos de **função** e de anotação são permitidos somente no CSDL V2 e posterior.

 

O elemento **Schema** usa o atributo **namespace** para definir o namespace para o tipo de entidade, tipo complexo e objetos de associação em um modelo conceitual. Em um namespace, dois objetos não podem ter o mesmo nome. Os namespaces podem abranger vários elementos de **esquema** e vários arquivos. CSDL.

Um namespace de modelo conceitual é diferente do namespace XML do elemento **Schema** . Um namespace de modelo conceitual (conforme definido pelo atributo **namespace** ) é um contêiner lógico para tipos de entidade, tipos complexos e tipos de associação. O namespace XML (indicado pelo atributo **xmlns** ) de um elemento **Schema** é o namespace padrão para elementos filho e atributos do elemento **Schema** . Os namespaces XML do formulário https://schemas.microsoft.com/ado/YYYY/MM/edm (em que AAAA e MM representam um ano e um mês respectivamente) são reservados para CSDL. Atributos e elementos personalizados não podem estar em namespaces que tenham esse formulário.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Schema** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Sim         | O namespace do modelo conceitual. O valor do atributo **namespace** é usado para formar o nome totalmente qualificado de um tipo. Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace simples. example. Model, o nome totalmente qualificado do **EntityType** será SimpleExampleModel. Customer. <br/> As seguintes cadeias de caracteres não podem ser usadas como o valor do atributo **namespace** : **Sistema**, **transitório**ou **EDM**. O valor do atributo **namespace** não pode ser o mesmo que o valor do atributo **namespace** no elemento de esquema SSDL. |
| **Alias**      | Não          | Um identificador usado no lugar do nome do namespace. Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace simples. example. Model e o valor do atributo **alias** for *Model*, você poderá usar Model. Customer como o nome totalmente qualificado do **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **esquema** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento de **esquema** que contém um elemento **EntityContainer** , dois elementos **EntityType** e um elemento **Association** .

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
 

 

## <a name="typeref-element-csdl"></a>Elemento TypeRef (CSDL)

O elemento **TypeRef** na linguagem de definição de esquema conceitual (CSDL) fornece uma referência a um tipo nomeado existente. O elemento **TypeRef** pode ser um filho do elemento CollectionType, que é usado para especificar que uma função tem uma coleção como um tipo de parâmetro ou de retorno.

Um elemento **TypeRef** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **TypeRef** . Observe que os atributos **DefaultValue**, **MaxLength**, **cadeia**, **Precision**, **Scale**, **Unicode**e **Collation** são aplicáveis somente ao **EDMSimpleTypes**.

| Nome do atributo                                                     | É obrigatório | Valor                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**                                                           | Não          | O nome do tipo que está sendo referenciado.                                                                                                                                                                                          |
| **Anula**                                                       | Não          | **True** (o valor padrão) ou **false** , dependendo se a propriedade pode ter um valor nulo. <br/> [!NOTE]                                                                                                                |
| > Em CSDL v1, uma propriedade de tipo complexo deve ter `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Não          | O valor padrão da propriedade.                                                                                                                                                                                              |
| **MaxLength**                                                      | Não          | O comprimento máximo do valor da propriedade.                                                                                                                                                                                       |
| **Cadeia**                                                    | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                                          |
| **Precisão**                                                      | Não          | A precisão do valor da propriedade.                                                                                                                                                                                            |
| **Ajustar Escala**                                                          | Não          | A escala do valor da propriedade.                                                                                                                                                                                                |
| **SRID**                                                           | Não          | Identificador de referência de sistema espacial. Válido somente para propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Não          | **True** ou **false** dependendo se o valor da propriedade será armazenado como uma cadeia de caracteres Unicode.                                                                                                                               |
| **Agrupamento**                                                      | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.                                                                                                                                                   |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **CollectionType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função definida pelo modelo que usa o elemento **TypeRef** (como um filho de um elemento **CollectionType** ) para especificar que a função aceita uma coleção de tipos de entidade **Department** .

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
 

 

## <a name="using-element-csdl"></a>Elemento using (CSDL)

O elemento **using** na linguagem de definição de esquema conceitual (CSDL) importa o conteúdo de um modelo conceitual que existe em um namespace diferente. Ao definir o valor do atributo **namespace** , você pode se referir a tipos de entidade, tipos complexos e tipos de associação que são definidos em outro modelo conceitual. Mais de um elemento **using** pode ser um filho de um elemento **Schema** .

> [!NOTE]
> O elemento **using** em CSDL não funciona exatamente como uma instrução **using** em uma linguagem de programação. Ao importar um namespace com uma instrução **using** em uma linguagem de programação, você não afeta os objetos no namespace original. No CSDL, um namespace importado pode conter um tipo de entidade derivado de um tipo de entidade no namespace original. Isso pode afetar os conjuntos de entidades declarados no namespace original.

 

O elemento **using** pode ter os seguintes elementos filho:

-   Documentação (zero ou um elemento permitido)
-   Elementos de anotação (zero ou mais elementos permitidos)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **using** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Sim         | O nome do namespace importado.                                                                                                                                                |
| **Alias**      | Sim         | Um identificador usado no lugar do nome do namespace. Embora esse atributo seja necessário, não é necessário que ele seja usado no lugar do nome do namespace para qualificar nomes de objeto. |

 

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **using** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

 

### <a name="example"></a>Exemplo

O exemplo a seguir demonstra o elemento **using** que está sendo usado para importar um namespace que é definido em outro lugar. Observe que o namespace para o elemento de **esquema** mostrado é `BooksModel`. A propriedade `Address` no `Publisher`**EntityType** é um tipo complexo que é definido no namespace `ExtendedBooksModel` (importado com o elemento **using** ).

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
 

 

## <a name="annotation-attributes-csdl"></a>Atributos de anotação (CSDL)

Os atributos de anotação em CSDL (linguagem de definição de esquema conceitual) são atributos XML personalizados no modelo conceitual. Além de ter uma estrutura XML válida, o seguinte deve ser verdadeiro dos atributos de anotação:

-   Os atributos de anotação não devem estar em nenhum namespace XML reservado para CSDL.
-   Mais de um atributo Annotation pode ser aplicado a um determinado elemento CSDL.
-   Os nomes totalmente qualificados de quaisquer dois atributos de anotação não devem ser iguais.

Os atributos de anotação podem ser usados para fornecer metadados extras sobre os elementos em um modelo conceitual. Os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityType** com um atributo de anotação (**CustomAttribute**). O exemplo também mostra um elemento Annotation aplicado ao elemento tipo de entidade.

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
 

O código a seguir recupera os metadados no atributo Annotation e grava-os no console:

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
 

O código acima pressupõe que o arquivo `School.csdl` está no diretório de saída do projeto e que você adicionou as seguintes instruções `Imports` e `Using` ao seu projeto:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Elementos de anotação (CSDL)

Os elementos Annotation na linguagem de definição de esquema conceitual (CSDL) são elementos XML personalizados no modelo conceitual. Além de ter uma estrutura XML válida, o seguinte deve ser verdadeiro para elementos de anotação:

-   Os elementos de anotação não devem estar em nenhum namespace XML reservado para CSDL.
-   Mais de um elemento Annotation pode ser um filho de um determinado elemento CSDL.
-   Os nomes totalmente qualificados de quaisquer dois elementos de anotação não devem ser iguais.
-   Os elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento CSDL.

Os elementos de anotação podem ser usados para fornecer metadados extras sobre os elementos em um modelo conceitual. A partir do .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityType** com um elemento Annotation (**customelement**). O exemplo também mostra um atributo Annotation aplicado ao elemento tipo de entidade.

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
 

O código a seguir recupera os metadados no elemento Annotation e grava-os no console:

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
 

O código acima pressupõe que o arquivo School. CSDL esteja no diretório de saída do projeto e que você tenha adicionado as instruções `Imports` e `Using` a seguir ao seu projeto:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>CSDL (tipos de modelo conceitual)

O CSDL (linguagem de definição de esquema conceitual) dá suporte a um conjunto de tipos de dados primitivos abstratos, chamados **EDMSimpleTypes**, que definem propriedades em um modelo conceitual. **EDMSimpleTypes** são proxies para tipos de dados primitivos que têm suporte no ambiente de armazenamento ou de hospedagem.

A tabela a seguir lista os tipos de dados primitivos que são suportados pelo CSDL. A tabela também lista as facetas que podem ser aplicadas a cada **EDMSimpleType**.

| EDMSimpleType                    | Descrição                                                | Facetas aplicáveis                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **EDM. Binary**                   | Contém dados binários.                                      | MaxLength, FixedLength, anulável, opção                                |
| **EDM. booliano**                  | Contém o valor **true** ou **false**.                  | Anulável, opção                                                        |
| **EDM. byte**                     | Contém um valor inteiro de 8 bits sem sinal.                  | Precisão, anulável, opção                                             |
| **EDM. DateTime**                 | Representa uma data e hora.                                | Precisão, anulável, opção                                             |
| **EDM. DateTimeOffset**           | Contém uma data e hora como um deslocamento em minutos GMT. | Precisão, anulável, opção                                             |
| **EDM. decimal**                  | Contém um valor numérico com precisão e escala fixa.   | Precisão, anulável, opção                                             |
| **EDM. Double**                   | Contém um número de ponto flutuante com precisão de 15 dígitos   | Precisão, anulável, opção                                             |
| **EDM. float**                    | Contém um número de ponto flutuante com precisão de 7 dígitos.   | Precisão, anulável, opção                                             |
| **EDM. GUID**                     | Contém um identificador exclusivo de 16 bytes.                      | Precisão, anulável, opção                                             |
| **EDM. Int16**                    | Contém um valor inteiro de 16 bits assinado.                    | Precisão, anulável, opção                                             |
| **EDM. Int32**                    | Contém um valor inteiro de 32 bits assinado.                    | Precisão, anulável, opção                                             |
| **EDM. Int64**                    | Contém um valor inteiro de 64 bits assinado.                    | Precisão, anulável, opção                                             |
| **EDM. SByte**                    | Contém um valor inteiro de 8 bits com sinal.                     | Precisão, anulável, opção                                             |
| **EDM. String**                   | Contém dados de caractere.                                   | Unicode, FixedLength, MaxLength, ordenação, precisão, anulável, opção |
| **EDM. time**                     | Contém uma hora.                                    | Precisão, anulável, opção                                             |
| **EDM. geography**                |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. GeographyPoint**           |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. GeographyLineString**      |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geographyPolygon que**         |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geographyMultiPoint que**      |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. GeographyMultiLineString** |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geographyMultiPolygon que**    |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geographcollection**      |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. Geometry**                 |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geometryPoint que**            |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geometryLineString que**       |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geometryPolygon que**          |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geometryMultiPoint que**       |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. GeometryMultiLineString**  |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. geometryMultiPolygon que**     |                                                            | Permite valor nulo, padrão, SRID                                                  |
| **EDM. GeometryCollection**       |                                                            | Permite valor nulo, padrão, SRID                                                  |

## <a name="facets-csdl"></a>Facetas (CSDL)

Facetas em CSDL (linguagem de definição de esquema conceitual) representam restrições em Propriedades de tipos de entidade e tipos complexos. As facetas aparecem como atributos XML nos seguintes elementos CSDL:

-   Propriedade
-   TypeRef
-   Parâmetro

A tabela a seguir descreve as facetas com suporte em CSDL. Todas as facetas são opcionais. Algumas facetas listadas abaixo são usadas pelo Entity Framework ao gerar um banco de dados de um modelo conceitual.

> [!NOTE]
> Para obter informações sobre tipos de dados em um modelo conceitual, consulte tipos de modelo conceituais (CSDL).

| Aspecto               | Descrição                                                                                                                                                                                                                                                   | Aplica-se a                                                                                                                                                                                                                                                                                                                                                                           | Usado para a geração de banco de dados | Usado pelo tempo de execução |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Agrupamento**       | Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.                                                                                                               | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Sim                              | Não                  |
| **ConcurrencyMode** | Indica que o valor da propriedade deve ser usado para verificação de simultaneidade otimista.                                                                                                                                                                    | Todas as propriedades **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Não                               | Sim                 |
| **Padrão**         | Especifica o valor de propriedade padrão se nenhum valor é fornecido em cima de instanciação.                                                                                                                                                                       | Todas as propriedades **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Sim                              | Sim                 |
| **Cadeia**     | Especifica se o comprimento do valor da propriedade pode variar.                                                                                                                                                                                                  | **EDM. Binary**, **EDM. String**                                                                                                                                                                                                                                                                                                                                                       | Sim                              | Não                  |
| **MaxLength**       | Especifica o comprimento máximo de valor de propriedade.                                                                                                                                                                                                           | **EDM. Binary**, **EDM. String**                                                                                                                                                                                                                                                                                                                                                       | Sim                              | Não                  |
| **Anula**        | Especifica se a propriedade pode ter um valor **nulo** .                                                                                                                                                                                                     | Todas as propriedades **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Sim                              | Sim                 |
| **Precisão**       | Para propriedades do tipo **decimal**, especifica o número de dígitos que um valor de propriedade pode ter. Para propriedades do tipo **time**, **DateTime**e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da propriedade. | **EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. decimal**, **EDM. time**                                                                                                                                                                                                                                                                                                              | Sim                              | Não                  |
| **Ajustar Escala**           | Especifica o número de dígitos à direita do ponto decimal para o valor da propriedade.                                                                                                                                                                      | **EDM. decimal**                                                                                                                                                                                                                                                                                                                                                                      | Sim                              | Não                  |
| **SRID**            | Especifica a ID do sistema de referência espacial do sistema. Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **EDM. geography, EDM. GeographyPoint, EDM. GeographyLineString, EDM. geographyPolygon que, EDM. geographyMultiPoint que, EDM. GeographyMultiLineString, EDM. geographyMultiPolygon que, EDM. geographcollection, EDM. Geometry, EDM. geometryPoint que, EDM. geometryLineString que, EDM. geometryPolygon que, EDM. geometryMultiPoint que, EDM. GeometryMultiLineString, EDM. geometryMultiPolygon que, EDM. GeometryCollection** | Não                               | Sim                 |
| **Unicode**         | Indica se o valor da propriedade é armazenado como Unicode.                                                                                                                                                                                                    | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Sim                              | Sim                 |

>[!NOTE]
> Ao gerar um banco de dados de um modelo conceitual, o assistente para gerar banco de dados reconhecerá o valor do atributo **StoreGeneratedPattern** em um elemento de **Propriedade** se ele estiver no seguinte namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation. Os valores com suporte para o atributo são **identidade** e **computados**. Um valor de **Identity** produzirá uma coluna de banco de dados com um valor de identidade que é gerado no banco de dados. Um valor de **computado** produzirá uma coluna com um valor que é computado no banco de dados.

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
