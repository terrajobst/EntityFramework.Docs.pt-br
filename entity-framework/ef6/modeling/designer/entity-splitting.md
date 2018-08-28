---
title: Separação da entidade Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 214561f0a0381bced3ceae0b6acfcd45f5dd65c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995613"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="15610-102">Separação da entidade Designer</span><span class="sxs-lookup"><span data-stu-id="15610-102">Designer Entity Splitting</span></span>
<span data-ttu-id="15610-103">Este passo a passo mostra como mapear um tipo de entidade para duas tabelas, modificando um modelo com o Entity Framework Designer (Designer de EF).</span><span class="sxs-lookup"><span data-stu-id="15610-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="15610-104">Você pode mapear uma entidade para várias tabelas quando as tabelas compartilham uma chave comum.</span><span class="sxs-lookup"><span data-stu-id="15610-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="15610-105">Os conceitos que se aplicam a um tipo de entidade de mapeamento com duas tabelas são facilmente estendidos para mapear um tipo de entidade para mais de duas tabelas.</span><span class="sxs-lookup"><span data-stu-id="15610-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="15610-106">A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="15610-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="15610-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="15610-108">Prerequisites</span></span>

<span data-ttu-id="15610-109">O Visual Studio 2012 ou Visual Studio 2010, Ultimate, Premium, Professional ou Web Express edition.</span><span class="sxs-lookup"><span data-stu-id="15610-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="15610-110">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="15610-110">Create the Database</span></span>

<span data-ttu-id="15610-111">O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:</span><span class="sxs-lookup"><span data-stu-id="15610-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="15610-112">Se você estiver usando o Visual Studio 2012, em seguida, você criará um banco de dados LocalDB.</span><span class="sxs-lookup"><span data-stu-id="15610-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="15610-113">Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="15610-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="15610-114">Primeiro, criaremos um banco de dados com duas tabelas, vamos combinar em uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="15610-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="15610-115">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15610-115">Open Visual Studio</span></span>
-   <span data-ttu-id="15610-116">**Exibição -&gt; Gerenciador de servidores**</span><span class="sxs-lookup"><span data-stu-id="15610-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="15610-117">Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**</span><span class="sxs-lookup"><span data-stu-id="15610-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="15610-118">Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar **Microsoft SQL Server** como fonte de dados</span><span class="sxs-lookup"><span data-stu-id="15610-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="15610-119">Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado</span><span class="sxs-lookup"><span data-stu-id="15610-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="15610-120">Insira **EntitySplitting** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="15610-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="15610-121">Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="15610-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="15610-122">O novo banco de dados agora aparecerão no Gerenciador de servidores</span><span class="sxs-lookup"><span data-stu-id="15610-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="15610-123">Se você estiver usando o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="15610-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="15610-124">Clique com botão direito no banco de dados no Gerenciador de servidores e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="15610-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="15610-125">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**</span><span class="sxs-lookup"><span data-stu-id="15610-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="15610-126">Se você estiver usando o Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="15610-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="15610-127">Selecione **dados -&gt; Transact SQL Editor -&gt; nova Conexão de consulta...**</span><span class="sxs-lookup"><span data-stu-id="15610-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="15610-128">Insira **.\\ SQLEXPRESS** como o nome do servidor e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="15610-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="15610-129">Selecione o **EntitySplitting** de banco de dados na lista suspensa na parte superior do editor de consultas</span><span class="sxs-lookup"><span data-stu-id="15610-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="15610-130">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **executar SQL**</span><span class="sxs-lookup"><span data-stu-id="15610-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="15610-131">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="15610-131">Create the Project</span></span>

