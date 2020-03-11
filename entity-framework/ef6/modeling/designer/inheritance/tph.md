---
title: Herança de TPH do designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418423"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="ce45f-102">Herança de TPH do designer</span><span class="sxs-lookup"><span data-stu-id="ce45f-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="ce45f-103">Este guia passo a passo mostra como implementar a herança de tabela por hierarquia (TPH) em seu modelo conceitual com o Entity Framework Designer (EF designer).</span><span class="sxs-lookup"><span data-stu-id="ce45f-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="ce45f-104">A herança de TPH usa uma tabela de banco de dados para manter todos os tipos de entidade em uma hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="ce45f-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="ce45f-105">Neste tutorial, mapearemos a tabela Person para três tipos de entidade: Person (o tipo base), Student (derivada de Person) e instrutor (derivado de Person).</span><span class="sxs-lookup"><span data-stu-id="ce45f-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="ce45f-106">Vamos criar um modelo conceitual do banco de dados (Database First) e, em seguida, alterar o modelo para implementar a herança TPH usando o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="ce45f-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="ce45f-107">É possível mapear para uma herança TPH usando Model First, mas você teria que escrever seu próprio fluxo de trabalho de geração de banco de dados, que é complexo.</span><span class="sxs-lookup"><span data-stu-id="ce45f-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="ce45f-108">Em seguida, você atribuiria esse fluxo de trabalho à propriedade de **fluxo de trabalho de geração de banco de dados** no designer do EF.</span><span class="sxs-lookup"><span data-stu-id="ce45f-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="ce45f-109">Uma alternativa mais fácil é usar Code First.</span><span class="sxs-lookup"><span data-stu-id="ce45f-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="ce45f-110">Outras opções de herança</span><span class="sxs-lookup"><span data-stu-id="ce45f-110">Other Inheritance Options</span></span>

<span data-ttu-id="ce45f-111">A tabela por tipo (TPT) é outro tipo de herança no qual as tabelas separadas no banco de dados são mapeadas para entidades que participam da herança.</span><span class="sxs-lookup"><span data-stu-id="ce45f-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="ce45f-112"> Para obter informações sobre como mapear a herança de tabela por tipo com o designer do EF, consulte [herança do TPT do EF designer](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="ce45f-112"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="ce45f-113">A herança de tipo de tabela por concreto (TPC) e os modelos de herança misto têm suporte pelo tempo de execução Entity Framework, mas não têm suporte do designer do EF.</span><span class="sxs-lookup"><span data-stu-id="ce45f-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="ce45f-114">Se você quiser usar o TPC ou a herança mista, terá duas opções: Use Code First ou edite manualmente o arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="ce45f-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="ce45f-115">Se você optar por trabalhar com o arquivo EDMX, a janela de detalhes de mapeamento será colocada em "modo de segurança" e você não poderá usar o designer para alterar os mapeamentos.</span><span class="sxs-lookup"><span data-stu-id="ce45f-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce45f-116">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="ce45f-116">Prerequisites</span></span>

<span data-ttu-id="ce45f-117">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="ce45f-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="ce45f-118">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ce45f-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="ce45f-119">O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="ce45f-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ce45f-120">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="ce45f-120">Set up the Project</span></span>

-   <span data-ttu-id="ce45f-121">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="ce45f-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="ce45f-122">Selecione **arquivo-&gt; projeto de novo&gt;**</span><span class="sxs-lookup"><span data-stu-id="ce45f-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="ce45f-123">No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="ce45f-124">Insira **TPHDBFirstSample** como o nome.</span><span class="sxs-lookup"><span data-stu-id="ce45f-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="ce45f-125">Selecione  **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="ce45f-126">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="ce45f-126">Create a Model</span></span>

