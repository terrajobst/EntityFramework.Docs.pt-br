---
title: Especificação de SSDL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: 35c560d88e5078a7fc4c07b76020f3ad7d0735e1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995273"
---
# <a name="ssdl-specification"></a>Especificação de SSDL
Linguagem de definição de esquema de Store (SSDL) é uma linguagem baseada em XML que descreve o modelo de armazenamento de um aplicativo do Entity Framework.

Em um aplicativo do Entity Framework, os metadados de modelo de armazenamento é carregado de um arquivo. SSDL (escrito em SSDL) em uma instância da System.Data.Metadata.Edm.StoreItemCollection em são acessados por meio de métodos no Classe System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework usa metadados do modelo de armazenamento para converter consultas no modelo conceitual para comandos específicos do repositório.

O Entity Framework Designer (Designer de EF) armazena informações de modelo de armazenamento em um arquivo. edmx em tempo de design. No momento da compilação o Entity Designer usa as informações em um arquivo. edmx para criar o arquivo. SSDL que é necessário pelo Entity Framework em tempo de execução.

Versões de SSDL são diferenciadas por namespaces XML.

| Versão SSDL | Namespace XML                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Elemento de associação (SSDL)

Uma **associação** elemento na linguagem de definição de esquema de armazenamento (SSDL) especifica colunas que participam da tabela em uma restrição de chave estrangeira no banco de dados subjacente. Dois elementos de final de filho obrigatório especificam tabelas nas extremidades da associação e a multiplicidade em cada extremidade. Um elemento ReferentialConstraint opcional especifica as entidade e dependentes extremidades de associação, bem como as colunas participantes. Se nenhum **ReferentialConstraint** elemento estiver presente, um elemento AssociationSetMapping deve ser usado para especificar os mapeamentos de coluna para a associação.

O **associação** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   End (exatamente dois)
-   ReferentialConstraint (zero ou um)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **associação** elemento.

| Nome do atributo | É obrigatório | Valor                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome da restrição de chave estrangeira correspondente no banco de dados subjacente. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **associação** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **associação** elemento que usa um **ReferentialConstraint** elemento para especificar as colunas que participam do **FK\_CustomerOrders**  restrição de chave estrangeira:

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

## <a name="associationset-element-ssdl"></a>Elemento AssociationSet (SSDL)

O **AssociationSet** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma restrição de chave estrangeira entre duas tabelas no banco de dados subjacente. As colunas da tabela que participam da restrição de chave estrangeira são especificadas em um elemento de associação. O **associação** elemento que corresponde a um determinado **AssociationSet** especificado no elemento a **associação** atributo do **AssociationSet**  elemento.

Conjuntos de associações de SSDL são mapeados para conjuntos de associações de CSDL por um elemento AssociationSetMapping. No entanto, se definir a associação de CSDL para um determinado association da CSDL é definida por meio de um elemento ReferentialConstraint, nenhum correspondente **AssociationSetMapping** elemento é necessário. Nesse caso, se um **AssociationSetMapping** elemento estiver presente, ele define os mapeamentos de serão substituídos pela **ReferentialConstraint** elemento.

O **AssociationSet** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   End (zero ou dois)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **AssociationSet** elemento.

| Nome do atributo  | É obrigatório | Valor                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Nome**        | Sim         | O nome da restrição de chave estrangeira que a associação definida representa.                          |
| **Associação** | Sim         | O nome da associação que define as colunas que participam na restrição de chave estrangeira. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **AssociationSet** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **AssociationSet** elemento que representa o `FK_CustomerOrders` restrição de chave estrangeira no banco de dados subjacente:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>Elemento CollectionType (SSDL)

O **CollectionType** elemento na linguagem de definição de esquema de armazenamento (SSDL) Especifica que o tipo de retorno da função é uma coleção. O **CollectionType** elemento é um filho do elemento ReturnType. O tipo de coleção é especificado usando o elemento RowType filho:

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **CollectionType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de linhas.

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

## <a name="commandtext-element-ssdl"></a>Elemento CommandText (SSDL)

