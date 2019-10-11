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
# <a name="ssdl-specification"></a>Especificação de SSDL
O SSDL (Store Schema Definition Language) é uma linguagem baseada em XML que descreve o modelo de armazenamento de um aplicativo Entity Framework.

Em um aplicativo Entity Framework, os metadados do modelo de armazenamento são carregados de um arquivo. SSDL (gravado em SSDL) em uma instância do System. Data. Metadata. Edm. StoreItemCollection e é acessível usando métodos no Classe System. Data. Metadata. Edm. MetadataWorkspace. Entity Framework usa metadados do modelo de armazenamento para converter consultas em relação ao modelo conceitual para armazenar comandos específicos do armazenamento.

O Entity Framework Designer (EF designer) armazena informações do modelo de armazenamento em um arquivo. edmx em tempo de design. No momento da compilação, o Entity Designer usa informações em um arquivo. edmx para criar o arquivo. SSDL necessário para Entity Framework em tempo de execução.

As versões de SSDL são diferenciadas por namespaces XML.

| Versão do SSDL | Namespace XML                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Elemento Association (SSDL)

Um elemento de **Associação** na linguagem de definição de esquema de repositório (SSDL) especifica colunas de tabela que participam de uma restrição de chave estrangeira no banco de dados subjacente. Dois elementos end filho obrigatórios especificam tabelas nas extremidades da associação e a multiplicidade em cada extremidade. Um elemento ReferentialConstraint opcional especifica as extremidades principais e dependentes da associação, bem como as colunas participantes. Se nenhum elemento **ReferentialConstraint** estiver presente, um elemento AssociationSetMapping deverá ser usado para especificar os mapeamentos de coluna para a associação.

O elemento **Association** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Término (exatamente duas)
-   ReferentialConstraint (zero ou um)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Association** .

| Nome do atributo | É obrigatório | Valor                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome da restrição de chave estrangeira correspondente no banco de dados subjacente. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Association** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Association** que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira **CE @ no__t-3CustomerOrders** :

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

O elemento **AssociationSet** na linguagem de definição de esquema de repositório (SSDL) representa uma restrição de chave estrangeira entre duas tabelas no banco de dados subjacente. As colunas da tabela que participam da restrição FOREIGN KEY são especificadas em um elemento Association. O elemento **Association** que corresponde a um determinado elemento **AssociationSet** é especificado no atributo **Association** do elemento **AssociationSet** .

Os conjuntos de associação SSDL são mapeados para conjuntos de associação CSDL por um elemento AssociationSetMapping. No entanto, se a associação de CSDL para um determinado conjunto de associação de CSDL for definida usando um elemento ReferentialConstraint, nenhum elemento **AssociationSetMapping** correspondente será necessário. Nesse caso, se um elemento **AssociationSetMapping** estiver presente, os mapeamentos que ele definir serão substituídos pelo elemento **ReferentialConstraint** .

O elemento **AssociationSet** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   End (zero ou dois)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **AssociationSet** .

| Nome do atributo  | É obrigatório | Valor                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Name**        | Sim         | O nome da restrição de chave estrangeira que o conjunto de associação representa.                          |
| **Associação** | Sim         | O nome da associação que define as colunas que participam da restrição FOREIGN KEY. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **AssociationSet** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **AssociationSet** que representa a restrição de chave estrangeira `FK_CustomerOrders` no banco de dados subjacente:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>Elemento CollectionType (SSDL)

O elemento **CollectionType** em repositório de linguagem de definição de esquema (SSDL) especifica que o tipo de retorno de uma função é uma coleção. O elemento **CollectionType** é um filho do elemento ReturnType. O tipo de coleção é especificado usando o elemento filho RowType:

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **CollectionType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas.

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

O elemento **CommandText** na Store (linguagem de definição de esquema) do repositório é um filho do elemento Function que permite que você defina uma instrução SQL que é executada no banco de dados. O elemento **CommandText** permite adicionar funcionalidade semelhante a um procedimento armazenado no banco de dados, mas você define o elemento **CommandText** no modelo de armazenamento.

