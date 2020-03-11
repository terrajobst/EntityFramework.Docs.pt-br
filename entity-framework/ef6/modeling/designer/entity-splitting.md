---
title: Divisão de entidade do designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418458"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="18024-102">Divisão de entidade do designer</span><span class="sxs-lookup"><span data-stu-id="18024-102">Designer Entity Splitting</span></span>
<span data-ttu-id="18024-103">Este tutorial mostra como mapear um tipo de entidade para duas tabelas modificando um modelo com o Entity Framework Designer (EF designer).</span><span class="sxs-lookup"><span data-stu-id="18024-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="18024-104">Você pode mapear uma entidade para várias tabelas quando as tabelas compartilham uma chave comum.</span><span class="sxs-lookup"><span data-stu-id="18024-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="18024-105">Os conceitos que se aplicam ao mapeamento de um tipo de entidade para duas tabelas são facilmente estendidos para mapear um tipo de entidade para mais de duas tabelas.</span><span class="sxs-lookup"><span data-stu-id="18024-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="18024-106">A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="18024-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="18024-108">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="18024-108">Prerequisites</span></span>

<span data-ttu-id="18024-109">Visual Studio 2012 ou Visual Studio 2010, Ultimate, Premium, Professional ou Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="18024-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="18024-110">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="18024-110">Create the Database</span></span>

<span data-ttu-id="18024-111">O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:</span><span class="sxs-lookup"><span data-stu-id="18024-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="18024-112">Se você estiver usando o Visual Studio 2012, criará um banco de dados LocalDB.</span><span class="sxs-lookup"><span data-stu-id="18024-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="18024-113">Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="18024-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="18024-114">Primeiro, criaremos um banco de dados com duas tabelas que vamos combinar em uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="18024-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="18024-115">Abra o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="18024-115">Open Visual Studio</span></span>
-   <span data-ttu-id="18024-116">**Exibir-&gt; Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="18024-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="18024-117">Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**</span><span class="sxs-lookup"><span data-stu-id="18024-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="18024-118">Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar **Microsoft SQL Server** como a fonte de dado</span><span class="sxs-lookup"><span data-stu-id="18024-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="18024-119">Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou</span><span class="sxs-lookup"><span data-stu-id="18024-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="18024-120">Insira **EntitySplitting** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="18024-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="18024-121">Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="18024-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="18024-122">O novo banco de dados aparecerá agora no Gerenciador de Servidores</span><span class="sxs-lookup"><span data-stu-id="18024-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="18024-123">Se você estiver usando o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="18024-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="18024-124">Clique com o botão direito do mouse no banco de dados em Gerenciador de Servidores e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="18024-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="18024-125">Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**</span><span class="sxs-lookup"><span data-stu-id="18024-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="18024-126">Se você estiver usando o Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="18024-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="18024-127">Selecione **Data-&gt; Editor Transact-SQL-&gt; nova conexão de consulta...**</span><span class="sxs-lookup"><span data-stu-id="18024-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="18024-128">Insira **.\\SQLExpress** como o nome do servidor e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="18024-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="18024-129">Selecione o banco de dados **EntitySplitting** na lista suspensa na parte superior do editor de consultas</span><span class="sxs-lookup"><span data-stu-id="18024-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="18024-130">Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **Executar SQL**</span><span class="sxs-lookup"><span data-stu-id="18024-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="18024-131">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="18024-131">Create the Project</span></span>

-   <span data-ttu-id="18024-132">No menu **Arquivo** , aponte para **Novo**e clique em **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="18024-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="18024-133">No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo **aplicativo de console** .</span><span class="sxs-lookup"><span data-stu-id="18024-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="18024-134">Insira **MapEntityToTablesSample** como o nome do projeto e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="18024-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="18024-135">Clique em **não** se for solicitado a salvar a consulta SQL criada na primeira seção.</span><span class="sxs-lookup"><span data-stu-id="18024-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="18024-136">Criar um modelo com base no banco de dados</span><span class="sxs-lookup"><span data-stu-id="18024-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="18024-137">Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**.</span><span class="sxs-lookup"><span data-stu-id="18024-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="18024-138">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.</span><span class="sxs-lookup"><span data-stu-id="18024-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="18024-139">Digite **MapEntityToTablesModel. edmx** para o nome do arquivo e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="18024-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="18024-140">Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar.**</span><span class="sxs-lookup"><span data-stu-id="18024-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="18024-141">Selecione a conexão **EntitySplitting** na lista suspensa e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="18024-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="18024-142">Na caixa de diálogo escolher seu objeto de banco de dados, marque a caixa ao lado do nó **tabelas** .</span><span class="sxs-lookup"><span data-stu-id="18024-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="18024-143">Isso adicionará todas as tabelas do banco de dados **EntitySplitting** ao modelo.</span><span class="sxs-lookup"><span data-stu-id="18024-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="18024-144">Clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="18024-144">Click **Finish**.</span></span>