O **CommandText** elemento na linguagem de definição de esquema de armazenamento (SSDL) é um filho do elemento de função que permite que você defina uma instrução SQL que é executada no banco de dados. O **CommandText** elemento permite que você adicione funcionalidade semelhante a um procedimento armazenado no banco de dados, mas definir a **CommandText** elemento no modelo de armazenamento.

O **CommandText** elemento não pode ter elementos filho. O corpo dos **CommandText** elemento deve ser uma instrução SQL válida para o banco de dados subjacente.

Atributos não são aplicáveis para o **CommandText** elemento.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **função** elemento com um filho **CommandText** elemento. Expor a **UpdateProductInOrder** funcionará como um método no ObjectContext, importá-los para o modelo conceitual.  

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

## <a name="definingquery-element-ssdl"></a>Elemento DefiningQuery (SSDL)

O **DefiningQuery** elemento na linguagem de definição de esquema de armazenamento (SSDL) permite que você execute uma instrução SQL diretamente no banco de dados subjacente. O **DefiningQuery** elemento normalmente é usado como um modo de exibição de banco de dados, mas o modo de exibição é definido no modelo de armazenamento, em vez do banco de dados. O modo de exibição definido em uma **DefiningQuery** elemento pode ser mapeado para um tipo de entidade no modelo conceitual por meio de um elemento EntitySetMapping. Esses mapeamentos são somente leitura.  

A sintaxe SSDL a seguir mostra a declaração de um **EntitySet** seguido de **DefiningQuery** elemento que contém uma consulta usada para recuperar o modo de exibição.

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

Você pode usar procedimentos armazenados no Entity Framework para habilitar cenários de leitura / gravação sobre modos de exibição. Você pode usar uma exibição da fonte de dados ou uma exibição de Entity SQL que a tabela base para recuperação de dados e para processamento por procedimentos armazenados de alterações.

Você pode usar o **DefiningQuery** elemento para o Microsoft SQL Server Compact 3.5 de destino. Embora o SQL Server Compact 3.5 não oferece suporte a procedimentos armazenados, você pode implementar uma funcionalidade semelhante a **DefiningQuery** elemento. É outro lugar onde ele pode ser útil na criação de procedimentos armazenados para superar uma incompatibilidade entre os tipos de dados usados na linguagem de programação e aqueles da fonte de dados. Você poderia escrever um **DefiningQuery** que leva a um determinado conjunto de parâmetros e, em seguida, chama um procedimento armazenado com um conjunto diferente de parâmetros, por exemplo, um procedimento armazenado que exclui dados.

## <a name="dependent-element-ssdl"></a>Elemento dependente (SSDL)

O **dependentes** na linguagem de definição de esquema de armazenamento (SSDL) é um elemento filho ao elemento ReferentialConstraint que define o final dependente de uma restrição de chave estrangeira (também chamado de uma restrição referencial). O **dependentes** elemento Especifica a coluna (ou colunas) em uma tabela que fazem referência a uma coluna de chave primária (ou colunas). **PropertyRef** elementos especificam quais colunas são referenciadas. O Principal elemento Especifica as colunas de chave primária referenciadas pelas colunas especificadas na **dependentes** elemento.

O **dependentes** elemento pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **dependentes** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Função**       | Sim         | O mesmo valor que o **função** (se usado) de atributo do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna de referência. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **dependentes** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento de associação que usa uma **ReferentialConstraint** elemento para especificar as colunas que participam do **FK\_CustomerOrders** chave estrangeira restrição. O **dependentes** elemento Especifica a **CustomerId** coluna do **ordem** tabela como o final dependente da restrição.

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

## <a name="documentation-element-ssdl"></a>Elemento Documentation (SSDL)

O **documentação** elemento na linguagem de definição de esquema de armazenamento (SSDL) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai.

O **documentação** elemento pode ter os seguintes elementos filho (na ordem listada):

-   **Resumo**: uma breve descrição do elemento pai. (zero ou um elemento)
-   **LongDescription**: uma descrição abrangente do elemento pai. (zero ou um elemento)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **documentação** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra a **documentação** como um elemento filho de um elemento EntityType.

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

## <a name="end-element-ssdl"></a>Elemento final (SSDL)

