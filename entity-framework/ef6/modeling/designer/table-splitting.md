---
title: A divisão de tabela Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
caps.latest.revision: 3
ms.openlocfilehash: 6ecf9a77f7c6a743e3902de65bb75349c6ef799f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119724"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="83001-102">Tabela do Designer de divisão</span><span class="sxs-lookup"><span data-stu-id="83001-102">Designer Table Splitting</span></span>
<span data-ttu-id="83001-103">Este passo a passo mostra como mapear vários tipos de entidade para uma única tabela, modificando um modelo com o Entity Framework Designer (Designer de EF).</span><span class="sxs-lookup"><span data-stu-id="83001-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="83001-104">Um motivo que você talvez queira usar a divisão de tabela está atrasando o carregamento de algumas propriedades ao usar o carregamento para carregar os objetos lento.</span><span class="sxs-lookup"><span data-stu-id="83001-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="83001-105">Você pode separar as propriedades que podem conter uma quantidade muito grande de dados em uma entidade separada e carregar apenas quando necessário.</span><span class="sxs-lookup"><span data-stu-id="83001-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="83001-106">A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="83001-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="83001-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="83001-108">Prerequisites</span></span>

<span data-ttu-id="83001-109">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="83001-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="83001-110">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83001-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="83001-111">O [banco de dados de exemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="83001-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="83001-112">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="83001-112">Set up the Project</span></span>

<span data-ttu-id="83001-113">Este passo a passo é usando o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="83001-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="83001-114">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="83001-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="83001-115">No menu **Arquivo**, aponte para **Novo** e clique em **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="83001-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="83001-116">No painel esquerdo, clique em Visual C\#e, em seguida, selecione o modelo de aplicativo de Console.</span><span class="sxs-lookup"><span data-stu-id="83001-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="83001-117">Insira **TableSplittingSample** como o nome do projeto e clique **Okey**.</span><span class="sxs-lookup"><span data-stu-id="83001-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="83001-118">Criar um modelo com base no banco de dados School</span><span class="sxs-lookup"><span data-stu-id="83001-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="83001-119">Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="83001-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="83001-120">Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.</span><span class="sxs-lookup"><span data-stu-id="83001-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="83001-121">Insira **TableSplittingModel.edmx** para o nome de arquivo e clique **Add**.</span><span class="sxs-lookup"><span data-stu-id="83001-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="83001-122">Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **Avançar.**</span><span class="sxs-lookup"><span data-stu-id="83001-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="83001-123">Clique em nova Conexão.</span><span class="sxs-lookup"><span data-stu-id="83001-123">Click New Connection.</span></span> <span data-ttu-id="83001-124">Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="83001-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="83001-125">A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="83001-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="83001-126">Na caixa de diálogo Choose Your Database Objects, desdobrar o **tabelas** nó e verifique se o **pessoa** tabela.</span><span class="sxs-lookup"><span data-stu-id="83001-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="83001-127">Isso adicionará a tabela especificada para o **School** modelo.</span><span class="sxs-lookup"><span data-stu-id="83001-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="83001-128">Clique em **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="83001-128">Click **Finish**.</span></span>