O elemento **CommandText** não pode ter elementos filho. O corpo do elemento **CommandText** deve ser uma instrução SQL válida para o banco de dados subjacente.

Nenhum atributo é aplicável ao elemento **CommandText** .

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Function** com um elemento de **CommandText** filho. Expor a função **UpdateProductInOrder** como um método no ObjectContext importando-o para o modelo conceitual.  

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

O elemento **DefiningQuery** na linguagem de definição de esquema de repositório (SSDL) permite executar uma instrução SQL diretamente no banco de dados subjacente. O elemento **DefiningQuery** é comumente usado como uma exibição de banco de dados, mas a exibição é definida no modelo de armazenamento em vez do banco de dados. O modo de exibição definido em um elemento **DefiningQuery** pode ser mapeado para um tipo de entidade no modelo conceitual por meio de um elemento EntitySetMapping. Esses mapeamentos são somente leitura.  

A sintaxe SSDL a seguir mostra a declaração de um **EntitySet** seguido pelo elemento **DefiningQuery** que contém uma consulta usada para recuperar a exibição.

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

Você pode usar procedimentos armazenados no Entity Framework para habilitar cenários de leitura/gravação em exibições. Você pode usar uma exibição da fonte de dados ou uma exibição Entity SQL como a tabela base para recuperar dados e para o processamento de alterações por procedimentos armazenados.

Você pode usar o elemento **DefiningQuery** para direcionar Microsoft SQL Server Compact 3,5. Embora SQL Server Compact 3,5 não ofereça suporte a procedimentos armazenados, você pode implementar uma funcionalidade semelhante com o elemento **DefiningQuery** . Outro lugar onde pode ser útil é a criação de procedimentos armazenados para superar uma incompatibilidade entre os tipos de dados usados na linguagem de programação e os da fonte de dados. Você poderia escrever um **DefiningQuery** que usa um determinado conjunto de parâmetros e, em seguida, chama um procedimento armazenado com um conjunto diferente de parâmetros, por exemplo, um procedimento armazenado que exclui dados.

## <a name="dependent-element-ssdl"></a>Elemento dependente (SSDL)

O elemento **dependente** na linguagem de definição de esquema de repositório (SSDL) é um elemento filho para o elemento ReferentialConstraint que define a extremidade dependente de uma restrição FOREIGN KEY (também chamada de restrição referencial). O elemento **dependente** especifica a coluna (ou colunas) em uma tabela que faz referência a uma coluna de chave primária (ou colunas). Os elementos **PropertyRef** especificam quais colunas são referenciadas. O elemento principal especifica as colunas de chave primária que são referenciadas por colunas que são especificadas no elemento **dependente** .

O elemento **dependente** pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **dependente** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Função**       | Sim         | O mesmo valor que o atributo de **função** (se usado) do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna de referência. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **dependente** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento Association que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira **CE @ no__t-2CustomerOrders** . O elemento **dependente** especifica a coluna **CustomerID** da tabela **Order** como a extremidade dependente da restrição.

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

O elemento **Documentation** na Store (linguagem de definição de esquema de repositório) pode ser usado para fornecer informações sobre um objeto que é definido em um elemento pai.

O elemento de **documentação** pode ter os seguintes elementos filho (na ordem listada):

-   **Resumo**: Uma breve descrição do elemento pai. (zero ou um elemento)
-   **LongDescription**: Uma descrição abrangente do elemento pai. (zero ou um elemento)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento de **documentação** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra o elemento de **documentação** como um elemento filho de um elemento EntityType.

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

O elemento **final** na Store de linguagem de definição de esquema (SSDL) especifica a tabela e o número de linhas em uma extremidade de uma restrição de chave estrangeira no banco de dados subjacente. O elemento **end** pode ser filho do elemento Association ou do elemento AssociationSet. Em cada caso, os possíveis elementos filho e os atributos aplicáveis são diferentes.

### <a name="end-element-as-a-child-of-the-association-element"></a>Elemento end como um filho do elemento Association