O **final** elemento na linguagem de definição de esquema de armazenamento (SSDL) Especifica a tabela e o número de linhas em uma extremidade de uma restrição de chave estrangeira no banco de dados subjacente. O **final** elemento pode ser um filho do elemento de associação ou o elemento AssociationSet. Em cada caso, os possíveis elementos filho e atributos aplicáveis são diferentes.

### <a name="end-element-as-a-child-of-the-association-element"></a>Elemento final como um filho do elemento de associação

Uma **final** elemento (como um filho a **associação** elemento) Especifica a tabela e o número de linhas no final de uma restrição de chave estrangeira com a **tipo** e **Multiplicidade** atributos, respectivamente. Extremidades de uma restrição de chave estrangeira são definidas como parte de uma associação de SSDL; uma associação de SSDL deve ter exatamente duas extremidades.

Uma **final** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   OnDelete (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **final** elemento quando ele for o filho de um **associação** elemento.

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**         | Sim         | O nome totalmente qualificado do conjunto de entidades SSDL é no final da restrição de chave estrangeira.                                                                                                                                                                                                                                                                                          |
| **Função**         | Não          | O valor de **função** atributo no elemento Principal ou dependente do elemento ReferentialConstraint correspondente (se usado).                                                                                                                                                                                                                                             |
| **Multiplicidade** | Sim         | **1**, **entre 0 e 1**, ou **\*** dependendo do número de linhas que podem estar no final da restrição de chave estrangeira. <br/> **1** indica que exatamente uma linha existe no final de restrição de chave estrangeira. <br/> **entre 0 e 1** indica que existe de zero ou uma linha no final de restrição de chave estrangeira. <br/> **\*** indica que zero, um ou mais linhas existem no final de restrição de chave estrangeira. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **final** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

#### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **associação** elemento que define o **FK\_CustomerOrders** restrição de chave estrangeira. O **multiplicidade** valores especificados em cada **final** elemento indicam que muitas linhas no **pedidos** tabela pode ser associada uma linha no **clientes**  tabela, mas apenas uma linha em de **clientes** tabela pode ser associada uma linha no **pedidos** tabela. Além disso, o **OnDelete** elemento indica que todas as linhas a **pedidos** tabelas que fazem referência a uma linha específica no **clientes** tabela será excluída se a linha na o **clientes** tabela for excluída.

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Elemento final como um filho do elemento AssociationSet

O **final** elemento (como um filho de **AssociationSet** elemento) especifica uma tabela em uma extremidade de uma restrição de chave estrangeira no banco de dados subjacente.

Uma **final** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Elementos de anotação (zero ou mais)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **final** elemento quando ele for o filho de um **AssociationSet** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Sim         | O nome do conjunto de entidades SSDL é no final da restrição de chave estrangeira.                                      |
| **Função**       | Não          | O valor de um dos **função** atributos especificados em um **final** elemento do elemento de associação correspondente. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **final** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

#### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityContainer** elemento com um **AssociationSet** elemento com dois **final** elementos:

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

## <a name="entitycontainer-element-ssdl"></a>Elemento EntityContainer (SSDL)

Uma **EntityContainer** elemento na linguagem de definição de esquema de armazenamento (SSDL) descreve a estrutura da fonte de dados subjacente em um aplicativo do Entity Framework: conjuntos de entidades SSDL (definidos em elementos EntitySet) representam tabelas um banco de dados, tipos de entidade SSDL (definidos em elementos EntityType) representam linhas em uma tabela e conjuntos de associações (definidos em elementos de AssociationSet) representam as restrições de chave estrangeira em um banco de dados. Um contêiner de entidade do modelo de armazenamento é mapeado para um contêiner de entidade do modelo conceitual através de elemento EntityContainerMapping.

Uma **EntityContainer** elemento pode ter zero ou mais elementos de documentação. Se um **documentação** elemento estiver presente, ela deve preceder todos os outros elementos filho.

Uma **EntityContainer** elemento pode ter zero ou mais dos seguintes elementos filho (na ordem listada):

-   EntitySet
-   AssociationSet
-   Elementos de anotação

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityContainer** elemento.

| Nome do atributo | É obrigatório | Valor                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome do contêiner de entidade. Esse nome não pode conter pontos (.). |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntityContainer** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityContainer** elemento que define dois conjuntos de entidades e um conjunto de associações. Observe que os nomes de tipo de associação e o tipo de entidade são qualificados pelo nome de namespace do modelo conceitual.

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

## <a name="entityset-element-ssdl"></a>Elemento EntitySet (SSDL)

Uma **EntitySet** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma tabela ou exibição no banco de dados subjacente. Um elemento EntityType em SSDL representa uma linha na tabela ou exibição. O **EntityType** atributo de uma **EntitySet** elemento Especifica o tipo de entidade específico SSDL que representa as linhas em um conjunto de entidades SSDL. O mapeamento entre um conjunto de entidades do CSDL e um conjunto de entidades SSDL é especificado em um elemento EntitySetMapping.

O **EntitySet** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   DefiningQuery (zero ou um elemento)
-   Elementos de anotação

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EntitySet** elemento.

> [!NOTE]
> Alguns atributos (não listados aqui) podem ser qualificados com o **armazenar** alias. Esses atributos são usados pelo Assistente de modelo de atualização ao atualizar um modelo.

| Nome do atributo | É obrigatório | Valor                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome do conjunto de entidades.                                                              |
| **EntityType** | Sim         | O nome totalmente qualificado do tipo de entidade para a qual o conjunto de entidades contém instâncias. |
| **Esquema**     | Não          | O esquema de banco de dados.                                                                     |
| **Tabela**      | Não          | A tabela de banco de dados.                                                                      |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntitySet** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityContainer** elemento que tem dois **EntitySet** elementos e um **AssociationSet** elemento:

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

## <a name="entitytype-element-ssdl"></a>Elemento EntityType (SSDL)

Uma **EntityType** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma linha em uma tabela ou exibição de banco de dados subjacente. Um elemento de EntitySet em SSDL representa a tabela ou exibição na qual as linhas ocorrerem. O **EntityType** atributo de uma **EntitySet** elemento Especifica o tipo de entidade específico SSDL que representa as linhas em um conjunto de entidades SSDL. O mapeamento entre um tipo de entidade SSDL e um tipo de entidade CSDL é especificado em um elemento EntityTypeMapping.

O **EntityType** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Chave (zero ou um elemento)
-   Elementos de anotação

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **EntityType** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome do tipo de entidade. Esse valor geralmente é igual ao nome da tabela na qual o tipo de entidade representa uma linha. Esse valor não pode conter nenhum pontos (.). |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **EntityType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityType** elemento com duas propriedades:

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

## <a name="function-element-ssdl"></a>Elemento Function (SSDL)

O **função** elemento na linguagem de definição de esquema de armazenamento (SSDL) especifica um procedimento armazenado que existe no banco de dados subjacente.

O **função** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Parâmetro (zero ou mais)
-   CommandText (zero ou um)
-   ReturnType (zero ou mais)
-   Elementos de anotação (zero ou mais)

Retornar de um tipo para uma função deve ser especificado com qualquer um de **ReturnType** elemento ou o **ReturnType** atributo (veja abaixo), mas não ambos.

Os procedimentos armazenados que são especificados no modelo de armazenamento podem ser importados para o modelo conceitual de um aplicativo. Para obter mais informações, consulte [consultando com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/query.md). O **função** elemento também pode ser usado para definir funções personalizadas no modelo de armazenamento.  

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **função** elemento.

> [!NOTE]
> Alguns atributos (não listados aqui) podem ser qualificados com o **armazenar** alias. Esses atributos são usados pelo Assistente de modelo de atualização ao atualizar um modelo.

| Nome do atributo             | É obrigatório | Valor                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**                   | Sim         | O nome do procedimento armazenado.                                                                                                                                                                                  |
| **ReturnType**             | Não          | O tipo de retorno do procedimento armazenado.                                                                                                                                                                           |
| **Aggregate**              | Não          | **Verdadeiro** se o procedimento armazenado retorna um valor de agregação; caso contrário **falso**.                                                                                                                                  |
| **Interno**                | Não          | **Verdadeiro** se a função for uma interna<sup>1</sup> função; caso contrário **False**.                                                                                                                                  |
| **StoreFunctionName**      | Não          | O nome do procedimento armazenado.                                                                                                                                                                                  |
| **NiladicFunction**        | Não          | **Verdadeiro** se a função for um niladic<sup>2</sup> função; **Falsos** caso contrário.                                                                                                                                   |
| **IsComposable**           | Não          | **Verdadeiro** se a função for um Combinável<sup>3</sup> função; **Falsos** caso contrário.                                                                                                                                |
| **ParameterTypeSemantics** | Não          | A enumeração que define a semântica de tipo usada para resolver as sobrecargas de função. A enumeração é definida no manifesto do provedor por definição de função. O valor padrão é **AllowImplicitConversion**. |
| **Esquema**                 | Não          | O nome do esquema no qual o procedimento armazenado é definido.                                                                                                                                                   |

<sup>1</sup> uma função interna é uma função que é definida no banco de dados. Para obter informações sobre as funções que são definidos no modelo de armazenamento, consulte o elemento CommandText (SSDL).

<sup>2</sup> uma função niladic é uma função que não aceita parâmetros e, quando chamado, não requer parênteses.

<sup>3</sup> duas funções são combináveis se a saída de uma função pode ser a entrada para a outra função.

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **função** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **função** elemento que corresponde do **UpdateOrderQuantity** procedimento armazenado. O procedimento armazenado aceita dois parâmetros e não retorna um valor.

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

## <a name="key-element-ssdl"></a>Elemento key (SSDL)

O **chave** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa a chave primária de uma tabela no banco de dados subjacente. **Chave** é um elemento filho de um elemento EntityType, que representa uma linha em uma tabela. A chave primária é definida na **chave** elemento fazendo referência a um ou mais elementos de propriedade são definidos na **EntityType** elemento.

O **chave** elemento pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais)
-   Elementos de anotação

