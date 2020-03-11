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
# <a name="defining-query---ef-designer"></a><span data-ttu-id="c200b-102">Definindo consulta-designer de EF</span><span class="sxs-lookup"><span data-stu-id="c200b-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="c200b-103">Este tutorial demonstra como adicionar uma consulta de definição e um tipo de entidade correspondente a um modelo usando o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="c200b-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="c200b-104">Uma consulta de definição é comumente usada para fornecer funcionalidade semelhante àquela fornecida por uma exibição de banco de dados, mas a exibição é definida no modelo, não no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c200b-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="c200b-105">Uma consulta de definição permite que você execute uma instrução SQL que é especificada no elemento **DefiningQuery** de um arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="c200b-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="c200b-106">Para obter mais informações, consulte **DefiningQuery** na [especificação SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="c200b-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="c200b-107">Ao usar as consultas de definição, você também precisa definir um tipo de entidade em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="c200b-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="c200b-108">O tipo de entidade é usado para superfície de dados expostos pela consulta de definição.</span><span class="sxs-lookup"><span data-stu-id="c200b-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="c200b-109">Observe que os dados exibidos por meio desse tipo de entidade são somente leitura.</span><span class="sxs-lookup"><span data-stu-id="c200b-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="c200b-110">Consultas parametrizadas não podem ser executadas como definição de consultas.</span><span class="sxs-lookup"><span data-stu-id="c200b-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="c200b-111">No entanto, os dados podem ser atualizados mapeando as funções de inserção, atualização e exclusão do tipo de entidade que superfícies os dados para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="c200b-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="c200b-112">Para obter mais informações, consulte [Inserir, atualizar e excluir com procedimentos armazenados](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="c200b-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="c200b-113">Este tópico mostra como executar as tarefas a seguir.</span><span class="sxs-lookup"><span data-stu-id="c200b-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="c200b-114">Adicionar uma consulta de definição</span><span class="sxs-lookup"><span data-stu-id="c200b-114">Add a Defining Query</span></span>
-   <span data-ttu-id="c200b-115">Adicionar um tipo de entidade ao modelo</span><span class="sxs-lookup"><span data-stu-id="c200b-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="c200b-116">Mapear a consulta de definição para o tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="c200b-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c200b-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="c200b-117">Prerequisites</span></span>

<span data-ttu-id="c200b-118">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="c200b-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="c200b-119">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c200b-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="c200b-120">O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="c200b-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="c200b-121">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="c200b-121">Set up the Project</span></span>