Um elemento **final** (como um filho do elemento **Association** ) especifica a tabela e o número de linhas no final de uma restrição FOREIGN KEY com os atributos **Type** e **multiplicidade** , respectivamente. As extremidades de uma restrição FOREIGN KEY são definidas como parte de uma associação SSDL; uma associação SSDL deve ter exatamente duas extremidades.

Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   OnDelete (zero ou um elemento)
-   Elementos de anotação (zero ou mais elementos)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **Association** .

| Nome do atributo   | É obrigatório | Valor                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tipo**         | Sim         | O nome totalmente qualificado do conjunto de entidades SSDL que está no final da restrição de chave estrangeira.                                                                                                                                                                                                                                                                                          |
| **Função**         | Não          | O valor do atributo **role** no elemento principal ou dependente do elemento ReferentialConstraint correspondente (se usado).                                                                                                                                                                                                                                             |
| **Multiplicidade** | Sim         | **1**, **0.. 1**ou **\*** , dependendo do número de linhas que podem estar no final da restrição de chave estrangeira. <br/> **1** indica que exatamente uma linha existe no final da restrição de chave estrangeira. <br/> **0.. 1** indica que zero ou uma linha existe no final da restrição de chave estrangeira. <br/> **\*** indica que zero, uma ou mais linhas existem no final da restrição de chave estrangeira. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Association** que define a restrição de chave estrangeira **CE @ no__t-2CustomerOrders** . Os valores de **multiplicidade** especificados em cada elemento de **extremidade** indicam que muitas linhas na tabela **Orders** podem ser associadas a uma linha na tabela **Customers** , mas apenas uma linha na tabela **Customers** pode ser associada a uma linha na tabela **Orders** . Além disso, o elemento **OnDelete** indica que todas as linhas na tabela **Orders** que fazem referência a uma linha específica na tabela **Customers** serão excluídas se a linha na tabela **Customers** for excluída.

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Elemento end como um filho do elemento AssociationSet

O elemento **end** (como um filho do elemento **AssociationSet** ) especifica uma tabela em uma extremidade de uma restrição FOREIGN KEY no banco de dados subjacente.

Um elemento **final** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Elementos de anotação (zero ou mais)

#### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **final** quando ele é o filho de um elemento **AssociationSet** .

| Nome do atributo | É obrigatório | Valor                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Sim         | O nome do conjunto de entidades SSDL que está no final da restrição de chave estrangeira.                                      |
| **Função**       | Não          | O valor de um dos atributos de **função** especificados em um elemento **end** do elemento Association correspondente. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **final** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

#### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityContainer** com um elemento **AssociationSet** com dois elementos **end** :

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

Um elemento **EntityContainer** na Store (linguagem de definição de esquema) de repositório descreve a estrutura da fonte de dados subjacente em um aplicativo Entity Framework: Os conjuntos de entidades SSDL (definidos em elementos EntitySet) representam tabelas em um banco de dados, os tipos de entidade SSDL (definidos em elementos EntityType) representam linhas em uma tabela e os conjuntos de associação (definidos em elementos AssociationSet) representam restrições Foreign Key em um banco. Um contêiner de entidade de modelo de armazenamento é mapeado para um contêiner de entidade de modelo conceitual por meio do elemento EntityContainerMapping.

Um elemento **EntityContainer** pode ter zero ou um elementos de documentação. Se um elemento de **documentação** estiver presente, ele deverá preceder todos os outros elementos filho.

Um elemento **EntityContainer** pode ter zero ou mais dos seguintes elementos filho (na ordem listada):

-   EntitySet
-   AssociationSet
-   Elementos de anotação

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityContainer** .

| Nome do atributo | É obrigatório | Valor                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Name**       | Sim         | O nome do contêiner de entidade. Este nome não pode conter pontos (.). |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityContainer** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityContainer** que define dois conjuntos de entidades e um conjunto de associação. Observe que o tipo de entidade e os nomes de tipo de associação são qualificados pelo nome do namespace do modelo conceitual.

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