Atributos não são aplicáveis para o **chave** elemento.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityType** elemento com uma chave que faz referência a uma propriedade:

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

## <a name="ondelete-element-ssdl"></a>Elemento OnDelete (SSDL)

O **OnDelete** elemento na linguagem de definição de esquema de armazenamento (SSDL) reflete o comportamento do banco de dados quando uma linha que participa de uma restrição de chave estrangeira é excluída. Se a ação for definida como **Cascade**, em seguida, as linhas que fazem referência a uma linha que está sendo excluída também serão excluídas. Se a ação for definida como **None**, em seguida, as linhas que fazem referência a uma linha que está sendo excluída também não são excluídas. Uma **OnDelete** é um elemento filho de um elemento final.

Uma **OnDelete** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **OnDelete** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Ação**     | Sim         | **CASCADE** ou **None**. (O valor **Restricted** é válida, mas tem o mesmo comportamento **None**.) |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **OnDelete** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **associação** elemento que define o **FK\_CustomerOrders** restrição de chave estrangeira. O **OnDelete** elemento indica que todas as linhas a **pedidos** tabelas que fazem referência a uma linha específica no **clientes** tabela será excluída se na linha do **Os clientes** tabela for excluída.

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

## <a name="parameter-element-ssdl"></a>Elemento Parameter (SSDL)

