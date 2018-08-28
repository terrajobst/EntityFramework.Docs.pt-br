---
title: Definição de consulta - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 60d5310589bb9bc3fdb971673422e80537357e55
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996301"
---
# <a name="defining-query---ef-designer"></a>Consulta de definição - EF Designer
Este passo a passo demonstra como adicionar uma definição de tipo de consulta e uma entidade correspondente a um modelo usando o EF Designer. Uma consulta de definição é comumente usada para fornecer funcionalidade semelhante àquela fornecida por uma exibição de banco de dados, mas a exibição é definida no modelo, não o banco de dados. Uma consulta de definição permite que você execute uma instrução SQL que é especificada na **DefiningQuery** elemento de um arquivo. edmx. Para obter mais informações, consulte **DefiningQuery** na [especificação de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Ao usar consultas de definição, você também precisa definir um tipo de entidade em seu modelo. O tipo de entidade é usado para dados expostos pela consulta de definição de superfície. Observe que os dados apresentados por meio desse tipo de entidade serão somente leitura.

Consultas parametrizadas não podem ser executadas como a definição de consultas. No entanto, os dados podem ser atualizados, mapeando o insert, update e delete de funções do tipo de entidade que exiba os dados para os procedimentos armazenados. Para obter mais informações, consulte [inserir, atualizar e excluir com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/cud.md).

Este tópico mostra como executar as seguintes tarefas.

-   Adicionar uma consulta de definição
-   Adicionar um tipo de entidade ao modelo
-   Mapear a consulta de definição para o tipo de entidade

## <a name="prerequisites"></a>Pré-requisitos

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

Este passo a passo é usando o Visual Studio 2012 ou mais recente.

-   Abra o Visual Studio.
-   No menu **Arquivo**, aponte para **Novo** e clique em **Projeto**.
-   No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **aplicativo de Console** modelo.
-   Insira **DefiningQuerySample** como o nome do projeto e clique **Okey**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Criar um modelo com base no banco de dados School

-   Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**.
-   Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.
-   Insira **DefiningQueryModel.edmx** para o nome de arquivo e clique **Add**.
-   Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **próxima**.
-   Clique em nova Conexão. Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.
    A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.
-   Na caixa de diálogo Choose Your Database Objects, verifique as **tabelas** nó. Isso adicionará todas as tabelas para o **School** modelo.
-   Clique em **Finalizar**.
-   No Gerenciador de soluções, clique com botão direito do **DefiningQueryModel.edmx** do arquivo e selecione **abrir com...** .
-   Selecione **Editor de XML (texto)**.

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   Clique em **Sim** se solicitado com a seguinte mensagem:

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Adicionar uma consulta de definição

Nesta etapa, usaremos o Editor de XML para adicionar uma definição de consulta e um tipo de entidade para a seção SSDL do arquivo. edmx. 

-   Adicionar um **EntitySet** elemento à seção SSDL do arquivo. edmx (linha de 5 por meio de 13). Especifique o seguinte:
    -   Somente o **nome** e **EntityType** atributos do **EntitySet** elemento são especificados.
    -   O nome totalmente qualificado do tipo de entidade é usado na **EntityType** atributo.
    -   A instrução SQL a ser executado é especificada na **DefiningQuery** elemento.

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

-   Adicione a **EntityType** elemento à seção SSDL dos. edmx. arquivo conforme mostrado abaixo. Observe o seguinte:
    -   O valor da **nome** atributo corresponde ao valor da **EntityType** atributo no **EntitySet** elemento acima, embora o nome totalmente qualificado do tipo de entidade é usado na **EntityType** atributo.
    -   Os nomes de propriedade correspondem aos nomes de coluna retornados pela instrução SQL no **DefiningQuery** elemento (acima).
    -   Neste exemplo, a chave de entidade é composta de três propriedades para garantir que um valor de chave exclusivo.

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
> Se posteriormente você executar o **Assistente de modelo de atualização** caixa de diálogo, as alterações feitas no modelo de armazenamento, incluindo a definição de consultas, será substituída.

 

## <a name="add-an-entity-type-to-the-model"></a>Adicionar um tipo de entidade ao modelo

Nesta etapa, adicionaremos o tipo de entidade para o modelo conceitual usando o EF Designer.  Observe o seguinte:

-   O **nome** da entidade corresponde ao valor da **EntityType** atributo no **EntitySet** acima do elemento.
-   Os nomes de propriedade correspondem aos nomes de coluna retornados pela instrução SQL no **DefiningQuery** acima do elemento.
-   Neste exemplo, a chave de entidade é composta de três propriedades para garantir que um valor de chave exclusivo.

Abra o modelo no Designer de EF.

-   Clique duas vezes o DefiningQueryModel.edmx.
-   Digamos **Sim** à mensagem a seguir:

    ![Warning2](~/ef6/media/warning2.png)

 

O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.

-   Clique com botão direito do designer na superfície e selecione **adicionar novo**-&gt;**Entity...** .
-   Especificar **GradeReport** para o nome da entidade e **CourseID** para o **propriedade Key**.
-   Clique com botão direito do **GradeReport** entidade e selecione **adicionar novo** - &gt; **propriedade escalar**.
-   Alterar o nome padrão da propriedade a ser **FirstName**.
-   Adicione outra propriedade escalar e especifique **LastName** para o nome.
-   Adicione outra propriedade escalar e especifique **classificação** para o nome.
-   No **propriedades** janela, altere o **classificação**do **tipo** propriedade a ser **Decimal**.
-   Selecione o **FirstName** e **Sobrenome** propriedades.
-   No **propriedades** janela, altere o **EntityKey** valor da propriedade **verdadeiro**.

Como resultado, os elementos a seguir foram adicionados para o **CSDL** seção do arquivo. edmx.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Mapear a consulta de definição para o tipo de entidade

Nesta etapa, usaremos a janela de detalhes de mapeamento para mapear conceitual e tipos de entidade de armazenamento.

-   Clique com botão direito do **GradeReport** entidade na superfície do design e selecione **mapeamento de tabela**.  
    O **Mapping Details** janela é exibida.
-   Selecione **GradeReport** da **&lt;adicionar uma tabela ou exibição&gt;** lista suspensa (localizada na **tabela**s).  
    Padrão de mapeamentos entre conceitual e de armazenamento **GradeReport** tipo de entidade são exibidos.  
    ![MappingDetails3](~/ef6/media/mappingdetails.png)

Como resultado, o **EntitySetMapping** elemento é adicionado à seção de mapeamento do arquivo. edmx. 

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

Agora você pode executar a consulta de definição usando o **GradeReport** tipo de entidade. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