Um elemento **EntitySet** na linguagem de definição de esquema de repositório (SSDL) representa uma tabela ou exibição no banco de dados subjacente. Um elemento EntityType em SSDL representa uma linha na tabela ou exibição. O atributo **EntityType** de um elemento **EntitySet** especifica o tipo de entidade SSDL específico que representa linhas em um conjunto de entidades SSDL. O mapeamento entre um conjunto de entidades CSDL e um conjunto de entidades SSDL é especificado em um elemento EntitySetMapping.

O elemento **EntitySet** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   DefiningQuery (zero ou um elemento)
-   Elementos de anotação

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntitySet** .

> [!NOTE]
> Alguns atributos (não listados aqui) podem ser qualificados com o alias da **loja** . Esses atributos são usados pelo assistente de modelo de atualização ao atualizar um modelo.

| Nome do atributo | É obrigatório | Valor                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome do conjunto de entidades.                                                              |
| **EntityType** | Sim         | O nome totalmente qualificado do tipo de entidade para o qual o conjunto de entidades contém instâncias. |
| **Esquema**     | Não          | O esquema do banco de dados.                                                                     |
| **Tabela**      | Não          | A tabela do banco de dados.                                                                      |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntitySet** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityContainer** que tem dois elementos **EntitySet** e um elemento **AssociationSet** :

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

Um elemento **EntityType** na linguagem de definição de esquema de repositório (SSDL) representa uma linha em uma tabela ou exibição do banco de dados subjacente. Um elemento EntitySet em SSDL representa a tabela ou exibição na qual as linhas ocorrem. O atributo **EntityType** de um elemento **EntitySet** especifica o tipo de entidade SSDL específico que representa linhas em um conjunto de entidades SSDL. O mapeamento entre um tipo de entidade SSDL e um tipo de entidade CSDL é especificado em um elemento EntityTypeMapping.

O elemento **EntityType** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um elemento)
-   Chave (zero ou um elemento)
-   Elementos de anotação

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **EntityType** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome do tipo de entidade. Esse valor é geralmente o mesmo que o nome da tabela na qual o tipo de entidade representa uma linha. Esse valor não pode conter pontos (.). |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **EntityType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityType** com duas propriedades:

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

O elemento **Function** na linguagem de definição de esquema de repositório (SSDL) especifica um procedimento armazenado que existe no banco de dados subjacente.

O elemento **Function** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Parâmetro (zero ou mais)
-   CommandText (zero ou um)
-   ReturnType (zero ou mais)
-   Elementos de anotação (zero ou mais)

Um tipo de retorno para uma função deve ser especificado com o elemento **ReturnType** ou o atributo **ReturnType** (veja abaixo), mas não ambos.

Os procedimentos armazenados que são especificados no modelo de armazenamento podem ser importados para o modelo conceitual de um aplicativo. Para obter mais informações, consulte [consultando com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/query.md). O elemento **Function** também pode ser usado para definir funções personalizadas no modelo de armazenamento.  

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Function** .

> [!NOTE]
> Alguns atributos (não listados aqui) podem ser qualificados com o alias da **loja** . Esses atributos são usados pelo assistente de modelo de atualização ao atualizar um modelo.

| Nome do atributo             | É obrigatório | Valor                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                   | Sim         | O nome do procedimento armazenado.                                                                                                                                                                                  |
| **ReturnType**             | Não          | O tipo de retorno do procedimento armazenado.                                                                                                                                                                           |
| **Aggregate**              | Não          | **True** se o procedimento armazenado retornar um valor de agregação; caso contrário, **false**.                                                                                                                                  |
| **BuiltIn**                | Não          | **True** se a função for uma função interna<sup>1</sup> ; caso contrário, **false**.                                                                                                                                  |
| **StoreFunctionName**      | Não          | O nome do procedimento armazenado.                                                                                                                                                                                  |
| **NiladicFunction**        | Não          | **True** se a função for uma função niladic<sup>2</sup> ; Caso contrário, **false** .                                                                                                                                   |
| **IsComposable**           | Não          | **True** se a função for uma função combinável<sup>3</sup> ; Caso contrário, **false** .                                                                                                                                |
| **ParameterTypeSemantics** | Não          | A enumeração que define a semântica de tipo usada para resolver sobrecargas de função. A enumeração é definida no manifesto do provedor por definição de função. O valor padrão é **AllowImplicitConversion**. |
| **Esquema**                 | Não          | O nome do esquema no qual o procedimento armazenado é definido.                                                                                                                                                   |