O **parâmetro** elemento na linguagem de definição de esquema de armazenamento (SSDL) é um filho do elemento de função que especifica os parâmetros para um procedimento armazenado no banco de dados.

O **parâmetro** elemento pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **parâmetro** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**       | Sim         | O nome do parâmetro.                                                                                                                                                                                                      |
| **Tipo**       | Sim         | O tipo de parâmetro.                                                                                                                                                                                                             |
| **Modo**       | Não          | **Na**, **Out**, ou **InOut** dependendo se o parâmetro for uma entrada, saída ou parâmetro de entrada/saída.                                                                                                                |
| **MaxLength**  | Não          | O comprimento máximo do parâmetro.                                                                                                                                                                                            |
| **Precisão**  | Não          | A precisão do parâmetro.                                                                                                                                                                                                 |
| **Ajustar Escala**      | Não          | A escala do parâmetro.                                                                                                                                                                                                     |
| **SRID**       | Não          | Identificador de referência espacial do sistema. Válido somente para parâmetros de tipos espaciais. Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **parâmetro** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **função** elemento que tem dois **parâmetro** elementos que especificam parâmetros de entrada:

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

## <a name="principal-element-ssdl"></a>Elemento de entidade de segurança (SSDL)

O **Principal** na linguagem de definição de esquema de armazenamento (SSDL) é um elemento filho ao elemento ReferentialConstraint que define o final principal de uma restrição de chave estrangeira (também chamado de uma restrição referencial). O **Principal** elemento Especifica a coluna de chave primária (ou colunas) em uma tabela que é referenciada por outra coluna (ou colunas). **PropertyRef** elementos especificam quais colunas são referenciadas. O elemento dependente Especifica colunas que fazem referência a colunas de chave primária que são especificadas na **Principal** elemento.

