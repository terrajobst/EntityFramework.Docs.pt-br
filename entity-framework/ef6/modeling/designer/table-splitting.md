---
title: Divisão de tabela do designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921779"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="757ba-102">Divisão de tabela do designer</span><span class="sxs-lookup"><span data-stu-id="757ba-102">Designer Table Splitting</span></span>
<span data-ttu-id="757ba-103">Este tutorial mostra como mapear vários tipos de entidade para uma única tabela modificando um modelo com a Entity Framework Designer (Designer de EF).</span><span class="sxs-lookup"><span data-stu-id="757ba-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="757ba-104">Um motivo para usar a divisão de tabela é atrasar o carregamento de algumas propriedades ao usar o carregamento lento para carregar seus objetos.</span><span class="sxs-lookup"><span data-stu-id="757ba-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span><span data-ttu-id="757ba-105"> Você pode separar as propriedades que podem conter uma quantidade muito grande de dados em uma entidade separada e carregá-los apenas quando necessário.</span><span class="sxs-lookup"><span data-stu-id="757ba-105"> You can separate the properties that might contain very large amount of data into a separate entity and only load it when required.</span></span>

<span data-ttu-id="757ba-106">A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="757ba-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="757ba-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="757ba-108">Prerequisites</span></span>

<span data-ttu-id="757ba-109">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="757ba-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="757ba-110">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="757ba-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="757ba-111">O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="757ba-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="757ba-112">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="757ba-112">Set up the Project</span></span>

<span data-ttu-id="757ba-113">Este tutorial está usando o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="757ba-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="757ba-114">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="757ba-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="757ba-115">No menu **Arquivo**, aponte para **Novo** e clique em **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="757ba-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="757ba-116">No painel esquerdo, clique em Visual C\#e selecione o modelo aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="757ba-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="757ba-117">Insira **TableSplittingSample** como o nome do projeto e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="757ba-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="757ba-118">Criar um modelo com base no banco de dados escolar</span><span class="sxs-lookup"><span data-stu-id="757ba-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="757ba-119">Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**.</span><span class="sxs-lookup"><span data-stu-id="757ba-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="757ba-120">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.</span><span class="sxs-lookup"><span data-stu-id="757ba-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="757ba-121">Digite **TableSplittingModel. edmx** para o nome do arquivo e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="757ba-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="757ba-122">Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar.**</span><span class="sxs-lookup"><span data-stu-id="757ba-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="757ba-123">Clique em Nova conexão.</span><span class="sxs-lookup"><span data-stu-id="757ba-123">Click New Connection.</span></span> <span data-ttu-id="757ba-124">Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(\\LocalDB) mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="757ba-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="757ba-125">A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="757ba-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="757ba-126">Na caixa de diálogo escolher seu objeto de banco de dados, desdobrar o nó **tabelas** e verificar a tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="757ba-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="757ba-127">Isso adicionará a tabela especificada ao modelo **escolar** .</span><span class="sxs-lookup"><span data-stu-id="757ba-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="757ba-128">Clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="757ba-128">Click **Finish**.</span></span>

<span data-ttu-id="757ba-129">O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="757ba-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="757ba-130">Todos os objetos que você selecionou na caixa de diálogo **escolher seus objetos** de banco de dados são adicionados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="757ba-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="757ba-131">Mapear duas entidades para uma única tabela</span><span class="sxs-lookup"><span data-stu-id="757ba-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="757ba-132">Nesta seção, você irá dividir a entidade **Person** em duas entidades e, em seguida, mapeá-las para uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="757ba-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="757ba-133">A entidade **Person** não contém nenhuma propriedade que possa conter uma grande quantidade de dados; Ele é usado apenas como exemplo.</span><span class="sxs-lookup"><span data-stu-id="757ba-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="757ba-134">Clique com o botão direito do mouse em uma área vazia da superfície de design, aponte para **Adicionar nova**e clique em **entidade**.</span><span class="sxs-lookup"><span data-stu-id="757ba-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="757ba-135">A caixa de diálogo **nova entidade** é exibida.</span><span class="sxs-lookup"><span data-stu-id="757ba-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="757ba-136">Digite **HireInfo** para o **nome da entidade** e **PersonID** para o nome da **propriedade de chave** .</span><span class="sxs-lookup"><span data-stu-id="757ba-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="757ba-137">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="757ba-137">Click **OK**.</span></span>
-   <span data-ttu-id="757ba-138">Um novo tipo de entidade é criado e exibido na superfície de design.</span><span class="sxs-lookup"><span data-stu-id="757ba-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="757ba-139">Selecione a \*\*\*\*  Propriedade HireDate do tipo de entidade **Person** e pressione as teclas **Ctrl + X** .</span><span class="sxs-lookup"><span data-stu-id="757ba-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="757ba-140">Selecione a entidade **HireInfo** e pressione **Ctrl + V** teclas.</span><span class="sxs-lookup"><span data-stu-id="757ba-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="757ba-141">Crie uma associação entre **Person** e **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="757ba-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="757ba-142">Para fazer isso, clique com o botão direito do mouse em uma área vazia da superfície de design, aponte para **Adicionar nova**e clique em **Associação**.</span><span class="sxs-lookup"><span data-stu-id="757ba-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="757ba-143">A caixa de diálogo **Adicionar Associação** é exibida.</span><span class="sxs-lookup"><span data-stu-id="757ba-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="757ba-144">O nome do **PersonHireInfo** é fornecido por padrão.</span><span class="sxs-lookup"><span data-stu-id="757ba-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="757ba-145">Especifique a multiplicidade **1 (uma)** em ambas as extremidades da relação.</span><span class="sxs-lookup"><span data-stu-id="757ba-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="757ba-146">Pressione **OK**.</span><span class="sxs-lookup"><span data-stu-id="757ba-146">Press **OK**.</span></span>