-   <span data-ttu-id="ce45f-127">Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e selecione **Adicionar-&gt; novo item**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="ce45f-128">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.</span><span class="sxs-lookup"><span data-stu-id="ce45f-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="ce45f-129">Digite **TPHModel. edmx** para o nome do arquivo e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="ce45f-130">Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="ce45f-131">Clique em **nova conexão**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-131">Click **New Connection**.</span></span>
    <span data-ttu-id="ce45f-132">Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(LocalDB)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="ce45f-133">A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="ce45f-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="ce45f-134">Na caixa de diálogo escolher seu objeto de banco de dados, no nó tabelas, selecione a tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="ce45f-135">Clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-135">Click **Finish**.</span></span>

<span data-ttu-id="ce45f-136">O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="ce45f-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="ce45f-137">Todos os objetos que você selecionou na caixa de diálogo escolher seus objetos de banco de dados são adicionados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="ce45f-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="ce45f-138">É assim que a tabela **Person** procura no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ce45f-138">That is how the **Person** table looks in the database.</span></span>

![Tabela Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="ce45f-140">Implementar a herança de tabela por hierarquia</span><span class="sxs-lookup"><span data-stu-id="ce45f-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="ce45f-141">A tabela **Person** tem a coluna **discriminadora** , que pode ter um dos dois valores: "Student" e "instrutor".</span><span class="sxs-lookup"><span data-stu-id="ce45f-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="ce45f-142">Dependendo do valor, a tabela **Person** será mapeada para a entidade **Student** ou o **instrutor** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="ce45f-143">A tabela **Person** também tem duas colunas, **contratou** e **EnrollmentDate**, que devem ser **anuláveis** porque uma pessoa não pode ser um aluno e um instrutor ao mesmo tempo (pelo menos não neste passo-a).</span><span class="sxs-lookup"><span data-stu-id="ce45f-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="ce45f-144">Adicionar novas entidades</span><span class="sxs-lookup"><span data-stu-id="ce45f-144">Add new Entities</span></span>

-   <span data-ttu-id="ce45f-145">Adicione uma nova entidade.</span><span class="sxs-lookup"><span data-stu-id="ce45f-145">Add a new entity.</span></span>
    <span data-ttu-id="ce45f-146">Para fazer isso, clique com o botão direito do mouse em um espaço vazio da superfície de design do Entity Framework Designer e selecione **entidade Add-&gt;** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="ce45f-147">Digite do **instrutor** para o **nome da entidade** e selecione **pessoa** na lista suspensa para o **tipo base**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="ce45f-148">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-148">Click **OK**.</span></span>
-   <span data-ttu-id="ce45f-149">Adicione outra nova entidade.</span><span class="sxs-lookup"><span data-stu-id="ce45f-149">Add another new entity.</span></span> <span data-ttu-id="ce45f-150">Digite **Student** para o **nome da entidade** e selecione **Person** na lista suspensa para o **tipo base**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="ce45f-151">Dois novos tipos de entidade foram adicionados à superfície de design.</span><span class="sxs-lookup"><span data-stu-id="ce45f-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="ce45f-152">Uma seta aponta dos novos tipos de entidade para a **pessoa** tipo de entidade; Isso indica que a **pessoa** é o tipo base para os novos tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="ce45f-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="ce45f-153">Clique com o botão direito do mouse na propriedade **Contratada** da entidade **Person** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="ce45f-154">Selecione **recortar** (ou use a tecla CTRL-X).</span><span class="sxs-lookup"><span data-stu-id="ce45f-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="ce45f-155">Clique com o botão direito do mouse na entidade do **instrutor** e selecione **colar** (ou use a tecla Ctrl-V).</span><span class="sxs-lookup"><span data-stu-id="ce45f-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="ce45f-156">Clique com o botão direito do mouse na propriedade **Contratada** e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="ce45f-157">Na janela de **Propriedades** , defina a propriedade **Nullable** como **false**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="ce45f-158">Clique com o botão direito do mouse na propriedade **EnrollmentDate** da entidade **Person** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="ce45f-159">Selecione **recortar** (ou use a tecla CTRL-X).</span><span class="sxs-lookup"><span data-stu-id="ce45f-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="ce45f-160">Clique com o botão direito do mouse na entidade **Student** e selecione **colar (ou use a tecla Ctrl-V).**</span><span class="sxs-lookup"><span data-stu-id="ce45f-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="ce45f-161">Selecione a propriedade de  **EnrollmentDate** e defina a propriedade **Nullable** como **false**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="ce45f-162">Selecione o tipo de entidade  **pessoa** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-162">Select the **Person** entity type.</span></span> <span data-ttu-id="ce45f-163">Na janela de **Propriedades** , defina sua propriedade **abstrata** como **true**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="ce45f-164">Exclua a propriedade **discriminadora** de **Person**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="ce45f-165">O motivo pelo qual ele deve ser excluído é explicado na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="ce45f-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="ce45f-166">Mapear as entidades</span><span class="sxs-lookup"><span data-stu-id="ce45f-166">Map the entities</span></span>

-   <span data-ttu-id="ce45f-167">Clique com o botão direito do mouse no **instrutor** e selecione **mapeamento de tabela.**</span><span class="sxs-lookup"><span data-stu-id="ce45f-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="ce45f-168">A entidade do instrutor é selecionada na janela detalhes do mapeamento.</span><span class="sxs-lookup"><span data-stu-id="ce45f-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="ce45f-169">Clique **&lt;adicionar uma tabela ou exibição&gt;**  na janela **detalhes do mapeamento** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="ce45f-170">O **&lt;adicionar uma tabela ou exibição&gt;** campo se torna uma lista suspensa de tabelas ou exibições para as quais a entidade selecionada pode ser mapeada.</span><span class="sxs-lookup"><span data-stu-id="ce45f-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="ce45f-171">Selecione **pessoa** na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="ce45f-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="ce45f-172">A janela **detalhes do mapeamento** é atualizada com mapeamentos de coluna padrão e uma opção para adicionar uma condição.</span><span class="sxs-lookup"><span data-stu-id="ce45f-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="ce45f-173">Clique em **&lt;adicionar uma condição&gt;** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="ce45f-174">O **&lt;adicionar uma condição&gt;** campo se torna uma lista suspensa de colunas para as quais as condições podem ser definidas.</span><span class="sxs-lookup"><span data-stu-id="ce45f-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="ce45f-175">Selecione  **discriminador** na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="ce45f-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="ce45f-176">Na coluna **operador** da janela **detalhes do mapeamento** , selecione = na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="ce45f-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="ce45f-177">Na coluna **valor/Propriedade** , digite **instrutor**.</span><span class="sxs-lookup"><span data-stu-id="ce45f-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="ce45f-178">O resultado final deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="ce45f-178">The end result should look like this:</span></span>

    ![Detalhes do mapeamento](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="ce45f-180">Repita essas etapas para o tipo de entidade **student** , mas torne a condição igual ao valor **Student** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="ce45f-181">*O motivo pelo qual queríamos remover a propriedade **discriminador** é porque você não pode mapear uma coluna de tabela mais de uma vez. Esta coluna será usada para mapeamento condicional e, portanto, não poderá ser usada para mapeamento de propriedade também. A única maneira de poder ser usada para ambos, se uma condição usar uma  \*\*nula*\* ou **não for nula** comparação.\*</span><span class="sxs-lookup"><span data-stu-id="ce45f-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="ce45f-182">A herança de tabela por hierarquia agora está implementada.</span><span class="sxs-lookup"><span data-stu-id="ce45f-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![TPH final](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="ce45f-184">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="ce45f-184">Use the Model</span></span>

<span data-ttu-id="ce45f-185">Abra o arquivo **Program.cs** em que o método **Main** está definido.</span><span class="sxs-lookup"><span data-stu-id="ce45f-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="ce45f-186">Cole o código a seguir na função **Main** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="ce45f-187">O código executa três consultas.</span><span class="sxs-lookup"><span data-stu-id="ce45f-187">The code executes three queries.</span></span> <span data-ttu-id="ce45f-188">A primeira consulta traz de volta todos os objetos **Person** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="ce45f-189">A segunda consulta usa o método **OfType** para retornar objetos do **instrutor** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="ce45f-190">A terceira consulta usa o método **OfType** para retornar objetos **Student** .</span><span class="sxs-lookup"><span data-stu-id="ce45f-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