O **Principal** elemento pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **Principal** elemento.

| Nome do atributo | É obrigatório | Valor                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Função**       | Sim         | O mesmo valor que o **função** (se usado) de atributo do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna referenciada. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **Principal** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento de associação que usa uma **ReferentialConstraint** elemento para especificar as colunas que participam do **FK\_CustomerOrders** chave estrangeira restrição. O **Principal** elemento Especifica a **CustomerId** coluna do **cliente** tabela como o final principal da restrição.

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

## <a name="property-element-ssdl"></a>Elemento de propriedade (SSDL)

O **propriedade** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma coluna em uma tabela no banco de dados subjacente. **Propriedade** elementos são filhos de elementos de EntityType, que representam as linhas em uma tabela. Cada **propriedade** elemento definido em um **EntityType** elemento representa uma coluna.

Um **propriedade** elemento não pode ter elementos filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **propriedade** elemento.

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**                  | Sim         | O nome da coluna correspondente.                                                                                                                                                                                           |
| **Tipo**                  | Sim         | O tipo da coluna correspondente.                                                                                                                                                                                           |
| **Permite valor nulo**              | Não          | **Verdadeiro** (o valor padrão) ou **falso** dependendo se a coluna correspondente pode ter um valor nulo.                                                                                                                  |
| **DefaultValue**          | Não          | O valor padrão da coluna correspondente.                                                                                                                                                                                  |
| **MaxLength**             | Não          | O comprimento máximo da coluna correspondente.                                                                                                                                                                                 |
| **FixedLength**           | Não          | **Verdadeiro** ou **falso** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                              |
| **Precisão**             | Não          | A precisão da coluna correspondente.                                                                                                                                                                                      |
| **Ajustar Escala**                 | Não          | A escala da coluna correspondente.                                                                                                                                                                                          |
| **Unicode**               | Não          | **Verdadeiro** ou **falso** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres Unicode.                                                                                                                   |
| **Agrupamento**             | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usado na fonte de dados.                                                                                                                                                   |
| **SRID**                  | Não          | Identificador de referência espacial do sistema. Válido somente para as propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | Não          | **None**, **Identity** (se o valor da coluna correspondente é uma identidade que é gerada no banco de dados), ou **calculado** (se o valor da coluna correspondente é calculado no banco de dados). Não válido para RowType propriedades. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **propriedade** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **EntityType** elemento com dois filhos **propriedade** elementos:

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

## <a name="propertyref-element-ssdl"></a>Elemento PropertyRef (SSDL)

O **PropertyRef** elemento na linguagem de definição de esquema de armazenamento (SSDL) faz referência a uma propriedade definida em um elemento EntityType para indicar que a propriedade realizará uma das seguintes funções:

-   Fazer parte da chave primária da tabela que o **EntityType** representa. Um ou mais **PropertyRef** elementos podem ser usados para definir uma chave primária. Para obter mais informações, consulte o elemento de chave.
-   Ser a parte final dependente ou entidade de segurança de uma restrição referencial. Para obter mais informações, consulte o elemento ReferentialConstraint.

O **PropertyRef** elemento só pode ter os seguintes elementos filho:

-   Documentação (zero ou um)
-   Elementos de anotação

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados para o **PropertyRef** elemento.

| Nome do atributo | É obrigatório | Valor                                |
|:---------------|:------------|:-------------------------------------|
| **Nome**       | Sim         | O nome da propriedade referenciada. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **PropertyRef** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para CSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **PropertyRef** elemento usado para definir uma chave primária, fazendo referência a uma propriedade que é definida em um **EntityType** elemento.

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