<span data-ttu-id="c200b-122">Este passo a passos é usar o Visual Studio 2012 ou mais recente.</span><span class="sxs-lookup"><span data-stu-id="c200b-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="c200b-123">Abra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c200b-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="c200b-124">No menu **Arquivo** , aponte para **Novo**e clique em **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="c200b-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="c200b-125">No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo **aplicativo de console** .</span><span class="sxs-lookup"><span data-stu-id="c200b-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="c200b-126">Insira **DefiningQuerySample** como o nome do projeto e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c200b-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="c200b-127">Criar um modelo com base no banco de dados escolar</span><span class="sxs-lookup"><span data-stu-id="c200b-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="c200b-128">Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**.</span><span class="sxs-lookup"><span data-stu-id="c200b-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="c200b-129">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.</span><span class="sxs-lookup"><span data-stu-id="c200b-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="c200b-130">Digite **DefiningQueryModel. edmx** para o nome do arquivo e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c200b-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="c200b-131">Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="c200b-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="c200b-132">Clique em Nova conexão.</span><span class="sxs-lookup"><span data-stu-id="c200b-132">Click New Connection.</span></span> <span data-ttu-id="c200b-133">Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(LocalDB)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c200b-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="c200b-134">A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="c200b-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="c200b-135">Na caixa de diálogo escolher seu objeto de banco de dados, verifique as **tabelas** nó.</span><span class="sxs-lookup"><span data-stu-id="c200b-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="c200b-136">Isso adicionará todas as tabelas ao modelo **escolar** .</span><span class="sxs-lookup"><span data-stu-id="c200b-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="c200b-137">Clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="c200b-137">Click **Finish**.</span></span>
-   <span data-ttu-id="c200b-138">Em Gerenciador de Soluções, clique com o botão direito do mouse no arquivo **DefiningQueryModel. edmx** e selecione **abrir com...** .</span><span class="sxs-lookup"><span data-stu-id="c200b-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="c200b-139">Selecione **Editor de XML (texto)** .</span><span class="sxs-lookup"><span data-stu-id="c200b-139">Select **XML (Text) Editor**.</span></span>

    ![Editor de XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="c200b-141">Clique em **Sim** se receber a mensagem a seguir:</span><span class="sxs-lookup"><span data-stu-id="c200b-141">Click **Yes** if prompted with the following message:</span></span>

    ![Aviso 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="c200b-143">Adicionar uma consulta de definição</span><span class="sxs-lookup"><span data-stu-id="c200b-143">Add a Defining Query</span></span>

<span data-ttu-id="c200b-144">Nesta etapa, usaremos o editor de XML para adicionar uma consulta de definição e um tipo de entidade à seção SSDL do arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="c200b-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="c200b-145">Adicione um elemento  **EntitySet** à seção SSDL do arquivo. edmx (linha 5 a 13).</span><span class="sxs-lookup"><span data-stu-id="c200b-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="c200b-146">Especifique o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c200b-146">Specify the following:</span></span>
    -   <span data-ttu-id="c200b-147">Somente o **nome** e **EntityType** atributos do elemento  **EntitySet** são especificados.</span><span class="sxs-lookup"><span data-stu-id="c200b-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="c200b-148">O nome totalmente qualificado do tipo de entidade é usado no atributo  **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="c200b-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="c200b-149">A instrução SQL a ser executada é especificada no elemento  **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="c200b-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="c200b-150">Adicione o elemento **EntityType** à seção SSDL do. edmx.</span><span class="sxs-lookup"><span data-stu-id="c200b-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="c200b-151">arquivo, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c200b-151">file as shown below.</span></span> <span data-ttu-id="c200b-152">Observe o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c200b-152">Note the following:</span></span>
    -   <span data-ttu-id="c200b-153">O valor do atributo **Name** corresponde ao valor do atributo **EntityType** no elemento **EntitySet** acima, embora o nome totalmente qualificado do tipo de entidade seja usado no atributo **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="c200b-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="c200b-154">Os nomes de propriedade correspondem aos nomes de coluna retornados pela instrução SQL no elemento **DefiningQuery** (acima).</span><span class="sxs-lookup"><span data-stu-id="c200b-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="c200b-155">Neste exemplo, a chave de entidade é composta de três propriedades para garantir um valor de chave exclusivo.</span><span class="sxs-lookup"><span data-stu-id="c200b-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="c200b-156">Se posteriormente você executar a caixa de diálogo **Assistente de atualização de modelo** , as alterações feitas no modelo de armazenamento, incluindo a definição de consultas, serão substituídas.</span><span class="sxs-lookup"><span data-stu-id="c200b-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="c200b-157">Adicionar um tipo de entidade ao modelo</span><span class="sxs-lookup"><span data-stu-id="c200b-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="c200b-158">Nesta etapa, adicionaremos o tipo de entidade ao modelo conceitual usando o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="c200b-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span> <span data-ttu-id="c200b-159"> Observe o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c200b-159"> Note the following:</span></span>

-   <span data-ttu-id="c200b-160">O **nome** da entidade corresponde ao valor do atributo **EntityType** no elemento **EntitySet** acima.</span><span class="sxs-lookup"><span data-stu-id="c200b-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="c200b-161">Os nomes de propriedade correspondem aos nomes de coluna retornados pela instrução SQL no elemento **DefiningQuery** acima.</span><span class="sxs-lookup"><span data-stu-id="c200b-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="c200b-162">Neste exemplo, a chave de entidade é composta de três propriedades para garantir um valor de chave exclusivo.</span><span class="sxs-lookup"><span data-stu-id="c200b-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="c200b-163">Abra o modelo no designer do EF.</span><span class="sxs-lookup"><span data-stu-id="c200b-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="c200b-164">Clique duas vezes em DefiningQueryModel. edmx.</span><span class="sxs-lookup"><span data-stu-id="c200b-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="c200b-165">Digamos que **Sim** para a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="c200b-165">Say **Yes** to the following message:</span></span>

    ![Aviso 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="c200b-167">O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="c200b-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="c200b-168">Clique com o botão direito do mouse na superfície do designer e selecione **Adicionar nova**-&gt;**entidade...** .</span><span class="sxs-lookup"><span data-stu-id="c200b-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="c200b-169">Especifique **GradeReport** para o nome da entidade e **cursoid** para a **propriedade de chave**.</span><span class="sxs-lookup"><span data-stu-id="c200b-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="c200b-170">Clique com o botão direito do mouse na entidade **GradeReport** e selecione **adicionar nova**-&gt; **Propriedade escalar**.</span><span class="sxs-lookup"><span data-stu-id="c200b-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="c200b-171">Altere o nome padrão da propriedade para **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="c200b-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="c200b-172">Adicione outra propriedade escalar e especifique **LastName** para o nome.</span><span class="sxs-lookup"><span data-stu-id="c200b-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="c200b-173">Adicione outra propriedade escalar e especifique a **classificação** para o nome.</span><span class="sxs-lookup"><span data-stu-id="c200b-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="c200b-174">Na janela **Propriedades** , altere a propriedade **tipo** da **grade**para **decimal**.</span><span class="sxs-lookup"><span data-stu-id="c200b-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="c200b-175">Selecione as propriedades **FirstName** e **LastName** .</span><span class="sxs-lookup"><span data-stu-id="c200b-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="c200b-176">Na janela **Propriedades** , altere o valor da propriedade **EntityKey** para **true**.</span><span class="sxs-lookup"><span data-stu-id="c200b-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="c200b-177">Como resultado, os elementos a seguir foram adicionados à seção **CSDL** do arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="c200b-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="c200b-178">Mapear a consulta de definição para o tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="c200b-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="c200b-179">Nesta etapa, usaremos a janela de detalhes de mapeamento para mapear os tipos de entidade conceitual e de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="c200b-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="c200b-180">Clique com o botão direito do mouse na entidade **GradeReport** na superfície de design e selecione **mapeamento de tabela**.</span><span class="sxs-lookup"><span data-stu-id="c200b-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="c200b-181">A janela **detalhes do mapeamento** é exibida.</span><span class="sxs-lookup"><span data-stu-id="c200b-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="c200b-182">Selecione **GradeReport** na lista suspensa **&lt;adicionar uma tabela ou exibição&gt;** (localizada em **tabela**s).</span><span class="sxs-lookup"><span data-stu-id="c200b-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="c200b-183">Os mapeamentos padrão entre o tipo de entidade conceitual e de armazenamento **GradeReport** são exibidos.</span><span class="sxs-lookup"><span data-stu-id="c200b-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="c200b-184">Mapeamento de ![Details3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="c200b-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="c200b-185">Como resultado, o elemento de **EntitySetMapping** é adicionado à seção de mapeamento do arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="c200b-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="c200b-186">Compile o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c200b-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="c200b-187">Chamar a consulta de definição em seu código</span><span class="sxs-lookup"><span data-stu-id="c200b-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="c200b-188">Agora você pode executar a consulta de definição usando o tipo de entidade **GradeReport** .</span><span class="sxs-lookup"><span data-stu-id="c200b-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