-   <span data-ttu-id="15610-132">No menu **Arquivo**, aponte para **Novo** e clique em **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="15610-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="15610-133">No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **aplicativo de Console** modelo.</span><span class="sxs-lookup"><span data-stu-id="15610-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="15610-134">Insira **MapEntityToTablesSample** como o nome do projeto e clique **Okey**.</span><span class="sxs-lookup"><span data-stu-id="15610-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="15610-135">Clique em **não** se solicitado a salvar a consulta SQL criada na primeira seção.</span><span class="sxs-lookup"><span data-stu-id="15610-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="15610-136">Criar um modelo baseado no banco de dados</span><span class="sxs-lookup"><span data-stu-id="15610-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="15610-137">Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="15610-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="15610-138">Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.</span><span class="sxs-lookup"><span data-stu-id="15610-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="15610-139">Insira **MapEntityToTablesModel.edmx** para o nome de arquivo e clique **Add**.</span><span class="sxs-lookup"><span data-stu-id="15610-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="15610-140">Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **Avançar.**</span><span class="sxs-lookup"><span data-stu-id="15610-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="15610-141">Selecione o **EntitySplitting** conexão na lista suspensa e clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="15610-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="15610-142">Na caixa de diálogo Choose Your Database Objects, marque a caixa ao lado de **tabelas** nó.</span><span class="sxs-lookup"><span data-stu-id="15610-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="15610-143">Isso adicionará todas as tabelas do **EntitySplitting** banco de dados para o modelo.</span><span class="sxs-lookup"><span data-stu-id="15610-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="15610-144">Clique em **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="15610-144">Click **Finish**.</span></span>

<span data-ttu-id="15610-145">O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="15610-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="15610-146">Mapear uma entidade para duas tabelas</span><span class="sxs-lookup"><span data-stu-id="15610-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="15610-147">Nesta etapa, atualizaremos o **pessoa** tipo de entidade para combinar dados do **pessoa** e **PersonInfo** tabelas.</span><span class="sxs-lookup"><span data-stu-id="15610-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="15610-148">Selecione o **Email** e **telefone** propriedades do * * PersonInfo * * entity e pressione **Ctrl + X** chaves.</span><span class="sxs-lookup"><span data-stu-id="15610-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="15610-149">Selecione o * * pessoa * * entity e pressione **Ctrl + V** chaves.</span><span class="sxs-lookup"><span data-stu-id="15610-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="15610-150">Na superfície de design, selecione o **PersonInfo** entidade e pressione **excluir** no teclado.</span><span class="sxs-lookup"><span data-stu-id="15610-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="15610-151">Clique em **nenhuma** quando perguntado se você deseja remover o **PersonInfo** tabela do modelo, estamos prestes a mapeá-lo para o **pessoa** entidade.</span><span class="sxs-lookup"><span data-stu-id="15610-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![DeleteTables](~/ef6/media/deletetables.png)

<span data-ttu-id="15610-153">As próximas etapas exigem o **Mapping Details** janela.</span><span class="sxs-lookup"><span data-stu-id="15610-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="15610-154">Se você não vir essa janela, clique com botão direito a superfície de design e selecione **Mapping Details**.</span><span class="sxs-lookup"><span data-stu-id="15610-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="15610-155">Selecione o **pessoa** tipo de entidade e clique em **&lt;adicionar uma tabela ou exibição&gt;** no **Mapping Details** janela.</span><span class="sxs-lookup"><span data-stu-id="15610-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="15610-156">Selecione * * PersonInfo * * na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="15610-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="15610-157">O **Mapping Details** janela é atualizada com mapeamentos de coluna padrão, essas são boas para nosso cenário.</span><span class="sxs-lookup"><span data-stu-id="15610-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="15610-158">O **pessoa** tipo de entidade agora está mapeado para o **pessoa** e **PersonInfo** tabelas.</span><span class="sxs-lookup"><span data-stu-id="15610-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapping2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="15610-160">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="15610-160">Use the Model</span></span>

-   <span data-ttu-id="15610-161">Cole o seguinte código no método Main.</span><span class="sxs-lookup"><span data-stu-id="15610-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="15610-162">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="15610-162">Compile and run the application.</span></span>

<span data-ttu-id="15610-163">Instruções T-SQL a seguir foram executadas no banco de dados como resultado da execução desse aplicativo.</span><span class="sxs-lookup"><span data-stu-id="15610-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="15610-164">Os dois seguintes **inserir** instruções foram executadas como resultado da execução de contexto. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="15610-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="15610-165">Eles levam os dados a partir o **pessoa** entidade e dividi-la entre a **pessoa** e **PersonInfo** tabelas.</span><span class="sxs-lookup"><span data-stu-id="15610-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Insert1](~/ef6/media/insert1.png)

    ![Insert2](~/ef6/media/insert2.png)
-   <span data-ttu-id="15610-168">O seguinte **selecionar** foi executado como resultado de enumerar as pessoas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15610-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="15610-169">Ele combina os dados a partir de **pessoa** e **PersonInfo** tabela.</span><span class="sxs-lookup"><span data-stu-id="15610-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Selecionar](~/ef6/media/select.png)