<span data-ttu-id="83001-129">O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="83001-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="83001-130">Todos os objetos que você selecionou na **Choose Your Database Objects** caixa de diálogo são adicionados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="83001-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="83001-131">Mapear duas entidades para uma única tabela</span><span class="sxs-lookup"><span data-stu-id="83001-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="83001-132">Nesta seção, você dividirá a **pessoa** entidade em duas entidades e, em seguida, mapeá-los para uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="83001-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="83001-133">O **pessoa** entidade não contém todas as propriedades que podem conter grandes quantidades de dados; ele é usado apenas como um exemplo.</span><span class="sxs-lookup"><span data-stu-id="83001-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="83001-134">Clique em uma área vazia da superfície de design, aponte para **adicionar novo**e clique em **entidade**.</span><span class="sxs-lookup"><span data-stu-id="83001-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="83001-135">O **nova entidade** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="83001-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="83001-136">Tipo de **HireInfo** para o **nome da entidade** e **PersonID** para o **propriedade Key** nome.</span><span class="sxs-lookup"><span data-stu-id="83001-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="83001-137">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="83001-137">Click **OK**.</span></span>
-   <span data-ttu-id="83001-138">Um novo tipo de entidade é criado e exibido na superfície de design.</span><span class="sxs-lookup"><span data-stu-id="83001-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="83001-139">Selecione o **HireDate** propriedade da **pessoa** tipo de entidade e pressione **Ctrl + X** chaves.</span><span class="sxs-lookup"><span data-stu-id="83001-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="83001-140">Selecione o **HireInfo** entidade e pressione **Ctrl + V** chaves.</span><span class="sxs-lookup"><span data-stu-id="83001-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="83001-141">Criar uma associação entre **pessoa** e **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="83001-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="83001-142">Para fazer isso, clique em uma área vazia da superfície de design, aponte para **adicionar novo**e clique em **associação**.</span><span class="sxs-lookup"><span data-stu-id="83001-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="83001-143">O **Adicionar associação** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="83001-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="83001-144">O **PersonHireInfo** nome for fornecido por padrão.</span><span class="sxs-lookup"><span data-stu-id="83001-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="83001-145">Especificar multiplicidade **1(One)** em ambas as extremidades da relação.</span><span class="sxs-lookup"><span data-stu-id="83001-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="83001-146">Pressione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="83001-146">Press **OK**.</span></span>

<span data-ttu-id="83001-147">A próxima etapa exige a **Mapping Details** janela.</span><span class="sxs-lookup"><span data-stu-id="83001-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="83001-148">Se você não vir essa janela, clique com botão direito a superfície de design e selecione **Mapping Details**.</span><span class="sxs-lookup"><span data-stu-id="83001-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="83001-149">Selecione o **HireInfo** tipo de entidade e clique em **&lt;adicionar uma tabela ou exibição&gt;** no **Mapping Details** janela.</span><span class="sxs-lookup"><span data-stu-id="83001-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="83001-150">Selecione **pessoa** da **&lt;adicionar uma tabela ou exibição&gt;** campo na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="83001-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="83001-151">A lista contém tabelas ou exibições para o qual a entidade selecionada pode ser mapeada.</span><span class="sxs-lookup"><span data-stu-id="83001-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="83001-152">As propriedades adequadas devem ser mapeadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="83001-152">The appropriate properties should be mapped by default.</span></span>

    ![Mapeamento](~/ef6/media/mapping.png)

-   <span data-ttu-id="83001-154">Selecione o **PersonHireInfo** associação na superfície de design.</span><span class="sxs-lookup"><span data-stu-id="83001-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="83001-155">Clique com botão direito a associação na superfície do design e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="83001-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="83001-156">No **propriedades** janela, selecione a **restrições referenciais** propriedade e clique no botão de reticências.</span><span class="sxs-lookup"><span data-stu-id="83001-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="83001-157">Selecione **pessoa** da **Principal** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="83001-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="83001-158">Pressione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="83001-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="83001-159">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="83001-159">Use the Model</span></span>

-   <span data-ttu-id="83001-160">Cole o seguinte código no método Main.</span><span class="sxs-lookup"><span data-stu-id="83001-160">Paste the following code in the Main method.</span></span>

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
-   <span data-ttu-id="83001-161">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="83001-161">Compile and run the application.</span></span>

<span data-ttu-id="83001-162">Instruções T-SQL a seguir foram executadas em relação a **School** banco de dados como resultado da execução desse aplicativo.</span><span class="sxs-lookup"><span data-stu-id="83001-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="83001-163">O seguinte **inserir** foi executado como resultado da execução de contexto. Dados de SaveChanges () e combina o **pessoa** e **HireInfo** entidades</span><span class="sxs-lookup"><span data-stu-id="83001-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Inserir](~/ef6/media/insert.png)

-   <span data-ttu-id="83001-165">O seguinte **selecionar** foi executado como resultado da execução de contexto. People.FirstOrDefault() e seleciona apenas as colunas mapeadas para **pessoa**</span><span class="sxs-lookup"><span data-stu-id="83001-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Select1](~/ef6/media/select1.png)

-   <span data-ttu-id="83001-167">O seguinte **selecionar** foi executado como resultado de acessar o existingPerson.Instructor de propriedade de navegação e seleciona apenas as colunas mapeadas para **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="83001-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Select2](~/ef6/media/select2.png)