## <a name="referentialconstraint-element-ssdl"></a>Elemento ReferentialConstraint (SSDL)

O **ReferentialConstraint** elemento na linguagem de definição de esquema de armazenamento (SSDL) representa uma restrição de chave estrangeira (também chamada de uma restrição de integridade referencial) no banco de dados subjacente. As extremidades de entidade e dependentes da restrição são especificadas pelos elementos filhos entidade e dependente, respectivamente. Colunas que integram as extremidades de entidade e dependentes são referenciadas com elementos PropertyRef.

O **ReferentialConstraint** é um elemento filho opcional do elemento de associação. Se um **ReferentialConstraint** elemento não será usado para mapear a restrição de chave estrangeira que é especificada na **associação** elemento, um AssociationSetMapping elemento deve ser usado para fazer isso.

O **ReferentialConstraint** elemento pode ter os seguintes elementos filho:

-   Documentação (zero ou um)
-   Entidade de segurança (exatamente uma)
-   Dependente (exatamente uma)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReferentialConstraint** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **associação** elemento que usa um **ReferentialConstraint** elemento para especificar as colunas que participam do **FK\_CustomerOrders**  restrição de chave estrangeira:

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

## <a name="returntype-element-ssdl"></a>Elemento ReturnType (SSDL)

O **ReturnType** elemento na linguagem de definição de esquema de armazenamento (SSDL) Especifica o tipo de retorno para uma função que é definido em um **função** elemento. Uma função de tipo de retorno também pode ser especificado com um **ReturnType** atributo.

O tipo de retorno de uma função é especificado com o **tipo** atributo ou o **ReturnType** elemento.

O **ReturnType** elemento pode ter os seguintes elementos filho:

- CollectionType (um)  

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado para o **ReturnType** elemento. No entanto, os atributos personalizados não podem pertencer a qualquer namespace XML que é reservado para o SSDL. Os nomes totalmente qualificados para os dois atributos personalizados não podem ser o mesmo.

### <a name="example"></a>Exemplo

O exemplo a seguir usa uma **função** que retorna uma coleção de linhas.

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


## <a name="rowtype-element-ssdl"></a>Elemento RowType (SSDL)

Um **RowType** elemento na linguagem de definição de esquema de armazenamento (SSDL) define uma estrutura sem nome como um retorno de tipo para uma função definida no repositório.

Um **RowType** é o elemento filho do **CollectionType** elemento:

Um **RowType** elemento pode ter os seguintes elementos filho:

- Propriedade (um ou mais)  

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função de armazenamento que usa um **CollectionType** elemento para especificar que a função retorna uma coleção de linhas (conforme especificado na **RowType** elemento).


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

## <a name="schema-element-ssdl"></a>Elemento de esquema (SSDL)

O **esquema** na linguagem de definição de esquema de armazenamento (SSDL) é o elemento raiz de uma definição de modelo de armazenamento. Ele contém definições para os contêineres que compõem um modelo de armazenamento, funções e objetos.

O **esquema** elemento pode conter zero ou mais dos seguintes elementos filho:

-   Associação
-   EntityType
-   EntityContainer
-   Função

O **esquema** elemento usa o **Namespace** atributo para definir o namespace para os objetos de associação e o tipo de entidade em um modelo de armazenamento. Dentro de um namespace, não possui dois objetos podem ter o mesmo nome.

