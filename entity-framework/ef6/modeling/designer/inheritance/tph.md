---
title: Herança TPH Designer – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
caps.latest.revision: 3
ms.openlocfilehash: 0a017d3b97808cede3134119940b2e5839d0f282
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119824"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="21170-102">Herança TPH Designer</span><span class="sxs-lookup"><span data-stu-id="21170-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="21170-103">Este passo a passo mostra como implementar a herança do tabela por hierarquia (TPH) em seu modelo conceitual com o Entity Framework Designer (Designer de EF).</span><span class="sxs-lookup"><span data-stu-id="21170-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="21170-104">Herança TPH usa uma tabela de banco de dados para manter os dados para todos os tipos de entidade em uma hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="21170-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="21170-105">Neste passo a passo, podemos mapeará a tabela Person para três tipos de entidade: pessoa (o tipo base), (deriva de pessoa) de aluno e instrutor (deriva de pessoa).</span><span class="sxs-lookup"><span data-stu-id="21170-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="21170-106">Vamos criar um modelo conceitual do banco de dados (banco de dados primeiro) e, em seguida, alterar o modelo para implementar a herança TPH usando o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="21170-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="21170-107">É possível mapear para uma herança TPH usando Model First, mas você precisaria escrever seu próprio fluxo de trabalho de geração de banco de dados que é complexo.</span><span class="sxs-lookup"><span data-stu-id="21170-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="21170-108">Em seguida, atribua esse fluxo de trabalho para o **fluxo de trabalho de geração de banco de dados** propriedade no EF Designer.</span><span class="sxs-lookup"><span data-stu-id="21170-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="21170-109">Uma alternativa mais fácil é usar o Code First.</span><span class="sxs-lookup"><span data-stu-id="21170-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="21170-110">Outras opções de herança</span><span class="sxs-lookup"><span data-stu-id="21170-110">Other Inheritance Options</span></span>

<span data-ttu-id="21170-111">Tabela por tipo (TPT) é outro tipo de herança em que tabelas separadas no banco de dados são mapeadas para entidades que participam de herança.</span><span class="sxs-lookup"><span data-stu-id="21170-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="21170-112">Para obter informações sobre como mapear a herança de tabela por tipo com o EF Designer, consulte [EF Designer TPT herança](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="21170-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="21170-113">Herança de tipo de tabela por concreto (TPC) e modelos de herança misto são compatíveis com o tempo de execução do Entity Framework, mas não são compatíveis com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="21170-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="21170-114">Se você quiser usar TPC ou herança mista, você tem duas opções: usar o Code First ou editar manualmente o arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="21170-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="21170-115">Se você optar por trabalhar com o arquivo EDMX, a janela de detalhes de mapeamento será colocada em "modo de segurança" e você não poderá usar o designer para alterar os mapeamentos.</span><span class="sxs-lookup"><span data-stu-id="21170-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21170-116">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="21170-116">Prerequisites</span></span>

