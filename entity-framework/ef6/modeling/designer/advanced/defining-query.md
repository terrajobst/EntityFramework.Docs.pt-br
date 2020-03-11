---
title: Definindo consulta-designer de EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418794"
---
# <a name="defining-query---ef-designer"></a>Definindo consulta-designer de EF
Este tutorial demonstra como adicionar uma consulta de definição e um tipo de entidade correspondente a um modelo usando o designer do EF. Uma consulta de definição é comumente usada para fornecer funcionalidade semelhante àquela fornecida por uma exibição de banco de dados, mas a exibição é definida no modelo, não no banco de dados. Uma consulta de definição permite que você execute uma instrução SQL que é especificada no elemento **DefiningQuery** de um arquivo. edmx. Para obter mais informações, consulte **DefiningQuery** na [especificação SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Ao usar as consultas de definição, você também precisa definir um tipo de entidade em seu modelo. O tipo de entidade é usado para superfície de dados expostos pela consulta de definição. Observe que os dados exibidos por meio desse tipo de entidade são somente leitura.

Consultas parametrizadas não podem ser executadas como definição de consultas. No entanto, os dados podem ser atualizados mapeando as funções de inserção, atualização e exclusão do tipo de entidade que superfícies os dados para procedimentos armazenados. Para obter mais informações, consulte [Inserir, atualizar e excluir com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/cud.md).

Este tópico mostra como executar as tarefas a seguir.

-   Adicionar uma consulta de definição
-   Adicionar um tipo de entidade ao modelo
-   Mapear a consulta de definição para o tipo de entidade

## <a name="prerequisites"></a>Prerequisites

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

Este passo a passos é usar o Visual Studio 2012 ou mais recente.

-   Abra o Visual Studio.
-   No menu **Arquivo** , aponte para **Novo**e clique em **Projeto**.
-   No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo **aplicativo de console** .
-   Insira **DefiningQuerySample** como o nome do projeto e clique em **OK**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Criar um modelo com base no banco de dados escolar

-   Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**.
-   Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.
-   Digite **DefiningQueryModel. edmx** para o nome do arquivo e clique em **Adicionar**.
-   Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**.
-   Clique em Nova conexão. Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(LocalDB)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.
    A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.
-   Na caixa de diálogo escolher seu objeto de banco de dados, verifique as **tabelas** nó. Isso adicionará todas as tabelas ao modelo **escolar** .
-   Clique em **concluir**.
-   Em Gerenciador de Soluções, clique com o botão direito do mouse no arquivo **DefiningQueryModel. edmx** e selecione **abrir com...** .
-   Selecione **Editor de XML (texto)** .

    ![Editor de XML](~/ef6/media/xmleditor.png)

-   Clique em **Sim** se receber a mensagem a seguir:

    ![Aviso 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Adicionar uma consulta de definição

Nesta etapa, usaremos o editor de XML para adicionar uma consulta de definição e um tipo de entidade à seção SSDL do arquivo. edmx. 

-   Adicione um elemento  **EntitySet** à seção SSDL do arquivo. edmx (linha 5 a 13). Especifique o seguinte:
    -   Somente o **nome** e **EntityType** atributos do elemento  **EntitySet** são especificados.
    -   O nome totalmente qualificado do tipo de entidade é usado no atributo  **EntityType** .
    -   A instrução SQL a ser executada é especificada no elemento  **DefiningQuery** .

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   Adicione o elemento **EntityType** à seção SSDL do. edmx. arquivo, conforme mostrado abaixo. Observe o seguinte:
    -   O valor do atributo **Name** corresponde ao valor do atributo **EntityType** no elemento **EntitySet** acima, embora o nome totalmente qualificado do tipo de entidade seja usado no atributo **EntityType** .
    -   Os nomes de propriedade correspondem aos nomes de coluna retornados pela instrução SQL no elemento **DefiningQuery** (acima).
    -   Neste exemplo, a chave de entidade é composta de três propriedades para garantir um valor de chave exclusivo.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> Se posteriormente você executar a caixa de diálogo **Assistente de atualização de modelo** , as alterações feitas no modelo de armazenamento, incluindo a definição de consultas, serão substituídas.

 

## <a name="add-an-entity-type-to-the-model"></a>Adicionar um tipo de entidade ao modelo

Nesta etapa, adicionaremos o tipo de entidade ao modelo conceitual usando o designer do EF.  Observe o seguinte:

-   O **nome** da entidade corresponde ao valor do atributo **EntityType** no elemento **EntitySet** acima.
-   Os nomes de propriedade correspondem aos nomes de coluna retornados pela instrução SQL no elemento **DefiningQuery** acima.
-   Neste exemplo, a chave de entidade é composta de três propriedades para garantir um valor de chave exclusivo.

Abra o modelo no designer do EF.

-   Clique duas vezes em DefiningQueryModel. edmx.
-   Digamos que **Sim** para a seguinte mensagem:

    ![Aviso 2](~/ef6/media/warning2.png)

 

O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.

-   Clique com o botão direito do mouse na superfície do designer e selecione **Adicionar nova**-&gt;**entidade...** .
-   Especifique **GradeReport** para o nome da entidade e **cursoid** para a **propriedade de chave**.
-   Clique com o botão direito do mouse na entidade **GradeReport** e selecione **adicionar nova**-&gt; **Propriedade escalar**.
-   Altere o nome padrão da propriedade para **FirstName**.
-   Adicione outra propriedade escalar e especifique **LastName** para o nome.
-   Adicione outra propriedade escalar e especifique a **classificação** para o nome.
-   Na janela **Propriedades** , altere a propriedade **tipo** da **grade**para **decimal**.
-   Selecione as propriedades **FirstName** e **LastName** .
-   Na janela **Propriedades** , altere o valor da propriedade **EntityKey** para **true**.

Como resultado, os elementos a seguir foram adicionados à seção **CSDL** do arquivo. edmx.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Mapear a consulta de definição para o tipo de entidade

Nesta etapa, usaremos a janela de detalhes de mapeamento para mapear os tipos de entidade conceitual e de armazenamento.

-   Clique com o botão direito do mouse na entidade **GradeReport** na superfície de design e selecione **mapeamento de tabela**.  
    A janela **detalhes do mapeamento** é exibida.
-   Selecione **GradeReport** na lista suspensa **&lt;adicionar uma tabela ou exibição&gt;** (localizada em **tabela**s).  
    Os mapeamentos padrão entre o tipo de entidade conceitual e de armazenamento **GradeReport** são exibidos.  
    Mapeamento de ![Details3](~/ef6/media/mappingdetails.png)

Como resultado, o elemento de **EntitySetMapping** é adicionado à seção de mapeamento do arquivo. edmx. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   Compile o aplicativo.

 

## <a name="call-the-defining-query-in-your-code"></a>Chamar a consulta de definição em seu código

Agora você pode executar a consulta de definição usando o tipo de entidade **GradeReport** . 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