<span data-ttu-id="18024-145">O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="18024-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="18024-146">Mapear uma entidade para duas tabelas</span><span class="sxs-lookup"><span data-stu-id="18024-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="18024-147">Nesta etapa, atualizaremos o tipo de entidade **Person** para combinar dados das tabelas **Person** e **personinfo** .</span><span class="sxs-lookup"><span data-stu-id="18024-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="18024-148">Selecione as propriedades de **email** e **telefone** da entidade **personinfo **e pressione **Ctrl + X** .</span><span class="sxs-lookup"><span data-stu-id="18024-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="18024-149">Selecione a entidade **Person **e pressione **Ctrl + V** teclas.</span><span class="sxs-lookup"><span data-stu-id="18024-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="18024-150">Na superfície de design, selecione a entidade **PersonInfo** e pressione o botão **excluir** no teclado.</span><span class="sxs-lookup"><span data-stu-id="18024-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="18024-151">Clique em **não** quando for perguntado se você deseja remover a tabela **personinfo** do modelo, estamos prestes a mapeá-la para a entidade **Person** .</span><span class="sxs-lookup"><span data-stu-id="18024-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Excluir tabelas](~/ef6/media/deletetables.png)

<span data-ttu-id="18024-153">As próximas etapas exigem a janela **detalhes do mapeamento** .</span><span class="sxs-lookup"><span data-stu-id="18024-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="18024-154">Se você não puder ver essa janela, clique com o botão direito do mouse na superfície de design e selecione **detalhes de mapeamento**.</span><span class="sxs-lookup"><span data-stu-id="18024-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="18024-155">Selecione o tipo de entidade **pessoa** e clique em **&lt;adicionar uma tabela ou exibição&gt;**  na janela  **detalhes do mapeamento** .</span><span class="sxs-lookup"><span data-stu-id="18024-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="18024-156">Selecione **PersonInfo ** na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="18024-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="18024-157">A janela de **detalhes de mapeamento** é atualizada com mapeamentos de coluna padrão, mas isso é bom para nosso cenário.</span><span class="sxs-lookup"><span data-stu-id="18024-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="18024-158">A **pessoa** tipo de entidade agora está mapeada para a **pessoa** e as tabelas de de **personinfo** .</span><span class="sxs-lookup"><span data-stu-id="18024-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapeamento 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="18024-160">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="18024-160">Use the Model</span></span>

-   <span data-ttu-id="18024-161">Cole o código a seguir no método Main.</span><span class="sxs-lookup"><span data-stu-id="18024-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="18024-162">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="18024-162">Compile and run the application.</span></span>

<span data-ttu-id="18024-163">As instruções T-SQL a seguir foram executadas no banco de dados como resultado da execução deste aplicativo.</span><span class="sxs-lookup"><span data-stu-id="18024-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="18024-164">As duas instruções **Insert** a seguir foram executadas como resultado da execução do contexto. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="18024-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="18024-165">Eles pegam os dados da entidade **Person** e os dividem entre as tabelas **Person** e **personinfo** .</span><span class="sxs-lookup"><span data-stu-id="18024-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Inserir 1](~/ef6/media/insert1.png)

    ![Inserir 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="18024-168">A **seleção** a seguir foi executada como resultado da enumeração das pessoas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="18024-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="18024-169">Ele combina os dados da tabela **Person** e **personinfo** .</span><span class="sxs-lookup"><span data-stu-id="18024-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Selecionar](~/ef6/media/select.png)