<sup>1</sup> uma função interna é uma função que é definida no banco de dados. Para obter informações sobre as funções definidas no modelo de armazenamento, consulte elemento CommandText (SSDL).

<sup>2</sup> uma função niladic é uma função que não aceita parâmetros e, quando chamado, não requer parênteses.

<sup>3</sup> duas funções são combináveis se a saída de uma função pode ser a entrada para a outra função.

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Function** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Function** que corresponde ao procedimento armazenado **UpdateOrderQuantity** . O procedimento armazenado aceita dois parâmetros e não retorna um valor.

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

## <a name="key-element-ssdl"></a>Elemento Key (SSDL)

O elemento de **chave** na linguagem de definição de esquema de repositório (SSDL) representa a chave primária de uma tabela no banco de dados subjacente. **Key** é um elemento filho de um elemento EntityType, que representa uma linha em uma tabela. A chave primária é definida no elemento de **chave** referenciando um ou mais elementos de propriedade que são definidos no elemento **EntityType** .

O elemento **Key** pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais)
-   Elementos de anotação

Nenhum atributo é aplicável ao elemento de **chave** .

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityType** com uma chave que faz referência a uma propriedade:

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

## <a name="ondelete-element-ssdl"></a>Elemento ondelery (SSDL)

O elemento **OnDelete** na Store (linguagem de definição de esquema) do repositório reflete o comportamento do banco de dados quando uma linha que participa de uma restrição de chave estrangeira é excluída. Se a ação for definida como **Cascade**, as linhas que fazem referência a uma linha que está sendo excluída também serão excluídas. Se a ação for definida como **nenhuma**, as linhas que fazem referência a uma linha que está sendo excluída também não serão excluídas. Um elemento **OnDelete** é um elemento filho de um elemento end.

Um elemento **OnDelete** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **OnDelete** .

| Nome do atributo | É obrigatório | Valor                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Ação**     | Sim         | **Cascade** ou **None**. (O valor **restrito** é válido, mas tem o mesmo comportamento que **nenhum**.) |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **OnDelete** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Association** que define a restrição de chave estrangeira **CE @ no__t-2CustomerOrders** . O elemento **OnDelete** indica que todas as linhas na tabela **Orders** que fazem referência a uma linha específica na tabela **Customers** serão excluídas se a linha na tabela **Customers** for excluída.

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

O elemento **Parameter** na linguagem de definição de esquema de repositório (SSDL) é um filho do elemento Function que especifica parâmetros para um procedimento armazenado no banco de dados.

O elemento **Parameter** pode ter os seguintes elementos filho (na ordem listada):

-   Documentação (zero ou um)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Parameter** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Sim         | O nome do parâmetro.                                                                                                                                                                                                      |
| **Tipo**       | Sim         | O tipo de parâmetro.                                                                                                                                                                                                             |
| **Modo**       | Não          | **In**, **out**ou **Inout** , dependendo se o parâmetro é um parâmetro de entrada, saída ou entrada/saída.                                                                                                                |
| **MaxLength**  | Não          | O comprimento máximo do parâmetro.                                                                                                                                                                                            |
| **Precisão**  | Não          | A precisão do parâmetro.                                                                                                                                                                                                 |
| **Ajustar Escala**      | Não          | A escala do parâmetro.                                                                                                                                                                                                     |
| **SRID**       | Não          | Identificador de referência de sistema espacial. Válido somente para parâmetros de tipos espaciais. Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Parameter** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Function** que tem dois elementos de **parâmetro** que especificam parâmetros de entrada:

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

## <a name="principal-element-ssdl"></a>Elemento principal (SSDL)