<span data-ttu-id="21170-117">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="21170-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="21170-118">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="21170-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="21170-119">O [banco de dados de exemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="21170-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="21170-120">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="21170-120">Set up the Project</span></span>

-   <span data-ttu-id="21170-121">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="21170-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="21170-122">Selecione **arquivo -&gt; New -&gt; projeto**</span><span class="sxs-lookup"><span data-stu-id="21170-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="21170-123">No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo.</span><span class="sxs-lookup"><span data-stu-id="21170-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="21170-124">Insira **TPHDBFirstSample** como o nome.</span><span class="sxs-lookup"><span data-stu-id="21170-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="21170-125">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="21170-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="21170-126">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="21170-126">Create a Model</span></span>

-   <span data-ttu-id="21170-127">O nome do projeto no Gerenciador de soluções e selecione **Add -&gt; Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="21170-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="21170-128">Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.</span><span class="sxs-lookup"><span data-stu-id="21170-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="21170-129">Insira **TPHModel.edmx** para o nome de arquivo e clique **Add**.</span><span class="sxs-lookup"><span data-stu-id="21170-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="21170-130">Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="21170-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="21170-131">Clique em **nova Conexão**.</span><span class="sxs-lookup"><span data-stu-id="21170-131">Click **New Connection**.</span></span>
    <span data-ttu-id="21170-132">Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="21170-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="21170-133">A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21170-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="21170-134">Na caixa de diálogo Choose Your Database Objects, sob o nó de tabelas, selecione a **pessoa** tabela.</span><span class="sxs-lookup"><span data-stu-id="21170-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="21170-135">Clique em **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="21170-135">Click **Finish**.</span></span>

<span data-ttu-id="21170-136">O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="21170-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="21170-137">Todos os objetos que você selecionou na caixa de diálogo Choose Your Database Objects são adicionados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="21170-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="21170-138">Isto é como o **pessoa** tabela fica no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="21170-138">That is how the **Person** table looks in the database.</span></span>

![PersonTable](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="21170-140">Implementar a herança de tabela por hierarquia</span><span class="sxs-lookup"><span data-stu-id="21170-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="21170-141">O **pessoa** a tabela tem o **discriminador** coluna, que pode ter um dos dois valores: "Aluno" e "Instrutor".</span><span class="sxs-lookup"><span data-stu-id="21170-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="21170-142">Dependendo do valor de **pessoa** tabela será mapeada para o **aluno** entidade ou o **instrutor** entidade.</span><span class="sxs-lookup"><span data-stu-id="21170-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="21170-143">O **pessoa** tabela também tem duas colunas, **HireDate** e **EnrollmentDate**, que deve ser **anulável** porque uma pessoa não pode ser um aluno e instrutor ao mesmo tempo (pelo menos não neste passo a passo).</span><span class="sxs-lookup"><span data-stu-id="21170-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="21170-144">Adicionar novas entidades</span><span class="sxs-lookup"><span data-stu-id="21170-144">Add new Entities</span></span>

-   <span data-ttu-id="21170-145">Adicione uma nova entidade.</span><span class="sxs-lookup"><span data-stu-id="21170-145">Add a new entity.</span></span>
    <span data-ttu-id="21170-146">Para fazer isso, clique com botão direito em uma área vazia da superfície de design do Entity Framework Designer e selecione **Add -&gt;entidade**.</span><span class="sxs-lookup"><span data-stu-id="21170-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="21170-147">Tipo de **instrutor** para o **nome da entidade** e selecione **pessoa** na lista suspensa para o **tipo Base**.</span><span class="sxs-lookup"><span data-stu-id="21170-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="21170-148">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="21170-148">Click **OK**.</span></span>
-   <span data-ttu-id="21170-149">Adicione outra nova entidade.</span><span class="sxs-lookup"><span data-stu-id="21170-149">Add another new entity.</span></span> <span data-ttu-id="21170-150">Tipo de **aluno** para o **nome da entidade** e selecione **pessoa** na lista suspensa para o **tipo Base**.</span><span class="sxs-lookup"><span data-stu-id="21170-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="21170-151">Dois novos tipos de entidade foram adicionados à superfície de design.</span><span class="sxs-lookup"><span data-stu-id="21170-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="21170-152">Uma seta aponta de novos tipos de entidade para o **pessoa** tipo de entidade; isso indica que **pessoa** é o tipo base para os novos tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="21170-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="21170-153">Clique com botão direito do **HireDate** propriedade do **pessoa** entidade.</span><span class="sxs-lookup"><span data-stu-id="21170-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="21170-154">Selecione **Recortar** (ou use a tecla Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="21170-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="21170-155">Clique com botão direito do **instrutor** entidade e selecione **colar** (ou use a tecla Ctrl + V).</span><span class="sxs-lookup"><span data-stu-id="21170-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="21170-156">Clique com botão direito do **HireDate** propriedade e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="21170-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="21170-157">No **propriedades** janela, defina as **Nullable** propriedade a ser **false**.</span><span class="sxs-lookup"><span data-stu-id="21170-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="21170-158">Clique com botão direito do **EnrollmentDate** propriedade do **pessoa** entidade.</span><span class="sxs-lookup"><span data-stu-id="21170-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="21170-159">Selecione **Recortar** (ou use a tecla Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="21170-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="21170-160">Clique com botão direito do **aluno** entidade e selecione **Colar (ou chave de uso de Ctrl + V).**</span><span class="sxs-lookup"><span data-stu-id="21170-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="21170-161">Selecione o **EnrollmentDate** propriedade e defina o **Nullable** propriedade a ser **false**.</span><span class="sxs-lookup"><span data-stu-id="21170-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="21170-162">Selecione o **pessoa** tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="21170-162">Select the **Person** entity type.</span></span> <span data-ttu-id="21170-163">No **propriedades** janela, defina seu **abstrata** propriedade a ser **verdadeiro**.</span><span class="sxs-lookup"><span data-stu-id="21170-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="21170-164">Excluir o **discriminador** propriedade de **pessoa**.</span><span class="sxs-lookup"><span data-stu-id="21170-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="21170-165">O motivo pelo qual que ele deve ser excluído é explicado na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="21170-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="21170-166">As entidades do mapa</span><span class="sxs-lookup"><span data-stu-id="21170-166">Map the entities</span></span>

-   <span data-ttu-id="21170-167">Clique com botão direito do **instrutor** e selecione **mapeamento de tabela.**</span><span class="sxs-lookup"><span data-stu-id="21170-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="21170-168">A entidade Instructor é selecionada na janela de detalhes de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="21170-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="21170-169">Clique em **&lt;adicionar uma tabela ou exibição&gt;** no **Mapping Details** janela.</span><span class="sxs-lookup"><span data-stu-id="21170-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="21170-170">O **&lt;adicionar uma tabela ou exibição&gt;** campo se torna uma lista suspensa de tabelas ou exibições para o qual a entidade selecionada pode ser mapeada.</span><span class="sxs-lookup"><span data-stu-id="21170-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="21170-171">Selecione **pessoa** na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="21170-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="21170-172">O **Mapping Details** janela é atualizada com mapeamentos de coluna padrão e uma opção para adicionar uma condição.</span><span class="sxs-lookup"><span data-stu-id="21170-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="21170-173">Clique em  **&lt;adicionar uma condição&gt;**.</span><span class="sxs-lookup"><span data-stu-id="21170-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="21170-174">O **&lt;adicionar uma condição&gt;** campo se torna uma lista suspensa de colunas para o qual as condições podem ser definidas.</span><span class="sxs-lookup"><span data-stu-id="21170-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="21170-175">Selecione **discriminador** na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="21170-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="21170-176">No **operador** coluna o **Mapping Details** janela, selecione = na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="21170-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="21170-177">No **propriedade/valor** coluna, digite **instrutor**.</span><span class="sxs-lookup"><span data-stu-id="21170-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="21170-178">O resultado final deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="21170-178">The end result should look like this:</span></span>

    ![MappingDetails2](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="21170-180">Repita essas etapas para o **aluno** tipo de entidade, mas verifique a condição igual a **aluno** valor.</span><span class="sxs-lookup"><span data-stu-id="21170-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="21170-181">*O motivo pelo qual desejamos remover os **discriminador** propriedade, é porque você não pode mapear uma coluna de tabela mais de uma vez. Esta coluna será usada para mapeamento condicional, então ele não pode ser usado para mapeamento de propriedade também. A única maneira que ele pode ser usado para ambos, se uma condição usa um **é nulo** ou **Is Not Null** comparação.*</span><span class="sxs-lookup"><span data-stu-id="21170-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="21170-182">Herança de tabela por hierarquia agora é implementada.</span><span class="sxs-lookup"><span data-stu-id="21170-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![FinalTPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="21170-184">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="21170-184">Use the Model</span></span>

<span data-ttu-id="21170-185">Abra o **Program.cs** do arquivo onde o **Main** método é definido.</span><span class="sxs-lookup"><span data-stu-id="21170-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="21170-186">Cole o seguinte código para o **Main** função.</span><span class="sxs-lookup"><span data-stu-id="21170-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="21170-187">O código é executado três consultas.</span><span class="sxs-lookup"><span data-stu-id="21170-187">The code executes three queries.</span></span> <span data-ttu-id="21170-188">A primeira consulta traz de volta todos os **pessoa** objetos.</span><span class="sxs-lookup"><span data-stu-id="21170-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="21170-189">A segunda consulta usa a **OfType** método retorne **instrutor** objetos.</span><span class="sxs-lookup"><span data-stu-id="21170-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="21170-190">A terceira consulta usa a **OfType** método retorne **aluno** objetos.</span><span class="sxs-lookup"><span data-stu-id="21170-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