Um namespace de modelo de armazenamento é diferente do namespace de XML o **esquema** elemento. Um namespace de modelo de armazenamento (conforme definido pela **Namespace** atributo) é um contêiner lógico para tipos de entidade e tipos de associação. O namespace de XML (indicado pelo **xmlns** atributo) de uma **esquema** elemento é o namespace padrão para os elementos filho e atributos dos **esquema** elemento. Namespaces XML do formulário http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (em que aaaa e MM representam um ano e mês, respectivamente) são reservados para SSDL. Atributos e elementos personalizados não podem ser nos namespaces que têm esse formulário.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos podem ser aplicados para o **esquema** elemento.

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | Sim         | O namespace do modelo de armazenamento. O valor de **Namespace** atributo é usado para formar o nome totalmente qualificado de um tipo. Por exemplo, se um **EntityType** denominado *Customer* está no namespace ExampleModel.Store e, em seguida, o nome totalmente qualificado do **EntityType** é ExampleModel.Store.Customer. <br/> As seguintes cadeias de caracteres não podem ser usadas como o valor para o **Namespace** atributo: **sistema**, **transitório**, ou **Edm**. O valor para o **Namespace** atributo não pode ser o mesmo que o valor para o **Namespace** atributo no elemento do esquema de CSDL. |
| **Alias**                 | Não          | Um identificador usado no lugar do nome de namespace. Por exemplo, se um **EntityType** denominado *Customer* está no namespace ExampleModel.Store e o valor da **Alias** atributo é *StorageModel*, em seguida, você pode usar StorageModel.Customer como o nome totalmente qualificado do **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Provedor**              | Sim         | O provedor de dados.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Sim         | Um token que indica ao provedor que manifesto do provedor para retornar. Nenhum formato para o token é definido. Valores para o token são definidos pelo provedor. Para obter informações sobre tokens de manifesto de provedor do SQL Server, consulte SqlClient para Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Exemplo

A exemplo a seguir mostra uma **esquema** elemento que contém uma **EntityContainer** elemento, dois **EntityType** elementos e um **associação** elemento.

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

## <a name="annotation-attributes"></a>Atributos de anotação

Atributos de anotação na linguagem de definição de esquema de armazenamento (SSDL) são atributos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre os elementos no modelo de armazenamento. Além de ter a estrutura XML válida, as seguintes restrições se aplicam aos atributos de anotação:

-   Atributos de anotação não devem estar em qualquer namespace XML que é reservado para o SSDL.
-   Os nomes totalmente qualificados de todos os atributos dois anotação não pode ser o mesmo.

Mais de um atributo de anotação pode ser aplicado a um determinado elemento SSDL. Os metadados contidos em elementos de anotação podem ser acessado no tempo de execução usando classes no namespace Metadata.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento EntityType que tem um atributo de anotação aplicado para o **OrderId** propriedade. O exemplo também mostram um elemento de anotação adicionado para o **EntityType** elemento.

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

## <a name="annotation-elements-ssdl"></a>Elementos de anotação (SSDL)

Elementos de anotação de linguagem de definição de esquema de armazenamento (SSDL) são elementos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre o modelo de armazenamento. Além de ter a estrutura XML válida, as seguintes restrições se aplicam a elementos de anotação:

-   Elementos de anotação não devem estar em qualquer namespace XML que é reservado para o SSDL.
-   Os nomes totalmente qualificados de quaisquer elementos de dois de anotação não pode ser o mesmo.
-   Elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento SSDL.

Mais de um elemento de anotação pode ser um filho de um determinado elemento SSDL. Começando com o .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessado no tempo de execução usando classes no namespace Metadata.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento EntityType que tem um elemento de anotação (**CustomElement**). O exemplo também mostra um atributo de anotação aplicado para o **OrderId** propriedade.

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

## <a name="facets-ssdl"></a>Facetas (SSDL)

Facetas na linguagem de definição de esquema de armazenamento (SSDL) representam as restrições nos tipos de coluna são especificadas em elementos de propriedade. As facetas são implementadas como atributos XML em **propriedade** elementos.

A tabela a seguir descreve as facetas que são suportadas em SSDL:

| Aspecto           | Descrição                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Agrupamento**   | Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.                                                                                                             |
| **FixedLength** | Especifica se o comprimento do valor da coluna pode variar.                                                                                                                                                                                                  |
| **MaxLength**   | Especifica o comprimento máximo do valor da coluna.                                                                                                                                                                                                           |
| **Precisão**   | Para propriedades do tipo **Decimal**, especifica o número de dígitos de um valor da propriedade pode ter. Para propriedades do tipo **tempo**, **DateTime**, e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da coluna. |
| **Ajustar Escala**       | Especifica o número de dígitos à direita do ponto decimal para o valor da coluna.                                                                                                                                                                      |
| **Unicode**     | Indica se o valor da coluna é armazenado como Unicode.                                                                                                                                                                                                    |