O elemento **principal** no SSDL (Store Schema Definition Language) é um elemento filho do elemento ReferentialConstraint que define a extremidade principal de uma restrição FOREIGN KEY (também chamada de restrição referencial). O elemento **principal** especifica a coluna de chave primária (ou colunas) em uma tabela que é referenciada por outra coluna (ou colunas). Os elementos **PropertyRef** especificam quais colunas são referenciadas. O elemento dependente especifica colunas que referenciam as colunas de chave primária que são especificadas no elemento **principal** .

O elemento **principal** pode ter os seguintes elementos filho (na ordem listada):

-   PropertyRef (um ou mais)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **principal** .

| Nome do atributo | É obrigatório | Valor                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Função**       | Sim         | O mesmo valor que o atributo de **função** (se usado) do elemento final correspondente; caso contrário, o nome da tabela que contém a coluna referenciada. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **principal** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento Association que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira **CE @ no__t-2CustomerOrders** . O elemento **principal** especifica a coluna **CustomerID** da tabela **Customer** como a extremidade principal da restrição.

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

## <a name="property-element-ssdl"></a>Elemento Property (SSDL)

O elemento de **Propriedade** na linguagem de definição de esquema de repositório (SSDL) representa uma coluna em uma tabela no banco de dados subjacente. Elementos de **Propriedade** são filhos de elementos EntityType, que representam linhas em uma tabela. Cada elemento de **Propriedade** definido em um elemento **EntityType** representa uma coluna.