<span data-ttu-id="757ba-147">A próxima etapa requer a janela **detalhes** do mapeamento.</span><span class="sxs-lookup"><span data-stu-id="757ba-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="757ba-148">Se você não puder ver essa janela, clique com o botão direito do mouse na superfície de design e selecione **detalhes de mapeamento**.</span><span class="sxs-lookup"><span data-stu-id="757ba-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="757ba-149">Selecione o tipo de entidade **HireInfo** e clique em **&lt;adicionar uma tabela&gt;ou exibição** na janela **detalhes** do mapeamento.</span><span class="sxs-lookup"><span data-stu-id="757ba-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="757ba-150">Selecione **pessoa** na lista suspensa  **&lt;adicionar uma tabela ou&gt;um**campo de exibição. </span><span class="sxs-lookup"><span data-stu-id="757ba-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="757ba-151">A lista contém tabelas ou exibições às quais a entidade selecionada pode ser mapeada.</span><span class="sxs-lookup"><span data-stu-id="757ba-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="757ba-152">As propriedades apropriadas devem ser mapeadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="757ba-152">The appropriate properties should be mapped by default.</span></span>

    ![correlação](~/ef6/media/mapping.png)

-   <span data-ttu-id="757ba-154">Selecione a associação **PersonHireInfo** na superfície de design.</span><span class="sxs-lookup"><span data-stu-id="757ba-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="757ba-155">Clique com o botão direito do mouse na associação na superfície de design e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="757ba-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="757ba-156">Na janela **Propriedades** , selecione a propriedade **restrições referenciais** e clique no botão reticências.</span><span class="sxs-lookup"><span data-stu-id="757ba-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="757ba-157">Selecione **pessoa** na lista suspensa **principal** .</span><span class="sxs-lookup"><span data-stu-id="757ba-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="757ba-158">Pressione **OK**.</span><span class="sxs-lookup"><span data-stu-id="757ba-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="757ba-159">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="757ba-159">Use the Model</span></span>

-   <span data-ttu-id="757ba-160">Cole o código a seguir no método Main.</span><span class="sxs-lookup"><span data-stu-id="757ba-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="757ba-161">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="757ba-161">Compile and run the application.</span></span>

<span data-ttu-id="757ba-162">As instruções T-SQL a seguir foram executadas no banco de dados **escolar** como resultado da execução deste aplicativo.</span><span class="sxs-lookup"><span data-stu-id="757ba-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="757ba-163">A **inserção** a seguir foi executada como resultado da execução do contexto. SaveChanges () e combina dados das entidades **Person** e **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="757ba-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Inserir](~/ef6/media/insert.png)

-   <span data-ttu-id="757ba-165">A **seleção** a seguir foi executada como resultado da execução do contexto. People. FirstOrDefault () e seleciona apenas as colunas mapeadas para **Person**</span><span class="sxs-lookup"><span data-stu-id="757ba-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Selecione 1](~/ef6/media/select1.png)

-   <span data-ttu-id="757ba-167">A **seleção** a seguir foi executada como resultado do acesso à propriedade de navegação ExistingPerson. instrutor e seleciona apenas as colunas mapeadas para **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="757ba-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Selecionar 2](~/ef6/media/select2.png)