Um elemento de **Propriedade** não pode ter nenhum elemento filho.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Property** .

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                  | Sim         | O nome da coluna correspondente.                                                                                                                                                                                           |
| **Tipo**                  | Sim         | O tipo da coluna correspondente.                                                                                                                                                                                           |
| **Anula**              | Não          | **True** (o valor padrão) ou **false** , dependendo se a coluna correspondente pode ter um valor nulo.                                                                                                                  |
| **DefaultValue**          | Não          | O valor padrão da coluna correspondente.                                                                                                                                                                                  |
| **MaxLength**             | Não          | O comprimento máximo da coluna correspondente.                                                                                                                                                                                 |
| **Cadeia**           | Não          | **True** ou **false** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres de comprimento fixo.                                                                                                              |
| **Precisão**             | Não          | A precisão da coluna correspondente.                                                                                                                                                                                      |
| **Ajustar Escala**                 | Não          | A escala da coluna correspondente.                                                                                                                                                                                          |
| **Unicode**               | Não          | **True** ou **false** dependendo se o valor da coluna correspondente será armazenado como uma cadeia de caracteres Unicode.                                                                                                                   |
| **Agrupamento**             | Não          | Uma cadeia de caracteres que especifica a sequência de agrupamento a ser usada na fonte de dados.                                                                                                                                                   |
| **SRID**                  | Não          | Identificador de referência de sistema espacial. Válido somente para propriedades de tipos espaciais. Para obter mais informações, consulte [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | Não          | **None**, **Identity** (se o valor da coluna correspondente for uma identidade gerada no banco de dados) ou **computado** (se o valor da coluna correspondente for computado no banco de dados). Não é válido para propriedades RowType. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **Property** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **EntityType** com dois elementos de **Propriedade** filho:

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

O elemento **PropertyRef** na linguagem de definição de esquema de repositório (SSDL) faz referência a uma propriedade definida em um elemento EntityType para indicar que a propriedade executará uma das seguintes funções:

-   Faça parte da chave primária da tabela que o **EntityType** representa. Um ou mais elementos **PropertyRef** podem ser usados para definir uma chave primária. Para obter mais informações, consulte Key Element.
-   Ser a extremidade dependente ou de entidade de uma restrição referencial. Para obter mais informações, consulte elemento ReferentialConstraint.

O elemento **PropertyRef** só pode ter os seguintes elementos filho:

-   Documentação (zero ou um)
-   Elementos de anotação

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **PropertyRef** .

| Nome do atributo | É obrigatório | Valor                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Sim         | O nome da propriedade referenciada. |

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **PropertyRef** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para CSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **PropertyRef** usado para definir uma chave primária referenciando uma propriedade que é definida em um elemento **EntityType** .

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

O elemento **ReferentialConstraint** na linguagem de definição de esquema de repositório (SSDL) representa uma restrição FOREIGN KEY (também chamada de restrição de integridade referencial) no banco de dados subjacente. As extremidades principais e dependentes da restrição são especificadas pelos elementos filho principal e dependente, respectivamente. As colunas que participam das extremidades principal e dependente são referenciadas com elementos PropertyRef.

O elemento **ReferentialConstraint** é um elemento filho opcional do elemento Association. Se um elemento **ReferentialConstraint** não for usado para mapear a restrição FOREIGN KEY especificada no elemento **Association** , um elemento AssociationSetMapping deverá ser usado para fazer isso.

O elemento **ReferentialConstraint** pode ter os seguintes elementos filho:

-   Documentação (zero ou um)
-   Entidade de segurança (exatamente uma)
-   Dependente (exatamente um)
-   Elementos de anotação (zero ou mais)

### <a name="applicable-attributes"></a>Atributos aplicáveis

Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReferentialConstraint** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento **Association** que usa um elemento **ReferentialConstraint** para especificar as colunas que participam da restrição de chave estrangeira **CE @ no__t-3CustomerOrders** :

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

O elemento **ReturnType** na linguagem de definição de esquema de repositório (SSDL) especifica o tipo de retorno para uma função que é definida em um elemento **Function** . Um tipo de retorno de função também pode ser especificado com um atributo **ReturnType** .

O tipo de retorno de uma função é especificado com o atributo **Type** ou o elemento **ReturnType** .

O elemento **ReturnType** pode ter os seguintes elementos filho:

- CollectionType (um)  

> [!NOTE]
> Qualquer número de atributos de anotação (atributos XML personalizados) pode ser aplicado ao elemento **ReturnType** . No entanto, os atributos personalizados podem não pertencer a nenhum namespace XML reservado para SSDL. Os nomes totalmente qualificados para dois atributos personalizados não podem ser iguais.

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

Um elemento **RowType** na linguagem de definição de esquema de repositório (SSDL) define uma estrutura sem nome como um tipo de retorno para uma função definida no repositório.

Um elemento **RowType** é o elemento filho do elemento **CollectionType** :

Um elemento **RowType** pode ter os seguintes elementos filho:

- Propriedade (um ou mais)  

### <a name="example"></a>Exemplo

O exemplo a seguir mostra uma função de repositório que usa um elemento **CollectionType** para especificar que a função retorna uma coleção de linhas (conforme especificado no elemento **RowType** ).


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

O elemento de **esquema** na linguagem de definição de esquema de repositório (SSDL) é o elemento raiz de uma definição de modelo de armazenamento. Ele contém definições para os objetos, funções e contêineres que compõem um modelo de armazenamento.

O elemento **Schema** pode conter zero ou mais dos seguintes elementos filho:

-   Associação
-   EntityType
-   EntityContainer
-   Função

O elemento **Schema** usa o atributo **namespace** para definir o namespace para o tipo de entidade e objetos de associação em um modelo de armazenamento. Em um namespace, dois objetos não podem ter o mesmo nome.

Um namespace do modelo de armazenamento é diferente do namespace XML do elemento **Schema** . Um namespace de modelo de armazenamento (conforme definido pelo atributo **namespace** ) é um contêiner lógico para tipos de entidade e tipos de associação. O namespace XML (indicado pelo atributo **xmlns** ) de um elemento **Schema** é o namespace padrão para elementos filho e atributos do elemento **Schema** . Os namespaces XML do formulário https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (em que AAAA e MM representam um ano e um mês respectivamente) são reservados para SSDL. Atributos e elementos personalizados não podem estar em namespaces que tenham esse formulário.

### <a name="applicable-attributes"></a>Atributos aplicáveis

A tabela a seguir descreve os atributos que podem ser aplicados ao elemento **Schema** .

| Nome do atributo            | É obrigatório | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | Sim         | O namespace do modelo de armazenamento. O valor do atributo **namespace** é usado para formar o nome totalmente qualificado de um tipo. Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace ExampleModel. Store, o nome totalmente qualificado do **EntityType** será ExampleModel. Store. Customer. <br/> As seguintes cadeias de caracteres não podem ser usadas como o valor do atributo **namespace** : **Sistema**, **transitório**ou **EDM**. O valor do atributo **namespace** não pode ser o mesmo que o valor do atributo **namespace** no elemento de esquema CSDL. |
| **Alias**                 | Não          | Um identificador usado no lugar do nome do namespace. Por exemplo, se um **EntityType** chamado *Customer* estiver no namespace ExampleModel. Store e o valor do atributo **alias** for *StorageModel*, você poderá usar StorageModel. Customer como o nome totalmente qualificado do  **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Provedor**              | Sim         | O provedor de dados.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Sim         | Um token que indica ao provedor que o manifesto do provedor deve retornar. Nenhum formato para o token está definido. Os valores para o token são definidos pelo provedor. Para obter informações sobre SQL Server tokens de manifesto do provedor, consulte SqlClient para Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento de **esquema** que contém um elemento **EntityContainer** , dois elementos **EntityType** e um elemento **Association** .

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

## <a name="annotation-attributes"></a>Atributos de anotação

Os atributos de anotação no SSDL (Store Schema Definition Language) são atributos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre os elementos no modelo de armazenamento. Além de ter uma estrutura XML válida, as seguintes restrições se aplicam aos atributos de anotação:

-   Os atributos de anotação não devem estar em nenhum namespace XML reservado para SSDL.
-   Os nomes totalmente qualificados de quaisquer dois atributos de anotação não devem ser iguais.

Mais de um atributo Annotation pode ser aplicado a um determinado elemento SSDL. Os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento EntityType que tem um atributo Annotation aplicado à propriedade **OrderID** . O exemplo também mostra um elemento Annotation adicionado ao elemento **EntityType** .

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

Os elementos de anotação no SSDL (Store Schema Definition Language) são elementos XML personalizados no modelo de armazenamento que fornecem metadados extras sobre o modelo de armazenamento. Além de ter uma estrutura XML válida, as seguintes restrições se aplicam aos elementos de anotação:

-   Os elementos de anotação não devem estar em nenhum namespace XML reservado para SSDL.
-   Os nomes totalmente qualificados de quaisquer dois elementos de anotação não devem ser iguais.
-   Os elementos de anotação devem aparecer após todos os outros elementos filho de um determinado elemento SSDL.

Mais de um elemento Annotation pode ser um filho de um determinado elemento SSDL. A partir do .NET Framework versão 4, os metadados contidos nos elementos de anotação podem ser acessados em tempo de execução usando classes no namespace System. Data. Metadata. Edm.

### <a name="example"></a>Exemplo

O exemplo a seguir mostra um elemento EntityType que tem um elemento Annotation (**customelement**). O exemplo também mostra um atributo Annotation aplicado à propriedade **OrderID** .

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

As facetas na linguagem de definição de esquema de repositório (SSDL) representam restrições em tipos de coluna que são especificados em elementos de propriedade. As facetas são implementadas como atributos XML em elementos de **Propriedade** .

A tabela a seguir descreve as facetas com suporte no SSDL:

| Aspecto           | Descrição                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Agrupamento**   | Especifica a sequência de agrupamento (ou sequência de classificação) a ser usadas para executar a comparação e em ordenação operações em valores de propriedade.                                                                                                             |
| **Cadeia** | Especifica se o comprimento do valor da coluna pode variar.                                                                                                                                                                                                  |
| **MaxLength**   | Especifica o comprimento máximo do valor da coluna.                                                                                                                                                                                                           |
| **Precisão**   | Para propriedades do tipo **decimal**, especifica o número de dígitos que um valor de propriedade pode ter. Para propriedades do tipo **time**, **DateTime**e **DateTimeOffset**, especifica o número de dígitos para a parte fracionária de segundos do valor da coluna. |
| **Ajustar Escala**       | Especifica o número de dígitos à direita do ponto decimal para o valor da coluna.                                                                                                                                                                      |
| **Unicode**     | Indica se o valor da coluna é armazenado como Unicode.                                                                                                                                                                                                    |
